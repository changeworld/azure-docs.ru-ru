---
title: Azure Service Fabric CLI — контейнер sfctl
description: Сведения о sfctl, интерфейсе командной строки Azure Service Fabric. Содержит список команд для контейнеров.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: f82883b68ab911fb0b89fc117d9a9d77e05a781a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "86245897"
---
# <a name="sfctl-container"></a>Контейнер sfctl
Выполнение связанных с контейнером команд в узле кластера.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| invoke-api | Вызывает API контейнера, развернутого на узле Service Fabric для указанного пакета кода. |
| журналы | Возвращает журналы контейнера, развернутого на узле Service Fabric. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Вызывает API контейнера, развернутого на узле Service Fabric для указанного пакета кода.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --application-id [обязательный параметр] | Идентификатор приложения. <br><br> Обычно это полное имя приложения без указания схемы универсального кода ресурса (URI) "fabric\:". Начиная с версии 6.0, иерархические имена разделяются знаком "\~". Например, если имя приложения — fabric\:/myapp/app1, то в версии 6.0 и более поздних версиях будет использоваться идентификатор приложения myapp\~app1, а в предыдущих версиях — myapp/app1. |
| --code-package-instance-id [обязательный параметр] | Уникальный идентификатор экземпляра пакета кода, развернутого на узле Service Fabric. <br><br> Его можно получить с помощью команды service code-package-list. |
| --code-package-name [обязательный параметр] | Имя пакета кода, указанное в манифесте службы и зарегистрированное для типа приложения в кластере Service Fabric. |
| --container-api-uri-path [обязательный параметр] | URI-адрес интерфейса REST API контейнера (вставьте {ID} вместо имени и идентификатора контейнера). |
| --node-name [обязательный параметр] | Имя узла. |
| --service-manifest-name [обязательный параметр] | Имя манифеста службы, зарегистрированное для типа приложения в кластере Service Fabric. |
| --container-api-body | Текст HTTP-запроса к интерфейсу REST API контейнера. |
| --container-api-content-type | Тип содержимого интерфейса REST API контейнера (значение по умолчанию: application/json). |
| --container-api-http-verb | HTTP-команда для интерфейса REST API контейнера (значение по умолчанию: GET). |
| --timeout -t | Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Повышение уровня детализации журнала для включения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат вывода.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug. |

## <a name="sfctl-container-logs"></a>sfctl container logs
Возвращает журналы контейнера, развернутого на узле Service Fabric.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --application-id [обязательный параметр] | Идентификатор приложения. <br><br> Обычно это полное имя приложения без указания схемы универсального кода ресурса (URI) "fabric\:". Начиная с версии 6.0, иерархические имена разделяются знаком "\~". Например, если имя приложения — fabric\:/myapp/app1, то в версии 6.0 и более поздних версиях будет использоваться идентификатор приложения myapp\~app1, а в предыдущих версиях — myapp/app1. |
| --code-package-instance-id [обязательный параметр] | Идентификатор экземпляра пакета кода, который можно получить с помощью команды service code-package-list. |
| --code-package-name [обязательный параметр] | Имя пакета кода, указанное в манифесте службы и зарегистрированное для типа приложения в кластере Service Fabric. |
| --node-name [обязательный параметр] | Имя узла. |
| --service-manifest-name [обязательный параметр] | Имя манифеста службы, зарегистрированное для типа приложения в кластере Service Fabric. |
| --tail | Число отображаемых строк из конца указанных журналов. Количество по умолчанию — 100. Значение all отображает полные журналы. |
| --timeout -t | Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Повышение уровня детализации журнала для включения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат вывода.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug. |


## <a name="next-steps"></a>Дальнейшие действия
- [Настройте](service-fabric-cli.md) Service Fabric CLI.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](./scripts/sfctl-upgrade-application.md).
