---
title: AD FS входа в Azure AD с помощью Connect Health | Документация Майкрософт
description: В этом документе описано, как интегрировать AD FS входа с отчетом Azure AD Connect Healthных входов.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/16/2021
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74769feba1d717a2f1a72d311f85bdfbeac7b7db
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103574799"
---
# <a name="ad-fs-sign-ins-in-azure-ad-with-connect-health---preview"></a>AD FS входа в Azure AD с помощью Connect Health — Предварительная версия

AD FS входы теперь можно интегрировать в отчет Azure Active Directory входов с помощью Connect Health. Отчет о событиях [входа в Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-all-sign-ins#:~:text=Interactive%20user%20sign-ins%20are%20sign-ins%20where%20a%20user,to%20Azure%20AD%20or%20to%20a%20helper%20app.) содержит сведения о входе пользователей, приложений и управляемых ресурсов в Azure AD и доступ к ресурсам. 

Агент Connect Health для AD FS сопоставляет несколько идентификаторов событий от AD FS, зависящих от версии сервера, чтобы предоставить сведения о запросе и сведения об ошибке в случае сбоя запроса. Эти сведения сопоставляются со схемой отчета о входе в Azure AD и отображаются в интерфейсе отчета Azure AD Sign-In. Наряду с отчетом доступен новый Log Analyticsный поток с данными AD FS и новым шаблоном книги Azure Monitor. Шаблон можно использовать и изменять для углубленного анализа таких сценариев, как AD FS блокировки учетных записей, неудачные попытки ввода пароля и пиковые попытки входа.

## <a name="prerequisites"></a>Предварительные требования
* Azure AD Connect Health для AD FS установлен и обновлен до последней версии.
* Глобальный администратор или роль читателя отчетов для просмотра входов в Azure AD

## <a name="what-data-is-displayed-in-the-report"></a>Какие данные отображаются в отчете?
Доступные данные представляют одни и те же данные, доступные для входа в Azure AD. Пять вкладок с информацией будут доступны в зависимости от типа входа: Azure AD или AD FS. Connect Health сопоставляет события из AD FS, зависящие от версии сервера, и сопоставляет их со схемой AD FS. 



#### <a name="user-sign-ins"></a>Вход пользователей 
На каждой вкладке в колонке входы отображаются следующие значения по умолчанию:
* дата входа;
* Идентификатор запроса
* Имя пользователя или идентификатор пользователя
* Состояние входа
* IP-адрес устройства, используемого для входа
* Идентификатор Sign-In

#### <a name="authentication-method-information"></a>Сведения о методе проверки подлинности
На вкладке Проверка подлинности могут отображаться следующие значения. Метод проверки подлинности берется из журналов аудита AD FS.

|Метод проверки подлинности|Описание|
|-----|-----|
|Формы|Проверка имени пользователя и пароля|
|Windows|Встроенная проверка подлинности Windows|
|Certificate|Проверка подлинности с помощью сертификатов смарт-карты или Виртуалсмарт|
|WindowsHelloForBusiness|Это поле предназначено для проверки подлинности с помощью Windows Hello для бизнеса. (Microsoft Passport проверка подлинности)|
|Устройство | Отображается, если для проверки подлинности устройства выбрана основная проверка подлинности из интрасети или экстрасети и выполняется проверка подлинности устройства.  В этом сценарии нет отдельной проверки подлинности пользователя.| 
|Федеративные|AD FS не выполнил проверку подлинности, но отправил ее сторонним поставщикам удостоверений.|
|Единый вход |Если использовался маркер единого входа, это поле будет отображаться. Если единый вход содержит MFA, он будет показан как многофакторный|
|Многофакторной|Если у маркера единого входа есть MFA, который использовался для проверки подлинности, это поле будет отображаться как многофакторная|
|Многофакторная идентификация Azure|Azure MFA выбран в качестве дополнительного поставщика проверки подлинности в AD FS и использовался для проверки подлинности.|
|адфсекстерналаусентикатионпровидер|Это поле имеет значение, если поставщик проверки подлинности стороннего поставщика был зарегистрирован и использован для проверки подлинности.|


#### <a name="ad-fs-additional-details"></a>AD FS дополнительные сведения
Для AD FSных входов доступны следующие сведения:
* Server Name (Имя сервера)
* Цепочка IP-адресов
* Протокол

### <a name="enabling-log-analytics-and-azure-monitor"></a>Включение Log Analytics и Azure Monitor
Log Analytics могут быть включены для AD FS входа в систему и могут использоваться с любыми другими Log Analytics интегрированными компонентами, такими как Sentinel.

> [!NOTE] 
> AD FSные входы в систему могут значительно увеличиться Log Analytics затрат в зависимости от объема операций входа, AD FS в Организации. Чтобы включить и отключить Log Analytics, установите флажок для потока.

Чтобы включить Log Analytics для функции, перейдите в колонку Log Analytics и выберите поток "Адфссигнинс". Этот выбор позволит AD FS входы в Log Analytics.

Чтобы получить доступ к обновленному шаблону книги Azure Monitor, перейдите в раздел "шаблоны Azure Monitor" и выберите книгу "входы".
Дополнительные сведения о книгах см. [Azure Monitor книгах](https://aka.ms/adfssigninspreview).




### <a name="frequently-asked-questions"></a>Часто задаваемые вопросы
***Какие типы входа могут быть видны?***
Отчет о входе поддерживает вход в систему с помощью протоколов O-AUTH, WS-подача, SAML и WS-Trust. 

***Как в отчете о входе отображаются различные типы входов?***
При выполнении простого входа единого входа в систему будет одна строка для входа с одним ИДЕНТИФИКАТОРом корреляции.
Если выполняется Однофакторная проверка подлинности, две строки будут заполнены одним и тем же ИДЕНТИФИКАТОРом корреляции, но с двумя разными методами проверки подлинности (например, Forms, SSO).
В случае многофакторной проверки подлинности будут содержаться три строки с общим ИДЕНТИФИКАТОРом корреляции и три соответствующих метода проверки подлинности (например, Forms, Азуремфа, многофакторный). В этом конкретном примере многофакторность в этом случае показывает, что для единого входа используется MFA.

***Какие ошибки можно увидеть в отчете?***
Полный список AD FS, связанных с ошибками, которые заполняются в отчете и описаниях Sign-In, см. в [справочнике по коду ошибки справки AD FS](https://adfshelp.microsoft.com/References/ConnectHealthErrorCodeReference) .

***Я вижу «00000000-0000-0000-0000-000000000000» в разделе «User» входа в систему. Что это значит?***
Если вход не выполнен и предпринятое имя участника-пользователя не совпадает с существующим именем участника-пользователя, поля "пользователь", "имя пользователя" и "идентификатор пользователя" будут иметь значение "00000000-0000-0000-0000-000000000000", а "идентификатор входа" будет заполнен попыткой ввода значения пользователем. В таких случаях пользователь, пытающийся выполнить вход, не существует.

***Как сопоставить локальные события с отчетом о входе в Azure AD?***
Агент Azure AD Connect Health для AD FS сопоставляет идентификаторы событий из AD FS, зависящих от версии сервера. События будут доступны в журнале безопасности серверов AD FS. 

***Почему я вижу NotSet или применимо в ИДЕНТИФИКАТОРе или имени приложения для некоторых AD FS входа?***
В отчете AD FS Sign-In отобразятся идентификаторы OAuth в поле Application ID (идентификатор приложения) для входа в OAuth. В сценариях входа WS-подач WS-Trust идентификатор приложения будет равен NotSet или применимо, а идентификаторы ресурсов и проверяющие стороны будут присутствовать в поле "идентификатор ресурса".

***Существуют ли другие известные проблемы с отчетом в предварительной версии?***
В отчете есть известная ошибка, из-за которой поле "требование к проверке подлинности" на вкладке "Основная информация" будет заполнено как значение однофакторной проверки подлинности для AD FSных входов, независимо от входа. Кроме того, на вкладке сведения о проверке подлинности в поле требование отображается «первичная или вторичная», где выполняется исправление для различения первичных или вторичных типов проверки подлинности.


## <a name="related-links"></a>Связанные ссылки
* [Azure AD Connect Health](./whatis-azure-ad-connect.md)
* [Установка агента Azure AD Connect Health](how-to-connect-health-agent-install.md)
* [Отчет о рискованных IP-адресах](how-to-connect-health-adfs-risky-ip.md)





