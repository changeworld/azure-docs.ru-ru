---
title: Общие сведения о политике хранения данных Azure SQL Server
description: Сведения о политике хранения данных в Azure SQL ребро
keywords: SQL Server, хранение данных
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: bb059a946c03f41e5b65944eec67070f84ee6b08
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "90976349"
---
# <a name="data-retention-overview"></a>Общие сведения о хранении данных

Сбор и хранение данных из подключенных устройств IoT важно для того, чтобы обеспечить оперативную и деловую аналитику. Однако при наличии объема данных, поступающих от этих устройств, организациям важно тщательно спланировать объем данных, которые они хотят хранить, и степень детализации. Несмотря на то что сохранение всех данных на уровне детализации нежелательно, это не всегда целесообразно. Кроме того, объем данных, которые можно хранить, ограничивается объемом хранилища, доступного на устройствах Интернета вещей или пограничных устройств. 

В Azure SQL пограничных баз данных администраторы могут определять политику хранения данных в базе данных SQL и ее базовых таблицах. После определения политики хранения данных будет выполнена фоновая задача системы для очистки устаревших (старых) данных из пользовательских таблиц. 

> [!Note]
> Данные, очищенные из таблицы, невозможно восстановить. Единственным возможным способом восстановления очищенных данных является восстановление базы данных из более старой резервной копии.

Краткие руководства:

- [Включение и отключение политик хранения данных](data-retention-enable-disable.md)
- [Управление историческими данными с помощью политики хранения](data-retention-cleanup.md)

## <a name="how-data-retention-works"></a>Принцип хранения данных

Чтобы настроить хранение данных, можно использовать инструкции DDL. Для получения дополнительных сведений [включите и отключите политики хранения данных](data-retention-enable-disable.md). Для автоматического удаления устаревших записей необходимо сначала включить хранение данных как для базы данных, так и для таблиц, которые необходимо очистить в этой базе данных. 

После настройки срока хранения данных для таблицы выполняется фоновая задача для обнаружения устаревших записей в таблице и удаления этих записей. Если по какой-либо причине автоматическая очистка задач не выполняется или не может поддерживать удаление, то для этих таблиц можно выполнить операцию очистки вручную. Дополнительные сведения об автоматической и ручной очистке см. в разделе [Автоматическая и ручная очистка](data-retention-cleanup.md).

## <a name="limitations-and-restrictions"></a>ограничения

- Хранение данных, если оно включено, автоматически отключается при восстановлении базы данных из полной резервной копии или при повторном присоединении. 
- Невозможно включить хранение данных для темпоральной таблицы журнала
- Невозможно изменить коломн фильтра хранения данных. Чтобы изменить столбец, отключите хранение данных в таблице.  

## <a name="next-steps"></a>Next Steps

- [Машинное обучение и искусственный интеллект с использованием ONNX в SQL для пограничных вычислений](onnx-overview.md)
- [Создание комплексного решения для Интернета вещей с использованием SQL для пограничных вычислений на основе IoT Edge](tutorial-deploy-azure-resources.md).
- [Потоковая передача данных в SQL Azure для пограничных вычислений](stream-data.md)
