---
title: Модели ценообразования для Azure Cosmos DB
description: В этой статье объясняется, как работает модель ценообразования Azure Cosmos DB и как она упрощает управление затратами и планирование затрат.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 573fc4fac413ceed50246bc6fb8df1d9db021c94
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98247477"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Модели ценообразования в Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Модель ценообразования Azure Cosmos DB упрощает управление затратами и планирование затрат. С Azure Cosmos DB вы платите за операции, выполняемые с базой данных, и за хранилище, используемое данными.

- **Операции с базой данных**. способ оплаты за операции с базой данных зависит от типа используемой учетной записи Azure Cosmos.

  - **Подготовленная пропускная способность**. [подготовленная](set-throughput.md) пропускная способность (также называемая зарезервированной пропускной способностью) обеспечивает высокую производительность в любом масштабе. Вы указываете необходимую пропускную способность в [единицах запросов](request-units.md) в секунду, а Azure Cosmos DB выделяет ресурсы, необходимые для предоставления настроенной пропускной способности. Вы можете [подготовить пропускную способность для базы данных или контейнера](set-throughput.md). В зависимости от потребностей рабочей нагрузки можно масштабировать пропускную способность в любое время или использовать [Автомасштабирование](provision-throughput-autoscale.md) (хотя для базы данных или контейнера требуется минимальная пропускная способность, чтобы гарантировать соглашение об уровне обслуживания). С вас взимается плата за максимальную подготовленную пропускную способность в течение каждого часа.

   > [!NOTE]
   > Так как подготовленная модель пропускной способности выделяет ресурсы для контейнера или базы данных, вам будет выставляться тариф за пропускную способность, подготовленную даже в том случае, если не выполняются какие-либо рабочие нагрузки.

  - **Бессерверные**: в режиме без [сервера](serverless.md) вам не нужно подготавливать пропускную способность при создании ресурсов в учетной записи Azure Cosmos. По окончании расчетного периода вы получаете счет за количество единиц запросов, использованных операциями базы данных.

- **Хранилище**. Вы оплачиваете по фиксированной ставке общий объем хранилища (в ГБ), потребляемый данными и индексами за указанный час. Плата за хранилище взимается по принципу потребления, поэтому вам не нужно резервировать хранилище заранее. Счет выставляется только за фактически потребляемый объем хранилища.

Модель ценообразования в Azure Cosmos DB одинакова для всех API. Дополнительные сведения см. на [странице цен на Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/), [сведения о Azure Cosmos DB счете](understand-your-bill.md) и [о том, как Azure Cosmos DBная модель ценообразования экономична для клиентов](total-cost-ownership.md).

При развертывании учетной записи Azure Cosmos DB в регионе, отличном от государственного, в США существует минимальная цена для пропускной способности на основе базы данных и контейнера в подготовленном режиме пропускной способности. Минимальная цена в бессерверном режиме отсутствует. Цены зависят от используемого региона. Дополнительные сведения о ценах см. на [странице цен на Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) .

## <a name="try-azure-cosmos-db-for-free"></a>Бесплатная пробная версия Azure Cosmos DB

Azure Cosmos DB предлагает разработчикам множество вариантов для бесплатной разработки. Вот какие параметры доступны:

* **Azure Cosmos DB уровня "бесплатный**". Azure Cosmos DB Free позволяет легко приступить к работе, разрабатывать и тестировать приложения, а также запускать небольшие рабочие нагрузки бесплатно. Если для учетной записи включен уровень Free, то в течение срока действия учетной записи вы получите первые 400 единиц запросов в секунду и 5 ГБ хранилища в учетной записи. У вас может быть до одной учетной записи уровня Free на одну подписку Azure, и при ее создании необходимо явно включить учетную запись. Чтобы начать работу, [Создайте новую учетную запись в портал Azure с включенным уровнем Free](create-cosmosdb-resources-portal.md) или используйте [шаблон ARM](./manage-with-templates.md#free-tier).

* **Бесплатная учетная запись Azure**. Azure предлагает [бесплатный уровень](https://azure.microsoft.com/free/) , который предоставляет $200 в кредитах Azure за первые 30 дней и ограниченное количество бесплатных служб в течение 12 месяцев. Дополнительные сведения см. на странице [создания бесплатной учетной записи Azure](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB предоставляется вместе с бесплатной учетной записью Azure. В частности, для Azure Cosmos DB в этой бесплатной учетной записи предлагается 25 ГБ хранилища и 400 единиц запросов в секунду для подготовленной пропускной способности в течение всего года.

* **Попробуйте Azure Cosmos DB бесплатно**: Azure Cosmos DB предлагает ограниченный срок действия с помощью пробной Azure Cosmos DB для бесплатных учетных записей. Вы можете создать учетную запись Azure Cosmos DB, создать в ней базу данных и коллекции, а затем выполнять примеры приложений по руководствам из нашей документации. Для запуска примера приложения не требуется подписка на учетную запись Azure или ввод данных кредитной карты. [Бесплатная пробная версия Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) позволяет использовать Azure Cosmos DB в течение одного месяца с возможностью продления учетной записи неограниченное количество раз.

* **Эмулятор Azure Cosmos DB**. эмулятор Azure Cosmos DB предоставляет локальную среду, которая эмулирует службу Azure Cosmos DB для целей разработки. Эмулятор для облачной службы предоставляется бесплатно и с высокой точностью. С помощью эмулятора Azure Cosmos DB вы можете локально разрабатывать и тестировать приложения, не создавая подписку Azure и не тратя средства. Вы можете разрабатывать приложения с помощью эмулятора локально перед переходом в рабочую среду. Когда функциональность приложения в эмуляторе будет вас полностью устраивать, вы сможете перейти на использование учетной записи Azure Cosmos DB в облаке, значительно экономя средства. См. дополнительные сведения о [разработке и тестировании с помощью Azure Cosmos DB](local-emulator.md).

## <a name="pricing-with-reserved-capacity"></a>Цены на зарезервированную мощность

Azure Cosmos DB [зарезервированная емкость](cosmos-db-reserved-capacity.md) позволяет экономить деньги при использовании подготовленного режима пропускной способности, Предварительная оплата за ресурсы Azure Cosmos DB в течение одного года или за три года. Предоплата на один или три года позволяет снизить общие расходы на 20–65 % по сравнению с обычными ценами. Зарезервированная мощность Azure Cosmos DB позволяет снизить затраты, предварительно оплатив подготовленную пропускную способность (ЕЗ/с) на период в один или три года, получая скидку на оплату подготовленной пропускной способности. 

Зарезервированная мощность позволяет использовать скидку на оплату, но не влияет на состояние среды выполнения для ресурсов Azure Cosmos DB. Зарезервированная мощность стабильно предоставляется для всех API, в том числе MongoDB, Cassandra, SQL, Gremlin и таблиц Azure, во всех регионах по всему миру. Дополнительные сведения о зарезервированной мощности см. в статье [Предоплата ресурсов Azure Cosmos DB с резервной мощностью](cosmos-db-reserved-capacity.md), а приобрести ее можно на [портале Azure](https://portal.azure.com/).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об оптимизации затрат для ресурсов Azure Cosmos DB см. в следующих статьях:

* [Optimizing for development and testing in Azure Cosmos DB](optimize-dev-test.md) (Оптимизация для разработки и тестирования в Azure Cosmos DB)
* Дополнительные сведения о [расшифровке счета Azure Cosmos DB](understand-your-bill.md)
* Дополнительные сведения об [оптимизации расходов на пропускную способность](optimize-cost-throughput.md)
* Дополнительные сведения об [оптимизации расходов на хранилище](optimize-cost-storage.md)
* Дополнительные сведения об [оптимизации расходов на операции чтения и записи](optimize-cost-reads-writes.md)
* Дополнительные сведения об [оптимизации затрат на запросы](./optimize-cost-reads-writes.md).
* Дополнительные сведения об [оптимизации расходов на учетные записи Cosmos с поддержкой нескольких регионов](optimize-cost-regions.md)
* Дополнительные сведения [о резервной мощности Azure Cosmos DB](cosmos-db-reserved-capacity.md).
* Дополнительные сведения [об эмуляторе Azure Cosmos DB](local-emulator.md).