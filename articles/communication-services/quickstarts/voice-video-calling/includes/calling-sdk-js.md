---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 49054d9bbde67dc3670ec444e4b60c3ddf503db5
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2021
ms.locfileid: "105645337"
---
## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.
- Развернутый ресурс Служб коммуникации. [Создайте ресурс Служб коммуникации.](../../create-communication-resource.md)
- Маркер доступа пользователя для включения вызывающего клиента. Дополнительные сведения см. в разделе [Создание маркеров доступа и управление ими](../../access-tokens.md).
- Необязательно. Выполните инструкции из краткого руководства, чтобы [Добавить в приложение голосовое вызов](../getting-started-with-calling.md).

## <a name="install-the-sdk"></a>Установка пакета SDK

> [!NOTE]
> В этом документе используется версия 1.0.0-Beta. 10 вызывающего пакета SDK.

Используйте `npm install` команду, чтобы установить вызовы и распространенные пакеты SDK служб связи Azure для JavaScript.
Этот документ ссылается на типы в версии 1.0.0-Beta. 10 вызывающей библиотеки.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save

```

## <a name="object-model"></a>Объектная модель

Следующие классы и интерфейсы обработают некоторые основные функции пакета SDK, вызывающего службы связи Azure:

| Имя                             | Описание                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| `CallClient`                      | Главная точка входа в вызывающий пакет SDK.                                                                       |
| `CallAgent`                        | Используется для запуска вызовов и управления ими.                                                                                            |
| `DeviceManager`                    | Используется для управления устройствами мультимедиа.                                                                                           |
| `AzureCommunicationTokenCredential` | Реализует `CommunicationTokenCredential` интерфейс, который используется для создания экземпляра `callAgent` . |

## <a name="initialize-a-callclient-instance-create-a-callagent-instance-and-access-devicemanager"></a>Инициализация экземпляра Каллклиент, создание экземпляра Каллажент и доступ к Девицеманажер

Создайте новый `CallClient` экземпляр. Для настройки доступны специальные параметры, например экземпляр Logger.

При наличии `CallClient` экземпляра можно создать `CallAgent` экземпляр, вызвав `createCallAgent` метод в `CallClient` экземпляре. Это действие асинхронно возвращает объект экземпляра `CallAgent`.

`createCallAgent`Метод использует `CommunicationTokenCredential` в качестве аргумента. Он принимает [маркер доступа пользователя](../../access-tokens.md).

После создания `callAgent` экземпляра `getDeviceManager` `CallClient` для доступа к нему можно использовать метод в экземпляре `deviceManager` .

```js
// Set the logger's log level
setLogLevel('verbose');
// Redirect logger output to wherever desired. By default it logs to console
AzureLogger.log = (...args) => { console.log(...args) };
const userToken = '<user token>';
callClient = new CallClient(options);
const tokenCredential = new AzureCommunicationTokenCredential(userToken);
const callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'optional ACS user name'});
const deviceManager = await callClient.getDeviceManager()
```

## <a name="place-a-call"></a>Осуществление вызовов

Чтобы создать и запустить вызов, используйте один из API в `callAgent` и предоставьте пользователя, созданного с помощью пакета SDK для удостоверений служб связи.

Операции создания и начала вызовов выполняются синхронно. Экземпляр Call позволяет подписываться на события вызова.

### <a name="place-a-1n-call-to-a-user-or-pstn"></a>Поместите Звонок 1: n пользователю или PSTN

Для вызова другого пользователя служб связи используйте `startCall` метод в `callAgent` и передайте получателя `CommunicationUserIdentifier` , [созданного с помощью библиотеки администрирования служб Communication Services](../../access-tokens.md).

```js
const userCallee = { communicationUserId: '<ACS_USER_ID>' }
const oneToOneCall = callAgent.startCall([userCallee]);
```

Чтобы позвонить в телефонную сеть с открытым коммутируемым доступом (PSTN), используйте `startCall` метод в `callAgent` и передайте значение получателя `PhoneNumberIdentifier` . Для осуществления вызовов к ТСОП необходимо настроить ресурс Служб коммуникации.

При вызове номера PSTN укажите идентификатор альтернативного звонящего. ИДЕНТИФИКАТОР альтернативного абонента — это номер телефона (на основе стандарта E. 164), определяющий вызывающий объект в вызове PSTN. Это номер телефона, который получатель вызова видит при входящем вызове.

> [!NOTE]
> Вызовы ТСОП сейчас доступны в закрытой предварительной версии. Для доступа [используйте программу раннего внедрения](https://aka.ms/ACS-EarlyAdopter).

Для вызова 1:1 используйте следующий код:

```js
const pstnCalee = { phoneNumber: '<ACS_USER_ID>' }
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const oneToOneCall = callAgent.startCall([pstnCallee], {alternateCallerId});
```

Для вызова 1: n используйте следующий код:

```js
const userCallee = { communicationUserId: <ACS_USER_ID> }
const pstnCallee = { phoneNumber: <PHONE_NUMBER>};
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const groupCall = callAgent.startCall([userCallee, pstnCallee], {alternateCallerId});

```

### <a name="place-a-11-call-with-video-camera"></a>Осуществление персонального вызова с использованием видеокамеры

> [!IMPORTANT]
> Сейчас допускается не более одного исходящего локального видеопотока в один момент времени.

Чтобы поместить видеовызов, необходимо указать свои камеры с помощью `getCameras()` метода в `deviceManager` .

После выбора камеры используйте ее для создания `LocalVideoStream` экземпляра. Передайте его в `videoOptions` качестве элемента в `localVideoStream` массиве в `startCall` метод.

```js
const deviceManager = await callClient.getDeviceManager();
const cameras = await deviceManager.getCameras();
const camera = cameras[0]
localVideoStream = new LocalVideoStream(camera);
const placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};
const call = callAgent.startCall(['acsUserId'], placeCallOptions);

```

Когда вызов подключается, он автоматически начинает отправку видеопотока от выбранной камеры другому участнику. Это также относится к `Call.Accept()` параметрам видео и `CallAgent.join()` видео.

### <a name="join-a-group-call"></a>Присоединение к групповому вызову

> [!NOTE]
> `groupId`Параметр считается системными метаданными и может использоваться корпорацией Майкрософт для операций, необходимых для запуска системы. Не включайте личные данные в `groupId` значение. Корпорация Майкрософт не считает этот параметр личными данными, и его содержимое может быть видимым для сотрудников корпорации Майкрософт или храниться в долгосрочной перспективе.
>
> `groupId`Параметр требует, чтобы данные были в формате GUID. Рекомендуется использовать созданные случайным образом идентификаторы GUID, которые не считаются личными данными в системах.
>

Чтобы начать новый вызов группы или присоединиться к текущему вызову группы, используйте `join` метод и передайте объект со `groupId` свойством. `groupId`Значение должно быть идентификатором GUID.

```js

const context = { groupId: <GUID>}
const call = callAgent.join(context);

```

### <a name="join-a-teams-meeting"></a>Присоединяйтесь к собранию команд

Чтобы присоединиться к собранию команд, используйте `join` метод и передайте ссылку на собрание или координаты.

Присоединение с помощью ссылки на собрание:

```js
const locator = { meetingLink: <meeting link>}
const call = callAgent.join(locator);
```

Присоединиться с помощью координат собрания:

```js
const locator = {
    threadId: <thread id>,
    organizerId: <organizer id>,
    tenantId: <tenant id>,
    messageId: <message id>
}
const call = callAgent.join(locator);
```

## <a name="receive-an-incoming-call"></a>Прием входящего вызова

`callAgent`Экземпляр создает `incomingCall` событие, когда вошедший в систему идентификатор получает входящий вызов. Чтобы прослушать это событие, подпишитесь с помощью одного из следующих параметров:

```js
const incomingCallHander = async (args: { incomingCall: IncomingCall }) => {

    //Get incoming call ID
    var incomingCallId = incomingCall.id

    // Get information about caller
    var callerInfo = incomingCall.callerInfo

    // Accept the call
    var call = await incomingCall.accept();

    // Reject the call
    incomingCall.reject();

    // Subscribe to callEnded event and get the call end reason
     incomingCall.on('callEnded', args => {
        console.log(args.callEndReason);
    });

    // callEndReason is also a property of IncomingCall
    var callEndReason = incomingCall.callEndReason;
};
callAgentInstance.on('incomingCall', incomingCallHander);
```

`incomingCall`Событие включает `incomingCall` экземпляр, который можно принять или отклонить.

## <a name="manage-calls"></a>Управление вызовами

Во время вызова можно получить доступ к свойствам вызова и управлять параметрами видео и аудио.

### <a name="check-call-properties"></a>Проверить свойства вызова

Получите уникальный идентификатор (String) для вызова:

   ```js
    const callId: string = call.id;
   ```

Узнайте о других участниках в вызове, изучив `remoteParticipants` коллекцию в экземпляре Call:

   ```js
   const remoteParticipants = call.remoteParticipants;
   ```

Идентификация вызывающего для входящего вызова:

   ```js
   const callerIdentity = call.callerInfo.identifier;
   ```

   `identifier` является одним из `CommunicationIdentifier` типов.

Получить состояние вызова:

   ```js
   const callState = call.state;
   ```

   Возвращает строку, которая представляет текущее состояние вызова:

  - `None`: Начальное состояние вызова.
  - `Connecting`: Начальное состояние перехода, когда вызов помещается или принимается.
  - `Ringing`: Для исходящего вызова указывает, что звонок вызывается для удаленных участников. `Incoming`На их стороне.
  - `EarlyMedia`: Указывает состояние, в котором воспроизводится объявление перед соединением вызова.
  - `Connected`: Указывает, что вызов подключен.
  - `LocalHold`: Указывает, что вызов помещается в удержание локальным участником. Между локальной конечной точкой и удаленными участниками не передаются никакие носители.
  - `RemoteHold`: Указывает, что вызов был помещен на удержание удаленным участником. Между локальной конечной точкой и удаленными участниками не передаются никакие носители.
  - `Disconnecting`: Состояние перехода перед тем, как вызов переходит в `Disconnected` состояние.
  - `Disconnected`: Окончательное состояние вызова. Если сетевое подключение потеряно, состояние изменится на `Disconnected` через две минуты.

Выясните причину завершения вызова, изучив `callEndReason` свойство:

   ```js
   const callEndReason = call.callEndReason;
   const callEndReasonCode = callEndReason.code // (number) code associated with the reason
   const callEndReasonSubCode = callEndReason.subCode // (number) subCode associated with the reason
   ```

Узнайте, является ли текущий вызов входящим или исходящим, проверив `direction` свойство. Он возвращает `CallDirection`.

  ```js
   const isIncoming = call.direction == 'Incoming';
   const isOutgoing = call.direction == 'Outgoing';
   ```

Проверьте, не отключен ли текущий микрофон. Он возвращает `Boolean`.

   ```js
   const muted = call.isMuted;
   ```

Выясните, отправляется ли поток обмена на экран из данной конечной точки, проверив `isScreenSharingOn` свойство. Он возвращает `Boolean`.

   ```js
   const isScreenSharingOn = call.isScreenSharingOn;
   ```

Проверьте активные видеопотоки, проверив `localVideoStreams` коллекцию. Он возвращает `LocalVideoStream` объекты.

   ```js
   const localVideoStreams = call.localVideoStreams;
   ```

### <a name="check-a-callended-event"></a>Проверка события Каллендед

Экземпляр `call` при завершении вызова создает событие `callEnded`. Чтобы прослушать это событие, подпишитесь, используя следующий код:

```js
const callEndHander = async (args: { callEndReason: CallEndReason }) => {
    console.log(args.callEndReason)
};

call.on('callEnded', callEndHander);
```

### <a name="mute-and-unmute"></a>Отключение и включение звука

Чтобы отключить или включить звук для локальной конечной точки, можно использовать `mute` `unmute` асинхронные API и.

```js

//mute local device
await call.mute();

//unmute local device
await call.unmute();

```

### <a name="start-and-stop-sending-local-video"></a>Запуск и остановка отправки локального видео

Чтобы запустить видео, необходимо указать камеры с помощью `getCameras` метода для `deviceManager` объекта. Затем создайте новый экземпляр `LocalVideoStream` , передав требуемую камеру в `startVideo` метод в качестве аргумента:

```js
const deviceManager = await callClient.getDeviceManager();
const cameras = await deviceManager.getCameras();
const camera = cameras[0]
const localVideoStream = new LocalVideoStream(camera);
await call.startVideo(localVideoStream);
```

После успешного начала отправки видео `LocalVideoStream` экземпляр добавляется в `localVideoStreams` коллекцию в экземпляре вызова.

```js
call.localVideoStreams[0] === localVideoStream;
```

Чтобы отключить локальное видео, передайте `localVideoStream` экземпляр, доступный в `localVideoStreams` коллекции:

```js
await call.stopVideo(localVideoStream);
```

При отправке видео с помощью вызова `switchSource` на экземпляре можно переключиться на другое устройство камеры `localVideoStream` :

```js
const cameras = await callClient.getDeviceManager().getCameras();
const camera = cameras[1];
localVideoStream.switchSource(camera);
```

## <a name="manage-remote-participants"></a>Управление удаленными участниками

Все удаленные участники представлены типом `RemoteParticipant` и доступны через коллекцию `remoteParticipants` в экземпляре вызова.

### <a name="list-the-participants-in-a-call"></a>Перечисление участников в вызове

`remoteParticipants`Коллекция возвращает список удаленных участников в вызове:

```js
call.remoteParticipants; // [remoteParticipant, remoteParticipant....]
```

### <a name="access-remote-participant-properties"></a>Доступ к свойствам удаленного участника

Удаленные участники имеют набор связанных свойств и коллекций:

- `CommunicationIdentifier`: Получение идентификатора удаленного участника. Identity является одним из `CommunicationIdentifier` типов:

  ```js
  const identifier = remoteParticipant.identifier;
  ```

  Это может быть один из следующих `CommunicationIdentifier` типов:

  - `{ communicationUserId: '<ACS_USER_ID'> }`: Объект, представляющий пользователя ACS.
  - `{ phoneNumber: '<E.164>' }`: Объект, представляющий номер телефона в формате E. 164.
  - `{ microsoftTeamsUserId: '<TEAMS_USER_ID>', isAnonymous?: boolean; cloud?: "public" | "dod" | "gcch" }`: Объект, представляющий пользователя команд.
  - `{ id: string }`: объект репредентинг идентификатор, который не соответствует ни одному из других типов идентификаторов

- `state`: Получение состояния удаленного участника.

  ```js
  const state = remoteParticipant.state;
  ```

  Состояние может быть следующим:

  - `Idle`: Начальное состояние.
  - `Connecting`: Переходное состояние во время подключения участника к вызову.
  - `Ringing`: Участник позвонит.
  - `Connected`: Участник подключен к вызову.
  - `Hold`: Участник находится в удержании.
  - `EarlyMedia`: Объявление, которое будет воспроизводиться до того, как участник подключится к вызову.
  - `Disconnected`: Конечное состояние. Участник отключен от вызова. Если удаленный участник потеряет подключение к сети, его состояние изменится на `Disconnected` через две минуты.

- `callEndReason`: Чтобы узнать, почему участник оставил вызов, проверьте `callEndReason` свойство:

  ```js
  const callEndReason = remoteParticipant.callEndReason;
  const callEndReasonCode = callEndReason.code // (number) code associated with the reason
  const callEndReasonSubCode = callEndReason.subCode // (number) subCode associated with the reason
  ```

- `isMuted` состояние. чтобы узнать, отключен ли удаленный участник, проверьте `isMuted` свойство. Он возвращает `Boolean`.

  ```js
  const isMuted = remoteParticipant.isMuted;
  ```

- `isSpeaking` состояние. чтобы узнать, говорит ли удаленный участник, проверьте `isSpeaking` свойство. Он возвращает `Boolean`.

  ```js
  const isSpeaking = remoteParticipant.isSpeaking;
  ```

- `videoStreams`. Чтобы проверить все потоки видео, отправляемые данным участником в этом вызове, проверьте `videoStreams` коллекцию. Он содержит `RemoteVideoStream` объекты.

  ```js
  const videoStreams = remoteParticipant.videoStreams; // [RemoteVideoStream, ...]
  ```
- `displayName`: Чтобы получить отображаемое имя для этого удаленного участника, проверьте `displayName` свойство, возвращаемое строкой. 

  ```js
  const displayName = remoteParticipant.displayName;
  ```

### <a name="add-a-participant-to-a-call"></a>Добавление участника в вызов

Чтобы добавить участника (пользователя или номер телефона) в вызов, можно использовать `addParticipant` . Укажите один из `Identifier` типов. Он возвращает `remoteParticipant` экземпляр.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
const remoteParticipant = call.addParticipant(userIdentifier);
const remoteParticipant = call.addParticipant(pstnIdentifier, {alternateCallerId: '<Alternate Caller ID>'});
```

### <a name="remove-a-participant-from-a-call"></a>Удаление участника из вызова

Чтобы удалить участника (пользователя или номер телефона) из вызова, можно вызвать `removeParticipant` . Необходимо передать один из `Identifier` типов. Это разрешается асинхронно после удаления участника из вызова. Участник также удаляется из `remoteParticipants` коллекции.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
await call.removeParticipant(userIdentifier);
await call.removeParticipant(pstnIdentifier);
```

## <a name="render-remote-participant-video-streams"></a>Отрисовка видеопотоков удаленных участников

Чтобы получить список видеопотоков и потоков демонстрации экрана, получаемых от удаленных участников, проверьте коллекции `videoStreams`:

```js
const remoteVideoStream: RemoteVideoStream = call.remoteParticipants[0].videoStreams[0];
const streamType: MediaStreamType = remoteVideoStream.mediaStreamType;
```

Для подготовки к просмотру `RemoteVideoStream` необходимо подписываться на `isAvailableChanged` событие. Если свойство `isAvailable` изменит значение на `true`, значит этот удаленный участник отправляет поток. После этого создайте новый экземпляр `VideoStreamRenderer` , а затем создайте новый `VideoStreamRendererView` экземпляр с помощью асинхронного `createView` метода.  Затем можно присоединиться `view.target` к любому элементу пользовательского интерфейса.

При каждом изменении состояния доступности удаленного потока вы можете удалить `VideoStreamRenderer` или определенное представление (`VideoStreamRendererView`) либо оставить все как есть, но это приведет к отображению пустого видеокадра.

```js
function subscribeToRemoteVideoStream(remoteVideoStream: RemoteVideoStream) {
    let videoStreamRenderer: VideoStreamRenderer = new VideoStreamRenderer(remoteVideoStream);
    const displayVideo = () => {
        const view = await videoStreamRenderer.createView();
        htmlElement.appendChild(view.target);
    }
    remoteVideoStream.on('isAvailableChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            displayVideo();
        } else {
            videoStreamRenderer.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        displayVideo();
    }
}
```

### <a name="remote-video-stream-properties"></a>Свойства удаленного видеопотока

Удаленные потоки видео имеют следующие свойства:

- `id`— Идентификатор удаленного потока видео.

  ```js
  const id: number = remoteVideoStream.id;
  ```

- `mediaStreamType`: Может иметь `Video` или `ScreenSharing` .

  ```js
  const type: MediaStreamType = remoteVideoStream.mediaStreamType;
  ```

- `isAvailable`: Указывает, является ли конечная точка удаленного участника активной отправкой потока.

  ```js
  const type: boolean = remoteVideoStream.isAvailable;
  ```

### <a name="videostreamrenderer-methods-and-properties"></a>Методы и свойства Видеостреамрендерер

Создание `VideoStreamRendererView` экземпляра, который может быть присоединен в пользовательском интерфейсе приложения для отрисовки удаленного видеопотока, использование асинхронного `createView()` метода, разрешение, когда поток готов к отображению и возвращает объект со `target` свойством, представляющим `video` элемент, который может быть добавлен в любом месте дерева DOM

  ```js
  videoStreamRenderer.createView()
  ```

Удалить `videoStreamRenderer` и все связанные `VideoStreamRendererView` экземпляры:

  ```js
  videoStreamRenderer.dispose()
  ```

### <a name="videostreamrendererview-methods-and-properties"></a>Методы и свойства Видеостреамрендерервиев

При создании `VideoStreamRendererView` можно указать `scalingMode` `isMirrored` Свойства и. `scalingMode` может иметь `Stretch` , `Crop` или `Fit` . Если `isMirrored` задано значение, отображаемый поток зеркально отражается по вертикали.

```js
const videoStreamRendererView: VideoStreamRendererView = await videoStreamRenderer.createView({ scalingMode, isMirrored });
```

Каждый `VideoStreamRendererView` экземпляр имеет `target` свойство, представляющее область отрисовки. Присоединить это свойство к пользовательскому интерфейсу приложения:

```js
htmlElement.appendChild(view.target);
```

Для обновления можно `scalingMode` вызвать `updateScalingMode` метод:

```js
view.updateScalingMode('Crop')
```

## <a name="device-management"></a>Управление устройствами

В `deviceManager` можно указать локальные устройства, которые могут передавать звуковые и видеопотоки в вызове. Он также помогает запрашивать разрешение на доступ к микрофону и камере другого пользователя с помощью собственного API браузера.

Доступ можно получить `deviceManager` , вызвав `callClient.getDeviceManager()` метод:

```js
const deviceManager = await callClient.getDeviceManager();
```

### <a name="get-local-devices"></a>Получение локальных устройств

Для доступа к локальным устройствам можно использовать методы перечисления в `deviceManager` .

```js
//  Get a list of available video devices for use.
const localCameras = await deviceManager.getCameras(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
const localMicrophones = await deviceManager.getMicrophones(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
const localSpeakers = await deviceManager.getSpeakers(); // [AudioDeviceInfo, AudioDeviceInfo...]
```

### <a name="set-the-default-microphone-and-speaker"></a>Установка микрофона и динамика по умолчанию

В `deviceManager` можно задать устройство по умолчанию, которое будет использоваться для запуска вызова. Если значения по умолчанию для клиента не заданы, службы связи используют параметры операционной системы по умолчанию.

```js
// Get the microphone device that is being used.
const defaultMicrophone = deviceManager.selectedMicrophone;

// Set the microphone device to use.
await deviceManager.selectMicrophone(localMicrophones[0]);

// Get the speaker device that is being used.
const defaultSpeaker = deviceManager.selectedSpeaker;

// Set the speaker device to use.
await deviceManager.selectSpeaker(localSpeakers[0]);
```

### <a name="local-camera-preview"></a>Предварительный просмотр изображения с локальной камеры

С помощью `deviceManager` и `VideoStreamRenderer` вы можете реализовать отрисовку потоков из локальной камеры. Этот поток не отправляется другим участникам, а используется как локальный канал предварительного просмотра.

```js
const cameras = await deviceManager.getCameras();
const camera = cameras[0];
const localCameraStream = new LocalVideoStream(camera);
const videoStreamRenderer = new VideoStreamRenderer(localCameraStream);
const view = await videoStreamRenderer.createView();
htmlElement.appendChild(view.target);

```

### <a name="request-permission-to-camera-and-microphone"></a>Запрос разрешения на камеру и микрофон

Запрашивать у пользователя разрешение на камеру и микрофон:

```js
const result = await deviceManager.askDevicePermission({audio: true, video: true});
```

Это разрешается с помощью объекта, который указывает `audio` , `video` предоставлены ли разрешения:

```js
console.log(result.audio);
console.log(result.video);
```

## <a name="record-calls"></a>Вызовы записей

[!INCLUDE [Private Preview Notice](../../../includes/private-preview-include-section.md)]

Запись вызовов является функцией расширения базового API `Call`. Сначала вам нужно получить объект API для функции записи:

```js
const callRecordingApi = call.api(Features.Recording);
```

Затем, чтобы проверить, записывается ли вызов, проверьте `isRecordingActive` свойство объекта `callRecordingApi` . Он возвращает `Boolean`.

```js
const isResordingActive = callRecordingApi.isRecordingActive;
```

Вы также можете подписаться на запись изменений:

```js
const isRecordingActiveChangedHandler = () => {
  console.log(callRecordingApi.isRecordingActive);
};

callRecordingApi.on('isRecordingActiveChanged', isRecordingActiveChangedHandler);

```

## <a name="transfer-calls"></a>Вызовы перемещения

Перенаправление вызовов является расширенной функцией основного API `Call`. Сначала необходимо получить объект API функции перемещения:

```js
const callTransferApi = call.api(Features.Transfer);
```

Передача вызовов требует трех сторон:

- *Получатель*: лицо, инициирующее запрос на перемещение.
- *Получатель*: лицо, которое переносится.
- *Цель передачи*: лицо, которому передается объект.

Передачи выполните следующие действия.

1. Между *передаваемой* и *передаваемой* стороной уже есть подключенный звонок. Объект, принимающий запрос *, принимает решение* *о перемещении* вызова от переданного *объекта к цели перемещения*.
1. Объект- *получатель* вызывает `transfer` API.
1. Переданный объект *принимает решение* о том, `accept` `reject` передается ли запрос на перемещение в *цель перемещения* с помощью `transferRequested` события.
1. *Целевой объект перемещения* получает входящий вызов только в том случае, если *получатель* принимает запрос на перемещение.

Для перемещения текущего вызова можно использовать `transfer` API. `transfer` принимает необязательный параметр `transferCallOptions` , который позволяет установить `disableForwardingAndUnanswered` флаг:

- `disableForwardingAndUnanswered = false`: Если *целевой объект передачи* не отвечает на вызов передачи, передача выполняется после перенаправления *цели передачи* и без ответа параметров.
- `disableForwardingAndUnanswered = true`: Если *целевой объект перемещения* не отвечает на вызов метода пересылки, то операция перемещения завершается.

```js
// transfer target can be an ACS user
const id = { communicationUserId: <ACS_USER_ID> };
```

```js
// call transfer API
const transfer = callTransferApi.transfer({targetParticipant: id});
```

`transfer`API позволяет подписываться на `transferStateChanged` события и `transferRequested` . `transferRequested`Событие поступает из `call` экземпляра; `transferStateChanged` событие и передает его `state` `error` из `transfer` экземпляра.

```js
// transfer state
const transferState = transfer.state; // None | Transferring | Transferred | Failed

// to check the transfer failure reason
const transferError = transfer.error; // transfer error code that describes the failure if a transfer request failed
```

*Получатель* может принять или отклонить запрос на перемещение, инициированный *перенаправлением* в `transferRequested` событии, с помощью `accept()` или `reject()` в `transferRequestedEventArgs` . Доступ к `targetParticipant` сведениям и `accept` `reject` методам можно получить в `transferRequestedEventArgs` .

```js
// Transferee to accept the transfer request
callTransferApi.on('transferRequested', args => {
  args.accept();
});

// Transferee to reject the transfer request
callTransferApi.on('transferRequested', args => {
  args.reject();
});
```

## <a name="learn-about-eventing-models"></a>Дополнительные сведения о моделях событий

Проверьте текущие значения и подпишитесь на события обновления для будущих значений.

### <a name="properties"></a>Свойства

```js
// Inspect the current value
console.log(object.property);

// Subscribe to value updates
object.on('propertyChanged', () => {
    // Inspect new value
    console.log(object.property)
});

// Unsubscribe from updates:
object.off('propertyChanged', () => {});



// Example for inspecting a call state
console.log(call.state);
call.on('stateChanged', () => {
    console.log(call.state);
});
call.off('stateChanged', () => {});
```

### <a name="collections"></a>Коллекции

```js
// Inspect the current collection
object.collection.forEach(v => {
    console.log(v);
});

// Subscribe to collection updates
object.on('collectionUpdated', e => {
    // Inspect new values added to the collection
    e.added.forEach(v => {
        console.log(v);
    });
    // Inspect values removed from the collection
    e.removed.forEach(v => {
        console.log(v);
    });
});

// Unsubscribe from updates:
object.off('collectionUpdated', () => {});

// Example for subscribing to remote participants and their video streams
call.remoteParticipants.forEach(p => {
    subscribeToRemoteParticipant(p);
})

call.on('remoteParticipantsUpdated', e => {
    e.added.forEach(p => { subscribeToRemoteParticipant(p) })
    e.removed.forEach(p => { unsubscribeFromRemoteParticipant(p) })
});

function subscribeToRemoteParticipant(p) {
    console.log(p.state);
    p.on('stateChanged', () => { console.log(p.state); });
    p.videoStreams.forEach(v => { subscribeToRemoteVideoStream(v) });
    p.on('videoStreamsUpdated', e => { e.added.forEach(v => { subscribeToRemoteVideoStream(v) }) })
}
```
