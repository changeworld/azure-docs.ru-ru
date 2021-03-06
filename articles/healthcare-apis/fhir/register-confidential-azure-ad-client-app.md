---
title: Регистрация конфиденциального клиентского приложения в Azure AD — API Azure для FHIR
description: Зарегистрируйте конфиденциальное клиентское приложение в Azure Active Directory, которое проходит проверку подлинности от имени пользователя и запрашивает доступ к приложениям ресурсов.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: matjazl
ms.openlocfilehash: c10b27d375e2bfb8c64130eceb416a633241cf68
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2021
ms.locfileid: "107284449"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Регистрация конфиденциального клиентского приложения в Azure Active Directory

В этом руководстве вы узнаете, как зарегистрировать конфиденциальное клиентское приложение в Azure Active Directory (Azure AD).  

Регистрация клиентского приложения — это представление Azure AD приложения, которое можно использовать для проверки подлинности от имени пользователя и запроса доступа к [приложениям ресурсов](register-resource-azure-ad-client-app.md). Конфиденциальное клиентское приложение — это приложение, которое может быть доверенным для хранения секрета и представлять этот секрет при запросе маркеров доступа. Примерами конфиденциальных приложений являются приложения на стороне сервера. 

Чтобы зарегистрировать новое конфиденциальное клиентское приложение, ознакомьтесь с приведенными ниже шагами. 

## <a name="register-a-new-application"></a>Регистрация нового приложения

1. На [портале Azure](https://portal.azure.com)выберите **Azure Active Directory**.

1. Щелкните **Регистрация приложений**. 

    :::image type="content" source="media/how-to-aad/portal-aad-new-app-registration.png" alt-text="портал Azure. Регистрация нового приложения.":::

1. Выберите **Новая регистрация**.

1. Присвойте приложению отображаемое имя пользователя.

1. Для **поддерживаемых типов учетных записей** выберите пользователей, которые могут использовать приложение, или получите доступ к API.

1. Используемых Укажите **URI перенаправления**. Эти сведения можно изменить позже, но если вы знакомы с URL-адресом ответа приложения, введите его сейчас.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png" alt-text="Регистрация нового конфиденциального клиентского приложения.":::

1. Выберите **Зарегистрировать**.

## <a name="api-permissions"></a>Разрешения API

Теперь, когда приложение зарегистрировано, необходимо выбрать, какие разрешения API должны запрашиваться этим приложением от имени пользователей.

1. Выберите **Разрешения API**.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png" alt-text="Конфиденциальный клиент. Разрешения API.":::

1. Выберите **Добавить разрешение**.

    Если вы используете API Azure для FHIR, вы добавите разрешение для API здравоохранения Azure, выполнив поиск по API **Azure для здравоохранения** в разделе **API, используемые моей организацией**. Результаты поиска для API здравоохранения Azure будут возвращаться, только если вы уже [развернули API Azure для FHIR](fhir-paas-powershell-quickstart.md).

    Если вы ссылаетесь на другое приложение ресурсов, выберите [регистрацию приложения API FHIR](register-resource-azure-ad-client-app.md) , созданную ранее в разделе **Мои API**.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Конфиденциальный клиент. Мои API-интерфейсы Организации" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::
    

1. Выберите области (разрешения), которые будет запрашивать конфиденциальное клиентское приложение от имени пользователя. Выберите **user_impersonation**, а затем — **Добавить разрешения**.

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Конфиденциальный клиент. Делегированные разрешения":::


## <a name="application-secret"></a>Секрет приложения

1. Выберите **сертификаты & секреты**, а затем выберите **новый секрет клиента**. 

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png" alt-text="Конфиденциальный клиент. Секрет приложения.":::

1. Введите **описание** для секрета клиента. Выберите в раскрывающемся меню **истечение срока** действия интервал срока действия, а затем нажмите кнопку **Добавить**.

   :::image type="content" source="media/how-to-aad/add-a-client-secret.png" alt-text="Добавьте секрет клиента.":::

1. После создания строки секрета клиента скопируйте ее **значение** и **идентификатор** и сохраните их в безопасном месте по своему усмотрению.

   :::image type="content" source="media/how-to-aad/client-secret-string-password.png" alt-text="Строка секрета клиента."::: 

> [!NOTE]
>Строка секрета клиента видна только один раз в портал Azure. При переходе с веб-страницы Certificates & Secrets, а затем обратно к ней строка значения превращается в маскированную. Важно сделать копию строки секрета клиента сразу после ее создания. Если у вас нет резервной копии секрета клиента, необходимо повторить описанные выше действия для его повторного создания.
 
## <a name="next-steps"></a>Дальнейшие шаги

В этой статье мы пошаговыми инструкциями по регистрации конфиденциального клиентского приложения в Azure AD. Вы также повлияли на шаги по добавлению разрешений API в API здравоохранения Azure. Наконец, было показано, как создать секрет приложения. Кроме того, вы можете узнать, как получить доступ к серверу FHIR, используя POST.
 
>[!div class="nextstepaction"]
>[Получение доступа к Azure API для FHIR с помощью Postman](access-fhir-postman-tutorial.md)
