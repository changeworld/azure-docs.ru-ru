---
title: Создание математических функций определения пользовательского интерфейса
description: Описывает функции, используемые при выполнении математических операций.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 022e59d847a4d89b4243223637fc6fd1e73255a9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87098172"
---
# <a name="createuidefinition-math-functions"></a>Математические функции CreateUiDefinition

Функции позволяют выполнять математические операции.

## <a name="add"></a>add

Складывает два числа и возвращает результат.

В следующем примере возвращается `3`.

```json
"[add(1, 2)]"
```

## <a name="ceil"></a>ceil

Возвращает наибольшее целочисленное значение, которое больше или равно указанному числу.

В следующем примере возвращается `4`.

```json
"[ceil(3.14)]"
```

## <a name="div"></a>div

Делит первое число на второе число и возвращает результат. Результат всегда представляет собой целое число.

В следующем примере возвращается `2`.

```json
"[div(6, 3)]"
```

## <a name="floor"></a>floor

Возвращает наибольшее целочисленное значение, которое меньше или равно указанному числу.

В следующем примере возвращается `3`.

```json
"[floor(3.14)]"
```

## <a name="max"></a>max

Возвращает большее из двух чисел.

В следующем примере возвращается `2`.

```json
"[max(1, 2)]"
```

## <a name="min"></a>мин

Возвращает меньшее из двух чисел.

В следующем примере возвращается `1`.

```json
"[min(1, 2)]"
```

## <a name="mod"></a>mod

Делит первое число на второе число и возвращает остаток.

В следующем примере возвращается `0`.

```json
"[mod(6, 3)]"
```

В следующем примере возвращается `2`.

```json
"[mod(6, 4)]"
```

## <a name="mul"></a>mul

Умножает два числа и возвращает результат.

В следующем примере возвращается `6`.

```json
"[mul(2, 3)]"
```

## <a name="rand"></a>rand

Возвращает случайное целое число в пределах заданного диапазона. Эта функция не создает криптографически безопасное случайное число.

В следующем примере может вернуться `42`.

```json
"[rand(-100, 100)]"
```

## <a name="range"></a>range

Создает последовательность целых чисел в пределах заданного диапазона.

В следующем примере возвращается `[1,2,3]`.

```json
"[range(1, 3)]"
```

## <a name="sub"></a>sub

Вычитает второе число из первого числа и возвращает результат.

В следующем примере возвращается `1`.

```json
"[sub(3, 2)]"
```

## <a name="next-steps"></a>Дальнейшие действия

* Общие сведения об Azure Resource Manager см. в [этой статье](../management/overview.md).
