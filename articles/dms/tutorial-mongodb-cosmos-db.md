---
title: Руководство по переносу MongoDB в API Azure Cosmos DB для MongoDB в автономном режиме
titleSuffix: Azure Database Migration Service
description: Перенесите локальный экземпляр MongoDB в API Azure Cosmos DB для MongoDB в автономном режиме с помощью Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 02/03/2021
ms.openlocfilehash: 8977bb90087d9d3d4962d7eda50903d97da93539
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106443873"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-db-api-for-mongodb-offline"></a>Руководство по переносу MongoDB в API Azure Cosmos DB для MongoDB в автономном режиме

С помощью Azure Database Migration Service можно выполнять автономную однократную миграцию баз данных из локального или облачного экземпляра MongoDB в API Azure Cosmos DB для MongoDB.

В этом руководстве вы узнаете, как:
> [!div class="checklist"]
>
> * Создайте экземпляр Azure Database Migration Service.
> * создание проекта миграции с использованием Azure Database Migration Service.
> * выполнение миграции.
> * Мониторинг миграции.

В этом руководстве вы перенесете набор данных в MongoDB, находящийся в виртуальной машине Azure. Для переноса набора данных в API Azure Cosmos DB для MongoDB вы воспользуетесь службой Azure Database Migration Service. Если у вас еще не настроен источник MongoDB, обратитесь к разделу [Установка и настройка базы данных MongoDB на виртуальной машине Windows в Azure](/previous-versions/azure/virtual-machines/windows/install-mongodb).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуется следующее:

* [Завершите шаги, которые необходимо выполнить перед миграцией](../cosmos-db/mongodb-pre-migration.md), включая оценку пропускной способности и выбор ключа секции.
* [Создайте учетную запись для API Azure Cosmos DB для MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
* Создайте виртуальную сеть Microsoft Azure для Azure Database Migration Service, используя Azure Resource Manager. Эта модель развертывания предоставляет возможность подключения типа "сеть-сеть" к локальным исходным серверам с помощью [Azure ExpressRoute](../expressroute/expressroute-introduction.md) или [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md). Дополнительные сведения о создании виртуальной сети см. в статье [Документация по виртуальной сети Azure](../virtual-network/index.yml). Уделите особое внимание кратким руководствам с пошаговыми инструкциями.

    > [!NOTE]
    > Если вы используете ExpressRoute с пиринговым подключением к сети, управляемой Майкрософт, во время настройки виртуальной сети добавьте в подсеть, в которой будет подготовлена служба, следующие [конечные точки](../virtual-network/virtual-network-service-endpoints-overview.md):
    >
    > * целевую конечную точку базы данных (например, конечная точка SQL, конечная точка Azure Cosmos DB и т. д.);
    > * конечную точку службы хранилища;
    > * конечную точку служебной шины.
    >
    > Такая конфигурация вызвана тем, что у Azure Database Migration Service нет подключения к Интернету.

* Убедитесь, что правила группы безопасности сети (NSG) для виртуальной сети не блокируют следующие порты: 53, 443, 445, 9354 и 10000–20000. Дополнительные сведения см. в статье [Фильтрация сетевого трафика с помощью групп безопасности сети](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Откройте брандмауэр Windows, чтобы предоставить Azure Database Migration Service доступ к исходному серверу MongoDB. По умолчанию это TCP-порт 27017.
* Если перед базой данных-источником развернуто устройство брандмауэра, вам может потребоваться добавить правила брандмауэра, чтобы разрешить службе Azure Database Migration Service обращаться к базе данных-источнику для выполнения миграции.

## <a name="configure-the-server-side-retry-feature"></a>Настройка функции повторных попыток на стороне сервера

При миграции с MongoDB на Azure Cosmos DB можно воспользоваться преимуществами возможностей по управлению ресурсами. Эти возможности позволяют полностью использовать подготовленные единицы запроса пропускной способности (ЕЗ/с). Во время миграции Azure Cosmos DB может отрегулировать определенный запрос к службе Database Migration Service, если размер этого запроса превышает число подготовленных ЕЗ/с контейнера. Такой запрос будет нужно повторить.

Служба Database Migration Service может выполнять повторные попытки. Важно понимать, что время кругового пути для сетевого прыжка между Database Migration Service и Azure Cosmos DB влияет на общее время отклика этого запроса. Оптимизация времени отклика для регулируемых запросов может сократить общее время, необходимое для миграции.

Функция повторных попыток на стороне сервера Azure Cosmos DB позволяет службе перехватывать коды ошибок регулирования и повторять попытки с уменьшенным временем кругового пути, что значительно сокращает время отклика для запроса.

Чтобы использовать повторные попытки на стороне сервера, на портале Azure Cosmos DB выберите **Компоненты** > **Повторные попытки на стороне сервера**.

![Снимок экрана, на котором показано, где находится функция повторных попыток на стороне сервера.](media/tutorial-mongodb-to-cosmosdb/mongo-server-side-retry-feature.png)

Если эта функция отключена, выберите **Включить**.

![Снимок экрана, на котором показано, как включить функцию повторных попыток на стороне сервера.](media/tutorial-mongodb-to-cosmosdb/mongo-server-side-retry-enable.png)

## <a name="register-the-resource-provider"></a>Регистрация поставщика ресурсов

1. Войдите на портал Azure, щелкните **Все службы** и выберите **Подписки**.

   ![Снимок экрана, на котором показаны подписки на портале.](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)

2. Выберите подписку, в которой нужно создать экземпляр Azure Database Migration Service, а затем щелкните **Поставщики ресурсов**.

    ![Снимок экрана, на котором показаны поставщики ресурсов.](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)

3. В поле поиска введите migration, а затем справа от **Microsoft.DataMigration** щелкните **Зарегистрировать**.

    ![Снимок экрана, на котором показано, как зарегистрировать поставщика ресурсов.](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Создание экземпляра

1. На портале Azure выберите **+Создать ресурс**, введите в поле поиска "Azure Database Migration Service", а затем в раскрывающемся списке выберите **Azure Database Migration Service**.

    ![Снимок экрана: Azure Marketplace.](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2. На экране **Azure Database Migration Service** выберите **Создать**.

    ![Снимок экрана, на котором показано, как создать экземпляр Azure Database Migration Service.](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3. На экране **Создание службы миграции** укажите имя службы, подписку и новую или существующую группу ресурсов.

4. Выберите расположение, в котором нужно создать экземпляр Azure Database Migration Service. 

5. Выберите существующую виртуальную сеть или создайте новую.

    Виртуальная сеть предоставляет Azure Database Migration Service доступ к исходному экземпляру MongoDB и целевой учетной записи Azure Cosmos DB.

    Сведения о том, как создать виртуальную сеть на портале Azure, см. в разделе [Создание виртуальной сети с помощью портала Azure](../virtual-network/quick-create-portal.md).

6. Выберите ценовую категорию.

    Дополнительные сведения о ценовых категориях и затратах см. на [странице с описанием цен](https://aka.ms/dms-pricing).

    ![Снимок экрана, на котором показаны параметры конфигурации для экземпляра Azure Database Migration Service.](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7. Выберите **Создать**, чтобы создать службу.

## <a name="create-a-migration-project"></a>Создание проекта миграции

После создания службы найдите и откройте ее на портале Azure. Затем создайте проект миграции.

1. На портале Azure щелкните **Все службы**, выполните поиск по запросу "Azure Database Migration Service" и выберите **Azure Database Migration Services** (Службы Azure Database Migration Service).

      ![Снимок экрана, на котором показано, как найти все экземпляры Azure Database Migration Service.](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. На экране **Службы миграции баз данных Azure** найдите и выберите имя созданного экземпляра Azure Database Migration Service.

3. Выберите **+ New Migration Project** (+ Новый проект миграции).

4. На странице **Новый проект миграции** укажите имя проекта и в текстовом поле **Тип исходного сервера** выберите **MongoDB**. В текстовом поле **Тип целевого сервера** выберите **CosmosDB (API MongoDB)** , а для параметра **Выберите тип действия** выберите значение **Автономная миграция данных**. 

    ![Снимок экрана, на котором показаны параметры проекта.](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5. Выберите **Создать и выполнить действие**, чтобы создать проект и выполнить действие миграции.

## <a name="specify-source-details"></a>Указание сведений об источнике

1. На экране **Сведения об источнике** задайте сведения о подключении для исходного сервера MongoDB.

   > [!IMPORTANT]
   > Azure Database Migration Service не поддерживает Azure Cosmos DB в качестве источника.

    Доступно три режима для подключения к источнику:
   * **Стандартный режим**, в котором принимается полное доменное имя или IP-адрес, номер порта и учетные данные для подключения.
   * **Режим строки подключения**, в котором принимается строка подключения MongoDB, как описано в разделе [Формат URI строки подключения](https://docs.mongodb.com/manual/reference/connection-string/).
   * **Данные из службы хранилища Azure**, в котором принимается URL-адрес SAS контейнера BLOB-объектов. Выберите **BLOB-объект содержит дампы BSON**, если в контейнере BLOB-объектов есть дампы BSON, созданные [средством bsondump](https://docs.mongodb.com/manual/reference/program/bsondump/). Не выбирайте этот параметр, если контейнер содержит файлы JSON.

     Если вы выбрали этот параметр, убедитесь, что строка подключения к учетной записи хранения имеет следующий формат:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Эту строку подключения SAS к контейнеру BLOB-объектов можно найти в Обозревателе службы хранилища Azure. Создание SAS для нужного контейнера позволит вам получить URL-адрес в необходимом формате.
     
     Кроме того, учитывайте следующие сведения, соответствующие данным о типе дампа в службе хранилища Azure:

     * Для дампов BSON данные в контейнере BLOB-объектов должны иметь формат bsondump. Поместите файлы данных в папки, имена которых соответствуют содержащим их базам данных, в формате *collection.bson*. Укажите имена для файлов метаданных, используя формат *collection.metadata.json*.

     * Для дампов JSON файлы в контейнере больших двоичных объектов должны размещаться в папках с именами содержащих баз данных. В каждой папке баз данных файлы данных должны быть помещены в подпапку с именем *data*. Имена для этих файлов необходимо задать с использованием формата *collection.json*. Поместите все файлы метаданных в подпапку с именем *metadata*. Имена для этих файлов необходимо задать с использованием такого же формата *collection.json*. Файлы метаданных должны быть в том же формате, что и файлы, созданные инструментом bsondump MongoDB.

    > [!IMPORTANT]
    > Мы не рекомендуем использовать самозаверяющий сертификат на сервере MongoDB. Но если он все же применяется, подключитесь к серверу в режиме строки подключения и убедитесь, что в строке подключения используются двойные кавычки ("").
    >
    >```
    >&sslVerifyCertificate=false
    >```

   Также можно использовать IP-адрес в ситуациях, когда разрешение имен DNS невозможно.

   ![Снимок экрана, на котором показаны сведения об источнике.](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. Щелкните **Сохранить**.

## <a name="specify-target-details"></a>Указание сведений о цели

1. На экране **Сведения о целевом объекте миграции** укажите сведения о подключении для целевой учетной записи Azure Cosmos DB. Эта учетная запись является предварительно подготовленной учетной записью API Azure Cosmos DB для MongoDB, в которую переносятся данные MongoDB.

    ![Снимок экрана, на котором показаны сведения о целевом объекте.](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. Щелкните **Сохранить**.

## <a name="map-to-target-databases"></a>Сопоставление с целевыми базами данных

1. На экране **Map to target databases** (Сопоставить с целевыми базами данных) сопоставьте исходную и целевую базы данных для миграции.

    Если в целевой базе данных содержится база данных с тем же именем, что у исходной базы данных, Azure Database Migration Service по умолчанию выберет целевую базу данных.

    Если рядом с именем базы данных отображается строка **Создать**, это означает, что Azure Database Migration Service не удалось найти целевую базу данных и служба автоматически создаст базу данных.

    На этом этапе миграции можно [подготовить пропускную способность](../cosmos-db/set-throughput.md). В Azure Cosmos DB пропускную способность можно подготовить на уровне базы данных или индивидуально для каждой коллекции. Пропускная способность измеряется в [единицах запроса](../cosmos-db/request-units.md). Дополнительные сведения о ценах Azure Cosmos DB см. [здесь](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Снимок экрана, на котором показано сопоставление с целевыми базами данных.](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. Щелкните **Сохранить**.
3. На экране **Параметр коллекции** разверните список коллекций и просмотрите коллекции, которые будут перенесены.

    Azure Database Migration Service автоматически выбирает все коллекции, которые существуют в исходном экземпляре MongoDB и отсутствуют в целевой учетной записи Azure Cosmos DB. Чтобы повторно перенести коллекции, уже содержащие данные, необходимо явным образом выбрать их в этой области.

    Вы можете указать количество ЕЗ, которое необходимо использовать коллекциям. Azure Database Migration Service предлагает интеллектуальные значения по умолчанию с учетом размера коллекции.

    > [!NOTE]
    > Выполняйте миграцию базы данных и коллекции параллельно. При необходимости можно использовать несколько экземпляров Azure Database Migration Service для ускорения миграции.

    Вы можете также указать ключ сегмента, чтобы воспользоваться преимуществами [секционирования в Azure Cosmos DB](../cosmos-db/partitioning-overview.md) для обеспечения наилучшей масштабируемости. Просмотрите [рекомендации по выбору ключа сегмента и секции](../cosmos-db/partitioning-overview.md#choose-partitionkey).

    ![Снимок экрана, на котором показан выбор таблиц коллекций.](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. Щелкните **Сохранить**.

5. На экране **Сводка по миграции** в текстовом поле **Имя активности** задайте имя действия миграции.

    ![Снимок экрана, на котором показана сводка миграции.](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>Выполнение миграции

Выберите **Запустить миграцию**. Появится окно действия миграции, в котором будет указано состояние действия **Не запущено**.

![Снимок экрана, на котором показано состояние действия.](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Мониторинг миграции

На экране действия миграции нажимайте кнопку **Обновить**, чтобы обновить содержимое экрана до тех пор, пока состояние миграции не изменится на **Завершено**.

> [!NOTE]
> Вы можете выбрать действие, чтобы получить сведения о метриках миграции на уровне базы данных и на уровне коллекции.

![Снимок экрана, на котором показано состояние действия "Завершено".](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-azure-cosmos-db"></a>Проверка данных в Azure Cosmos DB

После завершения миграции вы можете проверить свою учетную запись Azure Cosmos DB, чтобы убедиться, что все коллекции успешно перенесены.

![Снимок экрана: проверка учетной записи Azure Cosmos DB, чтобы убедиться, что все коллекции перенесены](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="post-migration-optimization"></a>Оптимизация после переноса

Для управления данными, перенесенными из базы данных MongoDB в API Azure Cosmos DB для MongoDB, можно подключиться к Azure Cosmos DB. Также можно выполнить другие действия по оптимизации после миграции. К ним могут относиться оптимизация политики индексирования, обновление уровня согласованности по умолчанию или настройка глобального распределения для учетной записи Azure Cosmos DB. Дополнительные сведения см. в разделе [Оптимизация после миграции](../cosmos-db/mongodb-post-migration.md).

## <a name="next-steps"></a>Дальнейшие действия

Инструкции по миграции для других сценариев см. в [Руководстве по миграции базы данных Azure](https://datamigration.microsoft.com/).



