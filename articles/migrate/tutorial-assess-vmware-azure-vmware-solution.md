---
title: Оценка готовности серверов VMware к миграции в Решение Azure VMware (AVS) с помощью Миграции Azure
description: Узнайте, как оценить сервера в окружении VMware для миграции в AVS с помощью Миграции Azure.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: MVC
ms.openlocfilehash: 31bf3909012231996bd340cfa4d388f0fe20a4f5
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104782157"
---
# <a name="tutorial-assess-vmware-servers-for-migration-to-avs"></a>Учебник по оценке готовности серверов VMware к миграции в AVS

В рамках перехода на Azure вам нужно оценить локальные рабочие нагрузки, чтобы измерить готовность облака, определить риски и оценить затраты и сложность.

В этой статье рассказывается, как оценивать обнаруженные виртуальные машины или сервера VMware для миграции в Решение Azure VMware (AVS) с помощью службы "Миграция Azure". AVS — это управляемая служба, которая позволяет запускать платформу VMware в Azure.

В этом руководстве описано следующее:
> [!div class="checklist"]
- Выполнение оценки на основе метаданных сервера и сведений о конфигурации.
- Выполнение оценки на основе данных производительности.

> [!NOTE]
> В учебниках показан самый быстрый способ выполнения сценария и используются параметры по умолчанию, где это возможно. 

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/), прежде чем начинать работу.



## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступить к работе с этим учебником по оценке серверов для миграции в AVS, убедитесь, что обнаружили серверы, которые хотите оценить:

- Чтобы обнаруживать сервера с помощью устройства службы "Миграция Azure", [выполните инструкции из этого учебника](tutorial-discover-vmware.md). 
- Чтобы обнаруживать серверы с помощью импортированного CSV-файла, [следуйте инструкциям из этого руководства](tutorial-discover-import.md).


## <a name="decide-which-assessment-to-run"></a>Выбор оценки для выполнения


Решите, следует ли выполнять оценку с помощью критериев определения размера, основанных на динамических данных производительности или данных либо метаданных конфигурации сервера, которые были собраны локально в исходном виде.

**Оценка** | **Сведения** | **Рекомендация**
--- | --- | ---
**Как в локальной среде** | Оценка на основе данных или метаданных конфигурации сервера.  | Рекомендуемый размер узла в AVS зависит от размера локальной виртуальной машины или сервера, а также от параметров типа узла, типа хранилища и числа допустимых сбоев, указанных при оценке.
**На основе производительности** | Оценка на основе собранных динамических данных производительности. | Рекомендуемый размер узла в AVS зависит от данных об использовании ЦП и памяти, а также от параметров типа узла, типа хранилища и числа допустимых сбоев, указанных при оценке.

> [!NOTE]
> Оценку Решения Azure VMware (AVS) можно создать только для виртуальных машин или серверов VMware.

## <a name="run-an-assessment"></a>Запуск оценки

Запустите оценку следующим образом:

1.  На странице **Обзор** выберите **Windows, Linux and SQL Server** (Windows, Linux и SQL Server) и выберите **Оценка и миграция серверов**.
    :::image type="content" source="./media/tutorial-assess-sql/assess-migrate.png" alt-text="Страница обзора для службы &quot;Миграция Azure&quot;":::

1. В **средстве "Обнаружение и оценка" службы "Миграция Azure"** щелкните **Оценка**.

   ![Экран "Расположение кнопки "Оценка"](./media/tutorial-assess-vmware-azure-vmware-solution/assess.png)

1. В разделе **Оценка серверов** > **Тип оценки** выберите **Azure VMware Solution (AVS)** (Решение Azure VMware (AVS)).

1. В **источнике обнаружения**:

    - Если вы обнаружили серверы с помощью устройства, выберите **Servers discovered from Azure Migrate appliance** (Серверы, обнаруженные с устройства Миграции Azure).
    - Если вы обнаружили серверы с помощью импортированного CSV-файла, выберите **Imported servers** (Импортированные серверы). 
    
1. Щелкните **Изменить**, чтобы просмотреть свойства оценки.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-servers.png" alt-text="Страница выбора параметров оценки":::
 

1. В разделе **Свойства оценки** > **Свойства целевого объекта**:

    - В поле **Целевое расположение** выберите регион Azure, в который необходимо выполнить миграцию.
       - Рекомендации по размеру и стоимости основаны на указанном вами расположении.
   - Для параметра **Тип хранилища** по умолчанию используется значение **vSAN**. Это тип хранилища по умолчанию для частного облака AVS.
   - **Зарезервированные экземпляры** в настоящее время не поддерживаются для узлов AVS.
1. В поле **Размер виртуальной машины**:
    - Для параметра **Тип узла** по умолчанию используется значение **AV36**. Миграция Azure рекомендует узел из узлов, необходимых для миграции серверов в AVS.
    - В раскрывающемся списке **Параметр FTT, уровень RAID** выберите допустимое число сбоев и сочетание RAID.  Выбранный параметр FTT в сочетании с требованиями локального диска сервера определяет общий размер хранилища vSAN, необходимого в AVS.
    - В поле **Превышение лимита для ЦП** укажите отношение количества виртуальных ядер, связанных с одним физическим ядром, в узле AVS. Превышение лимита более чем 4:1 может привести к снижению производительности, но может использоваться для рабочих нагрузок типа веб-сервера.
    - В поле **Коэффициент избыточного выделения памяти** укажите коэффициент избыточного выделения памяти в кластере. Значение 1 соответствует использованию памяти на 100 %, 0,5 — на 50 %, а 2 — на 200 % доступной памяти. Вы можете задать значения только в диапазоне от 0,5 до 10 с одним знаком после запятой.
    - В поле **Коэффициент дедупликации и сжатия**, укажите ожидаемый коэффициент дедупликации и сжатия для своих рабочих нагрузок. Фактическое значение можно получить из конфигурации локальной vSAN или хранилища (оно может зависеть от рабочей нагрузки). Значение 3 означает трехкратный коэффициент, то есть для диска размером 300 ГБ будет использоваться только 100 GB хранилища. Значение 1 означает, что дедупликация или сжатие отключены. Вы можете задать значения только в диапазоне от 1 до 10 с одним знаком после запятой.
1. В разделе **Размер узла**: 
    - В поле **Условия изменения размера** выберите, следует ли основывать оценку на статических метаданных или на данных, основанных на производительности. При использовании данных о производительности:
        - В поле **Журнал производительности** укажите период, данные за который должны стать основой для оценки.
        - В поле **Использование процентиля** укажите значение процентиля, которое будет использоваться для примера производительности. 
    - В поле **Фактор комфорта** укажите буфер, который вы хотите использовать во время оценки. Он учитывается, например, для сезонного использования и малого количества записей в журнале с потенциальным повышением в будущем. Например, если вы используете фактор комфорта "2":
    
        **Компонент** | **Эффективное использование** | **Добавление фактора комфорта (2.0)**
        --- | --- | ---
        Ядра | 2  | 4
        Память | 8 ГБ | 16 Гб  

1. В разделе **Цены**:
    - В разделе **Предложение** отображается [предложение Azure ](https://azure.microsoft.com/support/legal/offer-details/), которое вы используете. Оценка вычисляет стоимость этого предложения.
    - В поле **Валюта** выберите валюту выставления счетов для своей учетной записи.
    - В поле **Скидка (%)** добавьте все скидки, относящиеся к подписке и предоставляемые в рамках предложения Azure. Значение по умолчанию — 0 %.

1. При внесении изменений нажмите кнопку **Сохранить**.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/avs-view-all.png" alt-text="Свойства оценки":::

1. В поле **Оценка серверов** щелкните **Далее**.

1. В разделе **Select servers to assess** (Выбор серверов для оценки) > **Имя оценки** укажите имя для оценки. 
 
1. В разделе **Создание или выбор группы** выберите **Создать** и укажите имя группы. 
    
    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-group.png" alt-text="Экран &quot;Добавление серверов в группу&quot;":::
 
1. Выберите устройство, а также серверы, которые необходимо добавить в группу. Затем нажмите кнопку **Далее**.

1. На странице **Review + create assessment** (Проверка и создание оценки) проверьте сведения об оценке и щелкните **Создать оценку**, чтобы создать группу и запустить оценку.

    > [!NOTE]
    > Если вы хотите выполнить оценку на основе производительности, рекомендуется подождать по крайней мере один день после запуска обнаружения, прежде чем создавать оценку. Благодаря этому вы сможете получить данные о производительности с большей достоверностью. В идеале после запуска обнаружения следует дождаться заданной длительности производительности (день/неделя/месяц), что позволит получить оценку с высокой достоверностью.

## <a name="review-an-assessment"></a>Проверка оценки

В результатах оценки AVS описывается:

- Готовность к AVS. Подходят ли локальные серверы для миграции в Решение Azure VMware (AVS).
- Число узлов AVS. Предполагаемое количество узлов AVS, необходимых для запуска серверов.
- Использование ресурсов на узлах AVS: планируемое использование ЦП, памяти и хранилища на всех узлах.
    - При расчете показателя использования предварительно учитываются последующие затраты на управление кластерами, например vCenter Server, NSX Manager (крупные экземпляры) и NSX Edge. Если развертывается HCX, сюда входит также HCX Manager и устройство IX, потребляющее ~ 44 виртуальных ЦП (11 ЦП), 75 ГБ ОЗУ и 722 ГБ хранилища до сжатия и дедупликации. 
- Примерная ежемесячная стоимость. Приблизительные ежемесячные затраты на все узлы Решения Azure VMware (AVS), на которых будут выполняться локальные серверы.

## <a name="view-an-assessment"></a>Просмотр оценки

Чтобы просмотреть оценку, сделайте следующее.

1. В разделе **Windows, Linux and SQL Server (Windows, Linux и SQL Server)**  > **Azure Migrate: Discovery and assessment (Миграция Azure: обнаружение и оценка)** щелкните номер рядом с элементом **Решение Azure VMware**.

1. В разделе **Оценки** щелкните оценку, чтобы открыть сведения о ней. В качестве примера (оценки и затраты только для примера): 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/avs-assessment-summary.png" alt-text="Сводка оценки AVS":::

1. Проверьте сводку по оценке. Вы можете также изменить свойства оценки или повторно рассчитать оценку.
 

### <a name="review-readiness"></a>Готовность к рассмотрению

1. Нажмите **Готовность к работе в Azure**.
2. Проверьте состояние готовности в поле **Готовность к работе в Azure**.

    - **Ready for AVS** (Готово к AVS). Сервер можно переносить в Azure без каких-либо изменений. Этот сервер будет запущен в AVS с полной поддержкой AVS.
    - **Готово при выполнении условий**. У сервера могут быть проблемы совместимости с текущей версией vSphere. Для обеспечения полной функциональности в AVS может потребоваться установить инструменты VMware или настроить другие параметры.
    - **Not ready for AVS** (Не готово к AVS). Виртуальная машина не запустится в AVS. Например, если к локальному серверу VMware подключено внешнее устройство (например, устройство чтения компакт-дисков) и вы используете VMware VMotion, операция VMotion завершится ошибкой.
 - **Уровень готовности неизвестен**. Миграции Azure не удалось определить готовность сервера из-за недостатка метаданных, собранных в локальной среде.

3. Ознакомьтесь с предлагаемым инструментом.

    - VMware HCX или Enterprise. Если вы используете сервера VMware, мы рекомендуем использовать для миграции локальных рабочих нагрузок в частное облако Решения Azure VMware (AVS) решение VMware Hybrid Cloud Extension (HCX). (Learn More) Дополнительные сведения.
    - Неизвестно. Для серверов, импортированных с помощью CSV-файла, инструмент миграции по умолчанию неизвестен. Но для серверов VMware мы рекомендуем использовать решение VMware Hybrid Cloud Extension (HCX).
4. Щелкните состояние готовности к работе в AVS. Вы можете просмотреть сведения о готовности сервера и сведения о самом сервере, в том числе параметры вычислительной среды, хранилища и сети.

### <a name="review-cost-estimates"></a>Проверка оценки стоимости

В сводке по оценке отображается оценочная стоимость вычислений и хранения для запущенных серверов в Azure. 

1. Просмотрите общую сумму ежемесячных расходов. Затраты суммируются для всех серверов в оцениваемой группе.

    - Оценка расходов зависит от количества необходимых узлов AVS с учетом требований к ресурсам всех серверов.
    - Так как цены на AVS вычисляются на основании числа узлов, в общей стоимости не учитываются затраты на вычисления и распределение затрат на хранение.
    - Оценка стоимости предназначена для запуска локальных серверов в AVS. При оценке AVS не учитываются затраты на PaaS или SaaS.

2. Просмотрите примерные расходы на хранилище в месяц. В этом представлении показаны совокупные затраты на хранение для оцениваемой группы, распределенные по дискам хранения разных типов. 
3. Вы можете развернуть подробные сведения об определенных серверах.

### <a name="review-confidence-rating"></a>Просмотр рейтинга достоверности

Средство оценки серверов присваивает рейтинг достоверности оценкам, основанным на производительности. Оценка — от одной звезды (самая низкая) до пяти звезд (самая высокая).

![Оценка достоверности](./media/tutorial-assess-vmware-azure-vmware-solution/confidence-rating.png)

Оценка достоверности помогает определить надежность рекомендаций по выбору размера в оценке. Рейтинг достоверности этой оценки основывается на доступности точек данных, необходимых для ее вычисления.

> [!NOTE]
> Если оценка создается на основе CSV-файла, рейтинг достоверности такой оценке не присваивается.

Ниже приведены оценки достоверности.

**Доступность точки данных** | **Оценка достоверности**
--- | ---
0–20 % | 1 звезда
21–40 % | 2 звезды
41–60 % | 3 звезды
61–80 % | 4 звезды
81–100 % | 5 звезд

[Подробнее](concepts-assessment-calculation.md#confidence-ratings-performance-based) о рейтингах достоверности.

## <a name="next-steps"></a>Дальнейшие действия

- Найдите зависимости сервера с помощью [сопоставления зависимостей](concepts-dependency-visualization.md).
- Настройте сопоставления зависимостей [без агента](how-to-create-group-machine-dependencies-agentless.md) или [на основе агента](how-to-create-group-machine-dependencies.md).
