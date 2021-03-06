---
title: 'Копирование и вставка в виртуальную машину и из нее: Azure бастиона'
description: Из этой статьи вы узнаете, как копировать и вставлять в виртуальную машину Azure и из нее с помощью бастиона.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 03/22/2021
ms.author: cherylmc
ms.openlocfilehash: 4b0c2b734366f9a74a9b007ab9450ab4b4f51feb
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104800437"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>Копирование и вставка на виртуальную машину: Azure бастиона

Эта статья поможет вам скопировать и вставить текст в виртуальные машины и из них при использовании Azure бастиона. Перед началом работы с виртуальной машиной убедитесь, что выполнены действия по [созданию узла бастиона](./tutorial-create-host-portal.md). Затем подключитесь к виртуальной машине, которую вы хотите использовать, с помощью [RDP](bastion-connect-vm-rdp.md) или [SSH](bastion-connect-vm-ssh.md).

Для браузеров, поддерживающих расширенный доступ к API буфера обмена, можно копировать и вставлять текст между локальным устройством и удаленным сеансом так же, как и при копировании и вставке между приложениями на локальном устройстве. Для других браузеров можно использовать палитру средства доступа к буферу обмена бастиона.

>[!NOTE]
>В настоящее время поддерживается только копирование и вставка текста.
>

   ![Разрешить буфер обмена](./media/bastion-vm-manage/allow.png)

Поддерживается только текст для копирования и вставки. Для прямого копирования и вставки в браузере может появиться запрос на доступ к буферу обмена при инициализации сеанса бастиона. **Разрешить** веб-странице доступ к буферу обмена. При работе с компьютером Mac сочетание клавиш для вставки — SHIFT + CTRL + **V**.

## <a name="copy-to-a-remote-session"></a><a name="to"></a>Копирование в удаленный сеанс

После подключения к виртуальной машине с помощью [портал Azure ](https://portal.azure.com)выполните следующие действия.

1. Копировать текст или содержимое с локального устройства в локальный буфер обмена.
1. Во время удаленного сеанса откройте палитру средств доступа к буферу обмена бастиона, выбрав две стрелки. Стрелки расположены в левом центре сеанса.

   ![Снимок экрана, на котором показаны стрелки запуска для палитры инструментов, выделенной на левой стороне окна.](./media/bastion-vm-manage/left.png)

   ![На снимке экрана показан буфер обмена для текста, скопированного в бастиона.](./media/bastion-vm-manage/clipboard.png)
1. Как правило, скопированный текст автоматически отображается в палитре «бастиона Copy Вклеить». Если текст отсутствует, вставьте текст в текстовое поле палитры.
1. После того как текст появится в текстовой области, его можно вставить в удаленный сеанс.

   ![Снимок экрана, на котором изображена Выделенная кнопка "копировать/вставить" и образец текстовой строки, скопированной в удаленный сеанс.](./media/bastion-vm-manage/local.png)

## <a name="copy-from-a-remote-session"></a><a name="from"></a>Копирование из удаленного сеанса

После подключения к виртуальной машине с помощью [портал Azure ](https://portal.azure.com)выполните следующие действия.

1. Копировать текст или содержимое из удаленного сеанса в удаленный буфер обмена (с помощью клавиш CTRL + C).

   ![Палитра инструментов](./media/bastion-vm-manage/remote.png)
1. Во время удаленного сеанса откройте палитру средств доступа к буферу обмена бастиона, выбрав две стрелки. Стрелки расположены в левом центре сеанса.

   ![буфер обмена](./media/bastion-vm-manage/clipboard2.png)
1. Как правило, скопированный текст автоматически отображается в палитре «бастиона Copy Вклеить». Если текст отсутствует, вставьте текст в текстовое поле палитры.
1. После того как текст появится в текстовой области, его можно вставить на локальное устройство.

   ![вставка](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [часто задаваемыми вопросами о Бастионе Azure](bastion-faq.md).
