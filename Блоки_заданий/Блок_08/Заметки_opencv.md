# Блок 8. Заметки. OpenCV.

### Как в OpenCV хранятся изображения
В OpenCV изображения хранятся в виде массива пикселей. Если у изображения один канал (например, чёрно-белое изображение), то изображение будет двумерным массивом (массив строк, каждая строка -- массив пикселей). Если изображение имеет несколько каналов (например, цветное изображение в формате `RGB`), то массив будет трёхмерный, каждый пиксель будет уже не просто значением, а массивом значений для каждого из каналов.

### Обращение к пикселям
Прочитать или записать значение для какого-нибудь пикселя можно обратившись к нему по его координатам.

```python
px = image[5][10] # 11-й пиксель на 6-й строке
px = image[5, 10] # Ещё один способ обратиться к тому же пикселю

# Если канал один
image[5][10] = 150
# Если каналов три
image[5][10] = [10, 11, 12] 
image[5][10] = (10, 11, 12)
image[5][10][0] = 10
```

Также можно получить несколько пикселей, указав диапазон их координат.
```python
px_array = image[10:12, 10:12] # 4 пикселя с координатами (10,10), (10,11), (11,10), (11,11)
image[10:12, 10:12] = (0, 0, 0) # Перезаписать диапазон пикселей
```

### Размер изображения
Размер массива (размер изображения) можно получить, обратившись к полю `shape`, который вернёт массив.
```python
image.shape # [height, width], если канал один
            # [height, width, channels], если каналов несколько
# Массив можно сразу распаковать в переменные
height, width, channels = image.shape
```

Для изменения размера изрбражения можно использовать функцию `resize()`.
```python
# Задать ширину и высоту
cv2.resize(image, [width, height])
# Задать коэффициент увеличения
cv2.resize(image, [0, 0], fx=0.5, fy=0.5)
cv2.resize(image, None, fx=0.5, fy=0.5)
```
[Документация `resize()`](https://docs.opencv.org/4.x/da/d54/group__imgproc__transform.html#ga47a974309e9102f5f08231edc7e7529d)

### Конвертация цветовых пространств
В OpenCV изображения используются в формате `BGR` (blue, green, red). Чтобы конвертировать изображения в другой формат, например, `RGB`, `HSL` или чёрно-белый, используется функция `cv2.cvtColor()` (convert color).
```python
converted_image = cv2.cvtColor(image, code)
```
[Документация `cvtColor()`](https://docs.opencv.org/4.x/d8/d01/group__imgproc__color__conversions.html#gaf86c09fe702ed037c03c2bc603ceab14)

Каждый тип преобразования имеет свой числовой код (`code`), который передаётся в функцию. К этим кодам можно обращаться через константы openCV. Вот несколько полезных констант (полный список можно посмотреть в документации openCV).

| cv2 const | code | comment |
|--|--|--|
| cv2.COLOR_BGR2RGB | 4 | BGR -> RGB |
| cv2.COLOR_RGB2BGR | 4 | RGB -> BGR |
| cv2.COLOR_BGR2GRAY | 6 | BGR -> GRAY |
| cv2.COLOR_RGB2GRAY | 7 | RGB -> GRAY |
| cv2.COLOR_GRAY2BGR | 8 | GRAY -> BGR |
| cv2.COLOR_GRAY2RGB | 8 | GRAY -> RGB |
| cv2.COLOR_BGR2HSV | 40 | BGR -> HSV (Hue: 0–179) |
| cv2.COLOR_RGB2HSV | 41 | RGB -> HSV (Hue: 0–179) |
| cv2.COLOR_HSV2BGR | 54 | HSV -> BGR (Hue: 0–179) |
| cv2.COLOR_HSV2RGB | 55 | HSV -> RGB (Hue: 0–179) |
| cv2.COLOR_BGR2HSV_FULL | 66 | BGR -> HSV (Hue: 0–255) |
| cv2.COLOR_RGB2HSV_FULL | 67 | RGB -> HSV (Hue: 0–255) |
| cv2.COLOR_HSV2BGR_FULL | 70 | HSV -> BGR (Hue: 0–255) |
| cv2.COLOR_HSV2RGB_FULL | 71 | HSV -> RGB (Hue: 0–255) |

[Документация `ColorConversionCodes`](https://docs.opencv.org/4.x/d8/d01/group__imgproc__color__conversions.html#ga4e0972be5de079fed4e3a10e24ef5ef0)


```python
# Пример конвертации в чёрно-белый
converted_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

### Конкатенация массивов
Чтобы объединить столбцы или строки разных массивов можно использовать функции `hconcat()` и `vconcat()`.
#### По горизонтали (объединение столбцов)
```python
# Horizontal concatenation
image = cv2.hconcat([img1, img2, img3])
```
[Документация `hconcat()`](https://docs.opencv.org/4.x/d2/de8/group__core__array.html#gaf9771c991763233866bf76b5b5d1776f)
#### По вертикали (объединение строк)
```python
# Vertical concatenation
image = cv2.vconcat([img1, img2, img3])
```
[Документация `vconcat()`](https://docs.opencv.org/4.x/d2/de8/group__core__array.html#ga744f53b69f6e4f12156cdde4e76aed27)

### Отражение массива
Чтобы отразить массив по горизонтали/вертикали можно использовать функцию `flip()`
```python
image = cv2.flip(image, code)
# code=0 -- Отражение относительно оси X
# code>0 -- Отражение относительно оси Y
# code<0 -- Отражение относительно обеих осей
```
[Документация `flip()`](https://docs.opencv.org/4.x/d2/de8/group__core__array.html#gaca7be533e3dac7feb70fc60635adf441)

### Рисование на изображении
#### Линия
```python
# Обязательные аргументы
cv2.line(img, pt1, pt2, color)
cv2.line(img, [10, 10], [50, 50], [0, 0, 255])
# Все аргументы
cv2.line(img, pt1, pt2, color, thickness, lineType, shift)
```
[Документация `line()`](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga7078a9fae8c7e7d13d24dac2520ae4a2)

#### Прямоугольник
```python
# Обязательные аргументы
cv2.rectangle(img, pt1, pt2, color)
cv2.rectangle(img, [10, 10], [20, 20], [0, 0, 255])
# Все аргументы
cv2.rectangle(img, pt1, pt2, color, thickness, lineType, shift)
```
[Документация `rectangle()`](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga07d2f74cadcf8e305e810ce8eed13bc9)

#### Стрелка
```python
# Обязательные аргументы
cv2.arrowedLine(img, pt1, pt2, color)
cv2.arrowedLine(img, [10, 10], [50, 50], [0, 0, 255])
# Все аргументы
cv.arrowedLine(img, pt1, pt2, color, thickness, line_type, shift, tipLength)
```
[Документация `arrowedLine()`](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga0a165a3ca093fd488ac709fdf10c05b2)

#### Круг
```python
# Обязательные аргументы
cv2.circle(img, center, radius, color)
cv2.circle(img, [50, 50], 10, [0, 0, 255])
# Все аргументы
cv2.circle(img, center, radius, color, thickness, lineType, shift)
```
[Документация `circle()`](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#gaf10604b069374903dbd0f0488cb43670)

#### Текст
```python
# Обязательные аргументы
cv2.putText(img, text, org, fontFace, fontScale, color)
cv2.putText(img, "text", [50, 50], cv2.FONT_HERSHEY_SIMPLEX, 1, [0, 0, 255])
# Все аргументы
cv2.putText(img, text, org, fontFace, fontScale, color, thickness, lineType, bottomLeftOrigin)
```
[Документация `putText()`](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga5126f47f883d730f633d74f07456c576)
