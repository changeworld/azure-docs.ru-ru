---
title: Заметки о выпуске пакета StorSimple 8000 с обновлением 5,1
description: Описание новых функций, проблем и решений для решения задач, связанных с обновлением для StorSimple 8000 Series 5,1.
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.topic: conceptual
ms.date: 03/18/2021
ms.author: alkohli
ms.openlocfilehash: cdb971851ba678ce18f5a1c7954e5620740f3a4c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657575"
---
# <a name="storsimple-8000-series-update-51-release-notes"></a>Заметки о выпуске пакета StorSimple 8000 с обновлением 5,1

## <a name="overview"></a>Обзор

В следующих заметках о выпуске описаны новые функции и выявлены критические открытые проблемы, связанные с обновлением для StorSimple 8000 Series 5,1. Здесь также содержится список обновлений программного обеспечения StorSimple для этого выпуска.

Обновление 5,1 можно применить к любому устройству StorSimple с обновлением 5. Если вы используете версию ниже 5, сначала примените обновление 5, а затем примените 5,1. Версия устройства, связанная с обновлением 5,1, — 6.3.9600.17885.

Перед развертыванием обновления в решении StorSimple просмотрите сведения, содержащиеся в заметках о выпуске.

> [!IMPORTANT]
>
> * Обновление 5,1 является обязательным и должно быть установлено немедленно. Дополнительные сведения см. в статье как [применить обновление 5,1](storsimple-8000-install-update-51.md).
> * Обновление 5,1 содержит только обновления для системы безопасности. Установка этого обновления занимает около 30 минут. Настоятельно рекомендуется применить обновление 5,1, чтобы обеспечить работу устройства.
> * Для новых выпусков обновления могут отображаться не сразу, поскольку развертывание обновлений выполняется поэтапно. Подождите несколько дней и затем повторите попытку поиска обновлений, так как они станут доступными в ближайшее время.

## <a name="whats-new-in-update-51"></a>Новые возможности в обновлении 5,1

В обновление 5,1 внесены следующие основные улучшения и исправления ошибок:

* **Tls 1,2** — это обновление StorSimple будет применять TLS 1,2 на всех клиентах. Это обязательное обновления для всех устройств серии StorSimple 8000.

   Если вы видите следующее предупреждение, перед продолжением необходимо обновить программное обеспечение на устройстве.

   На одном или нескольких устройствах StorSimple выполняется старая версия программного обеспечения. Последнее доступное обновление для TLS 1,2 — это обязательное обновление, которое должно быть установлено на этих устройствах немедленно. TLS 1,2 используется для всех портал Azure связи и без этого обновления, устройство не сможет взаимодействовать со службой StorSimple.

## <a name="known-issues-in-update-51-from-previous-releases"></a>Известные проблемы в обновлении 5,1 из предыдущих выпусков

В обновлении 5,1 нет новых известных проблем. Список проблем, которые переносятся на обновление 5,1 из предыдущих выпусков, см. в [заметках о выпуске обновления 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="storsimple-cloud-appliance-updates-in-update-51"></a>Обновления облачных устройств StorSimple в обновлении 5,1

Это обновление не может применяться к StorSimple Cloud Appliance (также известному как виртуальное устройство). Новые облачные устройства необходимо создавать с помощью образа обновления 5,1. Дополнительные сведения о создании облачного устройства StorSimple см. в статье [Развертывание и администрирование облачного устройства StorSimple в Azure (обновление 3 и более поздние версии)](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Следующий шаг

Узнайте, как [установить обновление 5,1](storsimple-8000-install-update-51.md) на устройстве StorSimple.
