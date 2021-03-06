---
title: Анализ зависимостей в Azure — обнаружение и оценка переносится
description: Описывает, как использовать анализ зависимостей для оценки с помощью функции обнаружения и оценки службы "миграция Azure".
ms.topic: conceptual
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.date: 03/18/2021
ms.openlocfilehash: 184c8099c0e86d8f8744948137b344c732bbf7b8
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104778395"
---
# <a name="dependency-analysis"></a>анализ зависимостей

В этой статье описывается анализ зависимостей в службе "миграция Azure": обнаружение и оценка.

Анализ зависимостей определяет зависимости между обнаруженными локальными серверами. Он предоставляет следующие преимущества:

- Вы можете собирать серверы в группы для оценки, точнее, с большей уверенностью.
- Вы можете определять серверы, которые необходимо перенести вместе. Это особенно полезно, если вы не уверены, какие серверы входят в развертывание приложения, которое вы хотите перенести в Azure.
- Можно определить, используются ли серверы и какие серверы можно списать вместо переноса.
- Анализ зависимостей позволяет убедиться, что ничего не осталось, и таким образом избежать неожиданного простоя после миграции.
- [Ознакомьтесь](common-questions-discovery-assessment.md#what-is-dependency-visualization) с общими вопросами об анализе зависимостей.

## <a name="analysis-types"></a>Типы анализа

Существует два варианта развертывания анализа зависимостей.

**Параметр** | **Сведения** | **Общедоступное облако** | **Azure для государственных организаций**
----  |---- | ----
**Безагентное** | Опрашивает данные с серверов в VMware с помощью API-интерфейсов vSphere.<br/><br/> Устанавливать агенты на серверах не требуется.<br/><br/> Сейчас этот параметр доступен в предварительной версии только для серверов в VMware. | Поддерживается. | Поддерживается.
**Анализ на основе агентов** | Использует [решение сопоставление служб](../azure-monitor/vm/service-map.md) в Azure Monitor, чтобы включить визуализацию и анализ зависимостей.<br/><br/> Агенты необходимо установить на каждом локальном сервере, который необходимо проанализировать. | Поддерживается | Не поддерживается.

## <a name="agentless-analysis"></a>Анализ без агента

Анализ зависимостей без агента работает путем записи данных о соединении TCP с серверов, для которых он включен. На серверах не установлены агенты. Соединения с одним и тем же исходным сервером и процессом, а также целевым сервером, процессом и портом группируются логически в зависимость. Захваченные данные зависимостей можно визуализировать в представлении карт или экспортировать в виде CSV-файла. На серверах, которые вы хотите проанализировать, не установлены агенты.

### <a name="dependency-data"></a>Данные зависимостей

После начала обнаружения данных зависимостей начинается опрос:

- Устройство для миграции Azure опрашивает данные TCP-подключения с серверов каждые пять минут для сбора данных.
- Данные собираются с гостевых серверов через vCenter Server с помощью API-интерфейсов vSphere.
- Опрос собирает эти данные:

    - Имя процессов, имеющих активные соединения.
    - Имя приложения, выполняющего процессы, имеющие активные соединения.
    - Порт назначения для активных подключений.

- Собранные данные обрабатываются на устройстве Azure Migrate, чтобы вывести сведения об удостоверениях и отправлять их в службу "миграция Azure каждые шесть часов".


## <a name="agent-based-analysis"></a>Анализ на основе агентов

Для анализа на основе агентов служба "миграция Azure" использует решение [сопоставление служб](../azure-monitor/vm/service-map.md) в Azure Monitor. Установите [агент Microsoft Monitoring Agent/log Analytics](../azure-monitor/agents/agents-overview.md#log-analytics-agent) и [Агент зависимостей](../azure-monitor/agents/agents-overview.md#dependency-agent)на каждом сервере, который необходимо проанализировать.

### <a name="dependency-data"></a>Данные зависимостей

Анализ на основе агентов предоставляет следующие данные:

- Имя исходного сервера, процесс, имя приложения.
- Имя целевого сервера, процесс, имя приложения и порт.
- Количество соединений, задержка и сведения о передачи данных собираются и доступны для запросов Log Analytics.

## <a name="compare-agentless-and-agent-based"></a>Сравнение без агента и на основе агентов

Различия между визуализацией без агента и визуализацией на основе агентов приведены в таблице.

**Требование** | **Безагентное** | **На основе агентов**
--- | --- | ---
**Поддержка** | В режиме предварительной версии только для серверов на VMware. [Ознакомьтесь](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) с поддерживаемыми операционными системами. | В общем доступе (GA).
**Агент** | На серверах, которые вы хотите проанализировать, не требуются агенты. | Агенты необходимы на каждом локальном сервере, который необходимо проанализировать.
**Служба Log Analytics** | Не требуется. | Служба "миграция Azure" использует решение [сопоставление служб](../azure-monitor/vm/service-map.md) в [журналах Azure Monitor](../azure-monitor/logs/log-query-overview.md) для анализа зависимостей.<br/><br/> Вы связываете рабочую область Log Analytics с проектом. Рабочая область должна находиться в следующих регионах: Восточная часть США, Юго-Восточная Азия или Западная Европа. Рабочая область должна находиться в регионе, в котором [поддерживается Сопоставление служб](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).
**Процесс** | Захватывает данные подключения TCP. После обнаружения он собирает данные через пять минут. | Сопоставление служб агенты, установленные на сервере, собирают данные о TCP-процессах, а входящие и исходящие подключения для каждого процесса.
**Data** | Имя исходного сервера, процесс, имя приложения.<br/><br/> Имя целевого сервера, процесс, имя приложения и порт. | Имя исходного сервера, процесс, имя приложения.<br/><br/> Имя целевого сервера, процесс, имя приложения и порт.<br/><br/> Количество соединений, задержка и сведения о передачи данных собираются и доступны для запросов Log Analytics. 
**Визуализация** | Карту зависимостей отдельного сервера можно просмотреть в течение одного часа до 30 дней. | Схема зависимостей одного сервера.<br/><br/> Схема зависимостей группы серверов.<br/><br/>  Карту можно просматривать только в течение часа.<br/><br/> Добавление и удаление серверов в группе из представления карт.
Экспорт данных | Данные за последние 30 дней можно скачать в формате CSV. | Данные можно запрашивать с помощью Log Analytics.



## <a name="next-steps"></a>Следующие шаги

- [Настройка](how-to-create-group-machine-dependencies.md) визуализации зависимостей на основе агента.
- [Испытайте](how-to-create-group-machine-dependencies-agentless.md) визуализацию зависимостей без агента для серверов в VMware.
- Ознакомьтесь с [общими вопросами](common-questions-discovery-assessment.md#what-is-dependency-visualization) о визуализации зависимостей.
