---
title: Устранение проблем с пулом мощностей для Azure NetApp Files | Документация Майкрософт
description: Описывает потенциальные проблемы, которые могут возникнуть при управлении пулами ресурсов, и предоставляет решения для этих проблем.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/14/2021
ms.author: b-juche
ms.openlocfilehash: 759759b67582b241d0bab1e043dd15e54a804faf
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98251545"
---
# <a name="troubleshoot-capacity-pool-issues"></a>Устранение неполадок с пулами емкости

В этой статье описываются способы устранения проблем, которые могут возникнуть при управлении пулами ресурсов, включая операцию изменения пула. 

## <a name="issues-managing-a-capacity-pool"></a>Проблемы с управлением пулом емкости 

|     Условие ошибки    |     Решение    |
|-|-|
| Проблемы при создании пула ресурсов |  Убедитесь, что число пулов емкости не превышает предельное значение. См. раздел [ограничения ресурсов для Azure NetApp Files](azure-netapp-files-resource-limits.md).  Если число меньше ограничения, но возникают проблемы, отправьте запрос в службу поддержки и укажите имя пула емкости. |
| Проблемы при удалении пула ресурсов  |  Убедитесь, что удалены все Azure NetApp Files тома и моментальные снимки в подписке, в которую вы пытаетесь удалить пул емкости. <br> Если вы уже удалили все тома и моментальные снимки, и вы по-прежнему не можете удалить пул ресурсов, ссылки на ресурсы могут по-прежнему существовать без отображения на портале. В этом случае отправьте запрос в службу поддержки и укажите, что вы выполнили рекомендованные действия. |
| Сбой создания или изменения тома с `Requested throughput not available` ошибкой | Доступная пропускная способность для тома определяется размером пула ресурсов и уровнем обслуживания. Если пропускная способность не достаточна, следует увеличить размер пула или изменить пропускную способность существующего тома. | 

## <a name="issues-when-changing-the-capacity-pool-of-a-volume"></a>Проблемы при изменении пула емкости тома 

|     Условие ошибки    |     Решение    |
|-|-|
| Изменение пула емкости для тома не разрешено. | Возможно, вы еще не имеете права на использование этой функции. <br> Функция перемещения тома в другой пул емкости сейчас доступна в предварительной версии. Если вы используете эту функцию в первый раз, необходимо сначала зарегистрировать эту функцию и задать `-FeatureName ANFTierChange` . См. инструкции по регистрации в разделе [Динамическое изменение уровня обслуживания тома](dynamic-change-volume-service-level.md). |
| Размер пула емкости слишком мал для общего размера тома. |  Ошибка является следствием того, что целевой пул емкости не имеет доступной емкости для перемещаемого тома.  <br> Увеличьте размер целевого пула или выберите другой пул, который больше.  См. раздел [изменение размера пула ресурсов или тома](azure-netapp-files-resize-capacity-pools-or-volumes.md).   |
|  Невозможно завершить изменение пула, так как уже существует том с именем `'{source pool name}'` в целевом пуле. `'{target pool name}'` | Эта ошибка возникает, поскольку том с таким именем уже существует в целевом пуле емкости.  Выберите другой пул мощностей, у которого нет тома с таким же именем.   | 

## <a name="next-steps"></a>Дальнейшие действия  

* [Настройка пула емкости](azure-netapp-files-set-up-capacity-pool.md)
* [Управление пулом емкости качества обслуживания вручную](manage-manual-qos-capacity-pool.md)
* [Динамическое изменение уровня обслуживания тома](dynamic-change-volume-service-level.md)
* [Изменение размера пула емкости или тома](azure-netapp-files-resize-capacity-pools-or-volumes.md)
