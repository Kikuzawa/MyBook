#todo

Импорт библиотеки

`import matplotlib as mpl`

# Структура рисунка

![[Pasted image 20241112171354.png]]
![[Pasted image 20241112171400.png]]

# Matplotlib API

![[Pasted image 20241112171417.png]]

# Artists

![[Pasted image 20241112171427.png]]

# Алгоритм создания рисунка

![[Pasted image 20241112171445.png]]

# Интерфейс Pyplot

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
ax=fig.add_axes([0,0,1,1]) или ax = fig.add_axes([0, 0, 1, 1], polar=True)
plt.scatter(1.0, 1.0)
plt.savefig(‘pic.png’, fmt=‘png’)
plt.show()
```

![[Pasted image 20241112171527.png]]

![[Pasted image 20241112171537.png]]
![[Pasted image 20241112171543.png]]
![[Pasted image 20241112171549.png]]

# Графические команды

- plt.scatter() – маркер, точечное рисование
- plt.plot() - ломанная линия
- plt.text() – нанесение текста
- plt.bar() – столбчатая диаграмма
- plt.hist() – гистограмма
- plt.pie() – круговая диаграмма

# Пример

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
scatter1 = plt.scatter(0.0, 1.0)
graph1 = plt.plot([-1.0, 1.0], [0.0, 1.0])
text1 = plt.text(0.5, 0.5, 'Text on figure')
grid1 = plt.grid(True)
plt.show()
```
![[Pasted image 20241112171634.png]]

![[Pasted image 20241112171716.png]]

![[Pasted image 20241112171724.png]]

## Контейнер Figure

![[Pasted image 20241112171740.png]]

```Python
import matplotlib.pyplot as plt
import numpy as np
fig = plt.figure()
ax = fig.add_subplot(111)
box = [0.25, 0.5, 0.25, 0.25]
ax2 = fig.add_axes(box)
x = np.arange(0.0, 1.0, 0.1)
y = np.sin(x)*np.exp(x)
z = np.cos(x)*np.sin(x)
ax.plot(y)
ax.plot(z)
for ax in fig.axes:
ax.grid(True)
plt.show()
```

## Контейнер Axes

![[Pasted image 20241112171820.png]]

```Python
mport matplotlib.pyplot as plt import
numpy as np
x = [0,1,2,3,4,5,6,7,8,9,10]
y = [-1,-2,-3,-4,-5,-6,-7,-8,-9,-10,-11]
fig = plt.figure()
ax = fig.add_subplot(211)
line = ax.plot(x, y, '-', color='blue', linewidth=2)
ax2 = fig.add_subplot(212)
n, bins, rectangles = ax2.hist(np.random.randn(1000), 50, facecolor='yellow’)
for ax in fig.axes:
ax.grid(True)
plt.show()
```

## Область рисования

![[Pasted image 20241112171903.png]]

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
x = np.arange(20)
y = np.exp(-np.sin(x))
x0 = 0.05
y0 = 0.05
dx = 0.9
dy = 0.9
rect = [x0, y0, dx, dy]
ax = fig.add_axes(rect)
ax.plot(x, y, 'g’)
ax.text(7.5, 0.25, u'Зелёный график', color='g’)
ax.grid(True)
ax.set_title(u'Метод add_axes’)
ax.set_xlabel(u'Ось абсцисс’)
ax.set_ylabel(u'Ось ординат’)
plt.show()
```

## Область для рисования

![[Pasted image 20241112171958.png]]

```Python
ig = plt.figure()
x = np.arange(20)
y = np.exp(-np.sin(x))
ax = fig.add_subplot(111)
ax.plot(x, y, 'g’)
ax.text(7.5, 0.25, u'Зелёный график',
color='g’)
ax.grid(True)
plt.show()
ax.set_title(u'Метод add_subplot’)
ax.set_xlabel(u'Ось абсцисс’)
ax.set_ylabel(u'Ось ординат’)
plt.show()
```

## Мультиокна

![[Pasted image 20241112172041.png]]

```Python
fig, subplots = plt.subplots(nrows=2,ncols=2, sharex=True, sharey=True)
x = np.arange(20)
i = -1
for ax in fig.axes:
	i += 1
	y = np.random.rand(np.size(x))
	ax.grid(True)
	ax.text(0.5, 0.9, str(i+1), color='red')
	ax.plot(x, y)
plt.show()
```

---

![[Pasted image 20241112172121.png]]

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = plt.subplot2grid((2,2), (0, 0))
ax2 = plt.subplot2grid((2,2), (0, 1))
ax3 = plt.subplot2grid((2,2), (1, 0))
ax4 = plt.subplot2grid((2,2), (1, 1))
i = -1
jj = [0, 0, 1, 1]
kk = [0, 1, 0, 1]
for ax in fig.axes:
	i += 1
	stext = 'ax%d - %d, %d' % (i+1, jj[i], kk[i])
	ax.text(0.4, 0.5, stext, color='b’)
	ax.grid(True)
plt.show()
```

---

![[Pasted image 20241112172230.png]]

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
egrid = (4,3)
ax1 = plt.subplot2grid(egrid, (0, 0), colspan=3)
ax2 = plt.subplot2grid(egrid, (1, 0), rowspan=2)
ax3 = plt.subplot2grid(egrid, (1, 1), rowspan=2,colspan=2)
ax4 = plt.subplot2grid(egrid, (3, 0), colspan=2)
ax5 = plt.subplot2grid(egrid, (3, 2))
for i, ax in enumerate(fig.axes):
	ax.text(0.5, 0.5, "ax%d" % (i+1), va="center", ha="center", color='red',transform=ax.transAxes)
	ax.grid(True)
plt.tight_layot()
plt.show()
```


# Дополнительная координатная ось

![[Pasted image 20241112172324.png]]

```Python
x = np.arange(-2*np.pi, 2*np.pi, 0.2)
y = np.sin(x) * np.cos(x)
f = np.sin(x) + np.cos(x)
fig = plt.figure()
ax1 = fig.add_subplot(111)
ax2 = ax1.twinx()
line1 = ax1.plot(x, f, label = u'Сумма cos и sin', color='green’)
ax1.set_xlabel(u'Аргумент’)
ax1.set_ylabel(u'Функция 1', color='green’)
ax1.grid(True, color='green’)
ax1.tick_params(axis='y', which='major', labelcolor='green’)
ax1.legend(loc=2)
line2 = ax2.plot(x, y*333, label = u'Произведение cos и sin', color='red’)
ax2.set_ylabel(u'Функция 2', color='red’)
ax2.grid(True, color='red’)
ax2.tick_params(axis='y',
which='major', labelcolor='red’)
ax2.legend(loc=1)
ax1.set_title(u'Несколько разномасштабных переменных’)
plt.title(u'Несколько разномасштабных переменных’)
plt.show()
```


===

![[Pasted image 20241112172448.png]]
![[Pasted image 20241112172453.png]]