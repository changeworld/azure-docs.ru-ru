---
title: Типы приложений в версии 1.0 | Azure
description: В этой статье описываются типы приложений и сценарии, поддерживаемые конечной точкой Azure Active Directory версии 2.0.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 5ff2858dd8b91ba036c517cbff07be96a729ef8c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "88116450"
---
# <a name="application-types-in-v10"></a>Типы приложений в версии 1.0

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD) поддерживает проверку подлинности для различных современных архитектур приложений, основанных на стандартных отраслевых протоколах OAuth 2.0 или OpenID Connect.

На следующей схеме показаны эти сценарии и типы приложений, а также способы добавления различных компонентов:

![Типы приложений и сценарии](./media/authentication-scenarios/application-types-scenarios.png)

Ниже приведены пять основных сценариев приложений, которые поддерживает система Azure AD:

- **[Одностраничное приложение (SPA)](single-page-application.md)**: пользователю необходимо войти в одностраничное приложение, защищенное с помощью Azure AD.
- **[Веб-браузер в веб-приложение](web-app.md)**: пользователю необходимо войти в веб-приложение, защищенное с помощью Azure AD.
- **[Собственное приложение для веб-API](native-app.md)**. собственное приложение, работающее на телефоне, планшете или ПК, должно пройти проверку подлинности пользователя для получения ресурсов из веб-API, защищенного Azure AD.
- Из **[веб-приложения в веб-интерфейс API](web-api.md)**: веб-приложению необходимо получить ресурсы из веб-API, защищенного с помощью Azure AD.
- **[Управляющая программа или серверное приложение для веб-API](service-to-service.md)**: управляющее приложение или серверное приложение без пользовательского веб-интерфейса должны получать ресурсы из веб-API, защищенного с помощью Azure AD.

Прежде чем начать работу с кодом, воспользуйтесь этими ссылками, чтобы подробнее изучить каждый тип приложения и ознакомиться с общими сценариями. Вы также узнаете о различиях, которые нужно учитывать при написании конкретного приложения, работающего с конечной точкой версии 1.0 или 2.0.

> [!NOTE]
> Конечная точка версии 2.0 поддерживает не все сценарии и возможности Azure AD. Чтобы определить, стоит ли вам использовать конечную точку версии 2.0, ознакомьтесь с [ограничениями версии 2.0](./azure-ad-endpoint-comparison.md?bc=%2fazure%2factive-directory%2fazuread-dev%2fbreadcrumb%2ftoc.json&toc=%2fazure%2factive-directory%2fazuread-dev%2ftoc.json).

Вы можете разрабатывать любые приложения и сценарии, описанные здесь, используя различные языки и платформы. Все сценарии снабжены полными примерами кода, доступными в соответствующих руководствах: [примеры кода для сценариев с использованием версии 1.0](sample-v1-code.md) и [примеры кода для сценариев с использованием версии 2.0](../develop/sample-v2-code.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json). Примеры кода также можно скачать напрямую из соответствующих [репозиториев с примерами на сайте GitHub](https://github.com/Azure-Samples?q=active-directory).

Кроме того, если вашему приложению требуется конкретная часть или сегмент комплексного сценария, то в большинстве случаев эти функции можно добавлять независимым образом. Например, если имеется нативное приложение, которое вызывает веб-интерфейс API, можно легко добавить веб-приложение, которое также будет вызывать такой интерфейс.

## <a name="app-registration"></a>Регистрация приложений

### <a name="registering-an-app-that-uses-the-azure-ad-v10-endpoint"></a>Регистрация приложения, в котором используется конечная точку Azure AD версии 1.0

Любое приложение, передающее функцию проверку подлинности системе Azure AD, должно быть зарегистрировано в каталоге. Этот шаг включает указание Azure AD о приложении, включая URL-адрес, где он расположен, URL-адрес для отправки ответов после проверки подлинности, универсальный код ресурса (URI) для идентификации приложения и многое другое. Эта информация необходима по нескольким основным причинам:

* Службе Azure AD необходимо взаимодействовать с приложением при выполнении входа в систему или при обмене маркерами. Данные, передаваемые между Azure AD и приложением, включают в себя следующее:
  
  * **URI идентификатора приложения** — идентификатор приложения. Это значение отправляется в Azure AD во время проверки подлинности и указывает, для какого приложения вызывающая сторона желает получить маркер. Кроме того, это значение включается в маркер, чтобы приложение знало, что именно это значение запрашивалось.
  * **URL-адрес ответа** и **URI перенаправления** — для веб-API или веб-приложения URL-адрес ответа указывает то место, куда Azure AD будет отправлять ответ после аутентификации, в том числе маркер в случае успешной проверки подлинности. Для собственного приложения URI перенаправление — это уникальный идентификатор, на который Azure AD будет перенаправлять агент пользователя при запросе OAuth 2.0.
  * **Идентификатор приложения** — идентификатор для приложения, который создается при регистрации приложения в Azure AD. При запросе кода авторизации или маркера идентификатор приложения и ключ отправляются в Azure AD во время проверки подлинности.
  * **Ключ** — ключ, который отправляется вместе с идентификатором приложения при проверке подлинности в Azure AD для вызова веб-API.
* Службе Azure AD необходимо убедиться, что приложение содержит необходимые разрешения для доступа к данным вашего каталога, другим приложениям в вашей организации и т. д.

Дополнительные сведения см. в статье о [регистрации приложения](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="single-tenant-and-multi-tenant-apps"></a>Однотенантные и мультитенантные приложения

Процесс подготовки станет яснее, когда вы поймете, что существует две категории приложений, которые можно разрабатывать и интегрировать с Azure AD:

* **Однотенантное приложение** —такое приложение (для одного клиента) предназначено для использования в одной организации. Обычно это бизнес-приложения (LoB), которые создает разработчик компании. Однотенантное приложение предоставляет пользователям доступ только в одном каталоге. В результате, его достаточно подготовить в одном каталоге. Такие приложения обычно регистрируются разработчиком в организации.
* **Мультитенантное приложение** — такое приложение (для нескольких клиентов) предназначено для использования в нескольких организациях. Как правило, это приложения категории "программное обеспечение как услуга" (SaaS), которые создают независимые поставщики программных продуктов (ISV). Мультитенантные приложения необходимо подготавливать в каждом каталоге, где они будут использоваться. Следовательно, для их регистрации требуется предоставление разрешений пользователем или администратором. Процесс предоставление разрешений начинается, когда приложение регистрируется в каталоге и получает доступ к API-интерфейсу Graph или, возможно, к другому веб-интерфейсу API. Когда пользователь или администратор из другой организации регистрируется для использования приложения, для них появляется диалоговое окно, в котором отображаются разрешения, необходимые для приложения. Пользователь или администратор может предоставить приложению требуемые разрешения, что дает приложению доступ к необходимым данным и регистрирует приложение в их каталоге. Дополнительные сведения см. в разделе [Обзор платформы согласия](../develop/consent-framework.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

### <a name="additional-considerations-when-developing-single-tenant-or-multi-tenant-apps"></a>Дополнительные рекомендации при разработке однотенантных или мультитенантных приложений

При разработке мультитенантного приложения вместо однотенантного необходимо учесть некоторые дополнительные соображения. Например, если приложение становится доступным для пользователей в нескольких каталогах, необходим механизм для определения того, в каком клиенте они находятся. Однотенантное приложение должно присутствовать только в собственном каталоге для данного пользователя, тогда как мультитенантное приложение должно идентифицировать конкретного пользователя из любого каталога в Azure AD. Для выполнения этой задачи система Azure AD предоставляет общую конечную точку проверки подлинности, в которую мультитенантное приложение может направлять запросы на вход, вместо конечной точки для конкретного клиента. Для всех каталогов в Azure AD эта конечная точка — `https://login.microsoftonline.com/common`, тогда как конечной точкой для конкретного клиента может быть `https://login.microsoftonline.com/contoso.onmicrosoft.com`. Общая конечная точка особенно важна при разработке приложения, поскольку вам потребуется логика для обработки нескольких клиентов во время входа, выхода и проверки маркеров.

Если вы разрабатываете однотенантное приложение, но хотите сделать его доступным для многих организаций, вы можете легко внести изменения в само приложение и его конфигурацию в системе Azure AD и переделать его на мультитенантное. Кроме того, Azure AD использует один и тот же ключ подписи для всех маркеров во всех каталогах, независимо от того, выполняется ли проверка подлинности в однотенантном или мультитенантном приложении.

Каждый сценарий, рассматриваемый в этом документе, содержит подраздел, где описываются соответствующие требования по подготовке. Более подробные сведения о подготовке приложения в Azure AD и различиях между приложениями с одним и несколькими клиентами см. в статье [Интеграция приложений с Azure Active Directory](../develop/single-and-multi-tenant-apps.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) для получения дополнительных сведений. Продолжим рассмотрение типичных сценариев работы приложений в Azure AD.

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь с [основными сведениями о проверке подлинности](v1-authentication-scenarios.md) в Azure AD.