---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 2e0a7ca8fb9eaafbc50c0ce60799dd68d83b2fa3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98901158"
---
Ваше устройство связано с учетной записью хранения, которая используется в качестве места назначения для ваших данных в Azure. Управление доступом к учетной записи хранения осуществляется с помощью подписки и двух 512-разрядных ключей доступа, связанных с этой учетной записью хранения.

Один из ключей используется для проверки подлинности, когда Data Box Edge устройство получает доступ к учетной записи хранения. Другой ключ хранится в качестве резервного, что позволяет время от времени менять ключи.

В целях безопасности многие центры обработки данных требуют смену ключей. Рекомендации по смене ключей.

- Ключ учетной записи хранения похож на корневой пароль для вашей учетной записи хранения. Тщательно защитите ключ учетной записи. Не сообщайте пароль другим пользователям, не задавайте его явно в коде и не храните его нигде в виде обычного текста, доступного для других пользователей.
- [Повторно создайте ключ учетной записи](../articles/storage/common/storage-account-keys-manage.md#manually-rotate-access-keys) с помощью портал Azure, если вы считаете, что он может быть скомпрометирован.
- Администратор Azure должен периодически изменять или повторно создавать первичный или вторичный ключ, используя раздел "хранилище" портал Azure для прямого доступа к учетной записи хранения.
