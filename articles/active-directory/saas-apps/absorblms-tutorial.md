---
title: Руководство. Интеграция Azure Active Directory с Absorb LMS | Документы Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Absorb LMS.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/05/2021
ms.author: jeedes
ms.openlocfilehash: 268943463002ddd1dbdbf67df9587758f81f537f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101646526"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Руководство. Интеграция Azure Active Directory с Absorb LMS

В этом учебнике описано, как интегрировать приложение Absorb LMS с Azure Active Directory (Azure AD). Интеграция Absorb LMS с Azure AD обеспечивает следующие возможности:

* Управление доступом к Absorb LMS с помощью Azure AD.
* Автоматический вход пользователей в Absorb LMS с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Absorb LMS, вам потребуется:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Absorb LMS с поддержкой единого входа.

> [!NOTE]
> Эту интеграцию также можно использовать в облачной среде Azure AD для государственных организаций США. Это приложение можно найти в коллекции облачных приложений с поддержкой Azure AD для государственных организаций США и настроить таким же образом, как и в общедоступном облаке.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Absorb LMS поддерживает единый вход, инициированный **поставщиком услуг**.

> [!NOTE]
> Идентификатор этого приложения — фиксированное строковое значение, поэтому в одном клиенте можно настроить только один экземпляр.

## <a name="add-absorb-lms-from-the-gallery"></a>Добавление Absorb LMS из коллекции

Чтобы настроить интеграцию Absorb LMS с Azure AD, нужно добавить Absorb LMS из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Absorb LMS**.
1. Выберите **Absorb LMS** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-absorb-lms"></a>Настройка и проверка единого входа Azure AD для Absorb LMS

Настройте и проверьте единый вход Azure AD в Absorb LMS с помощью тестового пользователя **B. Simon**. Чтобы обеспечить работу единого входа, нужно установить связь между пользователем Azure AD и соответствующим пользователем в Absorb LMS.

Чтобы настроить и проверить единый вход Azure AD в Absorb LMS, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Absorb LMS](#configure-absorb-lms-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя в Absorb LMS](#create-absorb-lms-test-user)** требуется, чтобы в приложении существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Absorb LMS** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** для изменения параметров.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    Если вы используете **Absorb 5 — UI**, примените следующую конфигурацию.

    а. В текстовом поле **Идентификатор** введите URL-адрес в таком формате: `https://<SUBDOMAIN>.myabsorb.com/account/saml`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<SUBDOMAIN>.myabsorb.com/account/saml`.

    Если вы используете **Absorb 5 — New Learner Experience**, примените следующую конфигурацию.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<SUBDOMAIN>.myabsorb.com/api/rest/v2/authentication/saml`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<SUBDOMAIN>.myabsorb.com/api/rest/v2/authentication/saml`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Absorb LMS](https://support.absorblms.com/hc/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На следующем снимке экрана показан список атрибутов по умолчанию, в котором **nameidentifier** сопоставляется с **user.userprincipalname**.

    ![image](common/edit-attribute.png)

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

7. Скопируйте требуемый URL-адрес из раздела **Настройка Absorb LMS**.

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

В этом разделе объясняется, как включить единый вход в Azure для пользователя B. Simon, предоставив этому пользователю доступ к Absorb LMS.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Absorb LMS**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-absorb-lms-sso"></a>Настройка единого входа в Absorb LMS

1. В новом окне веб-браузера войдите на свой корпоративный веб-сайт Absorb LMS в качестве администратора.

2. Нажмите кнопку **Account** (Учетная запись) в правом верхнем углу.

    ![Кнопка "Account" (Учетная запись)](./media/absorblms-tutorial/account.png)

3. В области "Account" (Учетная запись) щелкните **Portal Settings** (Параметры портала).

    ![Ссылка "Portal Settings" (Параметры портала)](./media/absorblms-tutorial/portal.png)

4. Выберите вкладку **Manage SSO Settings** (Управление параметрами единого входа).

    ![Вкладка "Пользователи"](./media/absorblms-tutorial/sso.png)

5. На странице **Manage Single Sign-On Settings** (Управление параметрами единого входа) выполните следующие действия.

    ![Страница "Single Sign-On Configuration" (Конфигурация единого входа)](./media/absorblms-tutorial/settings.png)

    а. В текстовом поле **Имя** введите имя, например Azure AD Marketplace SSO.

    b. Выберите **SAML** в качестве **метода**.

    c. В Блокноте откройте сертификат, скачанный с портала Azure. Удалите теги **---BEGIN CERTIFICATE---** и **---END CERTIFICATE---**. Затем в поле **Key** (Ключ) вставьте оставшееся содержимое.

    d. В поле **Mode** (Режим) выберите **Identity Provider Initiated** (Инициируемый поставщиком удостоверений).

    д. В поле **Id Property** (Свойство идентификатора) выберите атрибут, который вы настроили в качестве идентификатора пользователя в Azure AD. Например, если вы выбрали в Azure AD *nameidentifier*, то выберите атрибут **Username**.

    е) Выберите **Sha256** в качестве **типа подписи**.

    ж. В поле **Login URL** (URL-адрес входа) вставьте значение **URL-адрес пользовательского доступа** со страницы **Свойства** приложения на портале Azure.

    h. В поле **Logout URL** (URL-адрес выхода) вставьте значение **URL-адрес выхода**, скопированное из окна **Настройка единого входа** на портале Azure.

    i. Установите переключатель **Automatically Redirect** (Автоматическое перенаправление) в значение **Вкл**.

6. Щелкните **Сохранить**.

    ![Включение параметра "Only Allow SSO Login" (Разрешить только единый вход)](./media/absorblms-tutorial/save.png)

### <a name="create-absorb-lms-test-user"></a>Создание тестового пользователя Absorb LMS

Чтобы пользователи Azure AD могли входить в Absorb LMS, их нужно настроить в Absorb LMS. В случае с Absorb LMS подготовка выполняется вручную.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Войдите на свой корпоративный веб-сайт Absorb LMS в качестве администратора.

2. В области **Users** (Пользователи) щелкните **Users** (Пользователи).

    ![Ссылка "Users" (Пользователи)](./media/absorblms-tutorial/users.png)

3. Выберите вкладку **Пользователь**.

    ![Раскрывающийся список "Add New" (Добавить)](./media/absorblms-tutorial/add.png)

4. На странице **Add User** (Добавление пользователя) сделайте следующее.

    ![Страница "Add User" (Добавление пользователя)](./media/absorblms-tutorial/user.png)

    а. В текстовом поле **First Name** (Имя) введите имя, например **Britta**.

    b. В текстовом поле **Last Name** (Фамилия) введите фамилию, например **Simon**.

    c. В текстовом поле **Username** (Имя пользователя) введите полное имя, например **Britta Simon**.

    d. В поле **Пароль** введите пароль пользователя.

    д. В поле **Confirm Password** (Подтверждение пароля) введите пароль еще раз.

    е) Переведите переключатель **Is Active** (Активность) в положение **Active** (Активный).

5. Щелкните **Сохранить**.

    ![Включение параметра "Only Allow SSO Login" (Разрешить только единый вход)](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > По умолчанию для подготовки пользователей единый вход не включен. Если клиент хочет включить эту функцию, он должен ее настроить с помощью [этой](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) документации. Кроме того, обратите внимание, что подготовка пользователей доступна только в **Absorb 5 — New Learner Experience** с URL-адресом ACS: `https://company.myabsorb.com/api/rest/v2/authentication/saml`

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов.

* Выберите "Тестировать приложение" на портале Azure, и вы автоматически войдете в приложение Absorb LMS, для которого настроен единый вход.

* Вы можете использовать портал "Мои приложения" корпорации Майкрософт. Щелкнув плитку Absorb LMS на портале "Мои приложения", вы автоматически войдете в приложение Absorb LMS, для которого настроили единый вход. Дополнительные сведения о портале "Мои приложения" см. в [этой статье](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Absorb LMS вы можете применить управление сеансами, которое в реальном времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).