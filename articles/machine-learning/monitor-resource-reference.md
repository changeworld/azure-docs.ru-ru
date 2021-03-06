---
title: Справочник по мониторингу Машинное обучение Azure данных | Документация Майкрософт
description: Справочная документация по Машинное обучение Azure мониторинга. Сведения о ресурсах & данных, собранных и доступных в Azure Monitor.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.custom: subject-monitoring
ms.date: 10/02/2020
ms.openlocfilehash: f130fc0c65c49c33c838812fc2758619e0d1bca0
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102521345"
---
# <a name="monitoring-azure-machine-learning-data-reference"></a>Справочник по мониторингу данных машинного обучения Azure

Сведения о данных и ресурсах, собираемых Azure Monitor из рабочей области Машинное обучение Azure. Подробные сведения о сборе и анализе данных мониторинга см. в разделе [мониторинг машинное обучение Azure](monitor-azure-machine-learning.md) .

## <a name="metrics"></a>Метрики

В этом разделе перечислены все автоматически собранные метрики платформы, собранные для Машинное обучение Azure. Поставщик ресурсов для этих метрик — [Microsoft. мачинелеарнингсервицес/workspaces](../azure-monitor/essentials/metrics-supported.md#microsoftmachinelearningservicesworkspaces).

**Модель**

| Метрика | Unit | Описание |
| ----- | ----- | ----- |
| Сбой развертывания модели | Count | Число неудачных развертываний моделей. |
| Развертывание модели начато | Count | Число запущенных развертываний моделей. |
| Развертывание модели прошло | Count | Число удачных развертываний модели. |
| Не удалось зарегистрировать модель | Count | Число неудачных регистраций модели. |
| Регистрация модели прошла | Count | Число удачных регистраций в модели. |

**Квота**

Сведения о квотах используются только для Машинное обучение Azureных вычислений.

| Метрика | Unit | Описание |
| ----- | ----- | ----- |
| Активные ядра | Count | Количество активных ядер вычислений. |
| Активные узлы | Count | Количество активных узлов. |
| Бездействующие ядра | Count | Число бездействующих ядер вычислений. |
| Бездействующие узлы | Count | Число бездействующих узлов вычислений. |
| Освобождение ядер | Count | Число покидает ядер. |
| Покинуть узлы | Count | Количество покидает узлов. |
| Вытесненные ядра | Count | Число ядер с вытеснением. |
| Замещенные узлы | Count | Количество вытесненных узлов. |
| Процент использования квоты | Процент | Процент используемой квоты. |
| Общее число ядер | Count | Общее число ядер. |
| Всего узлов | Count | Общее число узлов. |
| Непригодные для использования ядра | Count | Число неиспользуемых ядер. |
| Неиспользуемые узлы | Count | Число неиспользуемых узлов. |

**Ресурс**

| Метрика | Unit | Описание |
| ----- | ----- | ----- |
| CpuUtilization | Процент | Какой процент использования ЦП использовался для данного узла во время выполнения или задания. Эта метрика публикуется только при выполнении задания на узле. Одно задание может использовать один или несколько узлов. Эта метрика публикуется для каждого узла. |
| гпуутилизатион | Процент | Объем используемой доли GPU для данного узла во время выполнения или задания. Один узел может иметь один или несколько GPU. Эта метрика публикуется для каждого GPU на каждом узле. |

**Выполнить**

Сведения о выполнении обучения.

| Метрика | Unit | Описание |
| ----- | ----- | ----- |
| Завершенные запуски | Count | Число завершенных запусков. |
| Неудачные запуски | Count | Число неудачных выполнений. |
| Запущенные запуски | Count | Число запущенных запусков. |

## <a name="metric-dimensions"></a>Измерения метрик

Дополнительные сведения об измерениях метрик см. в разделе [Многомерные метрики](../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

Машинное обучение Azure имеет следующие измерения, связанные с метриками.

| Измерение | Описание |
| ---- | ---- |
| Имя кластера, | Имя ресурса вычислений кластера. Доступно для всех метрик квот. |
| Имя семейства виртуальных машин | Имя семейства виртуальных машин, используемое кластером. Доступно для использования квоты в процентах. |
| Приоритет виртуальной машины | Приоритет виртуальной машины. Доступно для использования квоты в процентах.
| CreatedTime | Доступно только для график и Гпуутилизатион. |
| DeviceId | ИДЕНТИФИКАТОР устройства (GPU). Доступно только для Гпуутилизатион. |
| NodeId | Идентификатор узла, созданного при выполнении задания. Доступно только для график и Гпуутилизатион. |
| RunId | Идентификатор запуска или задания. Доступно только для график и Гпуутилизатион. |
| ComputeType | Тип вычислений, который использовался при выполнении. Доступны только для завершенных запусков, неудачных запусков и запуска. |
| пипелинестептипе | Тип [пипелинестеп](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinestep) , используемый при выполнении. Доступны только для завершенных запусков, неудачных запусков и запуска. |
| публишедпипелинеид | Идентификатор опубликованного конвейера, используемого при выполнении. Доступны только для завершенных запусков, неудачных запусков и запуска. |
| рунтипе | Тип запуска. Доступны только для завершенных запусков, неудачных запусков и запуска. |

Допустимые значения для измерения Рунтипе:

| Значение | Описание |
| ----- | ----- |
| Эксперимент | Запуски без конвейера. |
| пипелинерун | Запуск конвейера, являющийся родительским для Степрун. |
| степрун | Запуск для этапа конвейера. |
| реуседстепрун | Запуск для этапа конвейера, который повторно использует предыдущий запуск. |

## <a name="activity-log"></a>Журнал действий

В следующей таблице перечислены операции, связанные с Машинное обучение Azure, которые могут быть созданы в журнале действий.

| Операция | Описание |
|:---|:---|
| Создает или обновляет рабочую область Машинное обучение | Рабочая область была создана или обновлена. |
| чекккомпутенамеаваилабилити | Проверить, используется ли имя для вычислений |
| Создает или обновляет ресурсы вычислений. | Ресурс вычислений создан или обновлен. |
| Удаляет ресурсы вычислений. | Ресурс вычислений удален |
| получение списка секретов; | В списке секретных ключей для Машинное обучение рабочей области |

## <a name="resource-logs"></a>Журналы ресурсов

В этом разделе перечислены типы журналов ресурсов, которые можно собираются для Машинное обучение Azure рабочей области.

Поставщик ресурсов и тип: [Microsoft. мачинелеарнингсервицес/Workspace](../azure-monitor/essentials/resource-logs-categories.md#microsoftmachinelearningservicesworkspaces).

| Category | Отображаемое имя |
| ----- | ----- |
| амлкомпутеклустеревент | амлкомпутеклустеревент |
| амлкомпутеклустернодивент | амлкомпутеклустернодивент |
| AmlComputeCpuGpuUtilization | AmlComputeCpuGpuUtilization |
| амлкомпутежобевент | амлкомпутежобевент |
| AmlRunStatusChangedEvent | AmlRunStatusChangedEvent |

## <a name="schemas"></a>Схемы

Следующие схемы используются Машинное обучение Azure

### <a name="amlcomputejobevents-table"></a>Таблица Амлкомпутежобевентс

| Свойство. | Описание |
|:--- |:---|
| TimeGenerated | Время создания записи журнала |
| OperationName | Имя операции, связанной с событием журнала |
| Category | Имя события журнала Амлкомпутеклустернодивент |
| JobId | Идентификатор отправленного задания |
| експериментид | ИДЕНТИФИКАТОР эксперимента |
| експериментнаме | Имя эксперимента |
| CustomerSubscriptionId | SubscriptionId, где был отправлен эксперимент и задание |
| WorkspaceName | Имя рабочей области машинного обучения |
| ClusterName | Имя кластера |
| ProvisioningState | Состояние отправки задания |
| ResourceGroupName | Имя группы ресурсов |
| JobName | Имя задания |
| ClusterId | ИДЕНТИФИКАТОР кластера |
| EventType | Тип события задания. Например, JobSubmitted, Жобруннинг, Жобфаилед, Жобсукцеедед. |
| ExecutionState | Состояние задания (запуск). Например, в очереди, работает, успешно, со сбоем |
| ErrorDetails | Сведения об ошибке задания |
| креатионапиверсион | Версия API, используемая для создания задания |
| клустерресаурцеграупнаме | Имя группы ресурсов кластера |
| тфворкеркаунт | Число рабочих ролей TF |
| тфпараметерсерверкаунт | Число серверов параметров TF |
| тултипе | Тип используемого средства |
| рунинконтаинер | Флаг, описывающий, следует ли запускать задание в контейнере |
| жоберрормессаже | подробное сообщение об ошибке задания |
| NodeId | Идентификатор узла, созданного при выполнении задания |

### <a name="amlcomputeclusterevents-table"></a>Таблица Амлкомпутеклустеревентс

| Свойство. | Описание |
|:--- |:--- |
| TimeGenerated | Время создания записи журнала |
| OperationName | Имя операции, связанной с событием журнала |
| Category | Имя события журнала Амлкомпутеклустернодивент |
| ProvisioningState | Состояние подготовки кластера |
| ClusterName | Имя кластера |
| ClusterType | Тип кластера |
| CreatedBy | Пользователь, создавший кластер |
| CoreCount | Число ядер в кластере |
| VmSize | Размер виртуальной машины кластера |
| вмприорити | Приоритет узлов, созданных в выделенном кластере/Ловприорити |
| скалингтипе | Тип масштабирования кластера вручную или автоматически |
| инитиалнодекаунт | Начальное число узлов в кластере |
| минимумнодекаунт | Минимальное число узлов в кластере |
| максимумнодекаунт | Максимальное число узлов в кластере |
| нодедеаллокатионоптион | Освобождение узла |
| Издатель | Издатель типа кластера |
| ПРЕДЛОЖЕНИЕ | Предложение, с которым создается кластер |
| Sku | Номер SKU узла или виртуальной машины, созданный в кластере |
| Версия | Версия образа, используемая при создании узла или виртуальной машины |
| SubnetId | SubnetId кластера |
| аллокатионстате | Состояние выделения кластера |
| куррентнодекаунт | Текущее число узлов в кластере |
| таржетнодекаунт | Число целевых узлов кластера при масштабировании вверх/вниз |
| EventType | Тип события во время создания кластера. |
| нодеидлетимесекондсбефорескаледовн | Время простоя в секундах до уменьшения масштаба кластера |
| PreemptedNodeCount | Число узлов с вытеснением в кластере |
| исресизегров | Флаг, указывающий, что масштабирование кластера |
| вмфамилинаме | Имя семейства виртуальных машин узлов, которые могут быть созданы в кластере |
| леавингнодекаунт | Освобождение количества узлов в кластере |
| UnusableNodeCount | Неиспользуемое количество узлов кластера |
| IdleNodeCount | Число бездействующих узлов в кластере |
| RunningNodeCount | Число выполняющихся узлов кластера |
| препарингнодекаунт | Подготовка числа узлов кластера |
| куотааллокатед | Выделенная квота для кластера |
| куотаутилизед | Использование квоты кластера |
| аллокатионстатетранситионтиме | Переход к времени из одного состояния в другое |
| клустерерроркодес | Код ошибки, полученный при создании или масштабировании кластера |
| креатионапиверсион | Версия API, используемая при создании кластера |

### <a name="amlcomputeclusternodeevents-table"></a>Таблица Амлкомпутеклустернодивентс

| Свойство. | Описание |
|:--- |:--- |
| TimeGenerated | Время создания записи журнала |
| OperationName | Имя операции, связанной с событием журнала |
| Category | Имя события журнала Амлкомпутеклустернодивент |
| ClusterName | Имя кластера |
| NodeId | Идентификатор созданного узла кластера |
| VmSize | Размер виртуальной машины узла |
| вмфамилинаме | Семейство виртуальных машин, к которому принадлежит узел |
| вмприорити | Приоритет создания выделенного узла/Ловприорити |
| Издатель | Издатель образа виртуальной машины. Например, Microsoft-dsvm |
| ПРЕДЛОЖЕНИЕ | Предложение, связанное с созданием виртуальной машины |
| Sku | Номер SKU созданного узла или виртуальной машины |
| Версия | Версия образа, используемая при создании узла или виртуальной машины |
| клустеркреатионтиме | Время создания кластера |
| ресизестарттиме | Время, когда запускается масштабирование кластера |
| ресизиндтиме | Время, когда заканчивается масштабирование кластера |
| нодеаллокатионтиме | Время выделения узла |
| нодебуттиме | Время загрузки узла |
| старттаскстарттиме | Время, когда задача была назначена узлу и запущена |
| старттаскендтиме | Время завершения задачи, назначенной узлу |
| TotalE2ETimeInSeconds | Общее время активности узла |


## <a name="see-also"></a>См. также раздел

- Описание Машинное обучение Azure мониторинга см. в разделе [monitoring машинное обучение Azure](monitor-azure-machine-learning.md) .
- Подробные сведения о мониторинге ресурсов Azure см. в статье [Мониторинг ресурсов Azure с помощью Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).
