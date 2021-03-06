---
title: Новые возможности агента серверов с поддержкой ARC в Azure
description: В этой статье содержатся заметки о выпуске агента серверов с поддержкой ARC в Azure. Для многих из обобщенных проблем имеются ссылки на дополнительные сведения.
ms.topic: conceptual
ms.date: 03/15/2021
ms.openlocfilehash: acf606ed1ad0f54c983b14a0141d0dc11e2c45d9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103470512"
---
# <a name="whats-new-with-azure-arc-enabled-servers-agent"></a>Новые возможности агента серверов с поддержкой ARC в Azure

Агент подключенного компьютера с поддержкой Arc Azure получает усовершенствования на постоянной основе. Чтобы вы оставались в курсе последних разработок, в этой статье предоставлены такие сведения:

- Последние выпуски.
- Известные проблемы
- Исправления ошибок

## <a name="march-2021"></a>Март 2021 г.

Версия 1,4

## <a name="new-feature"></a>Новая функция

- Добавлена поддержка частных конечных точек.
- Расширенный список кодов выхода для азкмажент.
- Теперь параметры конфигурации агента можно считывать из файла с параметром--config.

## <a name="fixed"></a>Фиксированный

Проверки конечной точки сети теперь выполняются быстрее.

## <a name="december-2020"></a>Декабрь 2020 г.

Версия: 1,3

### <a name="new-feature"></a>Новая функция

Добавлена поддержка Windows Server 2008 R2.

### <a name="fixed"></a>Фиксированный

Устранена проблема, препятствующая успешной установке расширения пользовательских скриптов в Linux.

## <a name="november-2020"></a>Ноябрь 2020 г.

Версия: 1,2

### <a name="fixed"></a>Фиксированный

Устранена проблема, при которой конфигурация прокси-сервера может быть потеряна после обновления в дистрибутивах на основе RPM.

## <a name="october-2020"></a>Октябрь 2020 г.

Версия: 1.1

### <a name="fixed"></a>Фиксированный

- Исправлен скрипт прокси-сервера для управления расположением файла другого управляющей программы GC.
- Изменения в надежности агента Гуестконфиг.
- Поддержка агента Гуестконфиг для региона US Gov (Вирджиния).
- Если произошел сбой, сообщения отчета о расширении агента Гуестконфиг будут более подробными.

## <a name="september-2020"></a>Сентябрь 2020 г.

Версия: 1,0 (общая доступность)

### <a name="plan-for-change"></a>план изменений

- Поддержка агентов предварительной версии (все версии старше 1,0) будут удалены в будущем обновлении службы.
- Удалена поддержка резервной конечной точки `.azure-automation.net` . Если у вас есть прокси-сервер, необходимо разрешить конечную точку `*.his.arc.azure.com` .
- Если агент подключенного компьютера установлен на виртуальной машине, размещенной в Azure, расширения виртуальной машины нельзя установить или изменить в ресурсе "серверы с поддержкой Arc". Это необходимо, чтобы избежать конфликтующих операций расширения, выполняемых из ресурсов **Microsoft. COMPUTE** и **Microsoft. хибридкомпуте** виртуальной машины. Используйте ресурс **Microsoft. COMPUTE** для компьютера для всех операций расширения.
- Имя процесса настройки гостевой системы изменилось с *GCD* на *гкад* в Linux и *гксервице* на *гкарксервице* в Windows.

### <a name="new-feature"></a>Новая функция

- Добавлен `azcmagent logs` параметр для получения сведений о поддержке.
- Добавлен `azcmagent license` параметр для вывода лицензионного соглашения.
- Добавлен `azcmagent show --json` параметр для вывода состояния агента в удобном для анализа формате.
- Добавлен флаг в `azcmagent show` выходных данных, указывающий, находится ли сервер на виртуальной машине, размещенной в Azure.
- Добавлен `azcmagent disconnect --force-local-only` параметр, позволяющий сбрасывать состояние локального агента при недоступности службы Azure.
- Добавлен `azcmagent connect --cloud` параметр для поддержки других облаков. В этом выпуске служба поддерживается только в Azure во время выпуска агента.
- Агент переведен на языки, поддерживаемые Azure.

### <a name="fixed"></a>Фиксированный

- Улучшения проверки подключения.
- Исправлены проблемы с параметрами прокси-сервера, которые теряются при обновлении агента в Linux.
- Устранены проблемы при попытке установить агент на сервере под Windows Server 2012 R2.
- Улучшения надежности установки расширения

## <a name="august-2020"></a>Август 2020 г.

Версия: 0,11

- Этот выпуск ранее объявил о поддержке для Ubuntu 20,04. Так как некоторые расширения виртуальной машины Azure не поддерживают Ubuntu 20,04, будет удалена поддержка этой версии Ubuntu.

- Улучшения надежности развертываний расширений.

### <a name="known-issues"></a>Известные проблемы

Если вы используете более раннюю версию агента Linux и она настроена для использования прокси-сервера, необходимо перенастроить параметр прокси-сервера после обновления. Для этого выполните команду `sudo azcmagent_proxy add http://proxyserver.local:83`.

## <a name="next-steps"></a>Дальнейшие действия

Прежде чем оценивать или включать серверы с поддержкой Azure Arc на нескольких гибридных компьютерах, ознакомьтесь со статьей [Общие сведения об агенте серверов с поддержкой Azure Arc](agent-overview.md), где описаны требования, методы развертывания и технические сведения об агенте.