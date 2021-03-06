# Класс CChannelwiseConvLayer

<!-- TOC -->

- [Класс CChannelwiseConvLayer](#класс-cchannelwiseconvlayer)
    - [Настройки](#настройки)
        - [Размеры фильтров](#размеры-фильтров)
        - [Шаг свертки](#шаг-свертки)
        - [Дополнительные столбцы и колонки (padding)](#дополнительные-столбцы-и-колонки-padding)
        - [Использование свободных членов](#использование-свободных-членов)
    - [Обучаемые параметры](#обучаемые-параметры)
        - [Фильтры](#фильтры)
        - [Свободные члены](#свободные-члены)
    - [Входы](#входы)
    - [Выходы](#выходы)

<!-- /TOC -->

Класс реализует слой, выполняющий поканальную свёртку над набором двумерных многоканальных изображений. При такой свертке каждый канал входного блоба независимо сворачивается с соответствующим каналом фильтра. Поддерживает `padding`.

## Настройки

### Размеры фильтров

```c++
void SetFilterHeight( int filterHeight );
void SetFilterWidth( int filterWidth );
void SetFilterCount( int filterCount );
```

Устанавливает количество и размеры фильтров.

### Шаг свертки

```c++
void SetStrideHeight( int strideHeight );
void SetStrideWidth( int strideWidth );
```

Устанавливает шаг свёртки. По умолчанию оба значения равны `1`.

### Дополнительные столбцы и колонки (padding)

```c++
void SetPaddingHeight( int paddingHeight );
void SetPaddingWidth( int paddingWidth );
```

Устанавливает количество дополнительных строк и/или столбцов, заполненных нулями, которые будут добавлены по краям изображения. Например, при `SetPaddingWidth( 1 );` к изображению будут добавлены 2 колонки (по одной слева и справа). По умолчанию равны `0`.

### Использование свободных членов

```c++
void SetZeroFreeTerm(bool isZeroFreeTerm);
```

Указывает, нужно ли использовать вектор свободных членов. Если передать `true`, содержимое вектора будет заполнено нулями, и он не будет обучаться. По умолчанию `false`.

## Обучаемые параметры

### Фильтры

```c++
CPtr<CDnnBlob> GetFilterData() const;
```

Фильтры представляют собой [блоб](../DnnBlob.md) размера

- `BatchLength`, `BatchWidth`, `ListSize` равны `1`;
- `Height` равен `GetFilterHeight()`;
- `Width` равен `GetFilterWidth()`;
- `Depth` равен `1`;
- `Channels` равен `Channels` у входов.

### Свободные члены

```c++
CPtr<CDnnBlob> GetFreeTermData() const;
```

Свободные члены представляют собой блоб суммарного размера равного количеству каналов у входов.

## Входы

На каждый вход подается блоб с набором изображений. Размеры блоба:

- `BatchLength * BatchWidth * ListSize` - количество изображений в наборе;
- `Height` - высота изображений;
- `Width` - ширина изображений;
- `Channels` - количество каналов у изображений.

Размеры блобов всех входов должны совпадать.

## Выходы

Для каждого входа соответствующий выход содержит блоб с результатом свертки.

Блоб с результатами имеет следующие размеры:

- `BatchLength` равен `BatchLength` входа;
- `BatchWidth` равен `BatchWidth` входа;
- `ListSize` равен `ListSize` входа;
- `Height` рассчитывается относительно входа по формуле  
`(2 * PaddingHeight + Height - FilterHeight)/StrideHeight + 1`;
- `Width` рассчитывается относительно входа по формуле  
`(2 * PaddingWidth + Width - FilterWidth)/StrideWidth + 1`;
- `Depth` равен `1`;
- `Channels` равен `Channels` входа.
