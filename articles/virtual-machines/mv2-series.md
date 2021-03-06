---
title: Виртуальные машины Azure серии Mv2
description: Спецификации виртуальных машин серии Mv2.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: b15fdc3826a72e9cfb039b6b2994179ab9565404
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102562526"
---
# <a name="mv2-series"></a>Серия Mv2

В серии Mv2 реализована высокая пропускная способность, платформа с низкой задержкой, работающая на процессоре Intel® Xeon® Platinum 8180M с частотой 2,5 ГГц (Skylake) с базовой частотой 2,5 ГГц и максимальной частотой Turbo 3,8 ГГц. Все размеры виртуальных машин серии Mv2 могут использовать постоянные диски уровня "Стандартный" и "Премиум". Экземпляры серии Mv2 — это оптимизированные для памяти размеры виртуальных машин, обеспечивающие непревзойденную вычислительную производительность для поддержки больших баз данных и рабочих нагрузок в памяти, с высоким соотношением памяти к ЦП, которое идеально подходит для серверов реляционных баз данных, больших кэшей и аналитики в памяти.

Виртуальная машина серии Mv2 с технологией Intel® Hyper-Threading Technology

[Хранилище класса Premium](premium-storage-performance.md): поддерживается<br>
[Кэширование хранилища класса Premium](premium-storage-performance.md): поддерживается<br>
[Динамическая миграция](maintenance-and-updates.md): не поддерживается<br>
[Обновления с сохранением памяти](maintenance-and-updates.md): не поддерживается<br>
[Поддержка создания виртуальных машин](generation-2.md): поколение 2<br>
[Ускоритель записи](./how-to-enable-write-accelerator.md): поддерживается<br>
[Ускоренная сеть](../virtual-network/create-vm-accelerated-networking-cli.md): поддерживается<br>
[Временные диски ОС](ephemeral-os-disks.md): не поддерживаются <br>
<br>

|Размер | vCPU | Память: ГиБ | Временное хранилище (SSD): ГиБ | Максимальное число дисков данных | Максимальная пропускная способность временного хранилища с кэшированием: операций ввода-вывода / Мбит/с (размер кэша в Гиб) | Максимальная пропускная способность дисков без кэширования: операций ввода-вывода в секунду / МБит/с | Максимальное число сетевых адаптеров | Ожидаемая пропускная способность сети (Мбит/с) |
|---|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M416ms_v2<sup>1</sup> | 416 | 11400 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |
| Standard_M416s_v2<sup>1</sup> | 416 | 5700 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |

<sup>1</sup> виртуальные машины серии Mv2 имеют только поколение 2 и поддерживают подмножество поддерживаемых образов версии 2. Полный список поддерживаемых образов для серии Mv2 см. ниже. Если вы используете Linux, см. инструкции по поиску и выбору образа в статье [Поддержка виртуальных машин поколения 2 в Azure](./generation-2.md) . Если вы используете Windows, см. инструкции по поиску и выбору образа в статье [Поддержка виртуальных машин поколения 2 в Azure](./generation-2.md) . 

- Windows Server 2019 или более поздней версии;
- SUSE Linux Enterprise Server 12 SP4 и более поздней версии или SUSE Linux Enterprise Server 15 SP1 и более поздних версий
- Red Hat Enterprise Linux 7,6, 7,7, 8,1 или более поздней версии 
- Oracle Enterprise Linux 7,7 или более поздней версии



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Другие размеры и сведения

- [Универсальные](sizes-general.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [Оптимизированные для GPU](sizes-gpu.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- [Предыдущие поколения](sizes-previous-gen.md)

Калькулятор цен: [Калькулятор цен](https://azure.microsoft.com/pricing/calculator/)

Дополнительные сведения о типах дисков: [типы дисков](./disks-types.md#ultra-disk)


## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.
