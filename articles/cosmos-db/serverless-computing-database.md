---
title: Бессерверные вычисления в базе данных с помощью функций Azure Cosmos DB и Azure
description: Узнайте, как можно использовать Azure Cosmos DB и служб "Функции Azure" для создания бессерверных вычислительных приложений, управляемых событиями.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 07/17/2019
ms.author: sngun
ms.openlocfilehash: 73a34cc27eaba33d04f4d31585c7f494f58e7274
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "93334080"
---
# <a name="serverless-database-computing-using-azure-cosmos-db-and-azure-functions"></a>Обработка данных бессерверных баз данных с помощью Azure Cosmos DB и Функций Azure
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Бессерверные вычисления дают возможность сосредоточиться на отдельных повторяемых элементах логики без отслеживания состояния. Эти элементы не требуют управления инфраструктурой и потребляют ресурсы в течение считанных секунд или миллисекунд, пока выполняются. В основе бессерверных вычислений лежат функции, которые предоставляются в экосистеме Azure посредством службы [Функции Azure](https://azure.microsoft.com/services/functions). Дополнительные сведения о других бессерверных средах выполнения в Azure доступны на странице [Бессерверные приложения в Azure](https://azure.microsoft.com/solutions/serverless/). 

Благодаря естественной интеграции [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) и службы "Функции Azure", можно создавать триггеры базы данных, входные привязки и выходные привязки непосредственно с помощью учетной записи Azure Cosmos DB. С помощью службы "Функции Azure" и Azure Cosmos DB можно создавать и развертывать бессерверные приложения, управляемые событиями, которые обеспечивают низкую задержку при обращении к сложным данным глобальной базы пользователей.

## <a name="overview"></a>Обзор

Служба "Функции Azure" и Azure Cosmos DB позволяют интегрировать базы данных и бессерверные приложения следующим образом.

* Создание **триггера функций Azure**, управляемых событиями, для Cosmos DB. Этот триггер использует потоки [веб-канала изменений](change-feed.md) для отслеживания изменений в контейнере Azure Cosmos. При внесении изменений в контейнер поток канала изменений отправляется в триггер, который вызывает функцию Azure.
* Кроме того, можно привязать функцию Azure к контейнеру Cosmos для Azure с помощью **входной привязки**. Входные привязки считывают данные из контейнера при выполнении функции.
* Привязка функции к контейнеру Cosmos для Azure с помощью **выходной привязки**. Выходные привязки записывают данные в контейнер после завершения выполнения функции.

> [!NOTE]
> Сейчас триггеры функций Azure, входные привязки и выходные привязки для Cosmos DB поддерживаются только для API SQL. Для всех других API Azure Cosmos DB доступ к базе данных из функции должен осуществляться с использованием статического клиента для API.


На следующей схеме показаны все три способа интеграции. 

:::image type="content" source="./media/serverless-computing-database/cosmos-db-azure-functions-integration.png" alt-text="Как интегрируются служба &quot;Функции Azure&quot; и Azure Cosmos DB" border="false":::

Триггеры, входные и выходные привязки функций Azure для Azure Cosmos DB можно использовать в следующих сочетаниях:

* Триггеры функций Azure для Cosmos DB можно использовать с выходной привязкой к другому контейнеру Azure Cosmos. После того, как функция выполнит действие с элементом в канале изменений, его можно будет записать в другой контейнер (если записать его в тот же контейнер, из которого он поступил, то это может привести к созданию рекурсивного цикла). Также можно использовать триггер функций Azure для Cosmos DB для эффективного переноса всех измененных элементов из одного контейнера в другой, используя выходную привязку.
* Входные и выходные привязки для Azure Cosmos DB можно использовать в одной и той же функции Azure. Это удобно в случаях, когда требуется найти определенные данные с помощью входной привязки, изменить их в функции Azure, а затем сохранить в том же или другом контейнере.
* Входная привязка к контейнеру Cosmos для Azure может использоваться в той же функции, что и триггер функций Azure для Cosmos DB, и может использоваться с выходной привязкой или без нее. С помощью такого сочетания можно передавать актуальные данные курса валют (полученные с помощью входной привязки к контейнеру данных обмена валют) в канал изменений для новых заказов в службе корзины для покупок. Обновленную итоговую сумму для корзины для покупок, в которой учтена конвертация валют, можно записать в третий контейнер с помощью выходной привязки.

## <a name="use-cases"></a>Варианты использования

В следующих вариантах использования демонстрируются несколько способов максимально эффективного использования данных Azure Cosmos DB с помощью подключения данных к функциям Azure, управляемым событиями.

### <a name="iot-use-case---azure-functions-trigger-and-output-binding-for-cosmos-db"></a>Вариант использования Интернета вещей — триггеры и выходные привязки функций Azure для Cosmos DB

В реализациях Интернета вещей, например, можно вызывать функцию при отображении индикатора проверки двигателя в подключенном автомобиле.

**Реализация:** Использование триггера и выходной привязки функций Azure для Cosmos DB

1. **Триггер функций Azure для Cosmos DB** используется для активации событий, относящихся к оповещениям автомобиля, например о том, что в подключенном автомобиле находится источник проверки.
2. Когда загорается индикатор проверки двигателя, данные датчика отправляются в Azure Cosmos DB.
3. Azure Cosmos DB создает или обновляет новые документы данных датчика, эти изменения передаются в поток для триггера функций Azure для Cosmos DB.
4. Триггер вызывается при каждом изменении данных в коллекции данных датчика, так как все изменения передаются потоком через канал изменений.
5. В функции задано пороговое условие, при достижении которого данные датчика передаются в отдел гарантийного обслуживания.
6. Кроме того, если температура превышает некоторое определенное значение, владельцу отправляется оповещение.
7. **Выходная привязка** функции обновляет запись автомобиля в другом контейнере Azure Cosmos, чтобы сохранить сведения о событии проверки обработчика.

На рисунке ниже приведен код, написанный на портале Azure для этого триггера.

:::image type="content" source="./media/serverless-computing-database/cosmos-db-trigger-portal.png" alt-text="Создание триггера функций Azure для Cosmos DB в портал Azure":::

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Вариант использования в сфере финансов. Триггер таймера и входная привязка

В сфере финансов можно вызывать функцию, когда баланс банковского счета становится меньше определенной суммы.

**Реализация:** триггер таймера с входной привязкой Azure Cosmos DB

1. С помощью [триггера таймера](../azure-functions/functions-bindings-timer.md)можно получить сведения о балансе банковского счета, хранящиеся в контейнере Azure Cosmos, через определенные интервалы времени с помощью **входной привязки**.
2. Если баланс меньше минимального порогового баланса, установленного пользователем, то выполняется действие из функции Azure.
3. Выходная привязка может быть [интеграцией SendGrid](../azure-functions/functions-bindings-sendgrid.md), которая отправляет сообщение из учетной записи службы на адреса электронной почты, указанные для учетных записей с низким балансом.

На следующих рисунках представлен код на портале Azure для этого сценария.

:::image type="content" source="./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png" alt-text="Файл index.js триггера таймера для сценария в сфере финансов":::

:::image type="content" source="./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png" alt-text="Файл Run.csx триггера таймера для сценария в сфере финансов":::

### <a name="gaming-use-case---azure-functions-trigger-and-output-binding-for-cosmos-db"></a>Вариант использования игр — триггеры и выходные привязки функций Azure для Cosmos DB 

В играх при создании пользователя можно выполнить поиск других пользователей, которые могут быть знакомы с ним, с помощью [API Gremlin Azure Cosmos DB](graph-introduction.md). Затем можно записать результаты в Azure Cosmos DB или базу данных SQL для простоты извлечения.

**Реализация:** Использование триггера и выходной привязки функций Azure для Cosmos DB

1. Используя [базу данных Azure Cosmos DB Graph](graph-introduction.md) для хранения всех пользователей, можно создать новую функцию с триггером функций Azure для Cosmos DB. 
2. При вставке нового пользователя вызывается функция, результат которой сохраняется с помощью **выходной привязки**.
3. Функция отправляет запрос к базе данных графа, чтобы найти всех пользователей, которые непосредственно связаны с новым пользователем, и получает этот набор данных.
4. Затем эти данные сохраняются в Azure Cosmos DB, после чего их легко получить любое внешнее приложение, показывающее новому пользователю подключенных друзей.

### <a name="retail-use-case---multiple-functions"></a>Вариант использования в сфере розничной торговли. Несколько функций

В сфере розничной торговли вы можете создать функции для предложения сопроводительных товаров, вызываемые, когда пользователь добавляет товар в свою корзину.

**Реализация:** Несколько триггеров функций Azure для Cosmos DB прослушивания одного контейнера

1. Вы можете создать несколько функций Azure, добавив триггеры функций Azure для Cosmos DB каждый из которых прослушивает один и тот же канал изменений данных корзины покупок. Обратите внимание, если несколько функций прослушивают один канал изменений, для каждой функции требуется новая коллекция аренды. Дополнительные сведения о коллекциях аренды см. в разделе [Основные сведения о библиотеке обработчика канала изменений](change-feed-processor.md).
2. Всякий раз, когда пользователь добавляет товар в корзину для покупок, каждая функция независимо вызывается каналом изменений из контейнера корзины для покупок.
   * Одна функция может использовать текущее содержимое корзины, чтобы изменить отображаемый набор дополнительных товаров, которые могут заинтересовать пользователя.
   * Другая функция может обновлять итоговые данные товаров.
   * Еще одна функция может отправлять сведения об определенных товарах, выбранных клиентом, в отдел маркетинга, который рассылает покупателям рекламу. 

     Любой отдел может создать функции Azure для Cosmos DB путем прослушивания веб-канала изменений и убедитесь, что они не будут откладывать критически важные события обработки заказов.

Во всех этих случаях использования, поскольку функция разделяет само приложение, вам не нужно каждый раз создавать новые экземпляры приложения. Вместо этого служба "Функции Azure" по мере необходимости запускает отдельные функции для выполнения отдельных процессов.

## <a name="tooling"></a>Инструментарий

Собственная интеграция между Azure Cosmos DB и функциями Azure доступна в портал Azure и в Visual Studio 2019.

* На портале функций Azure можно создать триггер. Инструкции для краткого руководства см. [в разделе Создание триггера функций Azure для Cosmos DB в портал Azure](../azure-functions/functions-create-cosmos-db-triggered-function.md).
* На портале Azure Cosmos DB можно добавить триггер функции Azure для Cosmos DB в существующее приложение функции Azure в той же группе ресурсов.
* В Visual Studio 2019 можно создать триггер с помощью [инструментов функций Azure](../azure-functions/functions-develop-vs.md):

    >[!VIDEO https://www.youtube.com/embed/iprndNsUeeg]

## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>В чем преимущества интеграции службы "Функции Azure" для бессерверных вычислений?

Служба "Функции Azure" дает возможность создавать масштабируемые единицы работы, или компактные элементы логики, которые могут выполняться по требованию, без подготовки ресурсов или управления инфраструктурой. Используя функции Azure, вам не нужно создавать полнофункциональное приложение для реагирования на изменения в базе данных Cosmos Azure. Вы можете создавать небольшие многократно используемые функции для конкретных задач. Кроме того, можно использовать данные Azure Cosmos DB как входные или выходные данные функции Azure, передаваемые в ответ на событие, например HTTP-запросы или триггер таймера.

Azure Cosmos DB является рекомендуемой базой данных для бессерверной вычислительной архитектуры по следующим причинам.

* **Мгновенный доступ ко всем данным**. У вас есть детализированный доступ к каждому хранимому значению, так как Azure Cosmos DB [автоматически индексирует](index-policy.md) все данные по умолчанию и немедленно предоставляет эти индексы. Это означает, что вы можете непрерывно запрашивать и обновлять имеющиеся элементы в базе данных, а также добавлять в нее новые элементы, используя мгновенный доступ с помощью службы "Функции Azure".

* **Отсутствие схем**. В Azure Cosmos DB нет схем, поэтому предоставляется уникальная возможность обрабатывать любые выходные данные функции Azure. Такой подход "обработка чего угодно" упрощает создание разнообразных функций, выводящих данные в Azure Cosmos DB.

* **Масштабируемая пропускная способность**. Пропускная способность в Azure Cosmos DB может масштабироваться мгновенно. Если у вас есть сотни или тысячи функций, которые отправляют запросы и записывают данные в один и тот же контейнер, можно увеличить число [единиц запроса в секунду](request-units.md), чтобы справиться с нагрузкой. Все функции могут работать параллельно, используя выделенное количество единиц запроса в секунду, при этом [согласованность](consistency-levels.md) данных гарантируется.

* **Глобальная репликация**. Можно выполнять репликацию данных Azure Cosmos DB [по всему миру](distribute-data-globally.md), чтобы сократить задержку, находя и предоставляя данные, расположенные ближе всего к вашим пользователям. Как и для всех запросов к Azure Cosmos DB, данные из триггеров, управляемых событиями, считываются из базы данных Azure Cosmos DB, расположенной ближе всего к пользователю.

Если вы собираетесь интегрировать службу "Функции Azure" для хранения данных и вам не нужно глубокое индексирование или если вам необходимо сохранять вложения и файлы мультимедиа, [триггер хранилища BLOB-объектов Azure](../azure-functions/functions-bindings-storage-blob.md) может быть лучшим вариантом.

Преимущества службы "Функции Azure": 

* **Управление событиями**. Служба "Функции Azure" управляется событиями и может ожидать передачи данных из канала изменений Azure Cosmos DB. Это означает, что вам не нужно создавать логику ожидания передачи данных, и можно просто следить за интересующими изменениями. 

* **Нет ограничений**. Функции выполняются в параллельном режиме, и служба запускает столько экземпляров, сколько требуется. Вы можете настроить соответствующие параметры.

* **Хорошо подходит для быстрых задач**. Служба запускает новые экземпляры функций всякий раз, когда происходит событие, и закрывает их сразу же после выполнения функции. Вы платите только за время выполнения функций.

Если вы не уверены, что подойдет для вашей реализации: Flow, Logic Apps, служба "Функции Azure" или веб-задания, ознакомьтесь с разделом [Сравнение Microsoft Flow, Logic Apps, функций и веб-заданий Azure](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="next-steps"></a>Дальнейшие действия

Теперь можно подключить Azure Cosmos DB и службу "Функции Azure" по-настоящему: 

* [Создание триггера функций Azure для Cosmos DB в портал Azure](../azure-functions/functions-create-cosmos-db-triggered-function.md)
* [Создание триггера HTTP в Функциях Azure с помощью входной привязки Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb.md?tabs=csharp)
* [Привязки и триггеры Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2.md)
