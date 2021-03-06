---
title: Подключение кэша HPC для Azure
description: Подключение клиентов к службе кэша Azure HPC
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/30/2020
ms.author: v-erkel
ms.openlocfilehash: 7f1d8d34d6351fc344fdb101ac8e9a96678df9d5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91651434"
---
# <a name="mount-the-azure-hpc-cache"></a>Mount the Azure HPC Cache (Подключение Azure HPC Cache)

После создания кэша клиенты NFS могут получить к нему доступ с помощью простой `mount` команды. Команда подключает конкретный целевой путь к хранилищу в кэше HPC Azure к локальному каталогу на клиентском компьютере.

Команда mount состоит из следующих элементов:

* Один из адресов подключения кэша (указан на странице обзора кэша).
* Путь к виртуальному пространству имен, заданный для целевого объекта хранилища (отображается на странице пространства имен кэша).
* Локальный путь для использования на клиенте
* Параметры команды, которые оптимизируют успешность этого типа подключения NFS

На странице **инструкции по подключению** для кэша собираются сведения и рекомендуемые параметры, а также создается команда подключения прототипа, которую можно скопировать. Дополнительные сведения см. [в статье Использование служебной программы по подключению](#use-the-mount-instructions-utility) .

## <a name="prepare-clients"></a>Подготовка клиентов

Убедитесь, что клиенты могут подключать кэш HPC для Azure, следуя указаниям в этом разделе.

### <a name="provide-network-access"></a>Предоставление доступа к сети

Клиентские компьютеры должны иметь сетевой доступ к виртуальной сети кэша и частной подсети.

Например, создайте виртуальные машины клиента в той же виртуальной сети или используйте конечную точку, шлюз или другое решение в виртуальной сети для доступа извне. (Помните, что ничего, кроме самого кэша, не следует размещать в подсети кэша.)

### <a name="install-utilities"></a>Установка служебных программ

Установите соответствующее служебное программное обеспечение Linux для поддержки команды NFS Mount:

* Для Red Hat Enterprise Linux или SuSE: `sudo yum install -y nfs-utils`
* Для Ubuntu или Debian: `sudo apt-get install nfs-common`

### <a name="create-a-local-path"></a>Создание локального пути

Создайте путь к локальному каталогу на каждом клиенте для подключения к кэшу. Создайте путь для каждого пути к пространству имен, которое необходимо подключить.

Например, `sudo mkdir -p /mnt/hpc-cache-1/target3`.

На странице [инструкции по подключению](#use-the-mount-instructions-utility) в портал Azure содержится команда prototype, которую можно скопировать.

При подключении клиентского компьютера к кэшу этот путь связывается с путем виртуального пространства имен, представляющего экспорт целевого объекта хранилища. Создайте каталоги для всех путей к виртуальному пространству имен, которые будут использоваться клиентом.

## <a name="use-the-mount-instructions-utility"></a>Использование служебной программы "инструкции по подключению"

Вы можете использовать страницу **инструкции по подключению** в портал Azure, чтобы создать копию команды подключения. Откройте страницу из раздела **Настройка** представления кэша на портале.

Перед использованием команды на клиенте убедитесь, что клиент отвечает предварительным требованиям и имеет программное обеспечение, необходимое для использования `mount` команды NFS, как описано выше в разделе [Подготовка клиентов](#prepare-clients).

![снимок экрана с экземпляром кэша Azure HPC на портале с загруженной страницей "Настройка инструкций по подключению >"](media/mount-instructions.png)

Выполните следующую процедуру, чтобы создать команду подключения.

1. Настройте поле " **путь клиента** ". В этом поле содержится пример команды, которую можно использовать для создания локального пути на клиенте. Клиент получает доступ к содержимому из кэша HPC Azure локально в этом каталоге.

   Щелкните поле и измените команду, чтобы она содержала нужное имя каталога. Имя отображается в конце строки после `sudo mkdir -p`

   ![снимок экрана: поле "путь клиента" с курсором, расположенным в конце](media/mount-edit-client.png)

   После завершения редактирования поля команда подключения в нижней части страницы обновляется с учетом нового пути клиента.

1. Выберите **адрес подключения кэша** из списка. В этом меню перечислены все [точки подключения клиента](#find-mount-command-components)кэша.

   Балансировка нагрузки клиента по всем доступным адресам подключения для повышения производительности кэша.

   ![снимок экрана: поле адреса подключения кэша с селектором, отображающим три IP-адреса для выбора](media/mount-select-ip.png)

1. Выберите **путь к виртуальному пространству имен** , который будет использоваться для клиента. Эти пути связаны с экспортами в серверной системе хранения.

   ![Снимок экрана, показывающий поле "путь к виртуальному пространству имен" с открытым селектором.](media/mount-select-target.png)

   Просмотреть и изменить пути к виртуальному пространству имен можно на странице портала **пространства имен** . Чтобы увидеть, как это сделать, прочитайте раздел [Настройка агрегированного пространства имен](add-namespace-paths.md) .

   Дополнительные сведения о функции агрегированного пространства имен в кэше Azure HPC см. в статье [планирование агрегированного пространства имен](hpc-cache-namespace.md).

1. Поле **команда подключения** на шаге 3 автоматически заполняется настраиваемой командой подключения, которая использует адрес подключения, виртуальный путь к пространству имен и путь клиента, заданные в предыдущих полях.

   Щелкните значок копирования справа от поля, чтобы автоматически скопировать его в буфер обмена.

   ![снимок экрана: поле команды подключения прототипа, показывающее текст для кнопки "Копировать в буфер обмена"](media/mount-command-copy.png)

1. Используйте скопированную команду подключения на клиентском компьютере, чтобы подключить его к кэшу HPC Azure. Команду можно выполнить непосредственно из командной строки клиента или включить команду подключения в скрипт или шаблон установки клиента.

## <a name="understand-mount-command-syntax"></a>Общие сведения о синтаксисе команд mount

Команда подключения имеет следующий вид:

> Sudo Mount {*Options*} *cache_mount_address*:/*namespace_path* *local_path*

Пример

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount -o hard,proto=tcp,mountproto=tcp,retry=30 10.0.0.28:/blob-demo-0722 hpccache
root@test-client:/tmp#
```

После выполнения этой команды содержимое экспорта хранилища будет отображаться в ``hpccache`` каталоге на клиенте.

### <a name="mount-command-options"></a>Параметры команды подключения

Для надежного подключения клиента передайте следующие параметры и аргументы в команде mount:

> mount-o жесткое, устанавливает = TCP, маунтпрото = TCP, Retry = 30 $ {CACHE_IP_ADDRESS}:/$ {NAMESPACE_PATH} $ {LOCAL_FILESYSTEM_MOUNT_POINT}

| Рекомендуемые параметры команды подключения | Описание |
--- | ---
``hard`` | Мягкое подключение к кэшу Azure HPC связано с ошибками приложений и возможной потерей данных.
``proto=tcp`` | Этот параметр поддерживает соответствующую обработку сетевых ошибок NFS.
``mountproto=tcp`` | Этот параметр поддерживает соответствующую обработку сетевых ошибок для операций подключения.
``retry=<value>`` | Задайте значение ``retry=30``, чтобы избежать временных ошибок при подключении. (При подключении переднего плана рекомендуется использовать другое значение.)

### <a name="find-mount-command-components"></a>Поиск компонентов команды подключения

Если вы хотите создать команду подключения без использования страницы " **инструкции по подключению** ", адреса подключения можно найти на странице " **Обзор** кэша" и пути к виртуальному пространству имен на странице " **пространство имен** ".

![снимок экрана со страницей обзора экземпляра кэша HPC Azure с выделенным полем вокруг списка адресов подключения в нижнем правом углу](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Адреса подключения к кэшу соответствуют сетевым интерфейсам в подсети кэша. В группе ресурсов эти сетевые карты перечислены с именами, которые заканчиваются на `-cluster-nic-` , и числом. Не изменяйте или не удаляйте эти интерфейсы, иначе кэш станет недоступным.

Пути к виртуальному пространству имен отображаются на странице параметров **пространства имен** кэша.

![снимок экрана: страница параметров >ного пространства имен на портале с выделенным полем вокруг первого столбца таблицы "путь к пространству имен"](media/view-namespace-paths.png)

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы переместить данные в целевые объекты хранилища кэша, прочитайте статью [заполнение нового хранилища BLOB-объектов Azure](hpc-cache-ingest.md).
