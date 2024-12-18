- Хранит набор элементов (не обязательно одного типа) в виде непрерывного блока в памяти, в котром элементы следуют строго друг за другом
- Доступ к отдельному элементу предоставляется по индексу (в Python начинается с 0)
- Поддерживает динамическое изменение размера

```python
mixed_list = [10, "20", 30.0, True]
```

# Модель памяти

Тогда чтобы получить доступ к i-ому элементу достаточно выполнить следующее простое вычисление: $address(i)=start+i*size$

![[Pasted image 20241112163850.png]]

![[Pasted image 20241112163856.png]]

# Понятие списка

```Python
>>> list('Python')
['P','y','t','h','o','n']
```

```Python
# Пустой список
>>> s = []
# Список с данными разных типов
>>> l = ['s','p',['isok'],2]
>>> s
[]
>>> l
['s','p',['isok'],2]
```

```Python
>>> a = ['P','y','t','h','o','n']
>>> b = [i*3 for i in a]
>>> b
['PPP','yyy','ttt','hhh','ooo','nnn']
```

```Python
>>> a = ['P','y','t','h','o','n']
>>> len(a)
6
```

```Python
>>> s = ['P','y','t','h','o','n']
>>> print(s[2],s[3])
t h
```

# Методы для работы со списками

- append(a) - добавляет элемент a в конец списка
```Python
>>> var = ['l','i','s','t']
>>> var.append('a')
>>> print(var)
['l','i','s','t','a']
```

- extend(L) - расширяет список, добавляя к концу все элементы списка L

```Python
>>> var = ['l','i','s','t']
>>> var.extend(['l','i','s','t'])
>>> print(var)
['l','i','s','t','a','l','i','s','t']
```

- insert(i, a) - вставляет на i позицию элемент a

```Python
>>> var = ['l','i','s','t']
>>> var.insert(2,'a')
>>> print(var)
['l','i','a','s','t']
```

- remove(a) - удаляет первое найденное значенеие элемента в списке со значением a, возвращает ошибку, если такого элемента не существует

```Python
>>> var = ['l','i','s','t','t']
>>> var.remove('t')
>>> print(var)
['l','i','s','t']
```

- pop(i) - удаляет i-ый элемент и возвращает его, если индекс не указан, удаляет последний элемент

```Python
>>> var = ['l','i','s','t']
>>> var.pop(o)
'l'
>>> print(var)
['i','s','t']
```

- index(a) - возвращает индекс элемента a

```Python
>>> var = ['l','i','s','t']
>>> var.count('t')
1
```

- sort(\[key=функция]) - сортирует список на основе функции, можно не прописывать функцию, тогла сортировка будей происходить по встроенному алгоритму

```Python
>>> var = ['l','i','s','t']
>>> var.sort()
>>> print(var)
['i','l','s','t']
```

- reverse() - разворачиет список
- 
```Python
>>> var = ['l','i','s','t']
>>> var.reverse()
>>> print(var)
['t','s','i','l']
```

- clear() - очищает список

```Python
>>> var = ['l','i','s','t']
>>> var.clear()
>>> print(var)
[]
```

# Вложенные списки

![[Pasted image 20241112165119.png]]
