---
title: Руководство. Интеграция единого входа Azure Active Directory с Saba Cloud | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Saba Cloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/22/2021
ms.author: jeedes
ms.openlocfilehash: 5ce9eb41755d7faa2ce00b38dfd971313443bfb7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572178"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-saba-cloud"></a>Руководство. Интеграция единого входа Azure Active Directory с Saba Cloud

В этом руководстве показано, как интегрировать Saba Cloud с Azure Active Directory (Azure AD). Интеграция Saba Cloud с Azure AD обеспечивает приведенные ниже возможности.

* С помощью Azure AD вы можете контролировать доступ к Saba Cloud.
* Вы можете включить автоматический вход пользователей в Saba Cloud с помощью их учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Saba Cloud с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Saba Cloud поддерживает единый вход, инициированный **пакетом обновления и IDP**.
* В Saba Cloud поддерживается **JIT-подготовка** пользователей.
* Теперь для мобильного приложения Saba Cloud можно настроить Azure AD для обеспечения единого входа. В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

## <a name="adding-saba-cloud-from-the-gallery"></a>Добавление Saba Cloud из коллекции

Чтобы настроить интеграцию Saba Cloud с Azure AD, необходимо добавить Saba Cloud из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Saba Cloud**.
1. Выберите **Saba Cloud** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-saba-cloud"></a>Настройка и проверка единого входа Azure AD для Saba Cloud

Настройте и проверьте единый вход Azure AD в Saba Cloud с помощью тестового пользователя **B.Simon**. Чтобы обеспечить работу единого входа, нужно установить связь между пользователем AAD и соответствующим пользователем в Saba Cloud.

Чтобы настроить и проверить единый вход Azure AD в Saba Cloud, выполните следующие действия:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Saba Cloud](#configure-saba-cloud-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Saba Cloud](#create-saba-cloud-test-user)** требуется для того, чтобы в Saba Cloud существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.
1. **[Проверка единого входа для Saba Cloud (Mobile)](#test-sso-for-saba-cloud-mobile)** требуется, чтобы убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Saba Cloud** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** для изменения параметров.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `<CUSTOMER_NAME>_SPLN_PRINCIPLE`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<SIGN-ON URL>/Saba/saml/SSO/alias/<ENTITY_ID>`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    а. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<CUSTOMER_NAME>.sabacloud.com`.

    b. В текстовом поле **Состояние ретранслятора** введите URL-адрес в следующем формате: `IDP_INIT---SAML_SSO_SITE=<SITE_ID> ` или, если для микросайта настроен SAML, введите URL-адрес в следующем формате: `IDP_INIT---SAML_SSO_SITE=<SITE_ID>---SAML_SSO_MICRO_SITE=<MicroSiteId>`

    > [!NOTE]
    > Дополнительные сведения о настройке RelayState см. по [этой](https://help.sabacloud.com/sabacloud/help-system/topics/help-system-idp-and-sp-initiated-sso-for-a-microsite.html) ссылке.

    > [!NOTE]
    > Эти значения приведены для примера. Замените эти значения фактическим идентификатором, URL-адресом ответа, URL-адресом для входа и состоянием ретранслятора. Чтобы получить эти значения, обратитесь в [группу поддержки клиентов Saba Cloud](mailto:support@saba.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Скопируйте требуемые URL-адреса в разделе **Настройка Saba Cloud**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B. Simon, предоставив этому пользователю доступ к Saba Cloud.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Saba Cloud**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-saba-cloud-sso"></a>Настройка единого входа в Saba Cloud

1. Войдите на корпоративный веб-сайт Saba Cloud в качестве администратора.
1. Щелкните значок **Меню**, затем **Администратор** и перейдите на вкладку **Системный администратор**.

    ![снимок экрана: системный администратор](./media/saba-cloud-tutorial/system.png)

1. В разделе **Настройка системы** выберите **Настройка единого входа SAML** и нажмите кнопку **Настроить единый вход SAML**. 

    ![Снимок экрана: настройка](./media/saba-cloud-tutorial/configure.png)

1. Во всплывающем окне в раскрывающемся списке выберите **Микросайт** и нажмите **Добавить и настроить**.

    ![снимок экрана: добавление сайта или микросайта](./media/saba-cloud-tutorial/microsite.png)

1. В разделе **Настройка IDP** щелкните **Обзор**, чтобы отправить файл **XML метаданных федерации**, загруженный с портала Azure. Установите флажок **IDP для конкретного сайта** и нажмите кнопку **Импорт**.

    ![снимок экрана: импорт сертификата](./media/saba-cloud-tutorial/certificate.png) 

1. В разделе **Настройка SP** скопируйте значение **Псевдоним сущности** и вставьте его в текстовое поле **Идентификатор (ИД сущности)** в разделе **Базовая конфигурация SAML** на портале Azure. Щелкните **Создать**.

    ![снимок экрана: настройка SP](./media/saba-cloud-tutorial/generate-metadata.png) 

1. В разделе **Настройка свойств** проверьте заполненные поля и нажмите кнопку **Сохранить**. 

    ![снимок экрана: настройка свойств](./media/saba-cloud-tutorial/configure-properties.png) 

### <a name="create-saba-cloud-test-user"></a>Создание тестового пользователя Saba Cloud

В этом разделе вы создадите в Saba Cloud пользователя Britta Simon. В Saba Cloud поддерживается JIT-подготовка пользователей, включенная по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Saba Cloud, он создается после проверки подлинности.

> [!NOTE]
> Сведения о включении SAML JIT-подготовки пользователей с Saba Cloud см. в [этой](https://help.sabacloud.com/sabacloud/help-system/topics/help-system-user-provisioning-with-saml.html) документации.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Saba Cloud, где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в Saba Cloud и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* Выберите **Тестировать приложение** на портале Azure, и вы автоматически войдете в приложение Saba Cloud, для которого настроен единый вход. 

Вы можете также использовать портал "Мои приложения" корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Saba Cloud на портале "Мои приложения", вы перейдете на страницу входа приложения для инициации потока входа (при настройке в режиме поставщика службы) или автоматически войдете в приложение Saba Cloud, для которого настроен единый вход (при настройке в режиме поставщика удостоверений). Дополнительные сведения о портале "Мои приложения" см. в [этой статье](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

> [!NOTE]
> Если URL-адрес входа в Azure AD не указан, то приложение рассматривается как режим, инициированный поставщиком удостоверений. А если URL-адрес входа введен, Azure AD будет всегда перенаправлять пользователя в приложение Saba Cloud для потока, инициированного поставщиком услуг.

## <a name="test-sso-for-saba-cloud-mobile"></a>Тестирование единого входа для Saba Cloud (Mobile)

1. Откройте приложение Saba Cloud Mobile, заполните поле **Имя сайта** и нажмите клавишу **Ввод**.

    ![Снимок экрана: имя сайта.](./media/saba-cloud-tutorial/site-name.png)

1. Введите **адрес электронной почты** и нажмите кнопку **Далее**.

    ![Снимок экрана: адрес электронной почты.](./media/saba-cloud-tutorial/email-address.png)

1. Наконец, после успешного входа появится страница приложения.

    ![Снимок экрана: успешный вход.](./media/saba-cloud-tutorial/dashboard.png)

## <a name="next-steps"></a>Дальнейшие действия

После настройки Saba Cloud вы сможете применить функцию управления сеансами, которая в реальном времени защищает конфиденциальные данные вашей организации от кражи и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


