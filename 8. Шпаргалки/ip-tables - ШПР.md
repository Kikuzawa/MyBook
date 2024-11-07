## Просмотр текущих правил:

```
iptables -L
```

## Просмотр правил с номерами строк:

```
iptables -L --line-numbers
```

## Добавление правила для разрешения входящего трафика на определенный порт:

```
iptables -A INPUT -p tcp --dport номер_порта -j ACCEPT
```

## Добавление правила для блокировки входящего трафика с определенного IP:

```
iptables -A INPUT -s IP-адрес -j DROP
```

## Блокировка всего входящего трафика:

```
iptables -P INPUT DROP
```

## Разрешение всех исходящих подключений:

```
iptables -P OUTPUT ACCEPT
```

## Разрешение трафика на локальном интерфейсе (localhost):

```
iptables -A INPUT -i lo -j ACCEPT
```

## Разрешение подключения по SSH (порт 22):

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## Удаление правила по номеру строки:

```
iptables -D INPUT номер_строки
```

## Удаление всех правил:

```
iptables -F
```

## Сохранение правил в файл:

```
iptables-save > /etc/iptables/rules.v4
```

## Загрузка правил из файла:

```
iptables-restore < /etc/iptables/rules.v4
```

## Перенаправление порта (настроить NAT):

```
iptables -t nat -A PREROUTING -p tcp --dport старый_порт -j REDIRECT --to-ports новый_порт
```

## Включение логирования блокированного трафика:

```
iptables -A INPUT -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
```

## Просмотр правил в таблице NAT:

```
iptables -t nat -L
```

## Блокировка входящего пинга (ICMP):

```
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

## Разрешение трафика только с конкретного IP-адреса:

```
iptables -A INPUT -s IP-адрес -j ACCEPT
iptables -A INPUT -j DROP
```

## Разрешение трафика для уже установленных подключений
```
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```
