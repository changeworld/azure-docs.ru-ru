---
title: Настройка TLS — портал Azure — база данных Azure для MySQL
description: Узнайте, как настроить конфигурацию TLS с помощью портал Azure для базы данных Azure для MySQL.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 5ecf2992fa9ea56f73748a9f1f98c75f9076c68f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656895"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Настройка параметров TLS в базе данных Azure для MySQL с помощью портал Azure

В этой статье описывается, как настроить сервер базы данных Azure для MySQL для применения минимальной версии TLS, разрешенной для подключений, чтобы пропускать и отклонять все соединения с более ранней версией TLS по сравнению с настроенной минимальной версией TLS, тем самым улучшая сетевую безопасность.

Вы можете применить версию TLS для подключения к своей базе данных Azure для MySQL. Теперь у клиентов есть возможность задать минимальную версию TLS для сервера базы данных. Например, если установить для минимальной версии TLS значение 1,0, вы должны разрешить клиентам подключаться с помощью TLS 1.0, 1.1 и 1,2. Кроме того, если задать для этого параметра значение 1,2, то разрешить только клиенты, подключающиеся по протоколу TLS 1.2 +, а все входящие подключения с TLS 1,0 и TLS 1,1 будут отклонены.

## <a name="prerequisites"></a>Предварительные требования

Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:

* [База данных Azure для MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>Настройка конфигурации TLS для базы данных Azure для MySQL

Чтобы задать минимальную версию TLS сервера MySQL, выполните следующие действия.

1. В [портал Azure](https://portal.azure.com/)выберите существующий сервер базы данных Azure для MySQL.

1. На странице "сервер MySQL" в разделе " **Параметры**" щелкните **Безопасность подключения** , чтобы открыть страницу настройки безопасности подключения.

1. В **минимальной версии TLS** выберите **1,2** , чтобы запретить подключения с использованием протокола tls версии ниже TLS 1,2 для сервера MySQL.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value.png" alt-text="Конфигурация TLS базы данных Azure для MySQL":::

1. **Сохраните** изменения. 

1. Уведомление подтверждает, что параметр безопасности подключения успешно включен и действует немедленно. Перезагрузка сервера не требуется или **не** выполнена. После сохранения изменений все новые подключения к серверу принимаются, только если версия TLS больше или равна минимальной версии TLS, установленной на портале.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value-success.png" alt-text="Настройка TLS для базы данных Azure для MySQL":::

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [создании оповещений о метриках](howto-alert-on-metric.md)
