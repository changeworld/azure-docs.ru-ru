---
title: Краткое руководство по подключению MXCHIP AZ3166 к Azure IoT Central
description: Узнайте, как с помощью программного обеспечения ОСРВ Azure для встраиваемых устройств подключить MXCHIP AZ3166 к Azure IoT и отправить данные телеметрии.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: c
ms.topic: quickstart
ms.date: 03/17/2021
ms.openlocfilehash: 160797367e2daf0cb6fe708d626cbf217c9992c8
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448612"
---
# <a name="quickstart-connect-an-mxchip-az3166-devkit-to-iot-central"></a>Краткое руководство по подключению MXCHIP AZ3166 DevKit к IoT Central

**Область применения**: [разработка встраиваемых устройств](about-iot-develop.md#embedded-device-development)<br>
**Полное время выполнение**: 30 минут

[![Просмотреть код](media/common/browse-code.svg)](https://github.com/azure-rtos/getting-started/tree/master/MXChip/AZ3166)

В этом руководстве показано, как с помощью ОСРВ Azure подключить MXCHIP AZ3166 IoT DevKit (далее MXCHIP DevKit) к Интернету вещей Azure. Эта статья является частью серии, посвященной [разработке встраиваемых устройств Интернета вещей Azure](quickstart-device-development.md). Эта серия знакомит разработчиков устройств с платформой ОСРВ Azure. В ней показано, как подключить к Интернету вещей Azure несколько комплектов для оценки устройств.

Вы выполните следующие задачи:

* установка набора встроенных средств разработки для программирования MXCHIP DevKit на языке C;
* создание образа и прошивка этого образа на MXCHIP DevKit;
* создание облачных компонентов, просмотр свойств, просмотр данных телеметрии устройств и вызов прямых команд с помощью Azure IoT Central.

## <a name="prerequisites"></a>Предварительные требования

* Компьютер под управлением Microsoft Windows 10.
* [Git](https://git-scm.com/downloads) для клонирования репозитория.
* Оборудование

    > * [MXCHIP AZ3166 IoT DevKit](https://aka.ms/iot-devkit) (MXCHIP DevKit).
    > * Устройство Wi-Fi с частотой 2,4 ГГц.
    > * Кабель с разъемами USB 2.0 A и Micro-USB.

## <a name="prepare-the-development-environment"></a>Подготовка среды разработки

Чтобы настроить среду разработки, прежде всего клонируйте репозиторий GitHub, который содержит все необходимые ресурсы для работы с этим руководством. Затем установите набор средств программирования.

### <a name="clone-the-repo-for-the-tutorial"></a>Клонирование репозитория для этого руководства

Клонируйте указанный ниже репозиторий, чтобы скачать все примеры кода для устройства, скрипты настройки и автономные версии документации. Если вы уже клонировали этот репозиторий, работая с другим руководством, этот процесс повторять не нужно.

Чтобы клонировать репозиторий, выполните следующую команду:

```shell
git clone --recursive https://github.com/azure-rtos/getting-started.git
```

### <a name="install-the-tools"></a>Установка инструментов

Клонированный репозиторий содержит скрипт настройки, который устанавливает и настраивает все необходимые средства. Если вы уже установили эти средства, работая с другим руководством из этой серии, этот процесс повторять не нужно.

> [!NOTE]
> Скрипт настройки устанавливает следующие средства:
> * [CMake](https://cmake.org) для сборки;
> * [ARM GCC](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm) для компиляции;
> * [Termite](https://www.compuphase.com/software_termite.htm) для мониторинга выходных данных подключенных устройств на последовательном порту.

Чтобы установить инструменты:

1. В проводнике откройте следующий путь репозитория и запустите скрипт настройки, размещенный в файле *get-toolchain.bat*:

    > *getting-started\tools\get-toolchain.bat*

1. Когда установка завершится, откройте новое окно консоли, чтобы проверить изменения конфигурации, внесенные скриптом настройки. В этой консоли вы будете выполнять все остальные задачи программирования, описанные в этом руководстве. Вы можете использовать Windows CMD, PowerShell или Git Bash для Windows.
1. Выполните следующий код и убедитесь, что в системе установлен пакет CMake версии 3.14 или выше.

    ```shell
    cmake --version
    ```

## <a name="create-the-cloud-components"></a>Создание облачных компонентов

### <a name="create-the-iot-central-application"></a>Создание приложения IoT Central

Существует несколько способов подключить устройства к Интернету вещей Azure. В этом разделе показано, как подключить устройство с помощью Azure IoT Central. IoT Central — это платформа приложений для Интернета вещей, которая удешевляет и упрощает процессы создания и администрирования решений для Интернета вещей.

Чтобы создать приложение:
1. На [портале Azure IoT Central](https://apps.azureiotcentral.com/) выберите **Мои приложения** в меню навигации сбоку.
1. Щелкните **+ Создать приложение**.
1. Выберите **Пользовательские приложения**.
1. Укажите имя и URL-адрес приложения.
1. Выберите тарифный план **Бесплатный**, чтобы активировать 7-дневную пробную версию.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-create-custom.png" alt-text="Создание пользовательского приложения в Azure IoT Central":::

1. Нажмите кнопку **создания**.

    Когда IoT Central завершит подготовку приложения, вы будете автоматически перенаправлены на панель мониторинга нового приложения.

    > [!NOTE]
    > Если у вас уже есть приложение IoT Central, вы можете использовать его для выполнения всех действий, описанных в этой статье, не создавая новое приложение.

### <a name="create-a-new-device"></a>Создание устройства

В этом разделе показано, как создать новое устройство с помощью панели мониторинга приложения IoT Central. Сведения о подключении для созданного устройства вам потребуются для безопасного подключения физического устройства, как описано в следующем разделе.

Чтобы создать устройство:
1. На панели мониторинга приложения выберите **Устройства** в меню навигации сбоку.
1. Щелкните **+ Создать**, чтобы открыть окно **Создание устройства**.
1. Для параметра "Шаблон устройства" сохраните значение **Не назначено**.
1. Укажите имя и идентификатор устройства.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-create-device.png" alt-text="Создание устройства в Azure IoT Central":::

1. Нажмите кнопку **Создать**.
1. Новое устройство сразу отобразится в списке **Все устройства**.  Щелкните имя устройства, чтобы просмотреть сведения о нем.
1. Щелкните **Подключиться** в строке меню вверху справа, чтобы отобразить сведения о подключении, которые потребуются для настройки устройства, как описано в следующем разделе.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-device-connection-info.png" alt-text="Просмотр сведений о подключении устройства":::

1. Запишите значения параметров строки подключения, которые отображаются в диалоговом окне **Подключение**. Эти значения вы на следующем шаге добавите в файл конфигурации.

    > * `ID scope`
    > * `Device ID`
    > * `Primary key`

## <a name="prepare-the-device"></a>Подготовка устройства

Чтобы подключить MXCHIP DevKit к Azure, вам нужно изменить файл конфигурации с параметрами Wi-Fi и Интернета вещей Azure, заново создать образ и записать его на устройство.

### <a name="add-configuration"></a>Добавление конфигурации

1. Откройте в текстовом редакторе следующий файл:

    > *getting-started\MXChip\AZ3166\app\azure_config.h*

1. Присвойте следующим константам для Wi-Fi значения, соответствующие вашей локальной среде.

    |Имя константы|Значение|
    |-------------|-----|
    |`WIFI_SSID` |{*Идентификатор SSID для Wi-Fi*}|
    |`WIFI_PASSWORD` |{*Пароль для Wi-Fi*}|
    |`WIFI_MODE` |{*Одно из значений режима Wi-Fi, перечисленных в файле*}|

1. Присвойте константам сведений об устройстве Интернета вещей Azure значения, которые вы сохранили после создания ресурсов Azure.

    |Имя константы|Значение|
    |-------------|-----|
    |`IOT_DPS_ID_SCOPE` |{*Значение области идентификатора*}|
    |`IOT_DPS_REGISTRATION_ID` |{*Значение идентификатора устройства*}|
    |`IOT_DEVICE_SAS_KEY` |{*Значение первичного ключа*}|

1. Сохраните файл и закройте его.

### <a name="build-the-image"></a>Создание образа

В консоли или проводнике выполните следующий скрипт *rebuild.bat*, чтобы создать образ:

> *getting-started\MXChip\AZ3166\tools\rebuild.bat*

Когда сборка завершится, проверьте наличие следующего двоичного файла:

> *getting-started\MXChip\AZ3166\build\app\mxchip_azure_iot.bin*

### <a name="flash-the-image"></a>Запись образа

1. На устройстве MXCHIP DevKit найдите кнопку **Reset** (Сброс) и порт Micro-USB. Эти элементы используются на следующих шагах. Они оба выделены на следующем рисунке:

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/mxchip-iot-devkit.png" alt-text="Расположение основных элементов на плате разработки MXChip DevKit":::

1. Подключите кабель Micro-USB к порту Micro-USB на устройстве MXCHIP DevKit, а затем к компьютеру.
1. В проводнике найдите двоичный файл, созданный, как описано в предыдущем разделе.
1. Скопируйте двоичный файл *mxchip_azure_iot.bin*.
1. В проводнике найдите устройство MXCHIP DevKit, подключенное к локальному компьютеру. Это устройство отображается в системе как диск с меткой **AZ3166**.
1. Вставьте двоичный файл в корневой каталог устройства MXCHIP DevKit. Запись образа начнется автоматически и завершится через несколько секунд.

    > [!NOTE]
    > В это время на устройстве MXCHIP DevKit будет мигать зеленый индикатор.

### <a name="confirm-device-connection-details"></a>Проверка сведений о подключении устройства

Приложение **Termite** позволяет отслеживать обмен данными, чтобы убедиться, что устройство настроено правильно.

1. Запустите **Termite**.
    > [!TIP]
    > Если не удается подключить Termite к DevKit, установите [драйвер ST-LINK](https://my.st.com/content/ccc/resource/technical/software/driver/files/stsw-link009.zip) и повторите попытку. Дополнительные действия см. в разделе [Устранение неполадок](https://github.com/azure-rtos/getting-started/blob/master/docs/troubleshooting.md).
1. Выберите **Параметры**.
1. В диалоговом окне **Serial port settings** (Параметры последовательного порта) проверьте следующие значений и при необходимости обновите их.
    * **Baud rate** (Скорость передачи): 115 200.
    * **Port** (Порт): имя порта, к которому подключено устройство MXCHIP DevKit. Если в раскрывающемся списке есть несколько вариантов, выберите нужный. Откройте **Диспетчер устройств** Windows и просмотрите раздел **Порты**, чтобы определить требуемое значение порта.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/termite-settings.png" alt-text="Проверка параметров в приложении Termite":::

1. Нажмите кнопку "ОК".
1. Нажмите на устройстве кнопку **Reset** (Сброс). Эта кнопка расположена рядом с разъемом Micro-USB имеет соответствующую маркировку.
1. В приложении **Termite** проверьте следующие значения контрольной точки и убедитесь, что устройство инициализировано и подключено к Интернету вещей Azure.

    ```output
    Starting Azure thread

    Initializing WiFi
        Connecting to SSID 'iot'
    SUCCESS: WiFi connected to iot

    Initializing DHCP
        IP address: 10.0.0.123
        Mask: 255.255.255.0
        Gateway: 10.0.0.1
    SUCCESS: DHCP initialized

    Initializing DNS client
        DNS address: 10.0.0.1
    SUCCESS: DNS client initialized

    Initializing SNTP client
        SNTP server 0.pool.ntp.org
        SNTP IP address: 185.242.56.3
        SNTP time update: Nov 16, 2020 23:47:35.385 UTC 
    SUCCESS: SNTP initialized

    Initializing Azure IoT DPS client
        DPS endpoint: global.azure-devices-provisioning.net
        DPS ID scope: ***
        Registration ID: ***
    SUCCESS: Azure IoT DPS client initialized

    Initializing Azure IoT Hub client
        Hub hostname: ***
        Device id: ***
        Model id: dtmi:azurertos:devkit:gsgmxchip;1
    Connected to IoTHub
    SUCCESS: Azure IoT Hub client initialized

    Starting Main loop
    ```

Не закрывайте Termite, чтобы отслеживать выходные данные устройства на следующих шагах.

## <a name="verify-the-device-status"></a>Просмотр состояния устройства

Чтобы просмотреть состояние устройства на портале IoT Central:
1. На панели мониторинга приложения выберите **Устройства** в меню навигации сбоку.
1. Убедитесь, что параметр **Состояние устройства** теперь получил значение **Подготовлено**.
1. Убедитесь, что для параметра **Шаблон устройства** указано значение **Getting Started Guide** (Руководство по началу работы).

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-device-view-status.png" alt-text="Просмотр состояния устройства на IoT Central":::

## <a name="view-telemetry"></a>Просмотр телеметрии

С помощью IoT Central можно просмотреть поток данных телеметрии, передаваемых с устройства в облако.

Чтобы просмотреть данные телеметрии на портале IoT Central:

1. На панели мониторинга приложения выберите **Устройства** в меню навигации сбоку.
1. Выберите нужное устройство в списке устройств.
1. Просмотрите данные телеметрии, которые устройство отправляет в облако, на вкладке **Обзор**.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-device-telemetry.png" alt-text="Просмотр данных телеметрии в IoT Central":::

    > [!NOTE]
    > Вы также можете отслеживать данные телеметрии, передаваемые с устройства, в приложении Termite.

## <a name="call-a-direct-method-on-the-device"></a>Вызов прямого метода на устройстве

IoT Central также позволяет вызывать прямой метод, который вы реализовали на устройстве. У прямых методов есть имя и может быть полезная нагрузка в формате JSON, настраиваемое подключение и время ожидания метода. В этом разделе показано, как вызвать метод, который позволяет включить и выключать индикатор.

Чтобы вызвать метод на портале IoT Central:

1. Откройте вкладку **Команда** на странице устройства.
1. Выберите **Состояние** и щелкните **Выполнить**.  На устройстве должен включиться индикатор.

    :::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-invoke-method.png" alt-text="Вызов прямого метода на устройстве":::

1. Снимите флажок **Состояние** и щелкните **Выполнить**. Индикатор на устройстве должен выключиться.

## <a name="view-device-information"></a>Просмотр сведений об устройстве

В IoT Central можно просмотреть сведения об устройстве.

На странице устройства выберите вкладку **Сведения**.

:::image type="content" source="media/quickstart-devkit-mxchip-az3166/iot-central-device-about.png" alt-text="Просмотр сведений об устройстве в IoT Central":::

## <a name="debugging"></a>Отладка

Сведения об отладке приложения см. в статье [Отладка с помощью Visual Studio Code](https://github.com/azure-rtos/getting-started/blob/master/docs/debugging.md).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вам больше не нужны ресурсы Azure, созданные при работе с этим руководством, их можно удалить на портале IoT Central. Если же вы хотите перейти к другим руководствам из этой серии, вы можете сохранить созданные ресурсы для дальнейшего использования.

Чтобы удалить пример приложения Azure IoT Central со всеми устройствами и ресурсами:
1. Выберите элементы **Администрирование** >  **[имя приложения]** .
1. Выберите команду **Удалить**.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве показано, как создать пользовательский образ с примером кода для ОСРВ Azure и сохранить этот образ на устройстве MXCHIP DevKit. Также здесь описано, как с помощью портала IoT Central создать ресурсы Azure, безопасно подключиться MXCHIP DevKit к Azure, просмотреть данные телеметрии и отправить сообщения.

* Разработчикам устройств мы рекомендуем перейти к другим руководствам из серии [Руководство по началу разработки встраиваемого устройства Интернета вещей Azure](quickstart-device-development.md).
* Если при выполнении действий, описанных в этом руководстве, у вас возникли проблемы с инициализацией устройства или подключением к нему, см. раздел [Устранение неполадок](https://github.com/azure-rtos/getting-started/blob/master/docs/troubleshooting.md).
* Дополнительные сведения о том, как использовать компоненты ОСРВ Azure в примере кода для этого руководства, см. в разделе [Использование ОСРВ Azure](https://github.com/azure-rtos/getting-started/blob/master/docs/using-azure-rtos.md) этого руководства по началу работы.

    > [!IMPORTANT]
    > ОСРВ Azure предоставляет изготовителям оборудования компоненты для безопасного обмена данными, а также создания кода и изоляции данных с помощью базовых аппаратных механизмов защиты, встроенных в микроконтроллеры и микропроцессоры. Но не забывайте, что каждый изготовитель оборудования отвечает за соответствие устройства требованиям безопасности.

