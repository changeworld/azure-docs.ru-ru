---
title: Тестирование производительности приложения на Хранилище дисков Azure
description: Узнайте о процессе тестирования производительности приложения в Azure.
author: roygara
ms.author: rogarana
ms.date: 01/29/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: bfda14acc2e50005e25faafa3037805af871c1df
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "99094648"
---
# <a name="benchmark-a-disk"></a>Тест производительности диска

Тестирование производительности — это процесс моделирования различных рабочих нагрузок приложения и измерения его производительности при каждой из них. Выполнив действия, описанные в [статье о проектировании для высокого уровня производительности](premium-storage-performance.md), вы узнали требования к производительности приложения. Запуская средства тестирования производительности на виртуальных машинах, где размещается приложение, можно определить уровни производительности, которые приложение может достичь с помощью расширенных твердотельных накопителей. В этой статье приведены примеры тестирования производительности Standard_D8ds_v4 виртуальной машины, подготовленной с помощью твердотельных накопителей Azure Premium.

Мы использовали стандартные средства тестирования производительности DiskSpd и FIO для Windows и Linux соответственно. Эти инструменты порождают несколько потоков, моделирующих производительность при рабочей нагрузке, и измеряют производительность системы. С помощью этих инструментов можно задать такие параметры, как размер блока и длина очереди, которые обычно нельзя менять в приложениях. Это обеспечивает большую гибкость для обеспечения максимальной производительности высокомасштабируемых виртуальных машин, подготовленных с помощью расширенных твердотельных накопителей, для различных типов рабочих нагрузок приложений. Дополнительные сведения о каждом средстве тестирования производительности см. на странице [DiskSpd](https://github.com/Microsoft/diskspd/wiki/) и [FIO](http://freecode.com/projects/fio).

Чтобы выполнить приведенные ниже примеры, создайте Standard_D8ds_v4 и прикрепите к виртуальной машине четыре твердотельные накопители уровня "Премиум". Из четырех дисков настройте 3 с кэшированием узлов как "None" и перемещайте их в том с именем NoCacheWrites. Для параметра кэширования оставшегося диска задайте значение ReadOnly и создайте том CacheReads с этим диском. С помощью этой программы можно увидеть максимальную производительность чтения и записи с Standard_D8ds_v4 виртуальной машины. Подробные инструкции по созданию Standard_D8ds_v4 с помощью твердотельных накопителей Premium см. в разделе [проектирование для обеспечения высокой производительности](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Дальнейшие действия

Перейдите к нашей статье о [проектировании для обеспечения высокой производительности](premium-storage-performance.md).

В этой статье вы создадите контрольный список, аналогичный существующему приложению для прототипа. С помощью инструментов тестирования производительности можно смоделировать рабочую нагрузку и измерить производительность прототипа приложения. В результате вы сможете определить, какой из классов диска будет соответствовать требованиям к производительности приложения, а какой превосходить их. Затем те же указания можно применить и для рабочего приложения.
