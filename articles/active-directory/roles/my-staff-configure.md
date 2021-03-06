---
title: Использование моих сотрудников для делегирования управления пользователями в Azure AD | Документация Майкрософт
description: Делегирование управления пользователями с помощью моих сотрудников и административных единиц
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.topic: how-to
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 03/11/2021
ms.author: rolyon
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 1a380c8a3d766c3c11d8cba1148383d924f65a1b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103225002"
---
# <a name="manage-your-users-with-my-staff"></a>Управляйте своими пользователями с помощью моих сотрудников

Мой персонал позволяет делегировать разрешения на доступ к учетной записи Azure AD, например менеджеру магазина или руководителю команды, чтобы обеспечить их сотрудников. Вместо того, чтобы полагаться на центральную службу поддержки, организации могут делегировать общие задачи, такие как сброс паролей или изменение номеров телефонов для локального руководителя группы. Благодаря моему персоналу пользователь, который не может получить доступ к своей учетной записи, может получить доступ всего за несколько щелчков, не требующих технической поддержки или персонала отдела ИТ.

Перед настройкой сотрудников Организации рекомендуется ознакомиться с этой документацией, а также с [пользовательской документацией](../user-help/my-staff-team-manager.md) , чтобы узнать, как она работает и как она влияет на пользователей. Пользовательскую документацию можно использовать для обучения и подготовки пользователей к новым интерфейсам и обеспечения успешного развертывания.

## <a name="how-my-staff-works"></a>Как работает мой персонал

Мои сотрудники основаны на административных единицах, которые являются контейнером ресурсов, которые можно использовать для ограничения области административного управления назначением ролей. Дополнительные сведения см. [в разделе Администрирование единиц управления в Azure Active Directory](administrative-units.md). В моем штате административные единицы могут использоваться для хранения группы пользователей в магазине или отделе. Затем руководитель группы может быть назначен административной роли в области одного или нескольких единиц.

## <a name="before-you-begin"></a>Перед началом

Для работы с этой статьей требуются следующие ресурсы и разрешения:

* Активная подписка Azure.

  * Если у вас еще нет подписки Azure, [создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Клиент Azure Active Directory должен быть связан с вашей подпиской.

  * Если потребуется, [создайте клиент Azure Active Directory](../fundamentals/sign-up-organization.md) или [свяжите подписку Azure со своей учетной записью](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
* Для включения проверки подлинности на основе SMS требуются права *глобального администратора* в клиенте Azure AD.
* Каждый пользователь, который включен в политике метода проверки подлинности текстовых сообщений, должен иметь лицензию, даже если они не используют их. Каждый включенный пользователь должен иметь одну из следующих лицензий Azure AD или Microsoft 365:

  * [Azure AD Premium P1 или P2](https://azure.microsoft.com/pricing/details/active-directory/)
  * [Microsoft 365 (M365) F1 или F3](https://www.microsoft.com/licensing/news/m365-firstline-workers)
  * [Enterprise Mobility + Security (EMS) E3 или E5](https://www.microsoft.com/microsoft-365/enterprise-mobility-security/compare-plans-and-pricing) или [Microsoft 365 (M365) E3 или E5](https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans)

## <a name="how-to-enable-my-staff"></a>Включение персонала

После настройки административных единиц можно применить эту область к пользователям, обращающимся к моим сотрудникам. Только пользователи, которым назначена административная роль, могут получить доступ к моему персоналу. Чтобы включить моего персонала, выполните следующие действия.

1. Войдите в портал Azure в качестве администратора пользователей.
2. Перейдите к **Azure Active Directory**  >  **Параметры пользователя**  >  Предварительный **Просмотр пользовательских** функций  >  **Управление параметрами просмотра пользовательских компонентов**.
3. В разделе **Администраторы могут получить доступ к моим сотрудникам** можно выбрать включение для всех пользователей, выбранных пользователей или отсутствие доступа пользователей.

> [!Note]
> Только пользователи, которым назначена роль администратора, могут получить доступ к моему персоналу. Если включить персонал для пользователя, которому не назначена роль администратора, он не сможет получить доступ к моему персоналу.

## <a name="conditional-access"></a>Условный доступ

Вы можете защитить портал моего персонала с помощью политики условного доступа Azure AD. Используйте его для таких задач, как требовать многофакторную проверку подлинности, прежде чем обращаться к моему персоналу.

Настоятельно рекомендуется защищать сотрудников с помощью [политик условного доступа Azure AD](../conditional-access/index.yml). Чтобы применить политику условного доступа к моим сотрудникам, сначала в течение нескольких минут вы должны посетить сайт "Мои сотрудники", чтобы автоматически подготавливать субъект-службу в клиенте для использования в условном доступе.

Вы увидите субъект-службу при создании политики условного доступа, которая применяется к облачному приложению My Staff.

![Создание политики условного доступа для приложения "Мои сотрудники"](./media/my-staff-configure/conditional-access.png)

## <a name="using-my-staff"></a>Использование моего персонала

Когда пользователь переходит к моему персоналу, он показывает имена [административных единиц](administrative-units.md) , для которых у них есть административные разрешения. В [пользовательской документации "Мои сотрудники](../user-help/my-staff-team-manager.md)" мы используем термин "расположение" для ссылки на административные единицы. Если разрешения администратора не имеют области административной единицы, то разрешения применяются в Организации. После включения сотрудников пользователи, которым назначена административная роль, могут получить к ним доступ с помощью [https://mystaff.microsoft.com](https://mystaff.microsoft.com) . Они могут выбрать административную единицу для просмотра пользователей в этом блоке, а также выбрать пользователя, чтобы открыть свой профиль.

## <a name="reset-a-users-password"></a>Сброс пароля пользователя

Прежде чем можно будет задержать пароли для локальных пользователей, необходимо выполнить следующие обязательные условия. Подробные инструкции см. в разделе [Включение самостоятельного сброса пароля](../authentication/tutorial-enable-sspr-writeback.md) .

* Настройка разрешений для обратной записи паролей
* Включение обратной записи паролей в Azure AD Connect
* Включение обратной записи паролей в самостоятельный сброс пароля Azure AD (SSPR)

Следующие роли имеют разрешение на сброс пароля пользователя:

* [Администратор проверки подлинности](permissions-reference.md#authentication-administrator)
* [Администратор привилегированной проверки подлинности](permissions-reference.md#privileged-authentication-administrator)
* [Глобальный администратор](permissions-reference.md#global-administrator)
* [Администратор службы поддержки](permissions-reference.md#helpdesk-administrator)
* [Администратор пользователей](permissions-reference.md#user-administrator)
* [Администратор паролей](permissions-reference.md#password-administrator)

В **окне Мой персонал** откройте профиль пользователя. Выберите **Сбросить пароль**.

* Если пользователь является только облаком, можно увидеть временный пароль, который можно предоставить пользователю.
* Если пользователь синхронизируется с локальной Active Directory, можно ввести пароль, соответствующий локальным политикам AD. Затем этот пароль можно предоставить пользователю.

    ![Индикатор хода выполнения сброса пароля и уведомление об успешном выполнении](./media/my-staff-configure/reset-password.png)

Пользователь должен изменить пароль при следующем входе в систему.

## <a name="manage-a-phone-number"></a>Управление номером телефона

В **окне Мой персонал** откройте профиль пользователя.

* Щелкните " **Добавить номер телефона** ", чтобы добавить номер телефона для пользователя
* Выберите **изменить номер телефона** , чтобы изменить номер телефона
* Выберите **удалить номер телефона** , чтобы удалить номер телефона пользователя.

В зависимости от параметров пользователь может использовать номер телефона, настроенный для входа с помощью SMS, выполнять многофакторную проверку подлинности и выполнять самостоятельный сброс пароля.

Для управления номером телефона пользователя необходимо назначить одну из следующих ролей:

* [Администратор проверки подлинности](permissions-reference.md#authentication-administrator)
* [Администратор привилегированной проверки подлинности](permissions-reference.md#privileged-authentication-administrator)
* [Глобальный администратор](permissions-reference.md#global-administrator)

## <a name="search"></a>Поиск

Вы можете искать административные единицы и пользователей в Организации с помощью панели поиска в моем персонале. Можно выполнять поиск по всем административным единицам и пользователям в Организации, но вы можете вносить изменения только для тех пользователей, которые находятся в административной единице, для которой были предоставлены разрешения администратора.

Можно также выполнить поиск пользователя в административной единице. Для этого используйте панель поиска в верхней части списка пользователей.

## <a name="audit-logs"></a>Журналы аудита

Вы можете просматривать журналы аудита для действий, выполненных в моем персонале на портале Azure Active Directory. Если журнал аудита был создан действием, выполненным в моем персонале, вы увидите, что это указано в разделе Дополнительные сведения в событии аудита.

## <a name="next-steps"></a>Следующие шаги

[Моя документация](../user-help/my-staff-team-manager.md) 
 для пользователя [Документация по административным единицам](administrative-units.md)
