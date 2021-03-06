---
title: Защита сообщений B2B с помощью сертификатов
description: Добавление сертификатов для защиты сообщений B2B в Azure Logic Apps с Пакет интеграции Enterprise
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/17/2018
ms.openlocfilehash: 03fc17c0d071cef4c8de92c6b50d60d961d18aef
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91565265"
---
# <a name="improve-security-for-b2b-messages-by-using-certificates"></a>Повышение безопасности сообщений B2B с помощью сертификатов

Если вам нужно поддерживать конфиденциальную связь B2B, вы можете повысить безопасность связи B2B в корпоративных приложениях интеграции, в частности в приложениях логики, путем добавления сертификатов в учетную запись интеграции. Сертификаты — это цифровые документы, которые проверяют удостоверения участников обмена электронными данными и помогают защитить связь следующими способами.

* Шифрование содержимого сообщения.
* Цифровая подпись сообщений.

В приложениях интеграции Enterprise можно использовать следующие сертификаты:

* [Открытые сертификаты](https://en.wikipedia.org/wiki/Public_key_certificate), которые необходимо приобрести в общедоступном через Интернет [центре сертификации (ЦС)](https://en.wikipedia.org/wiki/Certificate_authority), для которых не требуются ключи. 

* Закрытые сертификаты или [*самозаверяющие сертификаты*](https://en.wikipedia.org/wiki/Self-signed_certificate), которые можно создать и выдать самостоятельно. При этом для них требуются закрытые ключи. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upload-a-public-certificate"></a>Передача открытого сертификата

Чтобы использовать *открытый сертификат* в приложениях логики с возможностями "бизнес-бизнес", необходимо сначала передать его в учетную запись интеграции. После определения свойств в создаваемых [соглашениях](logic-apps-enterprise-integration-agreements.md) сертификат можно будет использовать для защиты сообщений "бизнес-бизнес".

1. Войдите на [портал Azure](https://portal.azure.com). В главном меню Azure выберите **All resources** (Все ресурсы). В поле поиска введите имя учетной записи интеграции и выберите ее, когда она появится.

   ![Поиск и выбор учетной записи интеграции](media/logic-apps-enterprise-integration-certificates/select-integration-account.png)  

2. В разделе **Компоненты** выберите плитку **Сертификаты**.

   ![Выбор плитки "Сертификаты"](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

3. В разделе **Сертификаты** выберите **Добавить**. В разделе **Добавление сертификата** предоставьте указанные сведения для вашего сертификата. Когда все будет готово, нажмите кнопку **ОК**.

   | Свойство. | Значение | Описание | 
   |----------|-------|-------------|
   | **имя**; | <*имя сертификата*> | Имя сертификата, в этом примере — "publicCert" | 
   | **Тип сертификата** | Общие | Тип вашего сертификата |
   | **Сертификат** | <*имя файла сертификата*> | Чтобы найти и выбрать файл сертификата, который требуется загрузить, выберите значок папки рядом с полем **Сертификат**. |
   ||||

   ![На снимке экрана показано, где можно нажать кнопку Добавить, чтобы предоставить сведения о сертификате.](media/logic-apps-enterprise-integration-certificates/public-certificate-details.png)

   После проверки сделанного выбора Azure отправит ваш сертификат.

   ![Снимок экрана, на котором показано, где Azure отображает новый сертификат.](media/logic-apps-enterprise-integration-certificates/new-public-certificate.png) 

## <a name="upload-a-private-certificate"></a>Передача закрытого сертификата

Чтобы использовать *закрытый сертификат* в приложениях логики с возможностями "бизнес-бизнес", необходимо сначала передать этот сертификат в учетную запись интеграции. Также потребуется закрытый ключ, который необходимо сначала добавить в [хранилище Azure Key Vault](../key-vault/general/overview.md). 

После определения свойств в создаваемых [соглашениях](logic-apps-enterprise-integration-agreements.md) сертификат можно будет использовать для защиты сообщений "бизнес-бизнес".

> [!NOTE]
> Для частных сертификатов обязательно добавьте соответствующий открытый сертификат, который отображается в параметрах **отправки и получения** [соглашения AS2](logic-apps-enterprise-integration-as2.md) для подписи и шифрования сообщений.

1. [Добавьте закрытый ключ в хранилище Azure Key Vault](../key-vault/certificates/certificate-scenarios.md#import-a-certificate) и укажите **имя ключа**.
   
2. Авторизуйте Azure Logic Apps для выполнения операций в Azure Key Vault. Для предоставления доступа субъекту-службе Logic Apps используйте команду PowerShell [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy), например:

   `Set-AzKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Войдите на [портал Azure](https://portal.azure.com). В главном меню Azure выберите **All resources** (Все ресурсы). В поле поиска введите имя учетной записи интеграции и выберите ее, когда она появится.

   ![Поиск учетной записи интеграции](media/logic-apps-enterprise-integration-certificates/select-integration-account.png) 

4. В разделе **Компоненты** выберите плитку **Сертификаты**.  

   ![Выбор плитки "Сертификаты"](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

5. В разделе **Сертификаты** выберите **Добавить**. В разделе **Добавление сертификата** предоставьте указанные сведения для вашего сертификата. Когда все будет готово, нажмите кнопку **ОК**.

   | Свойство. | Значение | Описание | 
   |----------|-------|-------------|
   | **имя**; | <*имя сертификата*> | Имя сертификата, в этом примере — "privateCert" | 
   | **Тип сертификата** | Private | Тип вашего сертификата |
   | **Сертификат** | <*имя файла сертификата*> | Чтобы найти и выбрать файл сертификата, который требуется загрузить, выберите значок папки рядом с полем **Сертификат**. При использовании хранилища ключей для закрытого ключа отправленный файл будет общедоступным сертификатом. | 
   | **Группа ресурсов** | <*Интеграция-учетная запись-ресурс-группа*> | Группа ресурсов учетной записи интеграции, в этом примере — "MyResourceGroup" | 
   | **хранилище ключей;** | <*ключ-хранилище — имя*> | Имя хранилища ключей Azure |
   | **Имя ключа** | <*имя ключа*> | Имя ключа |
   ||||

   ![Щелкните "Добавить" и укажите данные сертификата](media/logic-apps-enterprise-integration-certificates/private-certificate-details.png)

   После проверки сделанного выбора Azure отправит ваш сертификат.

   ![Отображение нового сертификата в Azure](media/logic-apps-enterprise-integration-certificates/new-private-certificate.png) 

## <a name="next-steps"></a>Дальнейшие действия

* [Создание соглашения "бизнес-бизнес"](logic-apps-enterprise-integration-agreements.md)
