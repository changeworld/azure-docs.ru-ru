---
title: Повышение устойчивости разрабатываемых приложений проверки подлинности и авторизации
titleSuffix: Microsoft identity platform
description: Обзор нашего руководства по обеспечению устойчивости для разработки приложений с помощью Azure Active Directory и платформы Microsoft Identity
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: d06e851390537bf94b59e656f84bf58fe7216410
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "96317358"
---
# <a name="increase-resilience-of-authentication-and-authorization-applications-you-develop"></a>Повышение устойчивости разрабатываемых приложений проверки подлинности и авторизации

Microsoft Identity использует современные проверки подлинности и авторизации на основе маркеров. Это означает, что приложение получает маркеры от поставщика удостоверений для проверки подлинности пользователя и авторизации приложения для вызова защищенных интерфейсов API.

Токен действителен в течение определенного промежутка времени, по истечении которого приложение должно получить новое. В редких случаях вызов для получения маркера может завершиться сбоем из-за проблемы, например сбоя сети или инфраструктуры или сбоя службы проверки подлинности. В этом документе описаны шаги, которые разработчик может предпринять для повышения устойчивости в своих приложениях, если происходит сбой при получении маркера.

В этих статьях содержатся рекомендации по повышению устойчивости в приложениях, использующих платформу Microsoft Identity и Azure Active Directory. Существуют рекомендации для клиентских приложений, работающих от имени вошедших в систему пользователей, а также управляющих приложений, работающих от имени пользователя. Они содержат рекомендации по использованию маркеров, а также к вызову ресурсов.

- [Создание устойчивости для приложений, которые входят в систему пользователей](resilience-client-app.md)
- [Создание устойчивости для приложений без пользователей](resilience-daemon-app.md)
- [Отказоустойчивость сборок в инфраструктуре управления удостоверениями и доступом](resilience-in-infrastructure.md)
- [Устойчивость сборок в CIAM Systems](resilience-b2c.md)
