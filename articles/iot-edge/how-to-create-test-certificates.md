---
title: Создание тестовых сертификатов — Azure IoT Edge | Документация Майкрософт
description: Создайте тестовые сертификаты и Узнайте, как их установить на устройство Azure IoT Edge, чтобы подготовиться к развертыванию в рабочей среде.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d8cf3dbe9d1dc2ad329a0b5ab8fa9554c85ae55c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103199096"
---
# <a name="create-demo-certificates-to-test-iot-edge-device-features"></a>Создание демонстрационных сертификатов для тестирования функций устройства IoT Edge

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

IoT Edge устройствам требуются сертификаты для безопасного обмена данными между средой выполнения, модулями и любыми подчиненными устройствами.
Если у вас нет центра сертификации для создания необходимых сертификатов, можно использовать демонстрационные сертификаты, чтобы испытать IoT Edge функций в тестовой среде.
В этой статье описываются функциональные возможности скриптов создания сертификатов, которые IoT Edge предоставляет для тестирования.

Срок действия этих сертификатов истекает через 30 дней и не должен использоваться в рабочем сценарии.

Вы можете создавать сертификаты на любом компьютере, а затем копировать их на устройство IoT Edge.
Для создания сертификатов проще использовать основной компьютер, а не создавать их на самом устройстве IoT Edge.
С помощью основного компьютера можно настроить сценарии один раз, а затем использовать их для создания сертификатов для нескольких устройств.

Выполните следующие действия, чтобы создать демонстрационные сертификаты для тестирования сценария IoT Edge.

1. [Настройте сценарии](#set-up-scripts) для создания сертификатов на устройстве.
2. [Создайте сертификат корневого ЦС](#create-root-ca-certificate) , который будет использоваться для подписи всех остальных сертификатов для вашего сценария.
3. Создайте сертификаты, необходимые для сценария, который необходимо протестировать:
   * [Создайте IOT Edge сертификаты удостоверений устройств](#create-iot-edge-device-identity-certificates) для подготовки устройств с помощью проверки подлинности сертификата X. 509 либо вручную, либо с помощью службы подготовки устройств для центра Интернета вещей.
   * [Создание IOT Edge сертификатов ЦС устройств](#create-iot-edge-device-ca-certificates) для IOT Edge устройств в сценариях шлюза.
   * [Создайте подчиненные сертификаты устройств](#create-downstream-device-certificates) для проверки подлинности подчиненных устройств в сценарии шлюза.

## <a name="prerequisites"></a>Предварительные требования

Компьютер разработки с установленным Git.

## <a name="set-up-scripts"></a>Настройка сценариев

Репозиторий IoT Edge в GitHub содержит скрипты создания сертификатов, которые можно использовать для создания демонстрационных сертификатов.
В этом разделе приводятся инструкции по подготовке сценариев для выполнения на компьютере под управлением Windows или Linux.
Если вы используете компьютер Linux, переходите к [настройке в Linux](#set-up-on-linux).

### <a name="set-up-on-windows"></a>Настройка в Windows

Чтобы создать демонстрационные сертификаты на устройстве Windows, необходимо установить OpenSSL, а затем клонировать скрипты создания и настроить их локально в PowerShell.

#### <a name="install-openssl"></a>Установка OpenSSL

Установите OpenSSL для Windows на компьютере, который используется для создания сертификатов.
Если на устройстве Windows уже установлен OpenSSL, убедитесь, что openssl.exe доступен в переменной среды PATH.

Существует несколько способов установки OpenSSL, включая следующие:

* **Проще:** Скачайте и установите все [сторонние двоичные файлы OpenSSL](https://wiki.openssl.org/index.php/Binaries), например, из [OpenSSL в SourceForge](https://sourceforge.net/projects/openssl/). Добавьте полный путь к файлу openssl.exe в переменную среды PATH.

* **Рекомендуемый способ.** Скачайте исходный код OpenSSL и создайте двоичные файлы на компьютере самостоятельно или с помощью [vcpkg](https://github.com/Microsoft/vcpkg). В следующих инструкциях используется vcpkg для скачивания исходного кода, а также компиляции и установки OpenSSL на компьютере Windows с помощью простых действий.

   1. Перейдите в каталог для установки vcpkg. Следуйте указаниям, чтобы скачать и установить [vcpkg](https://github.com/Microsoft/vcpkg).

   2. После установки vcpkg выполните следующую команду в командной строке PowerShell, чтобы установить пакет OpenSSL для Windows x64. Для завершения установки обычно требуется около 5 минут.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```

   3. Добавьте `<vcpkg path>\installed\x64-windows\tools\openssl` в переменную среды PATH, чтобы сделать файл openssl.exe доступным для вызова.

#### <a name="prepare-scripts-in-powershell"></a>Подготовка скриптов в PowerShell

Репозиторий Azure IoT Edge Git содержит скрипты, которые можно использовать для создания тестовых сертификатов.
В этом разделе вы создаете точную копию репозитория IoT Edge и выполняете скрипты.

1. Откройте окно PowerShell с правами администратора.

2. Клонируйте репозиторий Git службы IoT Edge, который содержит скрипты для создания демонстрационных сертификатов. Используйте команду `git clone` или [скачайте ZIP-файл](https://github.com/Azure/iotedge/archive/master.zip).

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Перейдите в каталог, в котором вы планируете работать. В рамках этой статьи мы назовем этот каталог *\<WRKDIR>* . Все сертификаты и ключи будут созданы в этом рабочем каталоге.

4. Скопируйте файлы конфигурации и скриптов из клонированного репозитория в рабочий каталог.

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Если репозиторий был скачан в виде ZIP-файла, имя папки будет равно, `iotedge-master` а остальная часть пути будет одинаковой.

5. Включите PowerShell для выполнения скриптов.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

6. Перенесите функции, используемые скриптами, в глобальное пространство имен PowerShell.

   ```powershell
   . .\ca-certs.ps1
   ```

   В окне PowerShell отобразится предупреждение о том, что сертификаты, созданные этим сценарием, предназначены только для тестирования и не должны использоваться в рабочих сценариях.

7. Убедитесь, что OpenSSL установлен правильно, и убедитесь в отсутствии конфликтов имен с существующими сертификатами. При возникновении проблем в выходных данных сценария должно быть описано, как их исправить в системе.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="set-up-on-linux"></a>Настройка в Linux

Чтобы создать демонстрационные сертификаты на устройстве Windows, необходимо клонировать скрипты создания и настроить их локально в bash.

1. Клонируйте репозиторий Git службы IoT Edge, который содержит скрипты для создания демонстрационных сертификатов.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Перейдите в каталог, в котором вы планируете работать. Мы будем называть этот каталог по всей статье как *\<WRKDIR>* . Все файлы сертификатов и ключей будут созданы в этом каталоге.
  
3. Скопируйте файлы конфигурации и скриптов из клонированного репозитория IoT Edge в рабочий каталог.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

## <a name="create-root-ca-certificate"></a>Создать сертификат корневого ЦС

Сертификат корневого ЦС используется для создания всех других демонстрационных сертификатов для тестирования IoT Edge сценария.
Вы можете использовать один и тот же корневой сертификат ЦС для создания демонстрационных сертификатов для нескольких IoT Edge или подчиненных устройств.

Если у вас уже есть один корневой сертификат ЦС в рабочей папке, не создавайте его.
Новый сертификат корневого ЦС перезапишет старый, а все последующие сертификаты, выполненные от старого, перестанут работать.
Если требуется несколько сертификатов корневого ЦС, следует управлять ими в отдельных папках.

Прежде чем продолжить выполнение действий, описанных в этом разделе, выполните действия, описанные в разделе [Настройка сценариев](#set-up-scripts) , чтобы подготовить рабочий каталог с помощью демонстрационных сценариев создания сертификата.

### <a name="windows"></a>Windows

1. Перейдите к рабочему каталогу, в который были помещены скрипты создания сертификатов.

1. Создайте сертификат корневого ЦС и подпишите один промежуточный сертификат. Все сертификаты помещаются в рабочий каталог.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей, но если статьи запрашивают **сертификат корневого ЦС**, используйте следующий файл:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

### <a name="linux"></a>Linux

1. Перейдите к рабочему каталогу, в который были помещены скрипты создания сертификатов.

1. Создайте корневой сертификат ЦС и один промежуточный сертификат.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей, но если статьи запрашивают **сертификат корневого ЦС**, используйте следующий файл:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

## <a name="create-iot-edge-device-identity-certificates"></a>Создание IoT Edge сертификатов удостоверений устройств

Сертификаты удостоверений устройств используются для инициализации устройств IoT Edge, если вы решили использовать проверку подлинности на сертификате X. 509. Эти сертификаты работают независимо от того, используется ли подготовка вручную или автоматическая подготовка с помощью службы подготовки устройств для центра Интернета вещей Azure (DPS).

Сертификаты удостоверений устройств находятся в разделе **подготовки** файла конфигурации на IOT Edge устройстве.

Прежде чем продолжить выполнение действий, описанных в этом разделе, выполните действия, описанные в разделах [Настройка сценариев](#set-up-scripts) и [Создание сертификатов корневого ЦС](#create-root-ca-certificate) .

### <a name="windows"></a>Windows

Создайте IoT Edge сертификат удостоверения устройства и закрытый ключ с помощью следующей команды:

```powershell
New-CACertsEdgeDeviceIdentity "<name>"
```

Имя, передаваемое в эту команду, будет ИДЕНТИФИКАТОРом устройства IoT Edge устройства в центре Интернета вещей.

Новая команда удостоверения устройства создает несколько файлов сертификатов и ключей, включая три, которые вы будете использовать при создании отдельной регистрации в DPS и установке среды выполнения IoT Edge:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>\certs\iot-edge-device-identity-<name>.cert.pem`
* `<WRKDIR>\private\iot-edge-device-identity-<name>.key.pem`

### <a name="linux"></a>Linux

Создайте IoT Edge сертификат удостоверения устройства и закрытый ключ с помощью следующей команды:

```bash
./certGen.sh create_edge_device_identity_certificate "<name>"
```

Имя, передаваемое в эту команду, будет ИДЕНТИФИКАТОРом устройства IoT Edge устройства в центре Интернета вещей.

Сценарий создает несколько файлов сертификатов и ключей, включая три, которые вы будете использовать при создании отдельной регистрации в DPS и установке среды выполнения IoT Edge:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>/certs/iot-edge-device-identity-<name>.cert.pem`
* `<WRKDIR>/private/iot-edge-device-identity-<name>.key.pem`

## <a name="create-iot-edge-device-ca-certificates"></a>Создание сертификатов ЦС устройств IoT Edge

Каждому IoT Edgeу устройству в рабочей среде требуется сертификат ЦС устройства, на который имеется ссылка в файле конфигурации.
Сертификат ЦС устройства отвечает за создание сертификатов для модулей, выполняющихся на устройстве.
Это также необходимо для сценариев шлюза, поскольку сертификат ЦС устройства — это то, как устройство IoT Edge проверяет свое удостоверение на наличие подчиненных устройств.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Сертификаты ЦС устройств находятся в разделе **Certificate** файла config. YAML на устройстве IOT Edge.
:::moniker-end

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"
Сертификаты ЦС устройств находятся в разделе **CA** в файле config. томл на устройстве IOT Edge.
:::moniker-end

Прежде чем продолжить выполнение действий, описанных в этом разделе, выполните действия, описанные в разделах [Настройка сценариев](#set-up-scripts) и [Создание сертификатов корневого ЦС](#create-root-ca-certificate) .

### <a name="windows"></a>Windows

1. Перейдите в рабочий каталог, содержащий скрипты создания сертификатов и сертификат корневого ЦС.

2. Создайте сертификат ЦС устройства IoT Edge и закрытый ключ с помощью следующей команды. Укажите имя сертификата ЦС.

   ```powershell
   New-CACertsEdgeDevice "<CA cert name>"
   ```

   Эта команда создает несколько файлов сертификатов и ключей. Следующий сертификат и пара ключей должны быть скопированы на устройство IoT Edge и ссылаться на него в файле конфигурации:

   * `<WRKDIR>\certs\iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-<CA cert name>.key.pem`

Имя, передаваемое команде **New-кацертседжедевице** , не должно совпадать с именем параметра HostName в файле конфигурации или идентификатором устройства в центре Интернета вещей.

### <a name="linux"></a>Linux

1. Перейдите в рабочий каталог, содержащий скрипты создания сертификатов и сертификат корневого ЦС.

2. Создайте сертификат ЦС устройства IoT Edge и закрытый ключ с помощью следующей команды. Укажите имя сертификата ЦС.

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "<CA cert name>"
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей. Следующий сертификат и пара ключей должны быть скопированы на устройство IoT Edge и ссылаться на него в файле конфигурации:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Имя, передаваемое команде **create_edge_device_ca_certificate** , не должно совпадать с именем параметра HostName в файле конфигурации или идентификатором устройства в центре Интернета вещей.

## <a name="create-downstream-device-certificates"></a>Создание подчиненных сертификатов устройств

Если вы настраиваете подчиненное устройство IoT для сценария шлюза и хотите использовать проверку подлинности X. 509, вы можете создать демонстрационные сертификаты для подчиненного устройства.
Если вы хотите использовать проверку подлинности с симметричным ключом, вам не нужно создавать дополнительные сертификаты для подчиненного устройства.
Проверить подлинность устройства IoT с помощью сертификатов X. 509 можно двумя способами: с помощью самозаверяющих сертификатов или с помощью сертификатов, подписанных центром сертификации.
Для проверки подлинности на основе самозаверяющего сертификата X.509 (иногда называется проверкой подлинности на основе отпечатков) необходимо создать новые сертификаты, которые размещаются на устройстве IoT.
Эти сертификаты имеют отпечаток, который указывается в центре Интернета вещей для проверки подлинности.
Для проверки подлинности на основе сертификата X.509, подписанного ЦС, требуется сертификат корневого ЦС, зарегистрированный в центре Интернета вещей, который используется для подписи сертификатов для устройства IoT.
Для любого устройства, использующего сертификат, выданный корневым ЦС или любым из его промежуточных сертификатов, будет разрешена проверка подлинности.

Сценарии создания сертификатов могут помочь в создании демонстрационных сертификатов для тестирования любого из этих сценариев проверки подлинности.

Прежде чем продолжить выполнение действий, описанных в этом разделе, выполните действия, описанные в разделах [Настройка сценариев](#set-up-scripts) и [Создание сертификатов корневого ЦС](#create-root-ca-certificate) .

### <a name="self-signed-certificates"></a>Самозаверяющие сертификаты

При проверке подлинности устройства IoT с помощью самозаверяющих сертификатов необходимо создать сертификаты устройств на основе сертификата корневого ЦС для вашего решения.
Затем вы получаете шестнадцатеричный отпечаток из сертификатов для предоставления в центр Интернета вещей.
Устройству Интернета вещей также требуется копия сертификатов устройств, чтобы она могла проходить проверку подлинности в центре Интернета вещей.

#### <a name="windows"></a>Windows

1. Перейдите в рабочий каталог, содержащий скрипты создания сертификатов и сертификат корневого ЦС.

2. Создайте два сертификата (основной и дополнительный) для подчиненного устройства. Простое соглашение об именовании — создание сертификатов с именем устройства IoT, а затем первичной или вторичной метки. Пример:

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей. Следующие пары сертификатов и ключей необходимо скопировать на подчиненное устройство Интернета вещей и ссылаться в приложениях, подключающихся к центру Интернета вещей:

   * `<WRKDIR>\certs\iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>\private\iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>\private\iot-device-<device name>-secondary.key.pem`

3. Получите отпечаток SHA1 (который называется отпечатком в контекстах центра Интернета вещей) из каждого сертификата. Отпечаток — это строка шестнадцатеричного символа 40. Используйте следующую команду OpenSSL для просмотра сертификата и поиска отпечатка:

   ```PowerShell
   openssl x509 -in <WRKDIR>\certs\iot-device-<device name>-primary.cert.pem -text -fingerprint
   ```

   Выполните эту команду дважды, один раз для основного и один раз для дополнительного сертификата. Отпечатки для обоих сертификатов предоставляются при регистрации нового устройства IoT с помощью самозаверяющих сертификатов X.509.

#### <a name="linux"></a>Linux

1. Перейдите в рабочий каталог, содержащий скрипты создания сертификатов и сертификат корневого ЦС.

2. Создайте два сертификата (основной и дополнительный) для подчиненного устройства. Простое соглашение об именовании — создание сертификатов с именем устройства IoT, а затем первичной или вторичной метки. Пример:

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей. Следующие пары сертификатов и ключей необходимо скопировать на подчиненное устройство Интернета вещей и ссылаться в приложениях, подключающихся к центру Интернета вещей:

   * `<WRKDIR>/certs/iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>/private/iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>/private/iot-device-<device name>-secondary.key.pem`

3. Получите отпечаток SHA1 (который называется отпечатком в контекстах центра Интернета вещей) из каждого сертификата. Отпечаток — это строка шестнадцатеричного символа 40. Используйте следующую команду OpenSSL для просмотра сертификата и поиска отпечатка:

   ```bash
   openssl x509 -in <WRKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   При регистрации нового устройства IoT с помощью самозаверяющих сертификатов X. 509 вы предоставляете основной и дополнительный отпечатки пальца.

### <a name="ca-signed-certificates"></a>Сертификаты, подписанные ЦС

При проверке подлинности устройства IoT с помощью самозаверяющих сертификатов необходимо передать сертификат корневого ЦС для решения в центр Интернета вещей.
Затем вы выполняете проверку, чтобы доказать центр Интернета вещей, которому принадлежит сертификат корневого ЦС.
Наконец, вы используете один и тот же корневой сертификат ЦС для создания сертификатов устройств для размещения на устройстве IoT, чтобы он мог проходить проверку подлинности в центре Интернета вещей.

Сертификаты в этом разделе предназначены для действий, описанных в статье [Настройка безопасности X. 509 в центре Интернета вещей Azure](../iot-hub/iot-hub-security-x509-get-started.md).

#### <a name="windows"></a>Windows

1. Отправьте файл сертификата корневого ЦС из рабочего каталога `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem` в центр Интернета вещей.

2. Используйте код, указанный в портал Azure, чтобы убедиться, что вы владеете сертификатом корневого ЦС.

   ```PowerShell
   New-CACertsVerificationCert "<verification code>"
   ```

3. Создайте цепочку сертификатов для подчиненного устройства. Используйте тот же идентификатор устройства, в котором зарегистрировано устройство в центре Интернета вещей.

   ```PowerShell
   New-CACertsDevice "<device id>"
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей. Следующие пары сертификатов и ключей необходимо скопировать на подчиненное устройство Интернета вещей и ссылаться в приложениях, подключающихся к центру Интернета вещей:

   * `<WRKDIR>\certs\iot-device-<device id>.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>-full-chain.cert.pem`  
   * `<WRKDIR>\private\iot-device-<device id>.key.pem`

#### <a name="linux"></a>Linux

1. Отправьте файл сертификата корневого ЦС из рабочего каталога `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem` в центр Интернета вещей.

2. Используйте код, указанный в портал Azure, чтобы убедиться, что вы владеете сертификатом корневого ЦС.

   ```bash
   ./certGen.sh create_verification_certificate "<verification code>"
   ```

3. Создайте цепочку сертификатов для подчиненного устройства. Используйте тот же идентификатор устройства, в котором зарегистрировано устройство в центре Интернета вещей.

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

   Эта команда скрипта создает несколько файлов сертификатов и ключей. Следующие пары сертификатов и ключей необходимо скопировать на подчиненное устройство Интернета вещей и ссылаться в приложениях, подключающихся к центру Интернета вещей:

   * `<WRKDIR>/certs/iot-device-<device id>.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device id>.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device id>-full-chain.cert.pem`  
   * `<WRKDIR>/private/iot-device-<device id>.key.pem`
