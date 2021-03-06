---
title: Руководство. Интеграция единого входа Azure Active Directory с WEDO | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и WEDO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/22/2020
ms.author: jeedes
ms.openlocfilehash: 529c4e6433d16f9d70530ba516b5ec1426806984
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "92519237"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wedo"></a>Руководство. Интеграция единого входа Azure Active Directory с WEDO

В этом руководстве описано, как интегрировать WEDO с Azure Active Directory (Azure AD). Интеграция WEDO с Azure AD обеспечивает следующие возможности:

* Управление доступом к WEDO с помощью Azure AD.
* Автоматический вход пользователей в WEDO с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка WEDO с поддержкой единого входа. Для получения подписки единого входа обратитесь в [группу поддержки клиентов WEDO](mailto:info@wedo.swiss).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* WEDO поддерживает вход, инициированный **поставщиком услуг или поставщиком удостоверений**

* [После настройки WEDO можно применять элементы управления сеансами, которые защищают от хищения и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Элементы управления сеансом являются расширением функции условного доступа. Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-wedo-from-the-gallery"></a>Добавление WEDO из коллекции

Чтобы настроить интеграцию WEDO с Azure AD, необходимо добавить WEDO из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **WEDO**.
1. Выберите **WEDO** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-wedo"></a>Настройка и проверка единого входа Azure AD для WEDO

Настройте и проверьте единый вход Azure AD в WEDO с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в WEDO.

Чтобы настроить и проверить единый вход Azure AD в WEDO, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в WEDO](#configure-wedo-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя в WEDO](#create-wedo-test-user)** нужно для того, чтобы в WEDO существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **WEDO** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<SUBDOMAIN>.wedo.swiss/sp/acs`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<SUBDOMAIN>.wedo.swiss/sp/acs`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<SUBDOMAIN>.wedo.swiss/`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [группу поддержки клиентов WEDO](mailto:info@wedo.swiss). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение WEDO ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    | Имя | Исходный атрибут|
    | ------------ | --------- |
    | email | user.email |
    | firstName | user.firstName |
    | lastName | user.lasttName |
    | userName | user.userName |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка WEDO**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к WEDO.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **WEDO**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-wedo-sso"></a>Настройка единого входа для WEDO

Выполните следующие действия, чтобы включить единый вход Azure AD в WEDO.

1. Войдите в [WEDO](https://login.wedo.swiss/). Необходимо иметь **роль администратора**.
1. В параметрах профиля выберите меню **Проверки подлинности** в разделе **Параметры сети**.
1. На странице **Проверка подлинности SAML** выполните следующие действия.

   ![Ссылка на проверку подлинности SAML](media/wedo-tutorial/network-security-authentification.png)

   а. Включение **проверки подлинности SAML**.

   b. Выберите вкладку **Identity Provider metadata (XML)** (Метаданные поставщика удостоверений (XML)).

   c. Откройте в Блокноте скачанный на портале Azure **XML-файл метаданных федерации**, и скопируйте содержимое этого файла и вставьте его в текстовое поле **X.509 Certificate**.

   d. В меню «Параметры» щелкните пункт **Сохранить**

### <a name="create-wedo-test-user"></a>Создание тестового пользователя WEDO

В этом разделе описано, как в WEDO создать тестового пользователя с именем Bob Simon. Информация должна соответствовать сведениям в разделе *Создание тестового пользователя Azure AD*.

1. Из параметра "Профиль" в WEDO выберите **Пользователи** в разделе *Параметры сети*.
1. Нажмите кнопку **Добавить пользователя**.
1. Введите сведения о пользователе в всплывающем окне "Добавить пользователя"

    а. Имя `B`.

    b. Фамилия `Simon`.

    c. Введите адрес электронной почты `username@companydomain.extension`. Например, `B.Simon@contoso.com`. Обязательно иметь электронную почту с доменом, что отвечает короткому названию вашей компании.

    d. Тип пользователя `User`.

    д) Щелкните **Create user** (Создать пользователя).

    е) На странице *Выберите команды* щелкните **Сохранить**.

    ж.  На странице *Пригласить пользователя* щелкните **Да**.

1. Проверка пользователя с помощью ссылки, полученной по электронной почте

> [!NOTE]
> Если вы хотите создать фиктивного пользователя (электронная почта выше не существует в вашей сети), свяжитесь с [нашей поддержкой](mailto:info@wedo.swiss), чтобы проверить пользователя*.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку WEDO на Панели доступа, вы автоматически войдете в приложение WEDO, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Пробное использование WEDO с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Protect apps with Microsoft Cloud App Security Conditional Access App Control](/cloud-app-security/proxy-intro-aad) (Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security)