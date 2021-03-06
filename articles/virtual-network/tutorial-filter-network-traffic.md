---
title: Руководство. Фильтрация сетевого трафика с помощью портала Azure
titlesuffix: Azure Virtual Network
description: В этом руководстве описано, как фильтровать сетевой трафик в подсети с помощью группы безопасности сети, используя портал Azure.
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: tutorial
ms.date: 03/06/2021
ms.author: kumud
ms.openlocfilehash: cfbb499c79761e1f2014c834e65dac35fe09ef90
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106057292"
---
# <a name="tutorial-filter-network-traffic-with-a-network-security-group-using-the-azure-portal"></a>Руководство. Фильтрация сетевого трафика с помощью групп безопасности сети, используя портал Azure

Вы можете отфильтровать входящий и исходящий трафик в подсети виртуальной сети с помощью группы безопасности сети.

Группы безопасности сети содержат правила безопасности, которые фильтруют трафик по IP-адресу, порту и протоколу. Правила безопасности применяются к ресурсам, развернутым в подсети. 

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Создание группы безопасности сети и правил безопасности.
> * Создание виртуальной сети и привязка группы безопасности сети к подсети.
> * Развертывание виртуальных машин в подсеть.
> * Тестирование фильтров трафика

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Создание виртуальной сети

1. Щелкните значок **Создать ресурс** в верхнем левом углу окна портала.

2. В поле поиска введите **Виртуальная сеть**. В результатах поиска выберите **Виртуальная сеть**.

3. На странице **Виртуальная сеть** выберите **Создать**.

4. В подменю **Создать виртуальную сеть** введите или выберите следующую информацию на вкладке **Основные сведения**:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** |   |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **Создать**.  </br> Введите **myResourceGroup**. </br> Щелкните **ОК**. |
    | **Сведения об экземпляре** |   |
    | Имя | Введите **myVNet**. |
    | Регион | Выберите регион **(США) Восточная часть США**. |

5. Перейдите на вкладку **Просмотр и создание** или нажмите синюю кнопку **Просмотр и создание** внизу страницы.

6. Нажмите кнопку **создания**.

## <a name="create-application-security-groups"></a>Создание групп безопасности приложений

Группа безопасности приложений позволяет группировать серверы с аналогичными функциями, например веб-серверы.

1. Щелкните значок **Создать ресурс** в верхнем левом углу окна портала.

2. В поле поиска введите **группа безопасности приложений**. В результатах поиска выберите **Группа безопасности приложений**.

3. На странице **Группа безопасности приложений** выберите **Создать**.

4. В подменю **Создание группы безопасности приложений** введите или выберите следующую информацию на вкладке **Основные сведения**:

    | Параметр | Значение |
    | ------- | ----- |
    |**Сведения о проекте** |  |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. |
    | **Сведения об экземпляре** |  |
    | Имя | Введите **myAsgWebServers**. |
    | Регион | Выберите регион **(США) Восточная часть США**. | 

5. Перейдите на вкладку **Просмотр и создание** или нажмите синюю кнопку **Просмотр и создание** внизу страницы.

6. Нажмите кнопку **создания**.

7. Повторите шаг 4, указав следующие значения:

    | Параметр | Значение |
    | ------- | ----- |
    |**Сведения о проекте** |  |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. |
    | **Сведения об экземпляре** |  |
    | Имя | Введите **myAsgMgmtServers**. |
    | Регион | Выберите регион **(США) Восточная часть США**. |

8. Перейдите на вкладку **Просмотр и создание** или нажмите синюю кнопку **Просмотр и создание** внизу страницы.

9. Нажмите кнопку **создания**.

## <a name="create-a-network-security-group"></a>Создание группы безопасности сети

Группа безопасности сети защищает трафик в вашей виртуальной сети.

1. Щелкните значок **Создать ресурс** в верхнем левом углу окна портала.

2. В поле поиска введите фразу **группа безопасности сети**. В результатах поиска выберите **Группа безопасности сети**.

3. На странице **Группа безопасности сети** выберите **Создать**.

4. В подменю **Создать группу безопасности сети** введите или выберите следующую информацию на вкладке **Основные сведения**:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** |   |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. |
    | **Сведения об экземпляре** |   |
    | Имя | Введите **myNSG**. |
    | Расположение | Выберите регион **(США) Восточная часть США**. | 

5. Перейдите на вкладку **Просмотр и создание** или нажмите синюю кнопку **Просмотр и создание** внизу страницы.

6. Нажмите кнопку **создания**.

## <a name="associate-network-security-group-to-subnet"></a>Связывание группы безопасности сети с подсетью

Проходя этот раздел, мы свяжем группу безопасности сети с созданной ранее подсетью виртуальной сети.

1. В поле **Поиск ресурсов, служб и документов** в верхней части портала введите **myNsg**. Когда виртуальная машина **myNsg** появится в результатах поиска, выберите ее.

2. На странице обзора **myNSG** выберите **Подсети** в разделе **Параметры**.

3. На странице **Параметры** нажмите кнопку **Связать**:

    :::image type="content" source="./media/tutorial-filter-network-traffic/associate-nsg-subnet.png" alt-text="Экран &quot;Связывание группы безопасности сети (NSG) с подсетью&quot;." border="true":::

3. В разделе **Связать подсеть** выберите **Виртуальная сеть**, а затем — **myVNet**. 

4. Выберите **Подсеть**, затем — **по умолчанию** и нажмите **ОК**.

## <a name="create-security-rules"></a>Создание правил безопасности

1. В разделе **Параметры** **myNSG** выберите **Правила безопасности для входящего трафика**.

2. В разделе **Правила безопасности для входящего трафика** выберите **+ Добавить**.

    :::image type="content" source="./media/tutorial-filter-network-traffic/add-inbound-rule.png" alt-text="Экран &quot;Добавление правила безопасности для входящего трафика&quot;." border="true":::

3. Создайте правило безопасности, которое позволяет использование портов 80 и 443 в группе безопасности приложения **myAsgWebServers**. В окне **Добавление правила безопасности для входящего трафика** введите или выберите следующее:

    | Параметр | Значение |
    | ------- | ----- |
    | Источник | Оставьте значение по умолчанию **Любое**. |
    | Диапазоны исходных портов | Оставьте значение по умолчанию **(*)** . |
    | Назначение | Выберите **Группа безопасности приложений**. |
    | Группа безопасности приложений для назначения | Выберите **myAsgWebServers**. |
    | Служба | Оставьте значение по умолчанию **Настраиваемое**. |
    | Диапазоны портов назначения | Введите **80,443**. |
    | Протокол | Выберите **TCP**. |
    | Действие | Оставьте значение по умолчанию **Разрешить**. |
    | Приоритет | Оставьте значение по умолчанию **100**. |
    | Имя | Введите **Allow-Web-All**. |

    :::image type="content" source="./media/tutorial-filter-network-traffic/inbound-security-rule.png" alt-text="Экран &quot;Правило безопасности для входящего трафика&quot;." border="true":::

3. Выполните шаги 2 еще раз, указав следующие значения.

    | Параметр | Значение |
    | ------- | ----- |
    | Источник | Оставьте значение по умолчанию **Любое**. |
    | Диапазоны исходных портов | Оставьте значение по умолчанию **(*)** . |
    | Назначение | Выберите **Группа безопасности приложений**. |
    | Группа безопасности приложений для назначения | Выберите **myAsgMgmtServers**. |
    | Служба | Оставьте значение по умолчанию **Настраиваемое**. |
    | Диапазоны портов назначения | Введите **3389**. |
    | Протокол | Выберите **TCP**. |
    | Действие | Оставьте значение по умолчанию **Разрешить**. |
    | Приоритет | Оставьте значение по умолчанию **110**. |
    | Имя | Введите **Allow-RDP-All**. |

    > [!CAUTION]
    > В этой статье RDP (порт 3389) доступен в Интернете для виртуальной машины, назначенной группе безопасности приложений **myAsgMgmtServers**. 
    >
    > В рабочих средах рекомендуется не открывать порт 3389 для доступа из Интернета, а подключиться к ресурсам Azure, которыми вы хотите управлять, с помощью VPN-подключения, частного сетевого подключения или Бастиона Azure.
    >
    > Дополнительные сведения о Бастионе Azure см. в статье [Что такое Бастион Azure](../bastion/bastion-overview.md).

Выполнив шаги 1–3, просмотрите созданные вами правила. Список должен выглядеть так, как показано на следующем примере:

:::image type="content" source="./media/tutorial-filter-network-traffic/security-rules.png" alt-text="Экран &quot;Правила безопасности&quot;." border="true":::

## <a name="create-virtual-machines"></a>Создание виртуальных машин

Создайте две виртуальные машины в виртуальной сети.

### <a name="create-the-first-vm"></a>Создание первой виртуальной машины

1. Щелкните значок **Создать ресурс** в верхнем левом углу окна портала.

2. Нажмите **Вычисления**, а затем выберите вариант **Виртуальная машина**.

3. В подменю **Создать виртуальную машину** введите или выберите следующую информацию на вкладке **Основные сведения**:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** |  |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. |
    | **Сведения об экземпляре** |   |
    | Имя виртуальной машины | Введите **myVMWeb**. |
    | Регион | Выберите регион **(США) Восточная часть США**. |
    | Параметры доступности | Оставьте значение по умолчанию No redundancy required (Избыточность не требуется). |
    | Образ — | Выберите **Windows Server 2019 Datacenter (поколение 1)** . |
    | Точечный экземпляр Azure | Оставьте значение по умолчанию (флажок снят). |
    | Размер | Выберите **Standard_D2s_V3**. |
    | **Учетная запись администратора** |   |
    | Имя пользователя | Введите имя пользователя. |
    | Пароль | Введите пароль. |
    | Подтверждение пароля | Введите пароль еще раз. |
    | **Правила входящего порта** |   |
    | Общедоступные входящие порты | Выберите **Отсутствует**. |

4. Перейдите на вкладку **Сеть**.

5. На вкладке **Сеть** введите или выберите следующие значения параметров.

    | Параметр | Значение |
    | ------- | ----- |
    | **Сетевой интерфейс** |   |
    | Виртуальная сеть | Выберите **myVNet**. |
    | Подсеть | Выберите **по умолчанию (10.0.0.0/24)** . |
    | Общедоступный IP-адрес | Оставьте значение по умолчанию "Новый общедоступный IP-адрес". |
    | Сетевая группа безопасности сетевого адаптера | Выберите **Отсутствует**. | 

6. Перейдите на вкладку **Просмотр и создание** или нажмите синюю кнопку **Просмотр и создание** внизу страницы.

7. Нажмите кнопку **создания**.

### <a name="create-the-second-vm"></a>Создание второй виртуальной машины

Повторите шаги 1–7 снова, но на шаге 3 присвойте виртуальной машине имя **myVmMgmt**. Развертывание виртуальной машины занимает несколько минут. 

Не переходите к следующему шагу, пока виртуальная машина не будет развернута.

## <a name="associate-network-interfaces-to-an-asg"></a>Связывание сетевых интерфейсов с группой безопасности приложений

При создании виртуальных машин портал создал сетевой интерфейс для каждой из них и подключил сетевой интерфейс. 

Добавьте сетевой интерфейс для каждой виртуальной машины в одну из ранее созданных групп безопасности приложений:

1. В поле **Поиск ресурсов, служб и документов** в верхней части портала и введите **myVMWeb**. Когда в результатах поиска появится виртуальная машина **myVMWeb**, выберите ее.

2. В разделе **Параметры** выберите **Сеть**.  

3. Перейдите на вкладку **Группы безопасности приложений**, а затем выберите **Настройка групп безопасности приложений**.

    :::image type="content" source="./media/tutorial-filter-network-traffic/configure-app-sec-groups.png" alt-text="Экран &quot;Настройка групп безопасности приложений&quot;." border="true":::

4. В окне **Настройка групп безопасности приложений** выберите **myAsgWebServers**. Щелкните **Сохранить**.

    :::image type="content" source="./media/tutorial-filter-network-traffic/select-asgs.png" alt-text="Экран &quot;Выбор групп безопасности приложений&quot;." border="true":::

5. Выполните шаги 1 и 2 снова, найдите виртуальную машину **myVmMgmt** и выберите группу безопасности приложений **myAsgMgmtServers**.

## <a name="test-traffic-filters"></a>Тестирование фильтров трафика

1. Подключитесь к виртуальной машине **myVmMgmt**. В верхней части портала в поле поиска введите **myVmMgmt**. Когда виртуальная машина **myVmMgmt** появится в результатах поиска, выберите ее. Нажмите кнопку **Подключиться**.

2. Щелкните **Скачать RDP-файл**.

3. Откройте скачанный RDP-файл и нажмите **Подключиться**. Введите имя пользователя и пароль, указанные при создании виртуальной машины.

4. Щелкните **ОК**.

5. При подключении может появиться предупреждение о сертификате. Если вы получили предупреждение, выберите **Да** или **Продолжить**, чтобы продолжить процесс подключения.

    Подключение установлено успешно, поскольку для порта 3389 разрешен входящий трафик из Интернета в группу безопасности приложений **myAsgMgmtServers**. 
    
    Сетевой интерфейс для **myVMMgmt** связан с группой безопасности приложений **myAsgMgmtServers** и разрешает подключение.

6. Откройте сеанс PowerShell на **myVMMgmt**. Подключитесь к **myVMWeb** с помощью следующего примера: 

    ```powershell
    mstsc /v:myVmWeb
    ```

    RDP-подключение из **myVMMgmt** к **myVMWeb** выполняется успешно, поскольку виртуальные машины в одной сети могут взаимодействовать друг с другом через любой порт по умолчанию.
    
    К виртуальной машине **myVMWeb** нельзя установить RDP-подключение из Интернета. Правило безопасности для **myAsgWebServers** предотвращает подключения к порту 3389 для входящего трафика из Интернета. По умолчанию входящий трафик из Интернета запрещен для всех ресурсов.

7. Для установки Microsoft IIS на виртуальной машине **myVmWeb** введите следующую команду из сеанса PowerShell на виртуальной машине **myVmWeb**:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

8. Когда установка служб IIS завершится, отключитесь от виртуальной машины **myVmWeb**. Сеанс удаленного рабочего стола виртуальной машины **myVmMgmt** для вас будет продолжен.

9. Отключитесь от виртуальной машины **myVmMgmt**.

10. В поле **Поиск ресурсов, служб и документов** в верхней части портала Azure введите с компьютера **myVmWeb**. Выберите виртуальную машину **myVmWeb**, когда она появится в результатах поиска. Запишите **общедоступный IP-адрес** своей виртуальной машины. Адрес, показанный на следующем примере, — 23.96.39.113, но ваш адрес другой:

    :::image type="content" source="./media/tutorial-filter-network-traffic/public-ip-address.png" alt-text="Общедоступный IP-адрес." border="true":::
    
11. Чтобы убедиться в наличии доступа к веб-серверу **myVmWeb** из Интернета, откройте веб-браузер на своем компьютере и перейдите по адресу `http://<public-ip-address-from-previous-step>`. 

Вы увидите экран приветствия службы IIS, поскольку через порт 80 разрешено получение входящего трафика из Интернета в группу безопасности приложений **myAsgWebServers**. 

Сетевой интерфейс, подключенный к **myVMMgmt**, связан с группой безопасности приложения **myAsgMgmtServers** и разрешает подключение. 

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите группу ресурсов и все содержащиеся в ней ресурсы, когда она станет не нужна.

1. В поле **Поиск** в верхней части портала введите **myResourceGroup**. Когда группа ресурсов **myResourceGroup** появится в результатах поиска, выберите ее.
2. Выберите **Удалить группу ресурсов**.
3. Введите **myResourceGroup** в поле **TYPE THE RESOURCE GROUP NAME:** (Введите имя группы ресурсов:) и нажмите кнопку **Удалить**.

## <a name="next-steps"></a>Дальнейшие действия

Изучив это руководство, вы:

* Создали группу безопасности сети и связали ее с подсетью виртуальной сети. 
* Создали группы безопасности приложений для сети и управления.
* Создали две виртуальные машины.
* Протестировали сетевую фильтрацию группы безопасности приложений.

Дополнительные сведения о группах безопасности сети см. в статьях [Безопасность сети](./network-security-groups-overview.md) и [Create, change, or delete a network security group](manage-network-security-group.md) (Создание, изменение или удаление группы безопасности сети).

Azure маршрутизирует трафик между подсетями по умолчанию. Вместо этого вы можете перенаправлять трафик между подсетями через виртуальную машину, которая используется в качестве брандмауэра. 

Чтобы узнать, как создать таблицу маршрутов, перейдите к следующему руководству.
> [!div class="nextstepaction"]
> [Создание таблицы маршрутов](./tutorial-create-route-table-portal.md)