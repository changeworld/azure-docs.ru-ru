---
title: Подходы к миграции пользователей
titleSuffix: Azure AD B2C
description: Перенос учетных записей пользователей из другого поставщика удостоверений в Azure AD B2C с помощью методов предварительной или неполной миграции.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/11/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d2d4a61f653c5bedb31223d2eb3d37b92a076821
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103010173"
---
# <a name="migrate-users-to-azure-ad-b2c"></a>Миграция пользователей в Azure AD B2C

Миграция с другого поставщика удостоверений на Azure Active Directory B2C (Azure AD B2C) также может потребовать миграции существующих учетных записей пользователей. Здесь обсуждаются два метода миграции: *Предварительная миграция* и *Простая миграция*. При использовании любого из этих подходов необходимо написать приложение или сценарий, который использует [Microsoft Graph API](microsoft-graph-operations.md) для создания учетных записей пользователей в Azure AD B2C.

## <a name="pre-migration"></a>Предварительная миграция

В процессе предварительной миграции приложение миграции выполняет следующие действия для каждой учетной записи пользователя:

1. Чтение учетной записи пользователя из старого поставщика удостоверений, включая текущие учетные данные (имя пользователя и пароль).
1. Создайте соответствующую учетную запись в каталоге Azure AD B2C с текущими учетными данными.

Используйте поток перед переносом в любой из этих двух ситуаций:

- У вас есть доступ к учетным данным пользователя в виде открытого текста (имя пользователя и пароль).
- Учетные данные шифруются, но их можно расшифровать.

Сведения о создании учетных записей пользователей программным путем см. [в разделе Управление учетными записями пользователей Azure AD B2C с помощью Microsoft Graph](microsoft-graph-operations.md).

## <a name="seamless-migration"></a>Простая миграция

Используйте простой поток миграции, если недоступность паролей в старом поставщике удостоверений невозможна. Например, такая необходимость может возникнуть в следующих случаях:

- Пароль хранится в одностороннего зашифрованном формате, например в хэш-функции.
- Пароль хранится в устаревшем поставщике удостоверений в недоступном виде. Например, когда поставщик удостоверений проверяет учетные данные путем вызова веб-службы.

Для беспрепятственного переноса по-прежнему требуется предварительная миграция учетных записей пользователей, но затем используется [настраиваемая политика](custom-policy-get-started.md) для запроса [REST API](custom-policy-rest-api-intro.md) (который создается), чтобы задать пароль каждого пользователя при первом входе в систему.

Таким образом, простой поток миграции имеет два этапа: *Предварительная миграция* и *Настройка учетных данных*.

### <a name="phase-1-pre-migration"></a>Этап 1. Предварительная миграция

1. Приложение для миграции считывает учетные записи пользователей из старого поставщика удостоверений.
1. Приложение для миграции создает соответствующие учетные записи пользователей в каталоге Azure AD B2C, но *устанавливает случайные пароли* , которые вы создаете.

### <a name="phase-2-set-credentials"></a>Этап 2. Настройка учетных данных

После выполнения предварительной миграции учетных записей пользовательская политика и REST API при входе пользователя выполняет следующие действия.

1. Считывает учетную запись пользователя Azure AD B2C, соответствующую заданному адресу электронной почты.
1. Проверьте, помечена ли учетная запись для миграции, путем оценки атрибута логического расширения.
    - Если атрибут расширения возвращает `True` , вызовите REST API, чтобы проверить пароль для устаревшего поставщика удостоверений.
      - Если REST API определит неверный пароль, верните пользователю понятную ошибку.
      - Если REST API определяет пароль правильно, запишите пароль в учетную запись Azure AD B2C и измените атрибут логического расширения на `False` .
    - Если атрибут логического расширения возвращает `False` , продолжайте процесс входа как обычная.

Пример пользовательской политики и REST API см. в примере [простой миграции пользователей](https://aka.ms/b2c-account-seamless-migration) на сайте GitHub.

![Блок-схема простого подхода к миграции пользователей](./media/user-migration/diagram-01-seamless-migration.png)<br />*Схема: простой поток миграции*

## <a name="best-practices"></a>Рекомендации

### <a name="security"></a>Безопасность

Простой подход к миграции использует собственный пользовательский REST API для проверки учетных данных пользователя в отношении устаревшего поставщика удостоверений.

**Необходимо защитить REST API от атак методом подбора.** Злоумышленник может отправить несколько паролей в конечном итоге угадать учетные данные пользователя. Чтобы избежать таких атак, прекращает обслуживать запросы к REST API, когда число попыток входа проходит определенное пороговое значение. Кроме того, обеспечьте безопасность обмена данными между Azure AD B2C и REST API. Сведения о защите RESTful API для рабочей среды см. в [этой статье](secure-rest-api.md).

### <a name="user-attributes"></a>Атрибуты пользователя

Не все сведения в устаревшем поставщике удостоверений должны быть перенесены в каталог Azure AD B2C. Укажите соответствующий набор атрибутов пользователя для хранения в Azure AD B2C перед миграцией.

- **Сохранить в** Azure AD B2C
  - Имя пользователя, пароль, адреса электронной почты, Номера телефонов, номера или идентификаторы членства.
  - Маркеры согласия для политики конфиденциальности и лицензионных соглашений для конечных пользователей.
- **Не** храните в Azure AD B2C
  - Конфиденциальные данные, такие как номера кредитных карт, номера социального страхования (SSN), медицинские записи или другие данные, регулируемые правительством или отраслевыми стандартами соответствия стандартам.
  - Параметры маркетинга или связи, поведение пользователей и аналитика.

### <a name="directory-clean-up"></a>Очистка каталога

Прежде чем начать процесс миграции, воспользуйтесь возможностью очистки каталога.

- Укажите набор пользовательских атрибутов, которые будут храниться в Azure AD B2C, и перенесите только необходимые данные. При необходимости можно создать [настраиваемые атрибуты](user-flow-custom-attributes.md) для хранения дополнительных данных о пользователе.
- Если выполняется миграция из среды с несколькими источниками проверки подлинности (например, каждое приложение имеет свой собственный каталог пользователя), выполните миграцию на единую учетную запись в Azure AD B2C.
- Если несколько приложений имеют разные имена пользователей, их можно хранить в Azure AD B2C учетной записи пользователя с помощью коллекции удостоверений. В отношении пароля предоставьте пользователю возможность выбрать один из них и установить его в каталоге. Например, при простой миграции только выбранный пароль должен храниться в учетной записи Azure AD B2C.
- Удалите неиспользуемые учетные записи пользователей перед миграцией или не перенесите устаревшие учетные записи.

### <a name="password-policy"></a>Политика паролей

Если переносимые учетные записи имеют более слабую стойкость пароля, чем [усиленный пароль](../active-directory/authentication/concept-sspr-policy.md) , принудительно установленный Azure AD B2C, можно отключить требование надежного пароля. Дополнительные сведения см. в разделе [свойство политики паролей](user-profile-attributes.md#password-policy-attribute).

## <a name="next-steps"></a>Следующие шаги

В репозитории [Azure-AD-B2C/User-migrations](https://github.com/azure-ad-b2c/user-migration) на сайте GitHub содержится простой пример пользовательской политики миграции и REST API пример кода:

[Простая настраиваемая политика миграции пользователей & REST API пример кода](https://aka.ms/b2c-account-seamless-migration)
