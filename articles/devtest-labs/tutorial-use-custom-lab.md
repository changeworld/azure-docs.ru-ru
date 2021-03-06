---
title: Доступ к лаборатории в Azure DevTest Labs | Документация Майкрософт
description: В этом руководстве объясняется, как войти в лабораторию, созданную с помощью Azure DevTest Labs, запросить виртуальные машины, использовать и затем освободить их.
ms.topic: tutorial
ms.date: 06/26/2020
ms.author: spelluru
ms.openlocfilehash: b4477e0b98ef534b8170ee297edf88ac6fa62dd7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "85476450"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Руководство. Доступ к лаборатории в Azure DevTest Labs
В рамках этого руководства применяется лаборатория, которую вы создали при работе с [руководством по созданию лаборатории в Azure DevTest Labs](tutorial-create-custom-lab.md).

Вот какие действия выполняются в этом руководстве:

> [!div class="checklist"]
> * запрос виртуальной машины из лаборатории;
> * Подключение к виртуальной машине
> * Освобождение виртуальной машины

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="access-the-lab"></a>Вход в лабораторию

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все ресурсы** в меню слева. 
3. Выберите **DevTest Labs** в качестве типа ресурса. 
4. Выберите лабораторию. 

    ![Выбор лаборатории](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Запрос виртуальной машины

1. В списке **Запрашиваемые виртуальные машины** выберите символ многоточия (**...**), а затем **Запросить машину**.

    ![Запрос виртуальной машины](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Убедитесь, что виртуальная машина появилась в списке **Мои виртуальные машины**.

    ![Мои виртуальные машины](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Подключение к виртуальной машине

1. Выберите виртуальную машину из списка. Для нее откроется **страница виртуальной машины**. Выберите **Подключиться** на панели инструментов.

    ![Подключение к виртуальной машине](./media/tutorial-use-custom-lab/connect-button.png)
2. Сохраните на жесткий диск скачанный **RDP**-файл и примените его для подключения к виртуальной машине. Введите имя пользователя и пароль, которые вы указали при создании виртуальной машины в предыдущем разделе. 

    Для подключения к виртуальной машине Linux в ней нужно активировать доступ по протоколу SSH и (или) RDP. Сведения о подключении к виртуальной машине Linux по протоколу RDP см. в статье [Установка и настройка удаленного рабочего стола для подключения к виртуальной машине Linux в Azure](../virtual-machines/linux/use-remote-desktop.md). 

    > [!NOTE]
    > Есть и другие способы перейти на страницу "Виртуальная машина" для вашей виртуальной машины. Вот некоторые из них: 
    > 
    > 1. Отобразите список всех виртуальных машин в подписке. Выберите свою виртуальную машину в соответствующем списке, чтобы перейти на страницу **Виртуальная машина**.
    > 2. Перейдите на страницу **Группа ресурсов** для группы ресурсов. Затем выберите виртуальную машину в списке ресурсов в группе ресурсов, чтобы перейти на страницу **Виртуальная машина**. 
    >
    > Не используйте кнопку **Подключить** на панели инструментов на странице **Виртуальная машина**, на которую вы перешли с помощью указанных методов. Вместо этого перейдите на страницу **Виртуальная машина** со страницы **DevTest Labs**, как показано в этой статье, и нажмите кнопку **Подключить** на панели инструментов.


## <a name="unclaim-the-vm"></a>Освобождение виртуальной машины
Когда вы закончите работу с виртуальной машиной, освободите ее, выполнив следующие действия: 

1. На странице виртуальной машины выберите действие **Отменить резервирование** на панели инструментов. 

    ![Освобождение виртуальной машины](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Перед освобождением виртуальной машины выполняется завершение ее работы. Состояние этой операции отображается в уведомлениях.  
3. Вернитесь на страницу DevTest Lab, щелкнув имя своей лаборатории в меню навигации сверху. 
    
    ![Возврат на страницу лаборатории](./media/tutorial-use-custom-lab/breadcrumb-to-lab.png)
1. Убедитесь, что имя виртуальной машины отображается в списке **Виртуальные машины, для которых разрешены заявки на доступ** в нижней части страницы.

    
## <a name="next-steps"></a>Дальнейшие действия
Из этого руководства вы узнали, как открыть и использовать лабораторию, которая была создана с помощью Azure DevTest Labs. Дополнительные сведения об открытии и использовании виртуальных машин в лаборатории вы найдете по приведенной ниже ссылке. 

> [!div class="nextstepaction"]
> [Как использовать виртуальные машины в лаборатории](devtest-lab-add-vm.md)

