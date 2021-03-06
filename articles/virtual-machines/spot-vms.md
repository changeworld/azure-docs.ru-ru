---
title: Использование виртуальных машин Azure для точки
description: Узнайте, как использовать виртуальные машины Azure для сохранения затрат.
author: JagVeerappan
ms.author: jagaveer
ms.service: virtual-machines
ms.subservice: spot
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 10/05/2020
ms.reviewer: cynthn
ms.openlocfilehash: fb53fc37227e040ed7bd7fc8e47de9aed538bc2e
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104721398"
---
# <a name="use-azure-spot-virtual-machines"></a>Использование виртуальных машин Azure для точки 

Используя виртуальные машины Azure, вы можете использовать преимущества нашей неиспользуемой емкости с значительной экономией затрат. В любой момент, когда Azure нужна емкость, инфраструктура Azure будет выключать виртуальные машины Azure. Таким образом, виртуальные машины Azure для виртуальных машин отлично подходят для рабочих нагрузок, которые могут обрабатывать прерывания, такие как задания пакетной обработки, среды разработки и тестирования, большие вычислительные рабочие нагрузки и многое другое.

Объем доступной емкости может варьироваться в зависимости от размера, региона, времени суток и других параметров. При развертывании виртуальных машин точки Azure в Azure будут выделены виртуальные машины, если они доступны, но соглашение об уровне обслуживания для этих виртуальных машин отсутствует. Виртуальная машина для машинного доступа Azure не поддерживает гарантии высокого уровня доступности. В любой момент, когда Azure потребуется резервная копия, инфраструктура Azure будет выключать виртуальные машины Azure с помощью уведомлений за 30 секунд. 


## <a name="eviction-policy"></a>Политика вытеснения

Виртуальные машины могут быть вытеснены в зависимости от доступной емкости или настроенной вами максимальной цены. При создании виртуальной машины в точке Azure можно задать политику вытеснения для отмены *распределения* (по умолчанию) или *Удалить*. 

Политика *освобождения* ПЕРЕмещает виртуальную машину в остановленное состояние, что позволяет повторно развернуть ее позже. Однако нет гарантии, что распределение будет выполнено успешно. Освобожденные виртуальные машины будут подсчитываться по квоте, а плата за ресурсы хранилища будет заряжена для базовых дисков. 

Если вы хотите, чтобы виртуальная машина была удалена при удалении, можно задать политику вытеснения для *удаления*. Удаленные виртуальные машины удаляются вместе с их базовыми дисками, поэтому вы не будете получать оплату за хранилище. 

Вы можете принять участие в получении уведомлений в виртуальной машине с помощью [Azure запланированные события](./linux/scheduled-events.md). Вы получите информацию о предстоящем вытеснении виртуальных машин, и у вас будет 30 секунд до вытеснения для завершения всех заданий и корректной остановки работы. 


| Параметр | Результат |
|--------|---------|
| Максимальная цена не ниже текущей цены. | Виртуальная машина будет развернута, пока есть доступная емкость и не превышена квота. |
| Максимальная цена ниже текущей цены. | Виртуальная машина не развертывается. Вы получите сообщение об ошибке с информацией о том, что максимальная цена должна быть не ниже текущей цены. |
| Перезапуск или остановка и освобождение виртуальной машины, если максимальная цена не ниже текущей цены | Виртуальная машина будет развернута, если есть доступная емкость и не превышена квота. |
| Перезапуск или остановка и освобождение виртуальной машины, если максимальная цена ниже текущей цены | Вы получите сообщение об ошибке с информацией о том, что максимальная цена должна быть не ниже текущей цены. | 
| Цена на виртуальную машину выросла и превысила максимальную цену. | Виртуальная машина вытесняется. Вы получите 30-секундное уведомление перед вытеснением. | 
| После вытеснения цена на виртуальную машину снова опускается ниже максимальной цены. | Виртуальная машина не перезапускается автоматически. Вы можете запустить виртуальную машину вручную, и она будет оплачиваться по текущей цене. |
| Если установлена максимальная цена `-1` | Виртуальная машина не будет вытесняться по критерию цены. Максимальной ценой всегда будет считаться текущая цена, вплоть до уровня стандартной цены на виртуальные машины. Цена никогда не будет превышать стандартную цену.| 
| Изменение максимальной цены | Для изменения максимальной цены виртуальную машину придется освободить. Освободите виртуальную машину, установите новую максимальную цену и обновите виртуальную машину. |


## <a name="limitations"></a>Ограничения

Следующие размеры виртуальной машины не поддерживаются для виртуальных машин Azure на месте:
 - Серия B
 - Рекламные версии любого размера (например, рекламные размеры Dv2, NV, NC и H)

Виртуальные машины Azure для машинного региона можно развернуть в любом регионе, кроме Microsoft Azure Китая 21Vianet.

<a name="channel"></a>

В настоящее время поддерживаются следующие [типы предложений](https://azure.microsoft.com/support/legal/offer-details/) :

-   Соглашение Enterprise 
-   Код предложения с оплатой по мере использования (003P)
-   Спонсорские (0036P и 0136P)
- Для поставщика облачных служб (CSP) обратитесь к своему партнеру.


## <a name="pricing"></a>Цены

Цены на виртуальные машины Azure для машинного хранения являются переменными на основе региона и SKU. Дополнительные сведения см. на страницах с информацией о ценах на виртуальные машины [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) и [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Вы также можете запросить сведения о ценах с помощью [API розничных цен Azure](/rest/api/cost-management/retail-prices/azure-retail-prices) , чтобы запросить информацию о ценах. Объект `meterName` и `skuName` будет содержать `Spot` .

Переменное ценообразование позволяет вам указать максимальную цену в долларах США с точностью до 5 знаков после запятой. Например, значение `0.98765` определяет максимальную цену 0,98765 долларов США в час. Если вы укажете для максимальной цены значение `-1`, виртуальная машине не будет вытесняться по критерию цены. Цена на такую виртуальную машину будет определяться меньшим из двух значений: текущая цена точечных виртуальных машин или цена на стандартные виртуальные машины, но только при условии наличия емкости и соблюдения квоты.

## <a name="pricing-and-eviction-history"></a>Журнал цен и вытеснения

Вы можете просмотреть исторические цены и ставки вытеснений для каждого размера в регионе на портале. Выберите **Просмотр истории цен и сравните цены в ближайших регионах** , чтобы просмотреть таблицу или график цен для определенного размера.  Ниже приведены примеры цен и вытеснения на следующих изображениях. 

**Диаграмма**.

:::image type="content" source="./media/spot-chart.png" alt-text="Снимок экрана параметров региона с разницей в ценах и процентах вытеснения в виде диаграммы.":::

**Таблица**:

:::image type="content" source="./media/spot-table.png" alt-text="Снимок экрана параметров региона с разницей в ценах и ставках вытеснения в виде таблицы.":::



##  <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**Вопрос.** После создания виртуальная машина с машинным именем Azure совпадает с обычной стандартной ВИРТУАЛЬНОЙ машиной?

Ответ **.** Да, за исключением соглашений об уровне обслуживания для виртуальных машин Azure, которые можно удалить в любое время.


**Вопрос.** Что делать, если вытеснена виртуальная машина, а емкость по-прежнему нужна?

Ответ **.** Мы рекомендуем использовать стандартные виртуальные машины вместо виртуальных машин Azure на месте, если вам нужна достаточная емкость.


**Вопрос.** Как квота управляет виртуальными машинами Azure?

Ответ **.** Виртуальные машины Azure для разных машин будут иметь отдельный пул квот. Эти квоты совместно используются всеми точечными виртуальными машинами и экземплярами масштабируемых наборов. Дополнительные сведения см. в статье [Подписка Azure, границы, квоты и ограничения службы](../azure-resource-manager/management/azure-subscription-service-limits.md).


**Вопрос.** Можно ли запросить дополнительную квоту для виртуальных машин Azure на месте?

Ответ **.** Да, вы сможете отправить запрос на увеличение квоты виртуальных машин Azure с помощью [стандартного процесса запроса квоты](../azure-portal/supportability/per-vm-quota-requests.md).


**Вопрос.** Где можно задать вопрос?

**Ответ.** Разместите вопрос с тегом `azure-spot` в разделе [вопросов и ответов](/answers/topics/azure-spot.html). 


**Вопрос.** Как изменить максимальную цену для плашечной виртуальной машины?

Ответ **.** Прежде чем можно будет изменить максимальную цену, необходимо освободить виртуальную машину. Затем можно изменить максимальную цену на портале в разделе **конфигурации** для виртуальной машины. 

## <a name="next-steps"></a>Следующие шаги
Используйте интерфейс [командной строки](./linux/spot-cli.md), [портал](spot-portal.md), [шаблон ARM](./linux/spot-template.md)или [PowerShell](./windows/spot-powershell.md) для развертывания виртуальных машин с точкию Azure.

Вы также можете развернуть [масштабируемый набор с помощью экземпляров виртуальных машин в Azure](../virtual-machine-scale-sets/use-spot.md).

Если возникает ошибка, см. раздел [коды ошибок](./error-codes-spot.md).
