---
title: Создание блокировки ресурса для таблицы API таблицы БД Azure Cosmos
description: Создание блокировки ресурса для таблицы API таблицы БД Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 04f8d541fc534a60550ba77d9775b340571a504f
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2021
ms.locfileid: "107788967"
---
# <a name="create-resource-lock-for-a-azure-cosmos-db-table-api-table-using-azure-cli"></a>Создание блокировки ресурса для таблицы API таблицы БД Azure Cosmos с помощью Azure CLI
[!INCLUDE[appliesto-table-api](../../../includes/appliesto-table-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Для работы с этой статьей требуется Azure CLI версии 2.9.1 или более поздней. Если вы используете Azure Cloud Shell, последняя версия уже установлена.

> [!IMPORTANT]
> Блокировки ресурсов не работают для изменений, внесенных пользователями, которые подключаются с помощью пакета Cosmos DB Table SDK, Azure Storage Table SDK и любых средств, которые используют ключи учетной записи, если только учетная запись Cosmos DB сначала не будет заблокирована с включенным свойством `disableKeyBasedMetadataWriteAccess`. Дополнительные сведения о том, как включить это свойство, см. в разделе [Предотвращение изменений в пакетах SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/table/lock.sh "Create a resource lock for an Azure Cosmos DB Table API table.")]

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az lock create](/cli/azure/lock#az_lock_create) | Создание блокировки. |
| [az lock list](/cli/azure/lock#az_lock_list) | Вывод сведений о блокировке. |
| [az lock show](/cli/azure/lock#az_lock_show) | Отображение свойств блокировки. |
| [az lock delete](/cli/azure/lock#az_lock_delete) | Удаление блокировки. |

## <a name="next-steps"></a>Дальнейшие действия

- [Блокировка ресурсов для предотвращения непредвиденных изменений](../../../../azure-resource-manager/management/lock-resources.md)

- [Документация по Azure Cosmos DB CLI](/cli/azure/cosmosdb).

- [Репозиторий GitHub для Azure Cosmos DB CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
