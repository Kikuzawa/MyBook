## Основной запуск для домена:

```
dnsenum домен.com
```

## Включение глубокого поиска поддоменов:

```
dnsenum --enum домен.com
```

## Запуск с указанием собственного DNS-сервера:

```
dnsenum --dnsserver DNS-сервер домен.com
```

## Брутфорс поддоменов с использованием списка слов:

```
dnsenum --dnsserver DNS-сервер --threads 5 --file список_слов.txt домен.com
```

## Указание количества потоков для брутфорса:

```
dnsenum --threads 10 домен.com
```

## Поиск информации о почтовых серверах (MX записи):

```
dnsenum --mx домен.com
```

## Поиск информации о DNS-серверах (NS записи):

```
dnsenum --ns домен.com
```

## Поиск SPF записей (Sender Policy Framework):

```
dnsenum --spf домен.com
```

## Поиск TXT записей:

```
dnsenum --txt домен.com
```

## Перечисление зон обратного поиска (Reverse DNS lookup):

```
dnsenum --noreverse домен.com
```

## Поиск сервера whois:

```
dnsenum --whois домен.com
```

## Экспорт результатов в файл:

```
dnsenum --output файл.xml домен.com
```

## Отключение проверки DNS кеширования:

```
dnsenum --nocache домен.com
```

## Пропуск брутфорса поддоменов:

```
dnsenum --nobrute домен.com
```

## Поиск зоны передачи (zone transfer):

```
dnsenum --zone домен.com
```

## Поиск доменов второго уровня:

```
dnsenum --sub домен.com
```