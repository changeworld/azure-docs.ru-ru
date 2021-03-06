---
title: О соединителях API в потоках самостоятельной регистрации в Azure AD
description: Используйте соединители API Azure Active Directory (Azure AD) для настройки и расширения возможностей самостоятельной регистрации пользователей с помощью веб-API.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a5563ff1f57f6b3684834a2488fc0665ac5eddd
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102610048"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Использование соединителей API для настройки и расширения самостоятельной регистрации 

## <a name="overview"></a>Обзор 
Как разработчик или ИТ-администратор, вы можете использовать соединители API для интеграции [пользователя регистрации самообслуживания](self-service-sign-up-overview.md) с веб-API для настройки процесса регистрации и интеграции с внешними системами. Например, с помощью соединителей API можно:

- [**Интеграция с настраиваемым рабочим процессом утверждения**](self-service-sign-up-add-approvals.md). Подключитесь к пользовательской системе утверждения для управления и ограничения создания учетной записи.
- [**Выполните проверку личности**](code-samples-self-service-sign-up.md#identity-verification). Используйте службу проверки личности, чтобы добавить дополнительный уровень безопасности для принятия решений о создании учетных записей.
- **Проверять входные данные пользователя.** Проверка на соответствие некорректным или недопустимым данным пользователя. Например, можно проверить данные, предоставленные пользователем, по существующим данным во внешнем хранилище данных или в списке разрешенных значений. Если значение недопустимо, можно попросить пользователя предоставить допустимые данные или запретить пользователю продолжать процесс регистрации.
- **Перезаписать атрибуты пользователя**. Переформатируйте или присвойте значение атрибуту, полученному от пользователя. Например, если пользователь вводит имя только строчными или заглавными буквами, вы можете изменить его формат, оставив прописной только первую букву. 
- **Запускать пользовательскую бизнес-логику**. Вы можете активировать нисходящие события в облачных системах для отправки push-уведомлений, обновления корпоративных баз данных, управления разрешениями, аудита баз данных и выполнения других настраиваемых действий.

Соединитель API предоставляет Azure Active Directory с информацией, необходимой для вызова конечной точки API, определяя URL-адрес конечной точки HTTP и проверку подлинности для вызова API. После настройки соединителя API его можно включить для определенного шага в потоке пользователя. Когда пользователь достигает этого этапа в потоке регистрации, соединитель API вызывается и представляется как запрос HTTP POST к API, отправляя сведения о пользователе ("утверждения") в качестве пар "ключ-значение" в теле JSON. Ответ API может повлиять на выполнение потока пользователя. Например, ответ API может блокировать регистрацию пользователя, попросите пользователя повторно ввести сведения или перезаписать и добавить атрибуты пользователя.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Где можно включить соединитель API в потоке пользователя

Существует два места в потоке пользователя, где можно включить соединитель API:

- После входа с помощью поставщика удостоверений
- Перед созданием пользователя

> [!IMPORTANT]
> В обоих случаях соединители API вызываются во время **регистрации** пользователя, а не для входа.

### <a name="after-signing-in-with-an-identity-provider"></a>После входа с помощью поставщика удостоверений

Соединитель API на этом этапе в процессе регистрации вызывается сразу после проверки подлинности пользователя с помощью поставщика удостоверений (например, Google, Facebook, & Azure AD). Этот шаг предшествует ***странице Коллекция атрибутов***, которая является формой, представленной пользователю для сбора атрибутов пользователя. Этот шаг не вызывается, если пользователь регистрируется с помощью локальной учетной записи. Ниже приведены примеры сценариев соединителя API, которые можно включить на этом шаге.

- Используйте адрес электронной почты или федеративный идентификатор, предоставленный пользователем для поиска утверждений в существующей системе. Верните эти утверждения из существующей системы, предварительно заполнив страницу коллекции атрибутов и сделав их доступными для возврата в маркер.
- Реализация списка разрешений или блокировок на основе социальных удостоверений.

### <a name="before-creating-the-user"></a>Перед созданием пользователя

Соединитель API на этом этапе в процессе регистрации вызывается после страницы коллекции атрибутов, если она включена. Этот шаг всегда вызывается перед созданием учетной записи пользователя. Ниже приведены примеры сценариев, которые можно включить на этом этапе во время регистрации.

- Проверьте входные данные пользователя и попросите пользователя повторно отправить данные.
- Блокировка регистрации пользователя на основе данных, вводимых пользователем.
- Выполните проверку личности.
- Запросите внешние системы для существующих данных о пользователе, чтобы вернуть их в маркер приложения или сохранить в Azure AD.

## <a name="next-steps"></a>Следующие шаги
- Узнайте, как [Добавить соединитель API в поток пользователя](self-service-sign-up-add-api-connector.md)
- Узнайте, как [добавить пользовательскую систему утверждения для самостоятельной регистрации](self-service-sign-up-add-approvals.md)