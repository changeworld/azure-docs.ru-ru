---
title: Устранение распространенных ошибок проверки подлинности, Azure Marketplace
description: Предоставляет справку по распространенным ошибкам проверки подлинности при использовании Портал Cloud Partner API в Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 01af5357e4ae2f4dfb317a0931a8d0bc2b2d54e1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "89487323"
---
# <a name="troubleshooting-common-authentication-errors"></a>Устранение распространенных ошибок аутентификации

> [!NOTE]
> Портал Cloud Partner API интегрированы с и будут продолжать работать в центре партнеров. Переход содержит небольшие изменения. Ознакомьтесь с изменениями, приведенными в [справочнике по портал Cloud Partner API](./cloud-partner-portal-api-overview.md) , чтобы убедиться, что код продолжит работать после перехода в центр партнеров. API-интерфейсы CPP следует использовать только для существующих продуктов, которые уже были интегрированы до перехода в центр партнеров. новые продукты должны использовать API-интерфейсы отправки центра партнеров.

Эта статья содержит информацию о распространенных ошибках аутентификации при использовании API-интерфейсов портала Cloud Partner.

## <a name="unauthorized-error"></a>Ошибка авторизации

Если постоянно возникают ошибки `401 unauthorized`, убедитесь, что у вас есть допустимый маркер доступа.  Если еще не сделали это, создайте базовое приложение Azure Active Directory (Azure AD) и субъект-службу как описано в [этой статье](../active-directory/develop/howto-create-service-principal-portal.md). Для проверки доступа используйте приложение или простой запрос HTTP POST.  Вы будете включать идентификатор клиента, идентификатор приложения, идентификатор объекта и секретный ключ для получения маркера доступа.

## <a name="forbidden-error"></a>Ошибка запрета на доступ

Если вы получаете ошибку `403 forbidden`, убедитесь, что на портале Cloud Partner к вашей учетной записи издателя добавлена верная субъект-служба. Чтобы добавить субъект-службу на портал, выполните действия, описанные на странице [Предварительные требования](./cloud-partner-portal-api-prerequisites.md).

Если субъект-служба была добавлена правильно, проверьте все остальные сведения. Обратите внимание на идентификатор объекта, который вводите на портале. На странице регистрации приложения Azure Active Directory существуют два идентификатора объектов, необходимо использовать локальный. Правильное значение можно найти, перейдя на страницу **Регистрация приложений**, а также если щелкнуть имя приложения в разделе **Управляемое приложение в локальном каталоге**. Это приведет к локальным свойствам приложения, где можно найти правильный идентификатор объекта на странице **Свойства**, как показано на рисунке ниже. Кроме того убедитесь, что при добавлении участника-службы и вызова API используется верный идентификатор издателя.
