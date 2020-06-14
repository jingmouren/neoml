# Класс CCrossEntropyLossLayer

<!-- TOC -->

- [Класс CCrossEntropyLossLayer](#класс-ccrossentropylosslayer)
    - [Настройки](#настройки)
        - [Применение Softmax](#применение-softmax)
        - [Вес слоя](#вес-слоя)
        - [Урезание градиентов](#урезание-градиентов)
        - [Подсчет градиентов второго входа](#подсчет-градиентов-второго-входа)
    - [Обучаемые параметры](#обучаемые-параметры)
    - [Входы](#входы)
    - [Выходы](#выходы)
        - [Значение функции потерь](#значение-функции-потерь)

<!-- /TOC -->

Класс реализует слой, вычисляющий кросс-энтропию для многоклассовой классификации.

Формула:

```c++
loss = -sum(y_i * log(z_i))
```

где:

- `i` пробегает по числу классов;
- `z_i` зависит от настроек:
  - если `IsSoftmaxApplied()`, то `z_i` это `i`-й элемент результата `softmax` над ответами сети;
  - иначе, `z_i` это `i`-й элемент ответа сети;
- `y_i` - правильный ответ (`0` или `1` в зависимости от того, к какому из классов принадлежит).

Следует обратить внимание на то, что внутри может вычисляться `softmax`. В таких случаях не рекомендуется подсоединять первый вход этого слоя к другому `softmax`.

## Настройки

### Применение Softmax

```c++
void SetApplySoftmax( bool applySoftmax )
```

Установка флага, включающего вычисление `softmax` относительно результата сети. По умолчанию `softmax` **вычисляется**.

### Вес слоя

```c++
void SetLossWeight( float lossWeight );
```

Установка коэффициента, на который будут домножаться градиенты этой функции потерь при обучении. По умолчанию `1`. Полезен при использовании нескольких функций потерь в одной сети.

### Урезание градиентов

```c++
void SetMaxGradientValue( float maxValue );
```

Установка максимального значения элемента в градиенте. Все значения градиента, по модулю превосходящие `GetMaxGradientValue()` будут приведены к значениям, равным по модулю `GetMaxGradientValue()`.

### Подсчет градиентов второго входа

```c++
void SetTrainLabels( bool toSet );
```

Установка флага, включающего подсчет градиентов относительно **второго** входа слоя.

## Обучаемые параметры

Слой не имеет обучаемых параметров.

## Входы

Слой имеет 2 или 3 входа:

1. Ответы сети, на которых необходимо вычислить функцию потерь. Содержит распределения вероятностей принадлежности `BatchLength * BatchWidth * ListSize` объектов над `Height * Width * Depth * Channels` классами. Если `IsSoftmaxApplied()` не включен, то необходимо, чтобы сумма по `Height * Width * Depth * Channels` для каждого объекта была равна `1`, и все элементы были неотрицательными.
2. Метки классов в одном из двух видов:
    * либо в виде блоба с данными типа `float`, имеющий те же размеры, что и блоб у первого входа. Этот блоб должен быть заполнен `0` и содержать `1` в координатах тех классов, к которым принадлежат соответствующие объекты.
    * либо в виде блоба с данными типа `int`, имеющий `BatchLength`, `BatchWidth` и `ListSize`, равные аналогичным у первого входа, и остальные размерности, равные `1`. Каждый объект в блобе содержит номер класса, к которому принадлежит этот объект.
3. *[Опционально]* Веса объектов. Блоб этого входа должен иметь те же `BatchLength`, `BatchWidth` и `ListSize`, что и у первого входа. `Height`, `Width`, `Depth` и `Channels` должны быть равны `1`.

## Выходы

Слой не имеет выходов.

### Значение функции потерь

```c++
float GetLastLoss() const;
```

Получение значения функции потерь на последнем запуске сети.