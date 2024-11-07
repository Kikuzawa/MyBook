## Сравнение [[Динамическая маршрутизация OSPF|OSPF]] и EIGRP

| OSPF | EIGRP |
| :--- | :---- |
| Протокол состояния каналов | Дистанционно векторный протокол |
| <span style="color:green">+ Лучше масштабируется, подходит для крупных сетей</span> | <span style="color:red">- Применим в малых и средних сетях</span> |
| <span style="color:red">- Труднее в настройке</span> | <span style="color:green">+ Более легок в настройке</span> |
| <span style="color:green">+ Меньшее время в сходимости</span> | <span style="color:green">+ Менее нагружает ресурсы</span> |
| <span style="color:green">+ Работает практически на любом оборудовании</span> | <span style="color:red">- Проприетарный протокол Cisco</span> |
| <span style="color:green">+ Наиболее популярен</span> |  |

## Каждый процесс EIGRP обслуживает 3 таблицы

- [[Таблица «соседей» (neighbor table)]]
- [[Таблица топологии сети]]
- [[Таблица маршрутизации]]
