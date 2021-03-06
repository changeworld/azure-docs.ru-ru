---
title: Визуализируйте данные Azure IoT Central на панели мониторинга Power BI | Документация Майкрософт
description: Используйте решение Power BI для Azure IoT Central для визуализации и анализа данных IoT Central.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/4/2019
ms.topic: conceptual
ms.openlocfilehash: ea4a47f1ba3eac39820e839a10330840f57afe42
ms.sourcegitcommit: a9ce1da049c019c86063acf442bb13f5a0dde213
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105629076"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Визуализация и анализ данных Azure IoT Central на панели мониторинга Power BI

*Этот раздел относится к администраторам и разработчикам решений.*

> [!Note] 
> В этом решении используются [устаревшие функции экспорта данных](./howto-export-data-legacy.md). Следите за обновленными рекомендациями по подключению к Power BI с помощью последнего экспорта данных.

:::image type="content" source="media/howto-connect-powerbi/iot-continuous-data-export.png" alt-text="Конвейер решения Power BI":::

Используйте решение Power BI для Azure IoT Central v3, чтобы создать мощную информационную панель Power BI для наблюдения за производительностью устройств IoT. С помощью панели мониторинга Power BI можно выполнять следующие действия.

- Отслеживать количество данных, которые отправляют устройства за период времени
- Сравнение томов данных между различными потоками телеметрии
- Фильтрация данных, отправляемых конкретными устройствами
- Просмотр последних данных телеметрии в таблице

Это решение настраивает конвейер, считывающий данные из вашей учетной записи хранения BLOB-объектов Azure для [экспорта устаревших данных](./howto-export-data-legacy.md) . Для обработки и преобразования данных конвейер использует функции Azure, фабрику данных Azure и базу данных SQL Azure. данные можно визуализировать и анализировать в Power BIном отчете, который загружается в виде PBIX-файла. Все ресурсы создаются в подписке Azure, поэтому каждый компонент можно настроить в соответствии со своими потребностями.

## <a name="prerequisites"></a>Предварительные условия

Чтобы выполнить действия, описанные в этом руководстве, вам потребуется активная подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

Для настройки решения требуются следующие ресурсы:

- Приложение IoT Central версии 3. Чтобы узнать, как проверить версию приложения, см. [сведения о приложении](./howto-get-app-info.md). Чтобы узнать, как создать приложение IoT Central, см. статью [Создание приложения IOT Central Azure](./quick-deploy-iot-central.md).
- Устаревший экспорт непрерывных данных, настроенный для экспорта телеметрии, устройств и шаблонов устройств в хранилище BLOB-объектов Azure. Дополнительные сведения см. в [документации по экспорту устаревших данных](howto-export-data-legacy.md).
  - Убедитесь, что только приложение IoT Central экспортирует данные в контейнер больших двоичных объектов.
  - [Устройства должны отсылать закодированные сообщения JSON](../../iot-hub/iot-hub-devguide-messages-d2c.md). Устройства должны указывать `contentType:application/JSON` `contentEncoding:utf-8` `contentEncoding:utf-16` Свойства системы сообщений, а также или или `contentEncoding:utf-32` .
- Power BI Desktop (последняя версия). См. [Power BI downloads](https://powerbi.microsoft.com/downloads/).
- Power BI Pro (если вы хотите предоставить доступ к панели мониторинга другим пользователям).

> [!NOTE]
> Если вы используете приложение IoT Central версии 2, см. статью [визуализация и анализ данных IOT Central Azure на панели мониторинга Power BI](/previous-versions/azure/iot-central/core/howto-connect-powerbi) на веб-сайте документации по предыдущим версиям.

## <a name="install"></a>Установка

Чтобы настроить конвейер, перейдите на страницу [Power BI решение для Azure IOT Central v3](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) на **Microsoft AppSource** сайте. Выберите **получить сейчас** и следуйте инструкциям.

При открытии PBIX-файла убедитесь, что прочитали и следуйте инструкциям на обложке страницы. Эти инструкции описывают, как подключить отчет к базе данных SQL.

## <a name="report"></a>Report

PBIX-файл содержит отчет **устройства и данные телеметрии** , в котором представлено историческое представление телеметрии, отправленных устройствами. Она обеспечивает разделение различных типов телеметрии, а также показывает последние данные телеметрии, отправленные устройствами.

:::image type="content" source="media/howto-connect-powerbi/report.png" alt-text="Power BIные устройства и отчет о телеметрии":::

## <a name="pipeline-resources"></a>Ресурсы конвейера

Вы можете получить доступ ко всем ресурсам Azure, которые составляют конвейер, в портал Azure. Все ресурсы находятся в группе ресурсов, созданной при настройке конвейера.

:::image type="content" source="media/howto-connect-powerbi/azure-deployment.png" alt-text="портал Azure представление группы ресурсов":::

В следующем списке приводится описание роли каждого ресурса в конвейере.

### <a name="azure-functions"></a>Функции Azure

Каждый раз триггеры приложения-функции Azure IoT Central записывает новый файл в хранилище BLOB-объектов. Функции извлекают данные из больших двоичных объектов в шаблонах телеметрии, устройств и устройств для заполнения промежуточных таблиц SQL, используемых фабрикой данных Azure.

### <a name="azure-data-factory"></a>Фабрика данных Azure

Фабрика данных Azure подключается к базе данных SQL в качестве связанной службы. Она запускает хранимые процедуры для обработки данных и сохранения их в таблицах анализа.

Фабрика данных Azure запускается каждые 15 минут, чтобы преобразовать последний пакет данных для загрузки в таблицы SQL (это текущее минимальное число для **триггера окна "переворачивающегося"**).

### <a name="azure-sql-database"></a>База данных SQL Azure

Фабрика данных Azure создает набор таблиц анализа для Power BI. Вы можете исследовать эти схемы в Power BI и использовать их для создания собственных визуализаций.

## <a name="estimated-costs"></a>Расчетная стоимость

Страница [Power BI решение для Azure IOT Central v3](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) на сайте Microsoft AppSource содержит ссылку на средство оценки затрат для развертываемых ресурсов.

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда вы узнали, как визуализировать данные в Power BI, рекомендуем следующий шаг — научиться [управлять устройствами](howto-manage-devices.md).
