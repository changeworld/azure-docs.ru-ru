---
title: Azure Stack пограничной мини-руководством по безопасности R | Документация Майкрософт
description: Описываются соглашения о безопасности, рекомендации, рекомендации и объясняется, как безопасно устанавливать и использовать мини-устройство с Azure Stack пограничным устройством R.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: alkohli
ms.openlocfilehash: eb42a9a77927d8577dfec3c9167294eb8f809fec
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "100382636"
---
# <a name="azure-stack-edge-mini-r-safety-instructions"></a>Azure Stack пограничных инструкций по безопасности R

![Значок предупреждения 1 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ чтение уведомления о безопасности значок "чтение ](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png)
 **сведений о безопасности и работоспособности** "

Ознакомьтесь со всеми сведениями о безопасности, приведенными в этой статье, прежде чем использовать мини-устройство с Azure Stack пограничным устройством R, составить один пакет аккумулятора, один подключенный к сети блок питания, один адаптер питания модуля и один серверный модуль. Несоблюдение инструкций может привести к пожару, электрическим ударам, травмаху или повреждению свойств. Прочитайте все сведения о безопасности ниже, прежде чем использовать Azure Stack пограничных Мини-R.

## <a name="safety-icon-conventions"></a>Условные обозначения сведений о безопасности

Ниже перечислены ключевые слова для подписывания предупреждений опасности.

| Значок | Описание |
|:--- |:--- |
| ![Символ опасности](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)| **Опасность:** Обозначает опасную ситуацию, которая, в противном случае, приведет к смерти или серьезной травме. <br> **Предупреждение:** Указывает на опасную ситуацию, которая может привести к смерти или серьезным травмам. <br> **Внимание!** Указывает на опасную ситуацию, которая может привести к незначительному или умеренному травме.|
|

При настройке и запуске мини-устройства R для Azure Stack пограничных устройств можно использовать следующие значки опасности:

| Значок | Описание |
|:--- |:--- |
| ![Сначала прочесть все инструкции](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png) | Сначала прочесть все инструкции |
| ![Значок "Примечание"](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **ПРИМЕЧАНИЕ.** | Указывает на важные сведения, не связанные с угрозой здоровью и жизни человека. |
| ![Символ опасности](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) | Символ опасности |
| ![Значок «Электрошок»](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) | Опасность поражения электрическим током |
| ![Только использование внутри](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) | Только использование внутри |
| ![Значок «Нет компонентов, подлежащих самостоятельному ремонту»](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) | Нет элементов, подобслуживаемых пользователем. Не проводите техническое обслуживание устройства без должной подготовки. |
|

## <a name="handling-precautions-and-site-selection"></a>Обработка мер предосторожности и выбора сайта

На Azure Stack пограничных устройствах R имеется следующая обработка предосторожностей и критериев выбора сайта:

![Значок предупреждения 2 значок ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ электрической электромагнитной сети ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ значок без обслуживаемых элементов ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **Предупреждение:**

* Проверьте состояние устройства *в момент получения*. Если корпус устройства поврежден, обратитесь в [Служба поддержки Майкрософт](azure-stack-edge-placeholder.md) , чтобы получить замену. Не пытайтесь работать с поврежденным устройством.
* Если вы подозреваете, что устройство работает неправильно, [обратитесь в Служба поддержки Майкрософт](azure-stack-edge-placeholder.md) , чтобы получить замену. Не пытайтесь проводить техническое обслуживание самостоятельно.
* Устройство содержит компоненты, которые не подлежат обслуживанию пользователем. Внутри корпуса присутствует опасный уровень напряжения, тока и электроэнергии. Не открывайте корпус. Верните устройство в Майкрософт для проведения обслуживания.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 3 внимание!**

Рекомендуется использовать систему:

* От источников тепла, включая прямые солнечные и аппаратные радиочастоту.
* В расположениях, не предоставляемых влажность или дождя.
* Размещается в пространстве, в котором уменьшается вибрация и снижается физическая нагрузка.  Система разработана для ударов и вибрации в соответствии с MIL-STD-810G.
* Изолировано от сильных электромагнитных полей, созданных электрическими устройствами.
* Не разрешайте системе никаких ликвидных или ни одного внешнего объекта. Не помещайте напитки или другие контейнеры жидкостей в систему или рядом с ней.

![Значок предупреждения 4 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок "обслуживаемые компоненты ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **" отсутствует внимание!**

* Это оборудование содержит литий-аккумулятор. Не пытайтесь обслуживать пакет аккумулятора. Батареи в этом оборудовании не обслуживаются пользователями. Риск развертывания, если батарея заменена неверным типом.

![Значок предупреждения 5 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **. внимание!**

Зарядка пакета аккумулятора только в том случае, если он входит в состав Azure Stack пограничной устройства R, не взимается как отдельное устройство.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 6 внимание!**

* Параметр включения/выключения в пакете аккумулятора предназначен для отключения аккумулятора только серверному модулю. Если адаптер питания подключен к пакету аккумулятора, питание передается в модуль сервера, даже если параметр находится в положении OFF.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 7 внимание!**

* Не записывает и не выкоротких пакетов аккумулятора. Его необходимо перезапустить или удалить должным образом.

![Значок предупреждения 8 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **. внимание!**

* В вместо использования указанного источника питания AC/DC эта система также имеет возможность использовать заданный в поле Тип 2590 аккумулятор. В этом случае конечный пользователь должен убедиться, что он соответствует всем применимым требованиям безопасности, транспорту, окружающей среде и другим нормативным и местным нормам.
* При работе с системой типа "батарея 2590" используйте батарею в условиях использования, заданных производителем батареи.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 9 внимание!**

Это устройство имеет два порта SFP +, которые могут использоваться с оптическими приемопередатчиками. Чтобы избежать опасных лазерных излучений, используйте только с приемопередатчиками класса 1.

## <a name="electrical-precautions"></a>Меры предосторожности в отношении электрооборудования

На Azure Stack пограничном устройстве R есть следующие электрические меры предосторожности:

![Значок предупреждения 10 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) ![ значок электрического удара ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **Предупреждение:**

При использовании с адаптером источника питания:

* Обеспечьте безопасное соединение с землей для кабеля электропитания. Кабель с чередованием (AC) имеет три провода заземления (вилка с заземлением). Эта вилка подходит только к заземленной розетке переменного тока. Не пренебрегайте заземляющим контактом.
* Так как штекер кабеля электропитания является основным средством отключения, убедитесь, что розетки расположены вблизи устройства и в удобном для вас месте.
* Отключите шнуры питания (выполнив подключение через разъем, а не шнур) и Разъедините все кабели, если выполняются следующие условия.

  * Кабель питания и вилка потерты или повреждены любым другим образом.
  * Устройство доступно для дождя, избыточного влажность или другого Liquids.
  * Устройство было удалено, и регистр устройств поврежден.
  * По вашему мнению, устройство требует обслуживания или ремонта.
* Перед перемещением устройства, или если вам кажется, что оно неисправно, полностью отключите его от электросети.

* Используйте подходящий источник питания с защитой от электрической перегрузки, чтобы обеспечить соответствие следующим требованиям:

* Напряжение: 100-240 вольт AC
* Текущий: 1,7 Амперес
* Частота: от 50 до 60 Гц

![Значок предупреждения 11 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок электрического электричества ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **Предупреждение:**

* Не пытайтесь изменить или использовать шнуры питания сети, отличные от устройств, поставляемых с оборудованием.

![Значок предупреждения 12 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок электрических ударов ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ . Используйте только ](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) **Предупреждение:**

* Источник питания, обозначенный этим символом, оценивается только для использования.

## <a name="regulatory-information"></a>Сведения о соответствии нормам

Ниже приведены сведения о соответствии нормативным требованиям для устройства Azure Stack пограничных устройств R, номера модели соответствия: TMA01.

Устройство мини R Azure Stack пограничной Организации предназначено для использования с НРТЛ в списке (UL, CSA, ETL и т. д.), а также для оборудования IEC/EN 60950-1 или IEC/EN 62368-1 (отмечено в соответствии с маркировкой CE).

В странах, отличных от США и Канады, сетевые кабели (не поставляемые вместе с оборудованием) не должны устанавливаться вместе с этим оборудованием, если их длина превышает 3 метров.

Оборудование предназначено для использования в следующих средах:

| Среда | Характеристики |
|:---  |:--- |
| Спецификации температуры системы | <ul><li>Температура хранения: – 20 &deg; c – 50 &deg; c (– 4 &deg; f-122 &deg; f)</li><li>Непрерывная работа: 0 &deg; c – 40 &deg; c (32 &deg; F – 104 &deg; f)</li></ul> |
| Спецификации относительной влажности (RH) | <ul><li>Хранилище: от 5% до 95% относительной влажности</li><li>Относительная влажность при эксплуатации: 10% до 90%</li></ul>|
| Максимальные характеристики высоты | <ul><li>Эксплуатация: 15 000 футов (4 572 метров)</li><li>Без операционной системы: 40 000 футов (12 192 метров)</li></ul>|

> ![Значок уведомления — 2 Примечание ](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **.** &nbsp; изменения или изменения, внесенные в оборудование, не утвержденные корпорацией Майкрософт, могут аннулировать права пользователя на эксплуатацию оборудования.

#### <a name="canada-and-usa"></a>Для КАНАДЫ и США:

> ![Значок уведомления — 3 Примечание ](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **.** &nbsp; это оборудование было протестировано и найдено в соответствии с ограничениями для класса a Digital Device, согласно части 15 правил FCC. Ограничения разработаны для того, чтобы обеспечить надежную защиту от негативного влияния при эксплуатации оборудования в коммерческой среде. Это оборудование генерирует, использует и излучает радиочастотную энергию. Поэтому ненадлежащая установка и использование могут привести к помехам в радиосвязи. Работа этого оборудования в жилых помещениях, вероятнее всего, может вызвать вредоносные помехи. в этом случае пользователю потребуется устранить помехи с учетом их собственных расходов.

USB-адаптер Нетжеар A6150 WiFi, поставляемый с этим оборудованием, предназначен для обработки вблизи отдела кадров, протестированного на соответствие требованиям, характерным для надеты. Ограничение в САР, заданное FCC, составляет 1,6 Вт/кг, если среднее значение превышает 1 g ткани. При включении продукта или его использовании во время надеты в тексте, сохраните на расстоянии 10 мм от тела, чтобы обеспечить соответствие требованиям к выдержки RF.

USB-адаптер Нетжеар A6150 WiFi соответствует стандарту ANSI/IEEE C 95.1-1999 и был протестирован в соответствии с методами и процедурами измерения, указанными в УТСО бюллетене 65 дополнения C.

Нетжеар A6150 для определенной ставки Абсорптион (САР): среднее 1,18 Вт/кг среднего по 1 g из ткани

USB-адаптер Нетжеар A6150 WiFi должен использоваться только с утвержденными антеннами. Это устройство и его антенны не должны размещаться или работать совместно с другими антеннами и передатчиками, за исключением случаев, в соответствии с процедурами многопередатчика FCC. Для продуктов, доступных на США рынке, можно работать только с каналом 1 ~ 11. Выбор других каналов невозможен.

Операция в полосе 5150 – 5250 МГц используется только для того, чтобы снизить вероятность возникновения вредоносных помех в совместных спутниковых маркетинговых системах.

![Предупреждение о нормативных требованиях — использование помещений](./media/azure-stack-edge-mini-r-safety/regulatory-information-indoor-use-only.png)

Пользователям рекомендуется, чтобы лепестки с высокой степенью выделяются как основные пользователи (приоритетные пользователи) полос 5250 – 5350 МГц и 5650 – 5850 МГц, и эти лепестки могут вызвать помехи и/или повредить устройства LE-LAN.

Это оборудование создает, использует и может исмешать энергопотребление радиочастоты и, если оно не установлено и не используется в соответствии с инструкциями, может вызвать опасные помехи для взаимодействия с радио. Однако нет никакой гарантии, что помехи не будут происходить в конкретной установке.

Если это оборудование вызывает опасные помехи для приема радио или телевизионных передач, которые можно определить путем отключения и включения оборудования, пользователю рекомендуется попытаться устранить помехи одним или несколькими из следующих мер.

- Переориентировать или переыскать полученную антенну.
- Увеличение расстояния между оборудованием и получателем.
- Подключите оборудование к розетке в цепи, отличной от той, к которой подключен получатель.
- Для получения помощи свяжитесь с дилером или опытным специалистом по радио и ТЕЛЕВИДЕНИю.

Чтобы получить дополнительные сведения о проблемах с помехами, перейдите на веб-сайт FCC по адресу [fcc.gov/cgb/consumerfacts/interference.html](https://www.fcc.gov/consumers/guides/interference-radio-tv-and-telephone-signals). Вы также можете позвонить на FCC по телефону 1-888-CALL FCC, чтобы запросить помехи и таблицы фактов с поддержкой телефонных помех.

Дополнительные сведения о безопасности радиофрекуенци можно найти на веб-сайте FCC по адресу [https://www.fcc.gov/general/radio-frequency-safety-0](https://www.fcc.gov/general/radio-frequency-safety-0) и на веб-сайте отрасли для Канады по адресу [http://www.ic.gc.ca/eic/site/smt-gst.nsf/eng/sf01904.html](http://www.ic.gc.ca/eic/site/smt-gst.nsf/eng/sf01904.html) .

Этот продукт демонстрирует соответствие требованиям EMC в условиях, которые включают использование совместимых периферийных устройств и экранированных кабелей между компонентами системы. Важно использовать совместимые периферийные устройства и экранированные кабели между компонентами системы, чтобы снизить вероятность возникновения помех для Радио, телевизионных наборов и других электронных устройств.

Это оборудование соответствует требованиям части 15 правил ФКС, а также промышленным стандартам Канады в области оборудования радиопередачи, не требующего лицензирования. Эксплуатация допускается при следующих двух условиях: 1) устройство не создает вредных помех; 2) устройство принимает любые помехи, включая помехи, которые могут привести к нарушению работы устройства.

![Предупреждение о соответствии нормативным требованиям 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)

CAN ICES-3(A)/NMB-3(A)

Корпорация Майкрософт, один из Microsoft Way, Redmond, WA 98052, США

США: (800) 426-94-00

Канада: (800) 933-47-50

Нетжеар A6150 WiFi, Идентификатор FCC USB-адаптера: PY318300429
 
МФ идентификатор USB-адаптера нетжеар A6150 WiFi: 4054A-18300429

USB-адаптер Нетжеар A6150 WiFi, поставляемый с этим оборудованием, соответствует стандарту САР для общих или неуправляемых ограничений экспозиции в МФ RSS – 102 и был протестирован в соответствии с методами и процедурами измерения, указанными в стандарте IEEE 1528. Поддерживать по крайней мере 10-миллиметровый расстояние для тела надеты условия.

USB-адаптер Нетжеар A6150 WiFi соответствует установленному пределу радиочастотной выдержки Канады, заданному для неуправляемой среды, и является надежной для предполагаемой операции, как описано в руководстве. Дальнейшее сокращение частоты радиоволн может быть достигнуто за счет того, что продукт должен быть максимально доступен из тела или путем настройки устройства на более низкую выходную мощность, если такая функция доступна.

В приведенном выше разделе США можно просмотреть таблицу с усреднением по 1 g для каждого продукта.

![Предупреждение о соответствии нормативным требованиям 2](./media/azure-stack-edge-mini-r-safety/regulatory-information-2.png)

#### <a name="european-union"></a>ЕВРОПЕЙСКИЙ СОЮЗ:

Запросите копию Декларации ЕС для этого оборудования. Отправьте сообщение электронной почты с запросом кода на адрес [CSI_Compliance@microsoft.com](mailto:CSI_Compliance@microsoft.com).

USB-адаптер Нетжеар A6150 WiFi, поставляемый с этим оборудованием, соответствует директиве 2014/53/ЕС и может также быть предоставлен по запросу.

![Значок предупреждения 13, ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **Предупреждение:**  

Устройство относится к оборудованию класса А. В домашних условиях этот продукт может вызывать радиопомехи, и в этом случае пользователю потребуется принять адекватные меры.

Утилизация использованных элементов питания, электрического и электронного оборудования:

![Значок предупреждения 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Такой символ, указанный на самом устройстве, его элементе питания или упаковке, означает, что устройство и любые его элементы питания не должны утилизироваться вместе с бытовыми отходами. Пользователь обязан передать устройство и его элементы питания в уполномоченный пункт сбора для утилизации элементов питания, электрического и электронного оборудования. Раздельный сбор и утилизация отходов помогают экономить природные ресурсы и предотвращать негативное влияние на здоровье людей и окружающую среду из-за наличия опасных веществ в составе элементов питания, электрического и электронного оборудования, а также их неправильной утилизации. За дополнительными сведениями о местах сбора использованных элементов питания и электрического оборудования обратитесь в местные органы власти, местную службу утилизации бытовых отходов или магазин, где вы приобрели устройство. Свяжитесь с erecycle@microsoft.com, чтобы получить дополнительную информацию по утилизации отходов электрического и электронного оборудования.

Устройство содержит кнопочный элемент питания.

USB-адаптер Нетжеар A6150 WiFi, поставляемый с этим оборудованием, предназначен для последующего вмешательства в отдел кадров и тестируется на соответствие требованиям (см. ниже) для основного текста. При переносе продукта или использовании его при надеты в своем тексте сохраняйте расстояние от 10mm в тексте, чтобы обеспечить соответствие требованиям RF.

**Нетжеар A6150 для определенной ставки абсорптион (САР):** среднее 0,54 Вт/кг по сравнению с 10G ткани

 
Это устройство может действовать во всех состояниях участников ЕС. Следите за национальными и местным законодательством, в которых используется устройство. Это устройство может использоваться только при работе в диапазоне частот 5150-5350 МГц в следующих странах:  

![Страны ЕС, для которых требуется только использование внутри](./media/azure-stack-edge-mini-r-safety/mini-r-safety-eu-indoor-use-only.png)

В соответствии со статьей 10.8 (a) и 10.8 (b) красного цвета, приведенная ниже таблица содержит сведения об используемых диапазонах частот и максимальной мощности радиочастотной передачи Нетжеар беспроводных продуктов для продажи в ЕС:

**Wi-Fi**

| Диапазон частот (МГц) | Используемые каналы | Максимальная мощность передачи (dBm/mW) |
| --------------------- | ------------- | --------------------------- |
| 2400 — 2483.5 | 1-13    | ОДФМ: 19,9 dBm (97,7 mW) <br> ККК: 17,9 dBm (61,7 mW) |
| 5150-5320   | 36-48   | 22,9 dBm (195 mW) |
| 5250-5350   | 52-64   | 22,9 dBm (195 mW) с помощью TPC <br> 19,9 dBm (97,7 mW), не являющийся TPC |
| 5470-5725   | 100-140 | 29,9 dBm (977 mW) с помощью TPC <br> 29,6 dBm (490 mW), не являющийся TPC |

Microsoft Ireland Sandyford Ind Est Dublin D18 KX32 IRL (Ирландия)

Телефон: +353 1 295 3826

Факс: +353 1 706 4110

#### <a name="singapore"></a>Сингапур:

USB-адаптер Нетжеар A6150 WiFi, поставляемый с этим оборудованием, соответствует стандартам ИМДА.


## <a name="next-steps"></a>Дальнейшие действия

- [Подготовка к развертыванию Azure Stack пограничных Мини-R](azure-stack-edge-mini-r-deploy-prep.md)
