---
title: Руководство. Интеграция единого входа Azure Active Directory со Nintex Promapp | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Nintex Promapp.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2020
ms.author: jeedes
ms.openlocfilehash: fc31195e7f544bdce7fe2f135a39cb9992875d0a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "92505924"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nintex-promapp"></a>Руководство. Интеграция единого входа Azure Active Directory с Nintex Promapp

В этом руководстве описано, как интегрировать Nintex Promapp с Azure Active Directory (Azure AD). Интеграция Nintex Promapp с Azure AD обеспечивает следующие возможности.

* Контроль доступа к Nintex Promapp с помощью Azure AD.
* Включение автоматического входа пользователей в Nintex Promapp с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Nintex Promapp с поддержкой единого входа (SSO).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Nintex Promapp поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**
* Nintex Promapp поддерживает **JIT**-подготовку пользователей.

## <a name="adding-nintex-promapp-from-the-gallery"></a>Добавление Nintex Promapp из коллекции

Чтобы настроить интеграцию Nintex Promapp с Azure AD, необходимо добавить Nintex Promapp из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Nintex Promapp**.
1. Выберите **Nintex Promapp** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-nintex-promapp"></a>Настройка и проверка единого входа Azure AD для Nintex Promapp

Настройте и проверьте единый вход Azure AD в Nintex Promapp с использованием тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в Nintex Promapp.

Чтобы настроить и проверить единый вход AAD в Nintex Promapp, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Nintex Promapp](#configure-nintex-promapp-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя Nintex Promapp](#create-nintex-promapp-test-user)** требуется для того, чтобы в Nintex Promapp существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Nintex Promapp** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    1. В поле **Идентификатор** введите URL-адрес в следующем формате:

        ```https
        https://go.promapp.com/TENANTNAME/
        https://au.promapp.com/TENANTNAME/
        https://us.promapp.com/TENANTNAME/
        https://eu.promapp.com/TENANTNAME/
        https://ca.promapp.com/TENANTNAME/
        ```

       > [!NOTE]
       > При интеграции Azure AD с Nintex Promapp сейчас поддерживается только аутентификация, инициированная службой. (То есть переход по URL-адресу Nintex Promapp запускает процесс аутентификации.) При этом поле **URL-адрес ответа** является обязательным.

    1. В поле **URL-адрес ответа** введите URL-адрес в следующем формате:

       `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate.aspx`

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate`

    > [!NOTE]
    > Эти значения представляют собой заполнители. Вместо них нужно указать фактический идентификатор, URL-адрес ответа и URL-адрес для входа. Чтобы получить эти значения, обратитесь в [службу поддержки Nintex Promapp](https://www.promapp.com/about-us/contact-us/). Можно также ознакомиться с шаблонами в диалоговом окне **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Nintex Promapp**.

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

В этом разделе описано, как разрешить пользователю B.Simon использовать единый вход Azure, предоставив этому пользователю доступ к Nintex Promapp.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Nintex Promapp**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-nintex-promapp-sso"></a>Настройка единого входа для Nintex Promapp

1. Войдите на корпоративный сайт Nintex Promapp с правами администратора.

2. В меню в верхней части окна нажмите **Admin** (Администрирование).

    ![Выбор меню администрирования][12]

3. Выберите **Configure** (Настроить):

    ![Выбор пункта меню настроек][13]

4. В диалоговом окне **Security** (Безопасность) сделайте следующее:

    ![Диалоговое окно безопасности][14]

    1. В поле **SSO Login URL** (URL-адрес для единого входа) вставьте **URL-адрес входа**, скопированный на портале Azure.

    1. В списке **SSO — Single Sign-on Mode** (Режим единого входа) выберите **Optional** (Необязательно). Щелкните **Сохранить**.

       > [!NOTE]
       > Режим Optional (Необязательно) предназначен только для тестирования. После настройки задайте для параметра **SSO — Single Sign-on Mode** (Режим единого входа) значение **Required** (Обязательно), чтобы аутентификация выполнялась в Azure AD для всех пользователей.

    1. Откройте в Блокноте сертификат, скачанный на портала Azure. Скопируйте содержимое сертификата без первой и последней строки (**---BEGIN CERTIFICATE---** и **---END CERTIFICATE---**). Вставьте содержимое в сертификата в поле **SSO-x.509 Certificate** (Сертификат единого входа x.509) и нажмите **Сохранить**.

### <a name="create-nintex-promapp-test-user"></a>Создание тестового пользователя в Nintex Promapp

В этом разделе описано, как в приложении Nintex Promapp создать пользователя с именем B.Simon. Приложение Nintex Promapp поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Nintex Promapp, он создается после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Nintex Promapp на Панели доступа, вы автоматически войдете в приложение Nintex Promapp, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Пробное использование Nintex Promapp с Azure AD](https://aad.portal.azure.com/)

<!--Image references-->

[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png