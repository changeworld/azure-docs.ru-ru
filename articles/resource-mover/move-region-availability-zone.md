---
title: Перемещение виртуальных машин Azure в зоны доступности в другом регионе с помощью перемещения ресурсов Azure
description: Узнайте, как переместить виртуальные машины Azure в зоны доступности с помощью перемещения ресурсов Azure.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 88006fb354af2673496c6476090d7f73c8a005e6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "95543006"
---
# <a name="move-azure-vms-to-an-availability-zone-in-another-region"></a>Перемещение виртуальных машин Azure в зону доступности в другом регионе

Из этой статьи вы узнаете, как перемещать виртуальные машины Azure (и связанные ресурсы сети или хранилища) в зону доступности в другом регионе Azure с помощью средства [перемещения ресурсов Azure](overview.md).

[Зоны доступности Azure](../availability-zones/az-overview.md#availability-zones) помогают защитить развертывание Azure от сбоев центра обработки данных. Каждая зона доступности состоит из одного или нескольких центров обработки данных, оснащенных независимыми системами электроснабжения, охлаждения и сетевого взаимодействия. Для обеспечения устойчивости существует как минимум три отдельные зоны во всех [включенных регионах](../availability-zones/az-region.md). С помощью перемещения ресурсов можно перемещаться:

- Виртуальная машина с одним экземпляром в зону доступности или группу доступности в целевом регионе.
- Виртуальная машина в группе доступности устанавливается в зону доступности или группу доступности в целевом регионе.
- Виртуальная машина в зоне доступности исходного региона для зоны доступности в целевом регионе.


> [!IMPORTANT]
> Azure Resource Mover сейчас поддерживается в общедоступной предварительной версии.

Если вы хотите переместить виртуальные машины в другую зону доступности в том же регионе, [Ознакомьтесь с этой статьей](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md).

## <a name="prerequisites"></a>Предварительные условия

- Доступ *владельца* к подписке, в которой находятся ресурсы, которые требуется переместить.
    - При первом добавлении ресурса для определенного сопоставления источника и назначения в подписке Azure средство перемещения ресурсов создает [управляемое системой удостоверение](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (ранее известное как управляемая служба идентификации (MSI)), которому доверяет подписка.
    - Чтобы создать удостоверение и назначить ему требуемую роль (участника или администратора доступа пользователя в исходной подписке), учетной записи, используемой для добавления ресурсов, требуются разрешения *владельца* в подписке. [Дополнительные сведения](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) о ролях Azure.
- Подписке требуется достаточно квота для создания исходных ресурсов в целевом регионе. В противном случае запросите дополнительные ограничения. [Подробнее](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Проверьте цены в целевом регионе, в который вы перемещаете виртуальные машины. Оцените затраты с помощью [калькулятора цен](https://azure.microsoft.com/pricing/calculator/).
    


## <a name="check-vm-requirements"></a>Проверка требований виртуальных машин

1. Убедитесь, что виртуальные машины, которые требуется переместить, поддерживаются.

    - [Проверьте](support-matrix-move-region-azure-vm.md#windows-vm-support) поддерживаемые виртуальные машины Windows.
    - [Проверьте](support-matrix-move-region-azure-vm.md#linux-vm-support) поддерживаемые виртуальные машины Linux и версии ядра.
    - Проверьте поддерживаемые параметры [вычислений](support-matrix-move-region-azure-vm.md#supported-vm-compute-settings), [хранилища](support-matrix-move-region-azure-vm.md#supported-vm-storage-settings) и [сети](support-matrix-move-region-azure-vm.md#supported-vm-networking-settings).
2. Убедитесь, что виртуальные машины, которые требуется переместить, включены.
3. Убедитесь, что виртуальные машины имеют последние Доверенные корневые сертификаты и обновленный список отзыва сертификатов (CRL). 
    - На виртуальных машинах Azure под Windows установите последние обновления Windows.
    - На виртуальных машинах под управлением Linux следуйте указаниям по использованию распространителя Linux, чтобы убедиться, что на компьютере установлены последние сертификаты и список отзыва сертификатов. 
4. Разрешите исходящие подключения от виртуальных машин:
    - При использовании прокси-сервера или брандмауэра на основе URL-адресов для управления исходящими подключениями разрешите использование этих [URL-адресов](support-matrix-move-region-azure-vm.md#url-access):
    - Если вы используете правила группы безопасности сети (NSG) для управления исходящими подключениями, создайте [правила тегов службы](support-matrix-move-region-azure-vm.md#nsg-rules).

## <a name="select-resources-to-move"></a>Выбор ресурсов для перемещения

Выберите ресурсы, которые требуется переместить.

- Вы можете выбрать любой поддерживаемый тип ресурсов в группах ресурсов в выбранном исходном регионе.
- Ресурсы перемещаются в целевой регион в подписке исходного региона. Если требуется изменить подписку, это можно сделать после перемещения ресурсов.

1. На портале Azure найдите и выберите средство перемещения ресурсов. Затем в разделе **Службы** выберите **Azure Resource Mover**.

    ![Поиск перемещения ресурсов](./media/move-region-availability-zone/search.png)

2. В разделе **Общие сведения** выберите начало **работы**.

    ![Кнопка для начала работы](./media/move-region-availability-zone/get-started.png)

3. В разделе **Перемещение ресурсов** > **Источник и назначение** выберите исходную подписку и регион.
4. В разделе **Назначение** выберите регион, в который необходимо переместить виртуальные машины. Затем нажмите кнопку **Далее**.

     ![Страница для заполнения исходной и целевой подписки или региона](./media/move-region-availability-zone/source-target.png)

6. В разделе **Перемещаемые ресурсы** выберите **Выбор ресурсов**.
7. В разделе **Выбор ресурсов** выберите виртуальную машину. Вы можете добавить только ресурсы, поддерживаемые для перемещения. Затем нажмите кнопку **Done**(Готово). В разделе **Перемещаемые ресурсы** нажмите кнопку **Далее**.

    ![Страница выбора виртуальных машин для перемещения](./media/move-region-availability-zone/select-vm.png)
8. В разделе **Просмотреть и добавить** проверьте параметры источника и назначения.

    ![Страница просмотра параметров и сведений о выполнении перемещения](./media/move-region-availability-zone/review.png)

9. Нажмите кнопку **Продолжить**, чтобы приступить к добавлению ресурсов.
10. После успешного завершения процесса добавления в области уведомления выберите **Adding resources for move** (Добавление ресурсов для перемещения).

    ![Сообщение в уведомлениях](./media/move-region-availability-zone/notification.png)

После щелчка на уведомлении ресурсы отображаются на странице **между регионами** .

> [!NOTE]
> После щелчка на уведомлении ресурсы отображаются на странице **между регионами** в состоянии " *Подготовка ожидается* ".
> - Если требуется удалить ресурс из перемещаемой коллекции, действия зависят от того, где происходит процесс перемещения. [Подробнее](remove-move-resources.md).

## <a name="resolve-dependencies"></a>Устранение ошибок, связанных с зависимостями

1. Если в списке ресурсов отображается сообщение *Validate dependencies* (Проверить зависимости) в столбце **Проблемы**, нажмите кнопку **Проверить зависимости**. Процесс проверки существ.
2. Если зависимости обнаружены, выберите **Добавить зависимости**. 
3. В разделе **Добавить зависимости** выберите зависимые ресурсы, а затем нажмите кнопку **Добавить зависимости**. Проверьте ход выполнения в области уведомлений.

    ![Кнопка добавления зависимостей](./media/move-region-availability-zone/add-dependencies.png)

3. При необходимости добавьте дополнительные зависимости и проверьте их снова. 

    ![Страница для добавления дополнительных зависимостей](./media/move-region-availability-zone/add-additional-dependencies.png)

4. На странице **Across regions** (Между регионами) убедитесь, что ресурсы теперь находятся в состоянии *ожидания подготовки* без неполадок.

    ![Страница с ресурсами с состоянием ожидания подготовки](./media/move-region-availability-zone/prepare-pending.png)

## <a name="move-the-source-resource-group"></a>Перемещение исходной группы ресурсов 

Прежде чем можно будет подготовить и переместить виртуальные машины, исходная группа ресурсов должна присутствовать в целевом регионе. 

### <a name="prepare-to-move-the-source-resource-group"></a>Подготовка к перемещению исходной группы ресурсов

Подготовка выполняется следующим образом.

1. В разделе **Across regions** (Между регионами) выберите исходную группу ресурсов, а затем нажмите кнопку **Подготовить**.
2. В разделе **Подготовка ресурсов** нажмите кнопку **Подготовить**.

    ![Кнопка для подготовки исходной группы ресурсов](./media/move-region-availability-zone/prepare-resource-group.png)

    Во время подготовки Resource Mover создает шаблоны Azure Resource Manager (ARM), используя параметры группы ресурсов. Ресурсы в группе ресурсов не затрагиваются.

> [!NOTE]
>  После подготовки группы ресурсов она находится в состоянии *ожидания начала перемещения*. 

![Состояние, показывающее состояние запуска "ожидание"](./media/move-region-availability-zone/initiate-resource-group-pending.png)

### <a name="move-the-source-resource-group"></a>Перемещение исходной группы ресурсов

Начните перемещение, выполнив следующие действия:

1. В разделе **Across regions** (Между регионами) выберите исходную группу ресурсов, а затем нажмите кнопку **Initiate move** (Начать перемещение).
2. В разделе **Перемещение ресурсов** выберите **Initiate move** (Начать перемещение). Группа ресурсов будет иметь состояние *Initiate move in progress* (Начало перемещения).
3. После начала перемещения создается целевая группа ресурсов на основе созданного шаблона Resource Manager. Исходная группа ресурсов будет иметь состояние *Commit move pending* (Ожидается фиксация перемещения).

![Состояние, показывающее перемещение фиксации](./media/move-region-availability-zone/commit-move-pending.png)

Чтобы зафиксировать и завершить процесс перемещения, сделайте следующее:

1. В **области в разных регионах** выберите группу ресурсов > **фиксация перемещения**
2. В разделе **Перемещение ресурсов** нажмите кнопку **Зафиксировать**.

> [!NOTE]
> После фиксации перемещения исходная группа ресурсов находится в состоянии *ожидания удаления источника*.


## <a name="add-a-target-availability-zone"></a>Добавление целевой зоны доступности

Прежде чем переместить остальные ресурсы, мы установим целевую зону доступности для виртуальной машины.

1. На странице **между регионами** щелкните ссылку в столбце **Целевая конфигурация** виртуальной машины, которую вы перемещаете.

    ![Свойства виртуальной машины](./media/move-region-availability-zone/select-vm-settings.png)

3. В **параметрах конфигурации** укажите параметр для целевой виртуальной машины. Вы можете настроить виртуальную машину в целевом регионе следующим образом:
    -  Создайте новый эквивалентный ресурс в целевом регионе. За исключением указанных параметров, целевой ресурс создается с теми же параметрами, что и у источника.
    - Выберите существующую виртуальную машину в целевом регионе. 

4. В области **зоны** выберите зону, в которой необходимо разместить виртуальную машину при ее перемещении.

    Изменения вносятся только в изменяемый ресурс. Все зависимые ресурсы необходимо обновлять отдельно.

5. В поле **SKU** укажите [уровень Azure](..//virtual-machines/sizes.md) , который вы хотите назначить целевой виртуальной машине.
6. В **группе доступности** выберите группу доступности, если требуется, чтобы ЦЕЛЕВая виртуальная машина выполнялась в группе доступности в зоне доступности.
7. Щелкните **Save changes** (Сохранить изменения).

    ![Параметры виртуальной машины](./media/move-region-availability-zone/vm-settings.png)


## <a name="prepare-resources-to-move"></a>Добавление ресурсов для перемещения

Теперь, когда исходная группа ресурсов перемещена, можно подготовиться к перемещению других ресурсов.

1. В разделе **Across regions** (Между регионами) выберите ресурсы для подготовки. 

    ![Страница выбора подготовки других ресурсов](./media/move-region-availability-zone/prepare-other.png)

2. Нажмите кнопку **Подготовить**. 

> [!NOTE]
> - В процессе подготовки на виртуальных машинах для репликации устанавливается Azure Site Recovery агент мобильности.
> - Данные виртуальной машины периодически реплицируются в целевой регион. Это не влияет на исходную виртуальную машину.
> - При перемещении ресурсов создаются шаблоны Resource Manager для других исходных ресурсов.
> - После подготовки ресурсов они находятся в состоянии *ожидания начала перемещения*.

![Страница с ресурсами в состоянии ожидания начала перемещения](./media/move-region-availability-zone/initiate-move-pending.png)

## <a name="initiate-the-move"></a>Начало перемещения

Теперь, когда ресурсы подготовлены, можно начать перемещение. 

1. В разделе **Across regions** (Между регионами) выберите ресурсы с состоянием *ожидания начала перемещения*. Затем нажмите кнопку **начать перемещение** .
2. В разделе **Перемещение ресурсов** нажмите кнопку **Initiate move** (Начать перемещение).

    ![Страница для инициации перемещения ресурсов](./media/move-region-availability-zone/initiate-move.png)

3. Отслеживать ход перемещения можно на панели уведомлений.


> [!NOTE]
> - Реплики виртуальных машин создаются в целевом регионе. Исходная виртуальная машина завершает работу, и возникает некоторое время простоя (обычно несколько минут).
> - Resource Mover воссоздает другие ресурсы с помощью подготовленных шаблонов Resource Manager. Обычно время простоя отсутствует.
> - После подготовки ресурсов они находятся в состоянии *ожидания фиксации перемещения* .


![Страница для отображения ресурсов в состоянии ожидания фиксации перемещения](./media/move-region-availability-zone/resources-commit-move-pending.png)

## <a name="discard-or-commit"></a>Отмена или фиксация

После первоначального перемещения нужно решить, следует ли фиксировать перемещение или отменить его. 

- **Отмена**. Если вы выполняете тестирование и не хотите фактически перемещать исходный ресурс, вы можете отменить перемещение. При отмене перемещения ресурс возвращается в состояние *ожидания начала перемещения*.
- **Фиксация**. Фиксация завершает перемещение в целевой регион. После фиксации исходный ресурс будет находиться в состоянии *ожидания удаления источника*. Затем вы можете его удалить.

## <a name="discard-the-move"></a>Отмена перемещения 

Вы можете отменить перемещение следующим образом:

1. В разделе **Across regions** (Между регионами) выберите ресурсы с состоянием *ожидания фиксации перемещения* и нажмите кнопку **Discard move** (Отменить перемещение).
2. В разделе **Discard move** (Отмена перемещения) нажмите кнопку **Отменить**.
3. Отслеживать ход перемещения можно на панели уведомлений.
 

> [!NOTE]
> Для виртуальных машин после отмены ресурсов они находятся в состоянии " *Ожидание запуска перемещения* ".

## <a name="commit-the-move"></a>Фиксация перемещения

Если вы хотите завершить процесс перемещения, зафиксируйте перемещение. 

1. В разделе **Across regions** (Между регионами) выберите ресурсы с состоянием *ожидания фиксации перемещения* и нажмите кнопку **Commit move** (Зафиксировать перемещение).
2. В разделе **Commit resources** (Фиксация ресурсов) нажмите кнопку **Зафиксировать**.

    ![Страница фиксации ресурсов для завершения перемещения](./media/move-region-availability-zone/commit-resources.png)

3. Отслеживать ход фиксации можно на панели уведомлений.

> [!NOTE]
> - После фиксации перемещения репликация виртуальных машин останавливается. Фиксация не влияет на исходную виртуальную машину.
> - Фиксация не влияет на исходные сетевые ресурсы.
> - После фиксации перемещения ресурсы находятся в состоянии *ожидания удаления источника*.

![Страница с ресурсами в состоянии ожидания удаления источника.](./media/move-region-availability-zone/delete-source-pending.png)

## <a name="configure-settings-after-the-move"></a>Настройка параметров после перемещения

Служба "Мобильность" не удаляется автоматически с виртуальных машин. Удалите ее вручную или оставьте, если планируется повторное перемещение сервера.


## <a name="delete-source-resources-after-commit"></a>Удаление исходных ресурсов после фиксации

После перемещения при необходимости можно удалить ресурсы в исходном регионе.

1. В разделе **Across Regions** (Между регионами) щелкните имя каждого исходного ресурса, который требуется удалить.
2. На странице свойства каждого ресурса нажмите кнопку **Удалить**.

## <a name="delete-additional-resources-created-for-move"></a>Удаление дополнительных ресурсов, созданных для перемещения

После перемещения можно вручную удалить коллекцию перемещения и созданные ресурсы Site Recovery.

- Коллекция перемещения по умолчанию скрыта. Чтобы увидеть ее, необходимо включить отображение скрытых ресурсов.
- Хранилище кэша имеет блокировку, которую необходимо убрать, прежде чем его можно будет удалить.

Удаление выполняется следующим образом: 

1. Найдите ресурсы в группе ресурсов ```RegionMoveRG-<sourceregion>-<target-region>```.
2. Убедитесь, что все виртуальные машины и другие исходные ресурсы в исходном регионе перемещены или удалены. Это гарантирует, что у вас отсутствуют ресурсы, ожидающие использования.
2. Удаление ресурсов:

    - Имя перемещаемой коллекции — ```movecollection-<sourceregion>-<target-region>```.
    - Имя учетной записи хранения — ```resmovecache<guid>```.
    - Имя хранилища — ```ResourceMove-<sourceregion>-<target-region>-GUID```.

## <a name="next-steps"></a>Дальнейшие действия

[Сведения о](about-move-process.md) процессе перемещения.