---
title: Обнаружение серверов в Hyper-V с помощью средства "Обнаружение и оценка" службы "Миграция Azure"
description: Узнайте, как обнаруживать локальные серверы на Hyper-V с помощью средства "Обнаружение и оценка" службы "Миграция Azure".
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 03/25/2021
ms.custom: mvc
ms.openlocfilehash: f461778f988fafeacc480e100b00be7d4c165dfb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105612523"
---
# <a name="tutorial-discover-servers-running-on-hyper-v-with-azure-migrate-discovery-and-assessment"></a>Руководство по обнаружению серверов, работающих в Hyper-V, с помощью средства "Обнаружение и оценка" службы "Миграция Azure"

В процессе переноса в Azure вы обнаружите локальные данные инвентаризации и рабочие нагрузки.

В этом руководстве показано, как обнаружить локальные физические серверы на узлах Hyper-V с помощью средства "Обнаружение и оценка" службы "Миграция Azure", используя упрощенное устройство Миграции Azure. Устройство развертывается как сервер, работающий в узле Hyper-V, для непрерывного обнаружения метаданных компьютеров и производительности.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Настройка учетной записи Azure
> * Подготовка среды Hyper-V к обнаружению.
> * Создайте проект.
> * Настройка устройства службы "Миграция Azure".
> * Запуск непрерывного обнаружения.

> [!NOTE]
> В учебниках показан самый быстрый способ выполнения сценария и используются параметры по умолчанию.  

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим учебником проверьте наличие необходимых компонентов.

**Требование** | **Сведения**
--- | ---
**Узел Hyper-V** | Узлы Hyper-V, в которых размещены серверы, могут быть изолированными или находиться в кластере.<br/><br/> Узел должен работать под управлением Windows Server 2019, Windows Server 2016 или Windows Server 2012 R2.<br/><br/> Убедитесь, что разрешены входящие подключения по порту WinRM 5985 (HTTP). Это требуется устройству для запрашивания метаданных сервера и данных о производительности через сеансы модели CIM.
**Развертывание устройства** | Чтобы выделить сервер для устройства, узлу Hyper-V понадобятся такие ресурсы:<br/><br/> — 16 ГБ ОЗУ, 8 виртуальных ЦП, 80 ГБ дискового пространства.<br/><br/> — внешний виртуальный коммутатор и доступ к Интернету для устройства (напрямую или через прокси-сервер).
**Серверы** | Серверы могут работать под управлением ОС Windows или Linux.

## <a name="prepare-an-azure-user-account"></a>Подготовка учетной записи пользователя Azure

Чтобы создать проект и зарегистрировать устройство службы "Миграция Azure", вам понадобится учетная запись с:
- разрешениями участника или владельца в подписке Azure;
- разрешениями на регистрацию приложений Azure Active Directory (AAD).

Если вы создали бесплатную учетную запись Azure, вы являетесь владельцем подписки. Если вы не являетесь владельцем подписки, назначьте разрешения совместно с владельцем следующим образом:

1. На портале Azure выполните поиск по фразе "подписки" и в разделе **Службы** выберите **Подписки**.

    ![Поле для поиска подписки Azure](./media/tutorial-discover-hyper-v/search-subscription.png)

2. На странице **Подписки** выберите подписку, в которой хотите создать проект.
3. На странице подписок последовательно выберите **Управление доступом (IAM)**  > **Проверить доступ**.
4. В разделе **Проверка доступа** найдите соответствующую учетную запись пользователя.
5. В разделе **Добавление назначения роли** щелкните **Добавить**.

    ![Поиск учетной записи пользователя для проверки доступа и назначения роли](./media/tutorial-discover-hyper-v/azure-account-access.png)

6. В разделе **Добавление назначения роли** выберите роль "Участник" или "Владелец", а затем — учетную запись (в нашем примере — azmigrateuser). Затем нажмите кнопку **Сохранить**.

    ![Открытие страницы "Добавить назначение ролей" для назначения роли учетной записи](./media/tutorial-discover-hyper-v/assign-role.png)

1. Чтобы зарегистрировать устройство, учетной записи Azure необходимы **разрешения на регистрацию приложений AAD**.
1. На портале Azure перейдите в раздел **Azure Active Directory** > **Пользователи** > **Параметры пользователей**.
1. В разделе **Параметры пользователя** проверьте, смогут ли пользователи Azure AD регистрировать приложения (по умолчанию задано значение **Да**).

    ![Проверка того, могут ли пользователи регистрировать приложения Active Directory в разделе "Параметры пользователя"](./media/tutorial-discover-hyper-v/register-apps.png)

9. Если для параметра "Регистрация приложений" задано значение "Нет", попросите администратора арендатора или глобального администратора назначить необходимое разрешение. Кроме того, администратор арендатора или глобальный администратор могут назначить учетной записи роль **Разработчик приложений**, позволяющую регистрировать приложения AAD. [Подробнее](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-hyper-v-hosts"></a>Подготовка узлов Hyper-V

Узлы Hyper-V можно подготовить вручную или с помощью скрипта. В таблице кратко описаны действия по подготовке. Скрипт автоматически выполняет подготовку узлов.

**Step** | **Скрипт** | **Вручную**
--- | --- | ---
Проверка требований к узлу | Проверяется, выполняется ли на узле поддерживаемая версия и роль Hyper-V.<br/><br/>На узле включается служба WinRM и открываются порты 5985 (HTTP) и 5986 (HTTPS) (требуется для сбора метаданных). | Узел должен работать под управлением Windows Server 2019, Windows Server 2016 или Windows Server 2012 R2.<br/><br/> Убедитесь, что разрешены входящие подключения по порту WinRM 5985 (HTTP). Это требуется устройству для запрашивания метаданных сервера и данных о производительности через сеансы модели CIM.<br/><br/> В настоящее время этот скрипт не поддерживается на узлах с языковым стандартом, отличным от английского.  
Проверка версии PowerShell | Проверяется, используете ли вы скрипт в поддерживаемой версии PowerShell. | Убедитесь, что на узле Hyper-V используется PowerShell версии 4.0 или более поздней.
Создание учетной записи | Проверяет наличие необходимых разрешений на узле Hyper-V.<br/><br/> Позволяет создать локальную учетную запись пользователя с нужными разрешениями. | Вариант 1. Подготовьте учетную запись, используя права администратора, на хост-компьютере Hyper-V.<br/><br/> Вариант 2. Подготовьте учетную запись локального администратора или учетную запись администратора домена и добавьте ее в следующие группы: "Пользователи удаленного управления", "Администраторы Hyper-V" и "Пользователи системного монитора".
Включение удаленного взаимодействия PowerShell | Включает в узле удаленное взаимодействие с PowerShell, чтобы устройство Миграции Azure могло выполнять команды PowerShell в узле через подключение WinRM. | Для настройки откройте на каждом узле консоль PowerShell с правами администратора и выполните команду ``` powershell Enable-PSRemoting -force ```.
Настройка служб интеграции Hyper-V | Проверяет, включены ли службы Integration Services Hyper-V на всех серверах, управляемых узлом. | [Включите службы интеграции Hyper-V](/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) на каждом сервере.<br/><br/> Если вы используете Windows Server 2003, [выполните эти инструкции](prepare-windows-server-2003-migration.md).
Делегирование учетных данных, если диски сервера находятся в удаленных общих папках SMB | Делегирование учетных данных | Чтобы включить CredSSP для делегирования учетных данных в узлах с серверами и дисками в общих папках SMB, выполните команду ```powershell Enable-WSManCredSSP -Role Server -Force ```.<br/><br/> Вы можете запустить эту команду удаленно на всех узлах Hyper-V.<br/><br/> Если вы добавляете новые главные узлы в кластер, они автоматически добавляются для обнаружения, но вам необходимо вручную включить CredSSP.<br/><br/> При настройке устройства вы закончите настройку шифрования CredSSP, [включив его на устройстве.](#delegate-credentials-for-smb-vhds) 

### <a name="run-the-script"></a>Выполнение скрипта

1. Скачайте скрипт из [Центра загрузки Майкрософт](https://aka.ms/migrate/script/hyperv). Скрипт криптографически подписывается корпорацией Майкрософт.
2. Проверьте целостность скрипта с помощью хэш-файлов MD5 или SHA256. Значения хэша указаны ниже. Выполните следующую команду, чтобы создать хэш для скрипта.

    ```powershell
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Пример использования:

    ```powershell
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1 SHA256
    ```
3. После проверки целостности скрипта запустите его на каждом узле Hyper-V с помощью следующей команды PowerShell с повышенными правами:

    ```powershell
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```
Хэш-значениями являются:

**Хэш** |  **Значение**
--- | ---
MD5 | 0ef418f31915d01f896ac42a80dc414e
SHA256 | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2

## <a name="set-up-a-project"></a>Настройка проекта

Настройте новый проект.

1. На портал Azure выберите **Все службы** и найдите службу **Миграция Azure**.
2. В разделе **Службы** выберите **Миграция Azure**.
3. В разделе **Обзор** выберите **Создать проект**.
5. В разделе **Создать проект** выберите подписку Azure и группу ресурсов. Если у вас еще нет группы ресурсов, создайте ее.
6. В разделе **Сведения о проекте** укажите имя проекта и регион для создания проекта. Просмотрите список поддерживаемых регионов для [общедоступного](migrate-support-matrix.md#supported-geographies-public-cloud) облака и облака для [государственных организаций](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Поля для имени и региона проекта](./media/tutorial-discover-hyper-v/new-project.png)

7. Нажмите кнопку **создания**.
8. Подождите несколько минут, пока завершится развертывание проекта. **Средство "Обнаружение и оценка" службы "Миграция Azure"** по умолчанию добавляется в каждый новый проект.

![На странице отображается добавленное по умолчанию средство "Обнаружение и оценка" службы "Миграция Azure"](./media/tutorial-discover-hyper-v/added-tool.png)

> [!NOTE]
> Если вы уже создали проект, вы можете использовать один и тот же проект для регистрации дополнительных устройств, чтобы обнаружить и оценить большее количество серверов. [См. дополнительные сведения](create-manage-projects.md#find-a-project).

## <a name="set-up-the-appliance"></a>Настройка устройства

Служба "Миграция Azure" использует упрощенное устройство Миграции Azure. Устройство выполняет обнаружение серверов и отправляет метаданные об их конфигурации и производительности в службу "Миграция Azure". Устройство можно настроить путем развертывания VHD-файла, который можно скачать из проекта.

> [!NOTE]
> Если по какой-либо причине вы не можете настроить устройство с помощью шаблона, настройте его с помощью скрипта PowerShell на существующем сервере Windows Server 2016. [Подробнее](deploy-appliance-script.md#set-up-the-appliance-for-hyper-v).

В рамках этого руководства для настройки устройства на сервере в среде Hyper-V используется следующая процедура:

1. Укажите имя устройства и создайте ключ проекта на портале.
1. Скачайте сжатый виртуальный жесткий диск Hyper-V с портала Azure.
1. Создайте устройство и убедитесь, что оно может подключиться к средству "Обнаружение и оценка" службы "Миграция Azure".
1. Выполните первоначальную настройку устройства и зарегистрируйте его в проекте с помощью ключа проекта.

### <a name="1-generate-the-project-key"></a>1. Создание ключа проекта

1. Откройте раздел **Цели миграции** > **Серверы** > **Azure Migrate: Discovery and assessment (Миграция Azure: обнаружение и оценка)** и выберите элемент **Обнаружение**.
2. В разделе **Discover servers (Обнаружение серверов)**  > **Are your servers virtualized? (Серверы виртуализированы?)** выберите вариант **Да, с Hyper-V**.
3. В разделе **1:Generate project key** (1. Создание ключа проекта) укажите имя устройства Миграции Azure, которое вы настроите для обнаружения серверов. Это имя должно содержать буквы и цифры (не более 14 символов).
1. Щелкните **Создать ключ**, чтобы начать создание необходимых ресурсов Azure. Не закрывайте страницу обнаружения серверов во время создания ресурсов.
1. После успешного создания ресурсов Azure будет создан **ключ проекта**.
1. Скопируйте этот ключ, так как он понадобится вам для завершения регистрации модуля во время его настройки.

### <a name="2-download-the-vhd"></a>2. Скачивание VHD-файла

В разделе **2: Download Azure Migrate appliance** (2. Скачивание виртуального модуля службы "Миграция Azure") выберите VHD-файл и щелкните **Загрузка**.

### <a name="verify-security"></a>Проверка безопасности

Прежде чем развертывать сжатый ZIP-файл, убедитесь, что он не поврежден.

1. На компьютере, на который был скачан файл, откройте командное окно с правами администратора.

2. Выполните следующую команду PowerShell, чтобы создать хэш ZIP-файла.
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Пример использования: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v3.20.09.25.zip -Algorithm SHA256```

3.  Проверьте последние версии и хэш-значения устройств:

    - Для общедоступного облака Azure:

        **Сценарий** | **Загрузить** | **SHA256**
        --- | --- | ---
        Hyper-V (8,91 ГБ) | [Последняя версия](https://go.microsoft.com/fwlink/?linkid=2140422) |  79c151588de049cc102f61b910d6136e02324dc8d8a14f47772da351b46d9127

    - Для Azure для государственных организаций:

        **Сценарий** _ | _ *Скачать** | **SHA256**
        --- | --- | ---
        Hyper-V (85,8 МБ) | [Последняя версия](https://go.microsoft.com/fwlink/?linkid=2140424) |  cfed44bb52c9ab3024a628dc7a5d0df8c624f156ec1ecc3507116bae330b257f

### <a name="3-create-an-appliance"></a>3. Создание устройства

Импортируйте скачанный файл и создайте устройство.

1. Извлеките сжатый VHD-файл в папку в узле Hyper-V, где будет размещено устройство. Извлекаются три папки.
2. Откройте диспетчер Hyper-V. В меню **Actions** (Действия) щелкните команду **Import Virtual Machine** (Импорт виртуальной машины).
2. В мастере импорта виртуальных машин в разделе **Before you begin** (Перед началом работы) нажмите кнопку **Next** (Далее).
3. В поле **Locate Folder** (Определить папку) укажите папку, содержащую извлеченный VHD. Затем нажмите кнопку **Далее**.
1. В меню **Select Virtual Machine** (Выбор виртуальной машины) нажмите кнопку **Next** (Далее).
2. В меню **Choose Import Type** (Выбор типа импорта) щелкните **Copy the virtual machine (create a new unique ID)** (Копировать виртуальную машину (создать уникальный идентификатор)). Затем нажмите кнопку **Далее**.
3. В меню **Choose Destination** (Выбор назначения) оставьте параметр по умолчанию. Щелкните **Далее**.
4. В меню **Storage Folders** (Папки хранилища) оставьте значение по умолчанию. Щелкните **Далее**.
5. В поле **Choose Network** (Выбор сети) укажите виртуальный коммутатор, который будет использоваться устройством. Ему требуется подключение к Интернету для отправки данных в Azure.
6. Просмотрите информацию на странице **Summary** (Сводка). Нажмите кнопку **Готово**.
7. Запустите устройство в диспетчере Hyper-V, в разделе **Virtual Machines** (Виртуальные машины).

### <a name="verify-appliance-access-to-azure"></a>Проверка доступа устройства к Azure

Убедитесь, что устройство может подключиться к URL-адресам Azure для [общедоступного](migrate-appliance.md#public-cloud-urls) облака и облака для [государственных организаций](migrate-appliance.md#government-cloud-urls).

### <a name="4-configure-the-appliance"></a>4. Настройка устройства

Настройте устройство в первый раз.

> [!NOTE]
> Если вы настраиваете устройство с помощью [скрипта PowerShell](deploy-appliance-script.md), а не скачанного виртуального жесткого диска, первые два этапа этой процедуры выполнять не нужно.

1. В диспетчере Hyper-V в разделе **Virtual Machines** (Виртуальные машины) щелкните устройство правой кнопкой мыши и выберите элемент **Connect** (Подключиться).
2. Укажите язык, часовой пояс и пароль для устройства.
3. Откройте браузер на любом компьютере, который может подключиться к устройству, и откройте URL-адрес веб-приложения устройства: **https://*имя или IP-адрес устройства*: 44368**.

   Кроме того, вы можете открыть приложение с рабочего стола устройства, щелкнув ярлык приложения.
1. Примите **условия лицензии** и прочитайте информацию от сторонних производителей.
1. В веб-приложении выберите **Настройка необходимых компонентов** и выполните приведенные ниже действия.
    - **Возможность подключения**: приложение проверит наличие доступа устройства к Интернету. Если устройство использует прокси-сервер:
      - Щелкните элемент **Настройка прокси-сервера** и укажите адрес прокси-сервера (в формате http://ProxyIPAddress или http://ProxyFQDN) ) и порт прослушивания.
      - Укажите учетные данные, если для прокси-сервера требуется аутентификация.
      - Поддерживается только прокси-сервер HTTP.
      - Если вы добавили сведения о прокси-сервере или отключили его и (или) проверку подлинности, щелкните **Сохранить**, чтобы снова запустить проверку подключения.
    - **Синхронизация времени**: время проверяется. Для правильной работы обнаружения серверов время на устройстве должно быть синхронизировано со временем в Интернете.
    - **Установка обновлений**: средство "Обнаружение и оценка" службы "Миграция Azure" проверяет, установлены ли на устройстве последние обновления. После завершения проверки вы можете щелкнуть пункт **View appliance services** (Просмотр служб модуля), чтобы просмотреть состояние и версии компонентов, используемых модулем.

### <a name="register-the-appliance-with-azure-migrate"></a>Регистрация устройства с помощью службы "Миграция Azure"

1. Вставьте **ключ проекта**, ранее скопированный на портале. Если у вас нет ключа, последовательно выберите **Azure Migrate: Discovery and assessment > Discover > Manage existing appliances** (Миграция Azure: обнаружение и оценка > Обнаружение > Управление существующими устройствами), выберите имя устройства, указанное вами во время создания ключа, и скопируйте соответствующий ключ.
1. Вам потребуется код устройства для аутентификации в Azure. Если щелкнуть **Вход**, откроется модальное окно с кодом устройства, как показано ниже.

    ![Модальное окно с кодом устройства](./media/tutorial-discover-vmware/device-code.png)

1. Щелкните **Копировать код и войти**, чтобы скопировать код устройства и открыть окно входа в Azure в новой вкладке браузера. Если этот параметр не отображается, убедитесь, что в браузере отключена блокировка всплывающих окон.
1. На новой вкладке вставьте код устройства и выполните вход, используя имя пользователя и пароль Azure.
   
   Вход с помощью PIN-кода не поддерживается.
3. Если вы случайно закроете вкладку входа, не выполнив вход, вам потребуется обновить вкладку браузера в диспетчере конфигурации устройства, чтобы снова активировать кнопку "Вход".
1. После успешного входа вернитесь на предыдущую вкладку с помощью диспетчера конфигурации устройства.
4. Если учетная запись пользователя Azure, используемая для ведения журнала, имеет необходимые разрешения на ресурсы Azure, созданные во время создания ключа, будет инициирована регистрация модуля.
1. После регистрации модуля вы можете просмотреть сведения о регистрации, щелкнув **Просмотреть подробности**.

### <a name="delegate-credentials-for-smb-vhds"></a>Делегирование учетных данных для виртуальных жестких дисков SMB

Если вы запускаете виртуальные жесткие диски в SMB, необходимо включить делегирование учетных данных с устройства на узлы Hyper-V. Чтобы сделать это на устройстве, выполните следующее:

1. На устройстве выполните эту команду. HyperVHost1/HyperVHost2 — это примеры имен узлов.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
    ```

2. Кроме того, это можно сделать в редакторе локальных групповых политик на устройстве:
    - В разделе **Политика локального компьютера** > **Конфигурация компьютера** щелкните **Административные шаблоны** > **Система** > **Передача учетных данных**.
    - Дважды щелкните **Разрешить передачу новых учетных данных** и выберите **Включено**.
    - В разделе **Параметры** щелкните **Показать** и добавьте в список каждый узел Hyper-V, который требуется обнаружить, используя **wsman/** в качестве префикса.
    - В области **Передача учетных данных** дважды щелкните **Разрешить делегирование новых учетных данных с проверкой подлинности сервера "только NTLM"**. Еще раз добавьте в список каждый узел Hyper-V, который требуется обнаружить, используя **wsman/** в качестве префикса.

## <a name="start-continuous-discovery"></a>Запуск непрерывного обнаружения

Подключитесь с устройства к узлам или кластерам Hyper-V и запустите обнаружение серверов.

1. В разделе **Step 1. Provide Hyper-V host credentials** (Шаг 1. Предоставление учетных данных узла Hyper-V) щелкните элемент **Добавить учетные данные**, чтобы указать понятное имя для учетных данных, добавьте значения **Имя пользователя** и **Пароль** для того узла или кластера Hyper-V, который будет использоваться устройством для обнаружения серверов. Щелкните **Save**(Сохранить).
1. Если вы хотите добавить несколько учетных данных одновременно, щелкните **Добавить еще**, чтобы сохранить и добавить дополнительные учетные данные. Для обнаружения серверов в среде Hyper-V поддерживается несколько вариантов учетных данных.
1. На **шаге 2 Provide Hyper-V host/cluster details** (Предоставление сведений об узле или кластере Hyper-V) щелкните **Add discovery source** (Добавить источник обнаружения), чтобы указать **IP-адрес или полное доменное имя** узла или кластера Hyper-V и понятное имя учетных данных для подключения к узлу или кластеру.
1. За один раз можно выбрать пункт **Add single item** (Добавить один элемент) или **Add multiple items** (Добавить несколько элементов). Здесь также есть параметр **Импорт CSV** для предоставления сведений об узле или кластере Hyper-V.

    - Если вы выберете пункт **Add single item** (Добавить один элемент), вам необходимо будет указать понятное имя для учетных данных и **IP-адрес или полное доменное имя** узла или кластера Hyper-V, а затем щелкнуть **Сохранить**.
    - Если вы выберете пункт **Add multiple items** (Добавить несколько элементов) _(выбрано по умолчанию)_ , вы сможете добавить несколько записей одновременно, указав в текстовом поле **IP-адрес или полное доменное имя** узла либо кластера Hyper-V и понятное имя для учетных данных. **Проверьте** добавленные записи и нажмите кнопку **Сохранить**.
    - Если вы выберете пункт **Импорт CSV**, вы сможете скачать CSV-файл шаблона и указать в нем **IP-адрес или полное доменное имя** узла или кластера Hyper-V и понятное имя для учетных данных. Затем импортируйте файл в виртуальный модуль, нажмите кнопку **Проверить** для проверки записей в файле и щелкните **Сохранить**.

1. При нажатии кнопки "Сохранить" виртуальный модуль попытается проверить подключение к добавленным узлам или кластерам Hyper-V и отобразит **состояние проверки** в таблице для каждого из них.
    - Вы можете просмотреть дополнительные сведения о проверенных узлах или кластерах, щелкнув их IP-адрес или полное доменное имя.
    - Если проверка узла завершилась неудачно, просмотрите ошибку, щелкнув пункт **Сбой проверки** в столбце "Состояние" таблицы. Устраните проблему и выполните проверку еще раз.
    - Чтобы удалить узлы или кластеры, щелкните **Удалить**.
    - Невозможно удалить конкретный узел из кластера. Вы можете удалить только весь кластер.
    - Вы можете добавить кластер, даже если возникли проблемы с конкретными узлами в кластере.
1. Вы можете **повторно проверить** подключение к узлам или кластерам в любое время перед началом обнаружения.
1. Щелкните элемент **Начать обнаружение**, чтобы начать обнаружение серверов с проверенными узлами или кластерами. После успешного запуска обнаружения вы можете проверить состояние обнаружения для каждого узла или кластера в таблице.

Запустится обнаружение. Чтобы метаданные обнаруженных серверов отобразились на портале Azure, потребуется около 2 минут на узел.

## <a name="verify-servers-in-the-portal"></a>Проверка серверов на портале

После завершения обнаружения вы можете проверить, отображаются ли серверы на портале Azure.

1. Откройте панель мониторинга службы "Миграция Azure".
2. На странице **Azure Migrate — Servers** > **Azure Migrate: Discovery and assessment** (Миграция Azure — серверы > Миграция Azure: обнаружение и оценка) щелкните значок, отображающий количество **обнаруженных серверов**.

## <a name="next-steps"></a>Дальнейшие действия

- [Оцените серверы в среде Hyper-V](tutorial-assess-hyper-v.md) на предмет готовности к миграции на виртуальные машины Azure.
- [Просмотрите данные](migrate-appliance.md#collected-data---hyper-v), собранные устройством во время обнаружения.