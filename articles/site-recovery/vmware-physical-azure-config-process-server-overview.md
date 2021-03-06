---
title: Сведения о Azure Site Recovery Configuration/Process/главные целевые серверы
description: В этой статье представлен обзор конфигурации, процессов и главных целевых серверов, которые используются при настройке аварийного восстановления локальных виртуальных машин VMware в Azure с помощью Azure Site Recovery
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: cd5ded18d1a8f1f5fd96212d37725bb5db13002f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "80062089"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Сведения о компонентах Site Recovery (конфигурация, процесс, главный целевой сервер)

В этой статье описываются конфигурации, процессы и главные целевые серверы, используемые службой [Site Recovery](site-recovery-overview.md) для репликации виртуальных машин VMware и физических серверов в Azure.

## <a name="configuration-server"></a>Сервер конфигурации

Для аварийного восстановления локальных виртуальных машин VMware и физических серверов разверните локальный сервер конфигурации Site Recovery.

**Параметр** | **Сведения** | **Ссылки**
--- | --- | ---
**Components**  | Компьютер сервера конфигурации выполняет все локальные Site Recovery компоненты, включая сервер конфигурации, сервер обработки и главный целевой сервер.<br/><br/> При настройке сервера конфигурации все компоненты устанавливаются автоматически. | [Ознакомьтесь](vmware-azure-common-questions.md#configuration-server) с вопросами и ответами на сервер конфигурации.
**Роль** | Сервер конфигурации используется для управления обменом данными между локальным источником и Azure, а также репликацией данных. | Узнайте больше об архитектуре аварийного восстановления [VMware](vmware-azure-architecture.md) и [физического сервера](physical-azure-architecture.md) в Azure.
**Требования к VMware** | Для аварийного восстановления локальных виртуальных машин VMware необходимо установить и запустить сервер конфигурации в качестве локальной, высокодоступной виртуальной машины VMware. | [Дополнительные сведения о](vmware-azure-deploy-configuration-server.md#prerequisites) предварительных требованиях.
**Развертывание VMware** | Рекомендуется развернуть сервер конфигурации с помощью скачанного шаблона OVA. Этот метод предоставляет простой способ настройки сервера конфигурации, который соответствует всем требованиям и необходимым условиям.<br/><br/> Если по какой-либо причине вы не можете развернуть виртуальную машину VMware с помощью шаблона OVA, можно настроить компьютеры сервера конфигурации вручную, как описано ниже, для аварийного восстановления физического компьютера. | [Развертывание](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template) с помощью шаблона OVA.
**Требования для физических серверов** | Для аварийного восстановления на локальных физических серверах необходимо вручную развернуть сервер конфигурации. | [Дополнительные сведения о](physical-azure-set-up-source.md#prerequisites) предварительных требованиях.
**Развертывание физического сервера** | Если ее невозможно установить как виртуальную машину VMware, ее можно установить на физическом сервере. | [Разверните](physical-azure-set-up-source.md#set-up-the-source-environment) сервер конфигурации вручную.

## <a name="process-server"></a>Сервер обработки

Сервер обработки обрабатывает данные репликации во время отработки отказа и восстановления размещения, а также устанавливает службу Mobility Service для локальных виртуальных машин VMware и физических серверов.

**Параметр** | **Сведения** | **Ссылки**
--- | --- | ---
**Развертывание**  | По умолчанию, когда сервер конфигурации развернут, устанавливается сервер обработки. <br/><br/> Локальный сервер обработки необходим для аварийного восстановления и репликации локальных виртуальных машин VMware и физических серверов. | [Подробнее](vmware-azure-architecture.md#architectural-components).
**Роль (локальная**) | Получает данные репликации с компьютеров, включенных для репликации. <br/><br/> Оптимизирует данные репликации с помощью кэширования, сжатия и шифрования и отправляет их в службу хранилища Azure. <br/><br/> Выполняет принудительную установку службы Site Recovery Mobility Service на локальных виртуальных машинах VMware и физических серверах, которые требуется реплицировать. <br/><br/> Выполняет автоматическое обнаружение локальных компьютеров. | [Подробнее](vmware-azure-enable-replication.md).
**Роль (восстановление размещения из Azure)** | После отработки отказа с локального сайта вы настраиваете сервер обработки в Azure как виртуальную машину Azure для обработки восстановления размещения в локальном расположении.<br/><br/> Сервер обработки в Azure является временным. Виртуальную машину Azure можно удалить после восстановления размещения. | [Подробнее](vmware-azure-set-up-process-server-azure.md).
**Масштабирование** | Для более крупных развертываний в локальной среде можно настроить дополнительные серверы обработки масштабирования. Дополнительные серверы масштабируют емкость путем обработки большого количества реплицируемых компьютеров и больших объемов трафика репликации.<br/><br/> Для балансировки нагрузки трафика репликации можно перемещать компьютеры между двумя серверами обработки. | [Подробнее](vmware-azure-set-up-process-server-scale.md).

## <a name="master-target-server"></a>Главный целевой сервер

Главный целевой сервер обрабатывает данные репликации при восстановлении размещения с переносом из Azure.

- По умолчанию главный целевой сервер устанавливается на сервере конфигурации.
- Для крупных развертываний вы можете добавить отдельный главный целевой сервер для восстановления размещения.

## <a name="next-steps"></a>Дальнейшие действия

- Изучите [архитектуру](vmware-azure-architecture.md) аварийного восстановления виртуальных машин VMware и физических серверов.
- Ознакомьтесь с [требованиями и необходимыми компонентами](vmware-physical-azure-support-matrix.md) для аварийного восстановления виртуальных машин VMware и физических серверов в Azure.
