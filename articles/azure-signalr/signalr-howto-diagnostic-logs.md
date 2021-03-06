---
title: Журналы ресурсов для службы SignalR Azure
description: Узнайте, как настроить журналы ресурсов для службы Azure SignalR и как использовать их для самостоятельного устранения неполадок.
author: wanlwanl
ms.service: signalr
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: wanl
ms.openlocfilehash: 5650ff0e039d1e9211b8d0013726e101efdfab78
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "100572256"
---
# <a name="resource-logs-for-azure-signalr-service"></a>Журналы ресурсов для службы SignalR Azure

В этом учебнике рассматриваются журналы ресурсов для службы Azure SignalR, способы их настройки и способы устранения неполадок. 

## <a name="prerequisites"></a>Предварительные условия
Чтобы включить журналы ресурсов, вам понадобится место для хранения данных журнала. В этом руководстве используется служба хранилища Azure и Log Analytics.

* Служба [хранилища Azure](../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) — сохранение журналов ресурсов для аудита политики, статического анализа или резервного копирования.
* [Log Analytics](../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace) — гибкий инструмент поиска по журналам и анализа, позволяющий анализировать необработанные журналы, созданные ресурсом Azure.

## <a name="set-up-resource-logs-for-an-azure-signalr-service"></a>Настройка журналов ресурсов для службы Azure SignalR

Вы можете просмотреть журналы ресурсов для службы Azure SignalR. Эти журналы предоставляют более подробные сведения о подключении к экземпляру службы Azure SignalR. В журналах ресурсов содержатся подробные сведения о каждом подключении. Например, основные сведения (идентификатор пользователя, идентификатор соединения и тип транспорта и т. д.) и сведения о событии (подключение, отключение и прерывание события и т. д.) соединения. Журналы ресурсов можно использовать для идентификации проблем, отслеживания и анализа подключений.

### <a name="enable-resource-logs"></a>Включение журналов ресурсов

Журналы ресурсов по умолчанию отключены. Чтобы включить журналы ресурсов, выполните следующие действия.

1. В [портал Azure](https://portal.azure.com)в разделе **мониторинг** щелкните **параметры диагностики**.

    ![Переход к панели с параметрами диагностики](./media/signalr-tutorial-diagnostic-logs/diagnostic-settings-menu-item.png)

1. Затем щелкните **Добавить параметр диагностики**.

    ![Добавление журналов ресурсов](./media/signalr-tutorial-diagnostic-logs/add-diagnostic-setting.png)

1. Задайте нужный целевой объект архива. Сейчас мы поддерживаем **архивацию в учетную запись хранения** и **отправляем log Analytics**.

1. Выберите журналы, которые необходимо архивировать.

    ![Панель параметров диагностики](./media/signalr-tutorial-diagnostic-logs/diagnostics-settings-pane.png)


1. Сохраните новые параметры диагностики.

Новые параметры вступят в силу в течение 10 минут. После этого журналы появятся в настроенной цели для архивирования на панели **Журналы диагностики**.

Дополнительные сведения о настройке диагностики см. в [обзоре журналов ресурсов Azure](../azure-monitor/essentials/platform-logs-overview.md).

### <a name="resource-logs-categories"></a>Категории журналов ресурсов

Служба Azure SignalR захватывает журналы ресурсов в одной категории:

* **Все журналы**: отслеживание подключений, подключающихся к службе Azure SignalR. Журналы содержат сведения о подключении и отключении, проверке подлинности и регулировании. Дополнительные сведения см. в следующем разделе.

### <a name="archive-to-a-storage-account"></a>"Архивировать в учетной записи хранения";

Журналы хранятся в учетной записи хранения, настроенной на панели **журналы диагностики** . Контейнер с именем `insights-logs-alllogs` создается автоматически для хранения журналов ресурсов. Внутри контейнера журналы хранятся в файле `resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX/y=YYYY/m=MM/d=DD/h=HH/m=00/PT1H.json` . По сути, путь объединяется с `resource ID` и `Date Time` . Файлы журнала разделены на `hour` . Таким образом, минуты всегда будут иметь значение `m=00` .

Все журналы хранятся в формате JSON (нотация объектов JavaScript). Каждая запись содержит строковые поля, использующие формат, описанный в следующих разделах.

Строки журнала архива JSON включают элементы, перечисленные в следующих таблицах:

**Формат**

Имя | Описание
------- | -------
time | Регистрировать время события
уровень | Уровень событий Log
resourceId | Идентификатор ресурса службы Azure SignalR
location | Расположение службы Azure SignalR
категория | Категория события журнала
operationName | Имя операции для события
callerIpAddress | IP-адрес сервера или клиента
properties | Подробные свойства, связанные с этим событием журнала. Дополнительные сведения см. в таблице свойств ниже.

**Таблица свойств**

Имя | Описание
------- | -------
type | Тип события журнала. В настоящее время мы предоставляем сведения о подключении к службе Azure SignalR. `ConnectivityLogs`Доступен только тип
коллекция | Коллекция событий журнала. Допустимые значения: `Connection` `Authorization` и `Throttling`
connectionId | Удостоверение подключения
transportType | Тип транспорта соединения. Допустимые значения: `Websockets` \| `ServerSentEvents` \|`LongPolling`
connectionType | Тип подключения. Допустимые значения: `Server` \| `Client`. `Server`: соединение со стороны сервера; `Client`: подключение со стороны клиента
userId | Удостоверение пользователя
message | Подробное сообщение о событии журнала

Ниже приведен пример строки JSON журнала архивирования.

```json
{
    "properties": {
        "message": "Entered Serverless mode.",
        "type": "ConnectivityLogs",
        "collection": "Connection",
        "connectionId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "userId": "User",
        "transportType": "WebSockets",
        "connectionType": "Client"
    },
    "operationName": "ServerlessModeEntered",
    "category": "AllLogs",
    "level": "Informational",
    "callerIpAddress": "xxx.xxx.xxx.xxx",
    "time": "2019-01-01T00:00:00Z",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX",
    "location": "xxxx"
}
```

### <a name="archive-logs-schema-for-log-analytics"></a>Схема архивов журналов для Log Analytics

Чтобы просмотреть журналы ресурсов, выполните следующие действия.

1. Щелкните `Logs` целевой log Analytics.

    ![Пункт меню Log Analytics](./media/signalr-tutorial-diagnostic-logs/log-analytics-menu-item.png)

2. Введите `SignalRServiceDiagnosticLogs` и выберите диапазон времени для запроса журналов ресурсов. Дополнительные сведения см. в статье Начало [работы с log Analytics в Azure Monitor](../azure-monitor/logs/log-analytics-tutorial.md)

    ![Журнал запросов в Log Analytics](./media/signalr-tutorial-diagnostic-logs/query-log-in-log-analytics.png)

Архивные столбцы журнала включают элементы, перечисленные в следующей таблице.

Имя | Описание
------- | ------- 
TimeGenerated | Регистрировать время события
Коллекция | Коллекция событий журнала. Допустимые значения: `Connection` `Authorization` и `Throttling`
OperationName | Имя операции для события
Расположение | Расположение службы Azure SignalR
Level | Уровень событий Log
CallerIpAddress | IP-адрес сервера или клиента
Сообщение | Подробное сообщение о событии журнала
UserId | Удостоверение пользователя
ConnectionId | Удостоверение подключения
ConnectionType | Тип подключения. Допустимые значения: `Server` \| `Client`. `Server`: соединение со стороны сервера; `Client`: подключение со стороны клиента
TransportType | Тип транспорта соединения. Допустимые значения: `Websockets` \| `ServerSentEvents` \|`LongPolling`

### <a name="troubleshooting-with-resource-logs"></a>Устранение неполадок с журналами ресурсов

Чтобы устранить неполадки в службе Azure SignalR, можно включить журналы на стороне сервера или клиента для записи ошибок. В настоящее время служба Azure SignalR предоставляет журналы ресурсов, также можно включить журналы для стороны службы.

При возникновении непредвиденного увеличения или удаления подключения можно воспользоваться журналами ресурсов для устранения неполадок.

Типичные проблемы часто возникают при непредвиденных изменениях количества подключений, при подключении к пределам подключений и при сбое авторизации. Сведения об устранении неполадок см. в следующих разделах.

#### <a name="unexpected-connection-number-changes"></a>Непредвиденные изменения номера подключения

##### <a name="unexpected-connection-dropping"></a>Непредвиденное удаление подключения

Если возникают непредвиденные подключения, сначала включите журналы на стороне службы, сервера и клиента.

Если подключение будет отключено, журналы ресурсов будут регистрировать это событие, которое будет отключено, вы увидите `ConnectionAborted` или `ConnectionEnded` в `operationName` .

Разница между `ConnectionAborted` и заключается в том `ConnectionEnded` , что `ConnectionEnded` ожидается отключение, которое активируется на стороне клиента или сервера. Несмотря на то, что `ConnectionAborted` обычно является непредвиденным событием удаления соединения, а причина прерывания будет предоставлена в `message` .

Причины аварийного завершения перечислены в следующей таблице.

Причина | Описание
------- | ------- 
Число подключений достигло предела | Число подключений достигает предельного значения текущей ценовой категории. Попробуйте увеличить единицу обслуживания
Сервер приложений закрыл подключение | Сервер приложений запускает абортион. Он может рассматриваться как ожидаемый абортион
Время ожидания проверки соединения | Обычно это вызвано проблемой в сети. Попробуйте проверить доступность сервера приложений из Интернета
Перезагрузка службы, повторное подключение | Служба Azure SignalR перегружается. Azure SignalR поддерживает автоматическое повторное подключение. Вы можете дождаться повторного подключения или вручную подключиться к службе Azure SignalR.
Внутренняя ошибка промежуточного сервера | В службе Azure SignalR возникает временная ошибка, которую следует восстановить с автоматического восстановления
Соединение с сервером разорвано | Подключение к серверу прекращается из-за неизвестной ошибки. сначала рекомендуется самостоятельно устранять неполадки с помощью журнала службы/сервера или клиента. Попробуйте исключить основные проблемы (например, проблемы с сетью, неполадки на стороне сервера приложений и т. д.). Если проблема не устранена, обратитесь к нам за помощью. Дополнительные сведения см. в разделе [Получение справки](#get-help) . 

##### <a name="unexpected-connection-growing"></a>Произошло неожиданное подключение

Чтобы устранить неполадки, связанные с непредвиденным подключением, сначала нужно отфильтровать дополнительные подключения. В тестовое подключение клиента можно добавить уникальный идентификатор тестового пользователя. Если вы видите несколько клиентских подключений с одним и тем же ИДЕНТИФИКАТОРом или IP-адресом тестового пользователя, то, скорее всего, на стороне клиента создается и устанавливается больше подключений, чем предполагается. Проверьте клиентскую часть.

#### <a name="authorization-failure"></a>Сбой авторизации

Если для клиентских запросов получено 401 несанкционированный доступ, проверьте журналы ресурсов. Если вы столкнулись `Failed to validate audience. Expected Audiences: <valid audience>. Actual Audiences: <actual audience>` с, это означает, что все аудитории в маркере доступа являются недопустимыми. Попробуйте использовать допустимые аудитории, предложенные в журнале.


#### <a name="throttling"></a>Регулирование

Если вы обнаружите, что вы не можете установить клиентские подключения SignalR в службу Azure SignalR, проверьте журналы ресурсов. При возникновении проблем `Connection count reaches limit` в журнале ресурсов устанавливается слишком много подключений к службе SignalR, что достигает предельного числа подключений. Рассмотрите возможность масштабирования службы SignalR. Если вы сталкиваетесь `Message count reaches limit` с журналом ресурсов, это означает, что вы используете бесплатный уровень и используете квоту сообщений. Если вы хотите отправить больше сообщений, рассмотрите возможность изменения службы SignalR на уровень Standard для отправки дополнительных сообщений. Дополнительные сведения см. на странице [цен на службу Azure SignalR](https://azure.microsoft.com/pricing/details/signalr-service/).

### <a name="get-help"></a>Получить помощь

Мы рекомендуем сначала устранить неполадки. Большинство проблем вызваны неполадками сервера приложений или сети. Выполните инструкции по [устранению неполадок с журналом ресурсов](#troubleshooting-with-resource-logs) и [базовым руководством по решению проблем](https://github.com/Azure/azure-signalr/blob/dev/docs/tsg.md) , чтобы найти основную причину.
Если проблему по-прежнему не удается устранить, попробуйте открыть проблему в GitHub или создать билет на портале Azure.
Сделайте следующее:
1. Диапазон времени около 30 минут при возникновении проблемы
2. Идентификатор ресурса службы SignalR Azure
3. Сведения о проблемах, как можно точнее: например, выборе не отправляет сообщения, сбрасывает подключения клиентов и т. д.
4. Журналы, собранные с сервера или со стороны клиента, и другие материалы, которые могут быть полезны
5. Используемых Воспроизвести код

> [!NOTE]
> Если вы открыли вопрос в GitHub, сохраните конфиденциальную информацию (например, "идентификатор ресурса", "журналы сервера/клиента") как частные, отправляйте только членам Организации в частном порядке.