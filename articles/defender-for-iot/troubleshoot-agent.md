---
title: Устранение неполадок при запуске агента безопасности (Linux)
description: Устранение неполадок при работе с защитником Azure для агентов безопасности IoT для Linux.
ms.topic: conceptual
ms.date: 09/09/2020
ms.openlocfilehash: 9c9c36b822ab6acb9f9a48d4ba809ad32f6f4695
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104782594"
---
# <a name="security-agent-troubleshoot-guide-linux"></a>Инструкции по устранению неполадок с агентом безопасности (Linux)

В этой статье объясняется, как решить потенциальные проблемы в процессе запуска агента безопасности.

Самозагрузка агента защитника Azure для IoT запускается сразу после установки. Процесс запуска агента включает в себя чтение локальной конфигурации, подключение к центру Интернета вещей Azure и получение конфигурации удаленного двойника. Сбой одного из этих действий может привести к сбою агента безопасности.

В этом руководство по устранению неполадок вы узнаете, как:

- Проверка работы агента безопасности
- Получить ошибки агента безопасности
- Изучите и исправьте ошибки агента безопасности

## <a name="validate-if-the-security-agent-is-running"></a>Проверка работы агента безопасности

1. Чтобы проверить, работает агент безопасности, подождите несколько минут после установки агента и выполните следующую команду.
     <br>

    **Агент C**

    ```bash
    grep "ASC for IoT Agent initialized" /var/log/syslog
    ```

    **Агент C#**

    ```bash
    grep "Agent is initialized!" /var/log/syslog
    ```

1. Если команда возвращает пустую строку, агенту безопасности не удалось успешно запуститься.

## <a name="force-stop-the-security-agent"></a>Принудительно отключить агент безопасности

В случаях, когда не удается запустить агент безопасности, завершите работу агента с помощью следующей команды, а затем перейдите к таблице ошибок, приведенной ниже.

```bash
systemctl stop ASCIoTAgent.service
```

## <a name="get-security-agent-errors"></a>Получить ошибки агента безопасности

1. Получите ошибки агента безопасности, выполнив следующую команду:

    ```bash
    grep ASCIoTAgent /var/log/syslog
    ```

1. Команда получения ошибки агента безопасности извлекает все журналы, созданные агентом защитника для IoT. Используйте следующую таблицу для анализа ошибок и выполнения правильных действий по исправлению.

> [!Note]
> Журналы ошибок отображаются в хронологическом порядке. Обязательно обратите внимание на метку времени каждой ошибки, чтобы помочь в исправлении.

## <a name="restart-the-agent"></a>Перезапуск агента

1. После обнаружения и исправления ошибки агента безопасности попытайтесь перезапустить агент, выполнив следующую команду.

    ```bash
    systemctl restart ASCIoTAgent.service
    ```

1. Повторите предыдущий процесс, чтобы извлечь ошибки, если агент продолжит процесс запуска.

## <a name="understand-security-agent-errors"></a>Общие сведения об ошибках агента безопасности

Большинство ошибок агента безопасности отображаются в следующем формате:

```
Defender for IoT agent encountered an error! Error in: {Error Code}, reason: {Error sub code}, extra details: {error specific details}
```

| Код ошибки | Код ошибки | Сведения об ошибке | Исправление C | Исправление C # |
|--|--|--|--|--|
| Локальная конфигурация | Отсутствует конфигурация | В локальном файле конфигурации отсутствует конфигурация. Сообщение об ошибке должно указать, какой ключ отсутствует. | Добавление отсутствующего ключа в LocalConfiguration.js/Вар/в файле. Дополнительные сведения см. в [справочнике по CS-локалконфиг-Reference](azure-iot-security-local-configuration-c.md) . | Добавление отсутствующего ключа в файл General.config см. Дополнительные сведения о [c#-локалконфиг-Reference](azure-iot-security-local-configuration-csharp.md) . |
| Локальная конфигурация | Не удается проанализировать конфигурацию | Не удается выполнить синтаксический анализ значения конфигурации. Сообщение об ошибке должно указывать, какой ключ не может быть проанализирован. Синтаксический анализ значения конфигурации невозможен, так как значение не находится в ожидаемом типе, или значение выходит за пределы допустимого диапазона. | Исправьте значение ключа в/Вар/LocalConfiguration.jsв файле, чтобы он соответствовал схеме Локалконфигуратион, см. Дополнительные сведения о [c#-локалконфиг-Reference](azure-iot-security-local-configuration-csharp.md) . | Исправьте значение ключа в файле General.config, чтобы оно совпадало со схемой. Дополнительные сведения см. в разделе [CS-локалконфиг-Reference](azure-iot-security-local-configuration-c.md) . |
| Локальная конфигурация | Формат файла | Не удалось проанализировать файл конфигурации. | Файл конфигурации поврежден, скачайте агент и повторите установку. | - |
| Удаленная конфигурация | Время ожидания | Агенту не удалось получить двойника модуля азуреиотсекурити в течение времени ожидания. | Убедитесь, что конфигурация проверки подлинности правильная, и повторите попытку. | Агенту не удалось получить двойника модуля азуреиотсекурити в течение времени ожидания. Убедитесь, что конфигурация проверки подлинности правильная, и повторите попытку. |
| Аутентификация | Файл не существует | Файл по указанному пути не существует. | Убедитесь, что файл существует по указанному пути, или перейдите к **LocalConfiguration.jsв** файле и измените конфигурацию **FilePath** . | Убедитесь, что файл существует по указанному пути, или перейдите к файлу **Authentication.config** и измените конфигурацию **FilePath** . |
| Аутентификация | Разрешение для файла | У агента недостаточно разрешений для открытия файла. | Предоставьте пользователю **асЦиотажент** разрешения на чтение файла по указанному пути. | Убедитесь, что файл доступен. |
| Аутентификация | Формат файла | Данный файл имеет неправильный формат. | Убедитесь, что файл имеет правильный формат. Поддерживаются следующие типы файлов: PFX и PEM. | Убедитесь, что файл является допустимым файлом сертификата. |
| Аутентификация | Не авторизовано | Агенту не удалось пройти проверку подлинности в центре Интернета вещей с указанными учетными данными. | Проверка конфигурации проверки подлинности в файле Локалконфигуратион. Просмотрите конфигурацию проверки подлинности и убедитесь, что все сведения верны, проверьте, соответствует ли секрет в файле удостоверению, прошедшему проверку подлинности. | Проверьте конфигурацию проверки подлинности в Authentication.config пройдите по конфигурации проверки подлинности и убедитесь, что все сведения верны, а затем проверьте, соответствует ли секрет в файле удостоверению, прошедшему проверку подлинности. |
| Аутентификация | Не найдено | Обнаружено устройство или модуль. | Проверка конфигурации проверки подлинности. Убедитесь, что имя узла указано правильно, что устройство существует в центре Интернета вещей и содержит модуль азуреиотсекурити двойника. | Проверка конфигурации проверки подлинности. Убедитесь, что имя узла указано правильно, что устройство существует в центре Интернета вещей и содержит модуль азуреиотсекурити двойника. |
| Аутентификация | Отсутствует конфигурация | В файле *Authentication.config* отсутствует конфигурация. Сообщение об ошибке должно указать, какой ключ отсутствует. | Добавьте отсутствующий ключ в *LocalConfiguration.jsв* файле. | Добавление отсутствующего ключа в файл *Authentication.config* см. Дополнительные сведения о [c#-локалконфиг-Reference](azure-iot-security-local-configuration-csharp.md) . |
| Аутентификация | Не удается проанализировать конфигурацию | Не удается выполнить синтаксический анализ значения конфигурации. Сообщение об ошибке должно указывать, какой ключ не может быть проанализирован. Не удается выполнить синтаксический анализ значения конфигурации, так как значение не относится к ожидаемому типу, или значение выходит за пределы допустимого диапазона. | Исправьте значение ключа в **LocalConfiguration.js** в файле. | Исправьте значение ключа в файле **Authentication.config** в соответствии со схемой. Дополнительные сведения см. в разделе [CS-локалконфиг-Reference](azure-iot-security-local-configuration-c.md) .|

## <a name="next-steps"></a>Следующие шаги

- Ознакомьтесь с [обзором](overview.md) службы "защитник для Интернета вещей"
- Дополнительные сведения о защитнике для [архитектуры](architecture.md) IOT
- Включение защитника для [службы](quickstart-onboard-iot-hub.md) IOT
- [Вопросы и ответы](resources-frequently-asked-questions.md) о службе "защитник для Интернета вещей"
- Узнайте, как получить доступ к [необработанным данным безопасности](how-to-security-data-access.md).
- Общие сведения о [рекомендациях](concept-recommendations.md)
- Общие сведения об [оповещениях](concept-security-alerts.md) системы безопасности
