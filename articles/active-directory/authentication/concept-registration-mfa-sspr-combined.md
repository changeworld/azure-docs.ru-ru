---
title: Объединенная регистрация для SSPR и многофакторной идентификации Azure AD — Azure Active Directory
description: Узнайте о объединенном интерфейсе регистрации для Azure Active Directory, чтобы разрешить пользователям регистрироваться как для многофакторной идентификации Azure AD, так и для самостоятельного сброса пароля.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 112ad0714c84cd3be08788b3277f52372f6d0373
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98938474"
---
# <a name="combined-security-information-registration-for-azure-active-directory-overview"></a>Общие сведения о общей регистрации безопасности для Azure Active Directory

Перед Объединенной регистрацией пользователи регистрировали методы проверки подлинности для многофакторной идентификации Azure AD и самостоятельного сброса пароля (SSPR) отдельно. Пользователи путают, что аналогичные методы использовались для многофакторной проверки подлинности и SSPR, но им пришлось зарегистрироваться для использования обеих функций. Теперь, используя объединенную регистрацию, пользователи могут зарегистрироваться один раз и воспользоваться преимуществами многофакторной проверки подлинности и SSPR.

> [!NOTE]
> Начиная с 15 августа 2020, все новые клиенты Azure AD будут автоматически включены для совместной регистрации. 

В этой статье описывается Объединенная регистрация безопасности. Чтобы приступить к работе с Объединенной регистрацией безопасности, см. следующую статью:

> [!div class="nextstepaction"]
> [Включить объединенную регистрацию безопасности](howto-registration-mfa-sspr-combined.md)

![Моя учетная запись, показывающая зарегистрированные сведения о безопасности для пользователя](media/concept-registration-mfa-sspr-combined/combined-security-info-defaults-registered.png)

Перед включением нового интерфейса ознакомьтесь с документацией, ориентированной на администраторов, и документацией, ориентированной на пользователей, чтобы убедиться в том, что вы понимаете функциональность и воздействие этой функции. Ознакомьтесь с [пользовательской документацией](../user-help/security-info-setup-signin.md) , чтобы подготовить пользователей к новым интерфейсам и обеспечить успешный выпуск.

Объединенная регистрация сведений о безопасности в Azure AD сейчас недоступна для национальных облаков, таких как Azure для Германии или Azure для Китая (21Vianet). Она доступна для государственных организаций США Azure.

> [!IMPORTANT]
> Пользователи, для которых включена первоначальная Предварительная версия и улучшенная Объединенная процедура регистрации, видят новое поведение. Пользователи, которые имеют доступ к обеим средам, видят только учетную запись My. *Моя учетная запись* соответствует внешнему интерфейсу регистрации и обеспечивает удобство работы пользователей. Пользователи могут видеть мою учетную запись, перейдя по [https://myaccount.microsoft.com](https://myaccount.microsoft.com) .
>
> При попытке получить доступ к параметру сведений о безопасности может появиться сообщение об ошибке, например: "не удается войти в систему". Убедитесь, что у вас нет объектов конфигурации или групповых политик, блокирующих сторонние файлы cookie в веб-браузере.

Страницы *моей учетной записи* локализуются на основе языковых параметров компьютера, обращающегося к странице. Корпорация Майкрософт сохраняет самый последний язык, используемый в кэше браузера, поэтому последующие попытки доступа к страницам продолжают отображаться на последнем используемом языке. Если очистить кэш, страницы будут повторно отображены.

Если требуется принудительно применить конкретный язык, можно добавить `?lng=<language>` в конец URL-адреса, где `<language>` — это код языка, который требуется отобразить.

![Настройка SSPR или других методов проверки безопасности](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Методы, доступные в Объединенной регистрации

Объединенная регистрация поддерживает следующие методы и действия проверки подлинности:

| Метод | Зарегистрировать | Change | Удалить |
| --- | --- | --- | --- |
| Microsoft Authenticator | Да (максимум 5) | Нет | Да |
| Другое приложение для проверки подлинности | Да (максимум 5) | Нет | Да |
| Аппаратный токен | Нет | Нет | Да |
| Телефон | Да | Да | Да |
| Дополнительный телефон | Да | Да | Да |
| Рабочий телефон | Да | Да | Да |
| электронная почта; | Да | Да | Да |
| Контрольные вопросы | Да | Нет | Да |
| Пароли приложений | Да | Нет | Да |
| Ключи безопасности FIDO2<br />*Управляемый режим только на странице [сведений о безопасности](https://mysignins.microsoft.com/security-info)*| Да | Да | Да |

> [!NOTE]
> Пароли приложений доступны только пользователям, которые были применены для многофакторной проверки подлинности. Пароли приложений недоступны пользователям, для которых включена многофакторная проверка подлинности через политику условного доступа.

Пользователи могут задать один из следующих параметров в качестве метода многофакторной проверки подлинности по умолчанию:

- Microsoft Authenticator — уведомление.
- Приложение для проверки подлинности или маркер оборудования — код.
- Телефонный звонок.
- Текстовое сообщение.

По мере продолжения добавления методов проверки подлинности в Azure AD эти методы доступны в Объединенной регистрации.

## <a name="combined-registration-modes"></a>Объединенные режимы регистрации

Существует два режима Объединенной регистрации: прерывание и управление.

- **Режим прерывания** — это интерфейс, аналогичный мастеру, представленный пользователям при регистрации или обновлении сведений о безопасности при входе.
- **Режим управления** является частью профиля пользователя и позволяет пользователям управлять сведениями для обеспечения безопасности.

В обоих режимах пользователи, которые ранее зарегистрировали метод, который можно использовать для многофакторной проверки подлинности, должны выполнить многофакторную проверку подлинности, прежде чем они смогут получить доступ к сведениям о безопасности. Пользователи должны подтвердить свои сведения, прежде чем продолжать использовать ранее зарегистрированные методы. 

### <a name="interrupt-mode"></a>Режим прерывания

Объединенная регистрация учитывает как многофакторную проверку подлинности, так и политики SSPR, если для клиента включены оба параметра. Эти политики определяют, был ли пользователь прерван для регистрации во время входа в систему и какие методы доступны для регистрации.

Ниже приведены примеры сценариев, в которых пользователям может быть предложено зарегистрировать или обновить сведения о безопасности.

- *Регистрация многофакторной проверки подлинности обеспечивается с помощью защиты идентификации:* Пользователям предлагается зарегистрироваться во время входа. Они регистрируют методы многофакторной проверки подлинности и методы SSPR (если пользователь включен для SSPR).
- *Регистрация многофакторной проверки подлинности обеспечивается с помощью многофакторной проверки подлинности для каждого пользователя:* Пользователям предлагается зарегистрироваться во время входа. Они регистрируют методы многофакторной проверки подлинности и методы SSPR (если пользователь включен для SSPR).
- *Регистрация многофакторной проверки подлинности обеспечивается с помощью условного доступа или других политик.* Пользователям предлагается зарегистрироваться, когда они используют ресурс, для которого требуется многофакторная проверка подлинности. Они регистрируют методы многофакторной проверки подлинности и методы SSPR (если пользователь включен для SSPR).
- *Регистрация SSPR применяется:* Пользователям предлагается зарегистрироваться во время входа. Они регистрируют только методы SSPR.
- *Принудительное обновление SSPR:* Пользователям необходимо просматривать сведения о безопасности через интервал, заданный администратором. Пользователи показывают свои сведения и могут подтвердить текущие сведения или внести изменения, если это необходимо.

При применении регистрации пользователям отображается минимальное число методов, необходимых для обеспечения совместимости с многофакторной проверкой подлинности и политиками SSPR, от большинства до наименьших безопасности.

Рассмотрим следующий пример сценария:

- Пользователь включен для SSPR. Политика SSPR требовала два метода сброса и включила код мобильного приложения, адрес электронной почты и телефон.
- Этот пользователь должен зарегистрировать два метода.
   - По умолчанию пользователь отображается как приложение проверки подлинности и телефон.
   - Пользователь может зарегистрировать электронную почту вместо приложения или телефона средства проверки подлинности.

Следующая блок-схема описывает, какие методы отображаются пользователю при прерывании регистрации во время входа в систему.

![Общая блок-схема сведений о безопасности](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Если у вас есть многофакторная проверка подлинности и SSPR, рекомендуется включить регистрацию многофакторной проверки подлинности.

Если политика SSPR требует, чтобы пользователи Просмотрели сведения о безопасности через регулярные промежутки времени, пользователи прерываются во время входа в систему и отображаются все зарегистрированные методы. Они могут подтвердить текущие сведения, если они актуальны, или же они могут вносить изменения, если это необходимо. При доступе к этой странице пользователи должны выполнить многофакторную проверку подлинности.

### <a name="manage-mode"></a>Режим управления

Пользователи могут получить доступ к режиму управления, перейдя к [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) или выбрав **сведения о безопасности** из моей учетной записи. После этого пользователи могут добавлять методы, удалять или изменять существующие методы, изменять метод по умолчанию и многое другое.

## <a name="key-usage-scenarios"></a>Сценарии использования ключей

### <a name="set-up-security-info-during-sign-in"></a>Настройка сведений для защиты при входе в систему

Администратор выполнил принудительную регистрацию.

Пользователь не установил все необходимые сведения о безопасности и перейдет к портал Azure. После ввода имени пользователя и пароля пользователю будет предложено настроить сведения для защиты учетной записи. Затем пользователь выполняет действия, описанные в мастере, чтобы настроить необходимые сведения для защиты учетной записи. Если параметры разрешены, пользователь может настроить методы, отличные от показанных по умолчанию. После завершения работы мастера пользователи просматривают методы, которые они настроили, и их метод по умолчанию для многофакторной проверки подлинности. Чтобы завершить процесс установки, пользователь подтверждает сведения и переходит к портал Azure.

### <a name="set-up-security-info-from-my-account"></a>Настройка сведений о безопасности из моей учетной записи

Администратор не выполнил принудительную регистрацию.

Пользователь, который еще не настроил все необходимые сведения для защиты, перейдет к [https://myaccount.microsoft.com](https://myaccount.microsoft.com) . Пользователь выбирает **сведения о безопасности** в левой области. После этого пользователь выбирает Добавление метода, выбирает любой из доступных методов и выполняет действия по его настройке. По завершении пользователь увидит метод, который был настроен на странице "сведения о безопасности".

### <a name="delete-security-info-from-my-account"></a>Удалить сведения о безопасности из моей учетной записи

Пользователь, который ранее настроил по крайней мере один метод, переходит к [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Пользователь выбирает удаление одного из ранее зарегистрированных методов. По завершении пользователь больше не видит этот метод на странице сведений о безопасности.

### <a name="change-the-default-method-from-my-account"></a>Изменение метода по умолчанию в моей учетной записи

Пользователь, который ранее настроил по крайней мере один метод, который можно использовать для многофакторной проверки подлинности, переходит к [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Пользователь изменяет текущий метод по умолчанию на другой метод по умолчанию. По завершении пользователь увидит новый метод по умолчанию на странице сведений о безопасности.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе, см. руководства по [включению самостоятельного сброса пароля](tutorial-enable-sspr.md) и [включению многофакторной идентификации Azure AD](tutorial-enable-azure-mfa.md).

Узнайте, как [включить объединенную регистрацию в клиенте](howto-registration-mfa-sspr-combined.md) или [заставить пользователей повторно зарегистрировать методы проверки подлинности](howto-mfa-userdevicesettings.md#manage-user-authentication-options).

Вы также можете ознакомиться с [доступными методами многофакторной идентификации Azure AD и SSPR](concept-authentication-methods.md).
