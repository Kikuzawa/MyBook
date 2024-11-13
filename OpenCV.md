OpenCV - Open-source Computer Vision Libraru

Библиотека компьютерного зрения с открытым исходным кодом — это open source библиотека компьютерного зрения, которая предназначена для анализа, классификации и обработки изображений

Широко используется в таких языках как C, C++, Python и Java.

- Панорамы улиц в картах Google и не только
- Система зрения робота PR2 компании Willow Garage
- Роботы для исследования поверхности Марса (проект NASA)
- Контроль качества монет (Центробанк Китай)
# Функциональность

![[Pasted image 20241113145628.png]]

# OpenCV в приложениях

![[Pasted image 20241113145644.png]]

# Загрузка изображения

```Python
import cv2
def loading_displaying_saving():
	img = cv2.imread('girl.jpg', cv2.IMREAD_GRAYSCALE)
	cv2.imshow('girl', img)
	cv2.waitKey(0)
	cv2.imwrite('graygirl.jpg', img
```

![[Pasted image 20241113145717.png]]

# BGR

![[Pasted image 20241113145732.png]]

# HSV

HSV — это Hue, Saturation и Value (оттенок, насыщенность и яркость).

Это цилиндрическое цветовое пространство. Цвета, или оттенки, меняются при движении по кругу цилиндра.

Вертикальная ось отвечает за яркость: от темного (0 в нижней части) до светлого сверху.

Третья ось, насыщенность, определяет тени оттенков при движении от центра к краю вдоль радиуса цилиндра (от менее к более насыщенному)

`hsv_nemo = cv2.cvtColor(nemo, cv2.COLOR_RGB2HSV)`

![[Pasted image 20241113145828.png]]

# Сегментация по цвету

```Python
light_orange = (1, 190, 200)
dark_orange = (18, 255, 255)
#бинарная маска
mask = cv2.inRange(hsv_nemo, light_orange, dark_orange)
result = cv2.bitwise_and(nemo, nemo, mask=mask)
plt.subplot(1, 2, 1)
plt.imshow(mask, cmap="gray")
plt.subplot(1, 2, 2)
plt.imshow(result)
plt.show()
```

![[Pasted image 20241113145855.png]]

```Python
light_white = (0, 0, 200)
dark_white = (145, 60, 255)
mask_white = cv2.inRange(hsv_nemo, light_white, dark_white)
result_white = cv2.bitwise_and(nemo, nemo, mask=mask_white)
plt.subplot(1, 2, 1)
plt.imshow(mask_white, cmap="gray")
plt.subplot(1, 2, 2)
plt.imshow(result_white)
plt.show()
```

![[Pasted image 20241113145916.png]]

```Python
final_mask = mask + mask_white
final_result = cv2.bitwise_and(nemo, nemo, mask=final_mask)
plt.subplot(1, 2, 1)
plt.imshow(final_mask, cmap="gray")
plt.subplot(1, 2, 2)
plt.imshow(final_result)
plt.show()
```

![[Pasted image 20241113145936.png]]

# Гауссовское размытие

```Python
blur = cv2.GaussianBlur(final_result, (7, 7), 0)
plt.imshow(blur)
plt.show()
```

![[Pasted image 20241113150001.png]]

# Фильтры
![[Pasted image 20241113150010.png]]

# Кадрирование

![[Pasted image 20241113150026.png]]

# Изменение размера

![[Pasted image 20241113150040.png]]

# Поворот

![[Pasted image 20241113150049.png]]

# Полутоновое и черно-белое изображение

