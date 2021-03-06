# Класс CGlobalMaxPoolingLayer

<!-- TOC -->

- [Класс CGlobalMaxPoolingLayer](#класс-cglobalmaxpoolinglayer)
    - [Настройки](#настройки)
        - [Количество наибольших элементов](#количество-наибольших-элементов)
    - [Обучаемые параметры](#обучаемые-параметры)
    - [Входы](#входы)
    - [Выходы](#выходы)

<!-- /TOC -->

Класс реализует слой, выполняющий операцию `Max Pooling` над размерностями `Height`, `Width`, `Depth` с возможностью нахождения нескольких наибольших элементов.

В случае возвращения единственного наибольшего элемента, работа слоя аналогична [`C3dMaxPoolingLayer`](3dMaxPoolingLayer.md) с размером фильтра, равному размеру входа.

## Настройки

### Количество наибольших элементов

```c++
void SetMaxCount(int enumSize);
```

Устанавливает количество наибольших (не обязательно равных между собой) элементов, которые нужно найти.

## Обучаемые параметры

Слой не имеет обучаемых параметров.

## Входы

На единственный вход подается [блоб](../DnnBlob.md) с набором изображений:

- `BatchLength * BatchWidth * ListSize` - количество изображений в наборе;
- `Height` - высота изображений;
- `Width` - ширина изображений;
- `Depth` - глубина изображений;
- `Channels` - количество каналов у изображений.

## Выходы

Слой имеет один или два выхода. Первый выход содержит блоб размера:

- `BatchLength` равен `BatchLength` входа;
- `BatchWidth` равен `BatchWidth` входа;
- `ListSize` равен `ListSize` входа;
- `Height` равен `1`;
- `Width` равен `GetMaxCount()`;
- `Depth` равен `1`;
- `Channels` равен `Channels` входа.

Второй, опциональный выход имеет те же размеры, но с данными типа `int` и содержит индексы соответствующих максимумов в блобе.
