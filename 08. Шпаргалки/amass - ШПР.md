## Основной запуск для сбора поддоменов:

```
amass enum -d домен.com
```

## Глубокая разведка с активным режимом:

```
amass enum -d домен.com -active
```

## Указание пассивного режима (без активного сканирования):

```
amass enum -d домен.com -passive
```

## Сбор данных из определенного источника:

```
amass enum -d домен.com -src
```

## Использование собственного файла с API-ключами:

```
amass enum -d домен.com -config путь_к_файлу/config.ini
```

## Указание файла с доменами:

```
amass enum -df путь_к_файлу_с_доменами.txt
```

## Экспорт результатов в файл:

```
amass enum -d домен.com -o результаты.txt
```

## Указание конкретного диапазона IP-адресов:

```
amass enum -d домен.com -r диапазон_IP
```

## Ограничение по времени выполнения:

```
amass enum -d домен.com -timeout 60
```

## Выполнение с сохранением результатов в формат JSON:

```
amass enum -d домен.com -o результаты.json
```

## Запуск с использованием прокси:

```
amass enum -d домен.com -proxy http://proxy:порт
```

## Проверка уже существующих результатов:

```
amass db -d домен.com
```

## Отправка результата напрямую в Grepable файл для анализа:

```
amass enum -d домен.com -o результат.grep
```

## Вывод детальной информации:

```
amass intel -whois -d домен.com
```

## Просмотр всех доступных доменов в базе данных:

```
amass db -list
```