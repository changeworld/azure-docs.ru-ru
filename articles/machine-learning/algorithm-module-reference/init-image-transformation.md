---
title: Преобразование изображения init Image Трансформатионпли
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль преобразования «инициализация изображений» в конструкторе Машинное обучение Azure для инициализации преобразования изображений.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: fc0eb196ed24e413c35d64f0571ff29dc3725032
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "93421283"
---
# <a name="init-image-transformation"></a>Инициализация преобразования изображений

В этой статье описывается, как использовать модуль **преобразования «инициализация изображений** » в конструкторе машинное обучение Azure для инициализации преобразования изображений, чтобы указать способ преобразования изображения.

## <a name="how-to-configure-init-image-transformation"></a>Настройка преобразования «инициализация изображений»

1.  Добавьте модуль **преобразования «инициализация изображений** » в конвейер в конструкторе. 

2.  В поле **изменить размер** укажите, следует ли изменить размер входного изображения пил до заданного размера. Если выбрано значение "true", то можно указать размер выходного изображения по **умолчанию — 256**. 

3.  В поле **обрезать по центру** укажите, следует ли обрезать заданный пил изображение в центре. При выборе значения "true" можно указать желаемый размер выходного изображения кадрирования в **размере кадрирования** по умолчанию 224.  

4.  Для параметра **Pad** укажите, следует ли заполнять заданный пил образ на всех сторонах, используя значение 0. Если выбрано значение "true", можно указать заполнение (количество добавляемых пикселей) для каждой границы в **заполнении**.

5.  Для параметра **цветовая колебание** укажите, следует ли случайным образом изменить яркость, контрастность и насыщенность изображения.

6.  Для **оттенков серого** укажите, следует ли преобразовать изображение в оттенки серого.

7.  При **обрезке случайного изменения размера** укажите, следует ли обрезать заданный пил образ до случайного размера и пропорций. Обрезка случайного размера (от 0,08 до 1,0) исходного размера и случайного пропорций (диапазон от 3/4 до 4/3) исходного пропорций. Размер этой кадрирования, в конце концов, изменится до заданного размера.
    Обычно это используется при обучении сетей для нарождения. При выборе значения "true" можно указать ожидаемый размер выходных данных каждого края в **случайном размере**, по умолчанию 256.

8.  Для **случайного кадрирования** укажите, следует ли обрезать заданный пил образ в случайном месте. При выборе значения "true" можно указать желаемый размер выходных данных кадрирования в **случайном размере кадрирования** по умолчанию 224.

9.  Для **случайного перелистывания по горизонтали** укажите, следует ли по горизонтали переворачивать данный образ пил случайным образом с вероятностью 0,5.

10.  Для **случайного вертикального перелистывания** укажите, следует ли переворачивать заданный пил образ случайным образом с вероятностью 0,5.

11.  Для **произвольного вращения** укажите, следует ли поворачивать изображение повернутым углом. Если выбрано значение "true", можно указать в диапазоне градусов, задав угол **поворота случайным образом**(-градусы + градусы) по умолчанию 0.

12.  Для **случайного аффинных** укажите, следует ли случайное преобразование аффинных преобразований «центрировать изображение» в качестве инварианта. Если выбрано значение "true", можно указать в диапазоне градусов, чтобы выбрать в случайных вращающихся **градусах**, то есть (-градусы + градусы) по умолчанию 0.

13.  Для **случайных градаций серого** укажите, следует ли случайным образом преобразовывать изображение в градации серого с вероятностью 0,1.

14.  Для **случайной перспективы** укажите, следует ли выполнять преобразование перспективы данного образа пил случайным образом с вероятностью 0,5.


16.  Подключитесь к модулю [Apply Image трансформации](apply-image-transformation.md) , чтобы применить преобразование, указанное выше, к набору данных входного изображения.

17. Отправьте конвейер.

## <a name="results"></a>Результаты

После завершения преобразования можно найти преобразованные изображения в выходных данных модуля Apply Image Transform ( [Применить преобразование к изображению](apply-image-transformation.md) ).


## <a name="technical-notes"></a>Технические примечания  

[https://pytorch.org/docs/stable/torchvision/transforms.html](https://pytorch.org/docs/stable/torchvision/transforms.html)Дополнительные сведения о преобразовании изображений см. в разделе.

###  <a name="module-parameters"></a>Параметры модуля  

| Имя                    | Диапазон   | Тип    | По умолчанию | Описание                              |
| ----------------------- | ------- | ------- | ------- | ---------------------------------------- |
| Изменить размер                  | Любой     | Логическое значение | True    | Изменить размер входного изображения пил до заданного размера |
| Размер                    | >= 1     | Целое число | 256     | Укажите желаемый размер выходных данных          |
| Обрезать по центру             | Любой     | Логическое значение | True    | Обрезает заданное изображение пил в центре  |
| Размер кадрирования               | >= 1     | Целое число | 224     | Укажите желаемый размер выходных данных кадрирования |
| Pad                     | Любой     | Логическое значение | Неверно   | Заполнять заданный образ пил на всех сторонах, используя заданное значение "Pad" |
| Заполнение                 | >= 0     | Целое число | 0       | Заполнение для каждой границы                   |
| Цветовая колебание            | Любой     | Логическое значение | Неверно   | Случайное изменение яркости, контрастности и насыщенности изображения |
| Оттенки серого               | Любой     | Логическое значение | Неверно   | Преобразовать изображение в градации серого               |
| Кадрирование случайного изменения размера     | Любой     | Логическое значение | Неверно   | Обрезать заданное пил изображение до случайного размера и пропорций |
| Случайный размер             | >= 1     | Целое число | 256     | Ожидаемый размер выходных данных каждого края        |
| Случайный Кадрирование             | Любой     | Логическое значение | Неверно   | Обрезать заданное изображение пил в случайном месте |
| Размер случайного кадрирования        | >= 1     | Целое число | 224     | Желаемый размер выходных данных кадрирования          |
| Случайное горизонтальное перелистывание  | Любой     | Логическое значение | True    | Горизонтально Переверните заданное изображение пил с заданной вероятностью |
| Перелистывание случайных вертикальных значений    | Любой     | Логическое значение | Неверно   | По вертикали Переверните заданное изображение пил случайным образом с заданной вероятностью |
| Случайный поворот         | Любой     | Логическое значение | Неверно   | Поворот изображения по последующим углом                |
| Угол поворота случайного угла | [0180] | Целое число | 0       | Диапазон градусов для выбора          |
| Случайное аффинное           | Любой     | Логическое значение | Неверно   | Случайное аффинное преобразование в неизменном виде "Центр сохранения изображений" |
| Случайные аффинное градусы   | [0180] | Целое число | 0       | Диапазон градусов для выбора          |
| Случайные оттенки серого        | Любой     | Логическое значение | Неверно   | Случайное преобразование изображения в градации серого с вероятностью 0,1 |
| Случайная перспектива      | Любой     | Логическое значение | Неверно   | Выполняет преобразование перспективы заданного образа пил случайным образом с вероятностью 0,5 |
| Случайное стирание          | Любой     | Логическое значение | Неверно   | Случайным образом выбирает прямоугольную область в изображении и стирает Пиксели с вероятностью 0,5 |

###  <a name="output"></a>Выходные данные  

| Имя                        | Type                    | Описание                              |
| --------------------------- | ----------------------- | ---------------------------------------- |
| Преобразование "выходное изображение" | трансформатиондиректори | Преобразование «выходное изображение», которое можно подключить к модулю **применения преобразования «применить изображение** ». |

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 