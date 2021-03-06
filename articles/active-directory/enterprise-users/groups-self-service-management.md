---
title: Настройка самостоятельного управления группами в Azure Active Directory | Документация Майкрософт
description: Создание групп безопасности или групп Microsoft 365 и управление ими в Azure Active Directory, а также запрос членства в группах безопасности или Microsoft 365
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: how-to
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6b2b8e3374c362f937aa5cfe106e8da9f9aa39f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "96548007"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Настройка самостоятельного управления группами в Azure Active Directory 

Вы можете разрешить пользователям создавать собственные группы безопасности или группы Microsoft 365 в Azure Active Directory (Azure AD) и управлять ими. Владелец группы может утверждать или отклонять запросы на членство, а также может делегировать управление членством в группе. Функции самостоятельного управления группами недоступны для групп безопасности с включенной почтой или списков рассылки.

## <a name="self-service-group-membership-defaults"></a>Параметры по умолчанию для членства в группах

При создании групп безопасности в портал Azure или с помощью Azure AD PowerShell только владельцы группы могут обновлять членство. Группы безопасности, созданные с помощью самообслуживания на [панели доступа](https://account.activedirectory.windowsazure.com/r#/joinGroups) и все группы Microsoft 365, доступны для присоединения для всех пользователей, будь то утверждено или автоматически утверждено владельцем. На панели доступа можно изменить параметры членства при создании группы.

Группы, созданные в | Поведение группы безопасности по умолчанию | Поведение группы Microsoft 365 по умолчанию
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](../enterprise-users/groups-settings-cmdlets.md) | Добавлять членов могут только владельцы<br>Видимый, но недоступный для объединения на панели доступа | Открыть для присоединиться ко всем пользователям
[Портал Azure](https://portal.azure.com) | Добавлять членов могут только владельцы<br>Видимый, но недоступный для объединения на панели доступа<br>Владелец не назначается автоматически при создании группы | Открыть для присоединиться ко всем пользователям
[Панель доступа](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Открыть для присоединиться ко всем пользователям<br>Параметры членства можно изменить при создании группы | Открыть для присоединиться ко всем пользователям<br>Параметры членства можно изменить при создании группы

## <a name="self-service-group-management-scenarios"></a>Сценарии управления группами самообслуживания

* **Делегированное управление группами** Примером является администратор, управляющий доступом к приложению SaaS, которое использует компания. Управление этими правами доступа становится трудоемким, поэтому администратор запрашивает у владельца компании разрешение на создание новой группы. Администратор назначает приложению доступ к новой группе и добавляет в группу всех пользователей, которые уже обращаются к приложению. Впоследствии владелец компании может добавить несколько пользователей, и им автоматически будет предоставлен доступ к приложению. Владельцу компании не нужно ждать, пока администратор выполнит свою работу. Если администратор предоставляет такое же разрешение руководителю в другой бизнес-группе, он также может управлять доступом для своих членов группы. Ни владелец предприятия, ни руководитель не могут просматривать членство в группах друг друга и управлять им. Администратор может просматривать всех пользователей, имеющих доступ к приложению, и блокировать права доступа при необходимости.
* **Самостоятельное управление группами** В качестве примера этого сценария можно использовать два пользователя, которые имеют независимо настроенные сайты SharePoint Online. Они хотят предоставить группам друг другу доступ к своим сайтам. Для этого они создают одну группу в Azure AD, а затем в SharePoint Online каждый из них выбирает одну и ту же группу для предоставления доступа к сайтам. Когда кому-нибудь требуется доступ, он запрашивает его на панели доступа, и после утверждения он автоматически получает доступ к обоим сайтам SharePoint Online. Позже один из них решает, что все пользователи сайта также должны получить доступ к определенному приложению SaaS. Администратор приложения SaaS может добавить права доступа к сайту SharePoint Online для приложения. Теперь любые утвержденные им запросы предоставят доступ к двум сайтам SharePoint Online и приложению SaaS.

## <a name="make-a-group-available-for-user-self-service"></a>Включение функции самообслуживания для пользователей группы

1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com) с помощью учетной записи глобального администратора каталога.
1. Выберите **группы**, а затем щелкните **Общие** параметры.
1. Задать **владельцам можно управлять запросами на членство в группах на панели доступа** , чтобы **Да**.
1. Установите для **параметра ограничить доступ к группам на панели доступа** значение **нет**.
1. Если вы настроили **пользователям возможность создавать группы безопасности на порталах Azure** , или **пользователи могут создавать группы Microsoft 365 на порталах Azure** , чтобы

    - **Да**. всем пользователям в вашей организации Azure AD разрешено создавать новые группы безопасности и добавлять в них участников. Эти новые группы также будут отображаться на панели доступа для всех остальных пользователей. Если параметр политики в группе разрешает ее, другие пользователи могут создавать запросы на присоединение к этим группам.
    - **Нет**: пользователи не могут создавать группы и изменять существующие группы, владельцем которых они являются. Однако они могут управлять членством в этих группах и утверждать запросы на присоединение к группам от других пользователей.

Вы также можете использовать **владельцев, которые могут назначать членов в качестве владельцев групп в портал Azure** , чтобы получить более детализированный контроль над управлением группами самообслуживания для пользователей.

Когда пользователи могут создавать группы, всем пользователям в организации разрешается создавать новые группы, после чего в качестве владельца по умолчанию можно добавлять участников в эти группы. Нельзя указать пользователей, которые могут создавать собственные группы. Вы можете указать отдельных лиц только для того, чтобы сделать другого члена группы владельцем группы.

> [!NOTE]
> Чтобы пользователи могли запрашивать присоединение к группе безопасности или группе Microsoft 365, а также для утверждения или запрета запросов на участие, требуется лицензия Azure Active Directory Premium (P1 или P2). Без лицензии Azure Active Directory Premium пользователи по-прежнему могут управлять своими группами на панели доступа, но не могут создать группу, для которой требуется утверждение владельца на панели доступа, и не могут запрашивать присоединение к группе.

## <a name="next-steps"></a>Дальнейшие действия

В следующих статьях содержатся дополнительные сведения об Azure Active Directory.

* [Управление доступом к ресурсам с помощью групп Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Настройка параметров групп с помощью командлетов Azure Active Directory](../enterprise-users/groups-settings-cmdlets.md)
* [Управление приложениями в Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Что такое Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
