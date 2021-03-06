---
title: Учебник по настройке устройства, обновления и времени для устройства Azure Stack Mini R на портале Azure
description: В учебнике по развертыванию Azure Stack Edge Mini R описывается, как настраивать параметры устройства, обновления и времени для физического устройства.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/14/2020
ms.author: alkohli
ms.openlocfilehash: 5dd37351295d45944ee2ed273a7acef44ed32865
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106065554"
---
# <a name="tutorial-configure-the-device-settings-for-azure-stack-edge-mini-r"></a>Руководство по настройке параметров устройства для Azure Stack Edge Mini R

В этом учебнике описано, как настроить параметры устройства Azure Stack Edge Mini R со встроенным GPU. Вы можете настроить имя устройства, сервер обновлений и времени через локальный пользовательский веб-интерфейс.

Настройка параметров устройства может занять 5–7 минут.

В этом руководстве вы рассмотрите следующее:

> [!div class="checklist"]
>
> * Предварительные требования
> * Настройка параметров устройств
> * Настройка обновлений. 
> * Настройка времени

## <a name="prerequisites"></a>Предварительные требования

Перед настройкой параметров устройства Azure Stack Edge Mini R с GPU проверьте следующие условия:

* Для физического устройства:

    - Вы установили физическое устройство, как описано в статье [Установка Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-install.md).
    - Вы настроили сеть, а также активировали и настроили сеть вычислений на устройстве, как описано в [учебнике по настройке параметров сети для Azure Stack Edge Mini R с GPU](azure-stack-edge-mini-r-deploy-configure-network-compute-web-proxy.md).


## <a name="configure-device-settings"></a>Настройка параметров устройств

Чтобы настроить параметры устройства, выполните указанные ниже действия:

1. На странице **Устройство** выполните следующие действия:

    1. Введите понятное имя устройства. Понятное имя должно содержать от 1 до 13 знаков, включая буквы, цифры и дефисы.

    2. Укажите **DNS-домен** устройства. Этот домен используется для настройки устройства в качестве файлового сервера.

    3. Чтобы проверить и применить настроенные параметры устройства, выберите **Применить**.

        ![Страница 1 "Устройство" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/set-up-device-1.png)

        Если вы изменили имя устройства и DNS-домен, самозаверяющие сертификаты, существующие на устройстве, не будут работать. 

        ![Страница 2 "Устройство" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/set-up-device-2.png)

        При настройке сертификатов необходимо выбрать один из следующих параметров. 
        
        - Создайте и скачайте сертификаты устройства. 
        - Используйте собственные сертификаты устройства, включая цепочку подписывания.
    

    4. При изменении имени устройства и DNS-домена создается конечная точка SMB.  

        ![Страница "Устройство" в локальном пользовательском веб-интерфейсе (3)](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/set-up-device-3.png)

    5. Когда завершится применение параметров, щелкните **Далее: Сервер обновлений**.


## <a name="configure-update"></a>Настройка обновлений.

1. На странице **Обновление** теперь можно настроить расположение для скачивания обновлений для устройства.  

    - Скачать обновления можно непосредственно с **сервера Центра обновления Майкрософт**.

        ![Страница "Сервер обновлений" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/update-server-1.png)

        Вы также можете развернуть обновления из **Windows Server Update Services** (WSUS). Укажите путь к серверу WSUS.
        
        ![Страница "Сервер обновлений" в локальном пользовательском веб-интерфейсе (2)](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/update-server-2.png)

        > [!NOTE] 
        > Если настроен отдельный сервер Центра обновления Windows и вы решили подключиться по протоколу *HTTPS* (вместо *HTTP*), для подключения к серверу обновления необходимо иметь сертификат цепочки подписывания. Сведения о создании и передаче сертификатов см. в статье об [управлении сертификатами](azure-stack-edge-gpu-manage-certificates.md). Для работы в режиме развертывания без подключения, например при распределении по уровням устройства Azure Stack Edge в Модульном центре обработки данных, включите параметр WSUS. Во время активации устройство проверяет наличие обновлений, и если сервер не настроен, то активация завершится ошибкой. 

2. Нажмите кнопку **Применить**.
3. После настройки сервера обновлений щелкните **Далее: Время**.
    

## <a name="configure-time"></a>Настройка времени

Чтобы настроить параметры времени на устройстве, выполните указанные ниже действия. 

> [!IMPORTANT]
> Хотя параметры времени являются необязательными, мы настоятельно рекомендуем настроить основной и вторичный NTP-сервер в локальной сети для устройства. Если локальный сервер недоступен, можно настроить общедоступные NTP-серверы.

NTP-серверы необходимы, так как устройство должно синхронизировать время, чтобы проходить проверку подлинности в облачных службах.

1. На странице **Время** вы можете выбрать часовой пояс, а также основной и вторичный NTP-серверы для устройства.  
    
    1. В раскрывающемся списке выберите **часовой пояс**, соответствующий географическому расположению, в котором будет развернуто устройство.
        Часовой пояс по умолчанию для устройства — тихоокеанское стандартное время (PST). Устройство будет использовать заданный часовой пояс для всех запланированных операций.

    2. В поле **Основной NTP-сервер** введите основной сервер устройства или примите значение по умолчанию time.windows.com.  
        Убедитесь, что ваша сеть позволяет передавать NTP-трафик из вашего центра обработки данных в Интернет.

    3. При необходимости в поле **Вторичный NTP-сервер** введите сервер-получатель устройства.

    4. Чтобы проверить и применить настроенные параметры времени, выберите **Применить**.

        ![Страница "Время" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-mini-r-deploy-set-up-device-update-time/time-settings-1.png)

2. Когда завершится применение параметров, щелкните **Далее: Сертификаты**.


## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы рассмотрите следующее:

> [!div class="checklist"]
>
> * Предварительные требования
> * Настройка параметров устройств
> * Настройка обновлений. 
> * Настройка времени

Сведения о настройке сертификатов для устройства Azure Stack Edge Mini R см. в следующей статье:

> [!div class="nextstepaction"]
> [Настройка сертификатов](./azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption.md)
