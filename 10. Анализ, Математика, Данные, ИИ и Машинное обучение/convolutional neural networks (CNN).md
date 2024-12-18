#todo 


Сверточные нейронные сети (convolutional neural networks, CNN) —переиспользование одних и тех же частей нейронной сети для работы с разными маленькими, локальными участками входов (1988). Прототип коры головного мозга.

Свертка — это процедура агрегации информации о пикселе и соседних с ним пикселях.


![[Pasted image 20241115105145.png]]


![[Pasted image 20241115105359.png]]

[[Зрительная кора головоного мозга]]

# Задачи, решаемые сверточными НС

Основная задача: обработка изображений, автоматизация извлечения признаков

![[Pasted image 20241115110633.png]]

![[Pasted image 20241115110638.png]]
# Свёртка ч/б изображения с фильтром FxF

![[Pasted image 20241115105743.png]]

# Свёртка цветного изображения с фильтром FxF

![[Pasted image 20241115105803.png]]

![[Pasted image 20241115105811.png]]

# Ядро свертки 1 на 1

![[Pasted image 20241115105820.png]]

![[Pasted image 20241115105826.png]]

# Принципы работы CNN

1. Фильтр
2. Формирование карты признаков
3. Уплотнение (объединение, pooling) карты признаков сверточного слоя -> уплотненная карта

![[Pasted image 20241115105928.png]]
![[Pasted image 20241115105933.png]]
![[Pasted image 20241115110001.png]]![[Pasted image 20241115110009.png]]
![[Pasted image 20241115110019.png]]
![[Pasted image 20241115110024.png]]


# Общая архитектура сверточной НС

![[Pasted image 20241115110040.png]]

![[Pasted image 20241115110046.png]]

![[Pasted image 20241115110159.png]]
# Пример работы CNN

![[Pasted image 20241115110054.png]]

# Упрощенная архитектура сети Яна Лекуна для [[Датасет MNIST|MNIST]]


![[Pasted image 20241115110139.png]]

![[Pasted image 20241115110146.png]]

# Дополнительные слови

Слой пакетной нормализации - это приём, который помогает упростить обучение очень глубоких нейронных сетей путём стандартизации входов в слой для каждого мини-пакета. Стандартизация входов стабилизирует процесс обучения и таким образом уменьшает количество эпох обучения глубоких нейросетей.

Слой исключения (Dropout) - это приём регуляризации, который справляется с переобучением и чрезмерным обобщением

# Примеры призаков

![[Pasted image 20241115110234.png]]

![[Pasted image 20241115110238.png]]

# Ключевые вопросы

Какие фильтры использовать?
◦ фильтры со случайными значениями на основе нормального или какого- либо другого распределения. При достаточном обучении и объёме данных нейросеть сама создаёт подходящие фильтры для извлечения наиболее значимых признаков

Сколько фильтров в каждой свертке?
◦ универсальное правило — использовать фильтры с нечётными размерами (3×3, 5×5, 7×7). Также крупным фильтрам обычно предпочитают маленькие, но возможны и компромиссные соотношения, которые надо вычислять эмпирически.

# [[Известные сверточные НС]]


# [[Трансфертное обучение (Transfer Learning)]]

# [[CNN в Keras]]

# [[Faster RCNN]]

# [[YOLO (You Look Only Once)]]



