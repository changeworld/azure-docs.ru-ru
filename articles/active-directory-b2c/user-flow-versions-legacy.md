---
title: Предыдущие версии потока пользователей в Azure Active Directory B2C | Документация Майкрософт
description: Сведения о устаревших версиях пользовательских потоков, доступных в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1fbe93c93b5ede2c6b031dab53a1450da473f802
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102517809"
---
# <a name="legacy-user-flow-versions-in-azure-active-directory-b2c"></a>Устаревшие версии потока пользователей в Azure Active Directory B2C

> [!IMPORTANT]
> В этой статье описывается метод устаревшей версии для пользовательских потоков, который предлагает версии v1 (готовые к производству) и версии 1.1 и v2 (Предварительная версия) для потоков пользователей. Среды, отличные от общедоступного облака Azure, будут продолжать использовать этот метод прежних версий. В общедоступном облаке Azure этот метод заменяется [новыми **рекомендуемыми** и **предварительными** версиями](user-flow-versions.md).
> 
Потоки пользователей в Azure Active Directory B2C (Azure AD B2C) помогают настроить общие [политики](user-flow-overview.md) , которые полностью описывают возможности идентификации клиентов. Эти возможности взаимодействия включают в себя вход в систему, регистрацию, сброс пароля и изменение профиля.

В приведенной ниже таблице, если пользовательский поток не определен как **рекомендуемый**, он считается в *предварительной версии*. Для рабочих приложений следует использовать только рекомендованные потоки пользователей.

## <a name="v1"></a>V1

| Поток пользователя | Рекомендуемая | Описание |
| --------- | ----------- | ----------- |
| Сброс паролей | Да | Позволяет пользователю выбрать новый пароль после подтверждения адреса электронной почты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>Параметры маркеров совместимости</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul> |
| Изменение профиля | Да | Позволяет пользователю настроить свои атрибуты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li></ul> |
| Вход с помощью РОПК | Нет | Позволяет пользователю с локальной учетной записью напрямую входить в собственные приложения (браузер не требуется). Используя этот поток пользователя, можно настроить следующее: <ul><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li></ul> |
| Вход | Нет | Позволяет пользователю входить в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>Блокирование входа</li><li>Принудительный сброс пароля</li><li>Возможность оставаться в системе</ul><br>С помощью этого потока пользователя нельзя настроить пользовательский интерфейс. |
| Регистрация | Нет | Позволяет пользователю создать учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul> |
| регистрация и вход | Да | Позволяет пользователю создать учетную запись или войти в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul>|

## <a name="v11"></a>V1.1

| Поток пользователя | Рекомендуемая | Описание |
| --------- | ----------- | ----------- |
| Сброс пароля v 1.1 | Нет | Позволяет пользователю выбрать новый пароль после проверки сообщения электронной почты (новый макет страницы доступен). Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>Параметры маркеров совместимости</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Поток пользователя | Рекомендуемая | Описание |
| --------- | ----------- | ----------- |
| Сброс пароля версии 2 | Нет | Позволяет пользователю выбрать новый пароль после подтверждения адреса электронной почты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>Параметры маркеров совместимости</li><li>[Фильтрация по возрасту](age-gating.md)</li><li>[требования к сложности пароля](password-complexity.md)</li></ul> |
| Редактирование профиля v2 | Да | Позволяет пользователю настроить свои атрибуты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li></ul> |
| Вход в систему версии 2 | Нет | Позволяет пользователю входить в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Фильтрация по возрасту](age-gating.md)</li><li>Страница входа в систему</li></ul> |
| Регистрация в версии 2 | Нет | Позволяет пользователю создать учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Фильтрация по возрасту](age-gating.md)</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul> |
| Регистрация и вход в версии 2 | Нет | Позволяет пользователю создать учетную запись или войти в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](multi-factor-authentication.md)</li><li>[Фильтрация по возрасту](age-gating.md)</li><li>[Требования к сложности пароля](password-complexity.md)</li></ul> |