---
title: Управление правилами брандмауэра — гипермасштабирование (Citus) — база данных Azure для PostgreSQL
description: Создание правил брандмауэра базы данных Azure для PostgreSQL и управление ими — гипермасштабирование (Citus) — портал Azure
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 9/11/2020
ms.openlocfilehash: dadd04497eae0e91bdf5ea3caad38beda35f7fa3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "91275427"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>Управление правилами брандмауэра базы данных Azure для PostgreSQL — гипермасштабирование (Citus)
Правила брандмауэра уровня сервера можно использовать для управления доступом к узлу-координатору гипермасштабирования (Citus) с указанного IP-адреса или диапазона IP-адресов.

## <a name="prerequisites"></a>Предварительные требования
Прежде чем приступить к выполнению этого руководства, необходимы следующие компоненты:
- Группа серверов [Создание базы данных Azure для PostgreSQL — гипермасштабирование (Citus)](quickstart-create-hyperscale-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Создание правила брандмауэра на уровне сервера с помощью портала Azure

> [!NOTE]
> Эти параметры также доступны во время создания группы серверов Базы данных Azure для PostgreSQL — гипермасштабирование (Citus). На вкладке **Сеть** щелкните **Общедоступная конечная точка**.

> :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png" alt-text="Портал Azure — вкладка &quot;Сеть&quot;":::

1. На странице группы серверов PostgreSQL под заголовком "Безопасность" щелкните **Сети**, чтобы открыть правила брандмауэра.

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png" alt-text="Портал Azure: нажатие кнопки &quot;Сети&quot;":::

2. Щелкните **Добавить текущий IP-адрес клиента**, чтобы создать правило брандмауэра с общедоступным IP-адресом вашего компьютера, который воспринимается системой Azure.

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png" alt-text="Портал Azure: нажатие кнопки &quot;Добавить IP-адрес клиента&quot;":::

Кроме того, вы можете нажать **+Добавить 0.0.0.0-255.255.255.255** (справа от варианта B), чтобы разрешить доступ к порту узла-координатора 5432 не только для вашего IP-адреса, но и для всего Интернета. В этом случае клиентам по-прежнему необходимо выполнить вход с правильным именем пользователя и паролем для использования кластера. Тем не менее мы рекомендуем предоставлять доступ для всего Интернета только на короткие периоды времени и только для нерабочих баз данных.

3. Проверьте IP-адрес перед сохранением конфигурации. В некоторых случаях IP-адрес, определенный порталом Azure, может отличаться от IP-адреса, используемого для доступа к Интернету и серверам Azure. Поэтому может потребоваться изменить начальный IP-адрес и конечный IP-адрес для правила, чтобы оно функционировало ожидаемым образом.
   Используйте поисковую систему или другой сетевой инструмент, чтобы проверить собственный IP-адрес. Например, выполните поиск текста "какой у меня IP-адрес".

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png" alt-text="Поиск текста &quot;what is my IP&quot; в Bing":::

4. Добавьте дополнительные адресные пространства. В правилах брандмауэра можно указать отдельный IP-адрес или диапазон адресов. Если вы хотите ограничить правило, указав отдельный IP-адрес, введите его в полях начального и конечного IP-адресов. Открытие брандмауэра позволяет администраторам, пользователям и приложениям получать доступ к узлу-координатору через порт 5432.

5. На панели инструментов нажмите кнопку **Сохранить**, чтобы сохранить это правило брандмауэра уровня сервера. Дождитесь подтверждения успешного обновления правил брандмауэра.

## <a name="connecting-from-azure"></a>Подключение из Azure

Существует простой способ предоставить доступ к базе данных Гипермасштабирования (Citus) приложениям, размещенным в Azure (например, приложениям веб-приложений Azure или приложениям, работающим на виртуальной машине Azure). Просто установите для параметра **Разрешить службам и ресурсам Azure доступ к этой группе серверов** значение **Да** на портале в области **Сети** и нажмите **Сохранить**.

> [!IMPORTANT]
> Этот параметр позволяет настроить брандмауэр так, чтобы разрешить все подключения из Azure, включая подключения из подписок других клиентов. При выборе этого параметра убедитесь, что используемое имя для входа и разрешения пользователя предоставляют доступ только авторизованным пользователям.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Управление существующими правилами брандмауэра с помощью портала Azure
Повторите действия для управлению правилами брандмауэра.
* Чтобы добавить текущий компьютер, нажмите кнопку + **Добавить текущий IP-адрес клиента**. Щелкните **Сохранить** , чтобы сохранить изменения.
* Чтобы добавить дополнительные IP-адреса, введите имя правила, начальный IP-адрес и конечный IP-адрес. Щелкните **Сохранить** , чтобы сохранить изменения.
* Чтобы изменить существующее правило, измените значения во всех полях в данном правиле. Щелкните **Сохранить** , чтобы сохранить изменения.
* Чтобы удалить имеющееся правило, щелкните многоточие [...], а затем щелкните **Удалить**. Щелкните **Сохранить** , чтобы сохранить изменения.

## <a name="next-steps"></a>Дальнейшие действия
- Узнайте больше о [концепции правил брандмауэра](concepts-hyperscale-firewall-rules.md), включая способы устранения проблем с подключением.
