---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 22a9cf3338f422341928a77f2bf14c497aa2ba31
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105563788"
---
## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/dotnet/) бесплатно.
- Установка [интерфейса командной строки Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?tabs=azure-cli) 

## <a name="create-azure-communication-resource"></a>Создание ресурса Служб коммуникации Azure

Чтобы создать ресурс Служб коммуникации Azure, [войдите в Azure CLI](/cli/azure/authenticate-azure-cli). Это можно сделать с помощью терминала, используя команду ```az login``` и указав учетные данные. Выполните следующую команду для создания рабочей области.

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

Если вы хотите выбрать конкретную подписку, можно также указать флаг ```--subscription``` и указать идентификатор подписки.
```
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup> --subscription "<subscriptionID>"
```

Теперь можно настроить ресурс Служб коммуникации с помощью следующих параметров:

* Группа ресурсов
* Имя ресурса Служб коммуникации
* Географический регион, с которым будет связан ресурс

На следующем шаге можно назначить ресурсу теги. Теги можно использовать для упорядочения ресурсов в Azure. Дополнительные сведения о тегах см. в [документации по маркировке ресурсов](../../../azure-resource-manager/management/tag-resources.md).

## <a name="manage-your-communication-services-resource"></a>Управление ресурсом Служб коммуникации

Чтобы добавить теги в ресурс Служб коммуникации, выполните следующие команды: Вы также можете ориентироваться на конкретную подписку.

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal1" --resource-group "<resourceGroup>"

az communication update --name "<communicationName>" --tags newTag="newVal2" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"
```

Подробные сведения о дополнительных командах см. в статье об [az communication](/cli/azure/ext/communication/communication).
