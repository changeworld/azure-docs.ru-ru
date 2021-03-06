---
title: включить файл
description: файл
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: mingshen-ms
ms.author: krsh
ms.date: 03/25/2021
ms.openlocfilehash: 8898a762e8a1e7a2d5c104f99d12032c676a5ca4
ms.sourcegitcommit: a9ce1da049c019c86063acf442bb13f5a0dde213
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105630312"
---
## <a name="generalize-the-image"></a>Обобщение образа

Все образы в Azure Marketplace должны быть доступны для повторного использования в первоначальном виде. Для этой цели виртуальный жесткий диск операционной системы необходимо подготовить (обобщить). Эта операция удаляет с виртуальной машины все идентификаторы, относящиеся к определенному экземпляру, и программные драйверы.

### <a name="for-windows"></a>Для Windows

Диски ОС Windows обобщены с помощью средства [Sysprep](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) . Если позже вы обновите или перенастройте ОС, необходимо снова запустить Sysprep.

> [!WARNING]
> После запуска программы Sysprep выключите виртуальную машину, пока она не будет развернута, так как обновления могут запускаться автоматически. Это завершение работы не позволит последующим обновлениям вносить в операционную систему или установленные службы изменения, относящиеся к конкретному экземпляру. Дополнительные сведения о запуске sysprep см. в разделе о [действиях по обобщению VHD](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Для Linux

Следующий процесс обобщает виртуальную машину Linux и повторно развертывает ее как отдельную виртуальную машину. Дополнительные сведения см. на странице [Создание управляемого образа виртуальной машины или виртуального жесткого диска](../../virtual-machines/linux/capture-image.md). При достижении раздела «Создание виртуальной машины из образа для записи» можно прерывать работу.

1. Удалите агент Linux для Azure.
    1. Подключитесь к виртуальной машине Linux c помощью клиента SSH.
    2. В окне SSH введите следующую команду: `sudo waagent –deprovision+user` .
    3. Для продолжения введите Y (можно добавить параметр -force в предыдущую команду, чтобы предотвратить появление запроса на подтверждение).
    4. После выполнения команды введите **Exit** , чтобы закрыть SSH-клиент.
2. Останавливает виртуальную машину.
    1. На портале Azure выберите нужную группу ресурсов (RG) и отмените распределение виртуальной машины.
    2. Теперь ваша виртуальная машина является обобщенной, и вы можете создать новую виртуальную машину с помощью этого диска виртуальной машины.

### <a name="capture-image"></a>Запись образа

Когда виртуальная машина будет готова, ее можно записать в галерее образов Azure Shared. Для записи выполните следующие действия.

1. На [портал Azure](https://ms.portal.azure.com/)перейдите на страницу виртуальной машины.
2. Выберите **захват**.
3. В разделе **Share Image to Shared Image Gallery (общий доступ к коллекции образов**) выберите **Да, предоставьте общий доступ к коллекции в виде версии образа**.
4. В разделе **состояние операционной системы** выберите обобщенные.
5. Выберите коллекцию целевых образов или **Создайте новую**.
6. Выберите определение целевого образа или **Создайте новое**.
7. Укажите **номер версии** для образа.
8. Выберите **Просмотр и создание**, чтобы проверить выбранные параметры.
9. После прохождения проверки выберите **создать**.

Для публикации подписка Azure, содержащая SIG, должна находиться в том же клиенте, что и учетная запись издателя. Кроме того, учетная запись издателя должна иметь доступ владельца к SIG. 

Чтобы предоставить доступ:

1. Перейдите в коллекцию общих образов.
2. На панели слева выберите **Управление доступом** (IAM).
3. Выберите **Добавить** , а затем **добавить назначение ролей**.
4. Выберите **роль** или **владельца**.
5. В разделе **назначение доступа для** выберите **пользователя, группы или субъекта-службы**.
6. Выберите адрес электронной почты Azure пользователя, который будет публиковать образ.
7. Щелкните **Сохранить**.

:::image type="content" source="../media/create-vm/add-role-assignment.png" alt-text="Отображает окно Добавление назначения ролей.":::

> [!NOTE]
> Вам не нужно создавать URI SAS, так как теперь вы можете опубликовать образ SIG в центре партнеров. Однако если вам по-прежнему нужно обратиться к шагам создания URI SAS, см. статью [Создание URI SAS для образа виртуальной машины](../azure-vm-get-sas-uri.md).
