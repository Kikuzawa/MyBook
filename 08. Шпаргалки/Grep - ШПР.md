## Поиск строки в файле:

```
grep 'текст' имя_файла
```

## Поиск строки в нескольких файлах:

```
grep 'текст' файл1 файл2 файл3
```

## Поиск строки во всех файлах в директории:

```
grep 'текст' *
```

## Поиск строки с учетом регистра:

```
grep -i 'текст' имя_файла
```

## Поиск строки с регулярным выражением:

```
grep -E 'регулярное_выражение' имя_файла
```

## Поиск строки, не содержащей текст:

```
grep -v 'текст' имя_файла
```

## Показать номер строки с совпадением:

```
grep -n 'текст' имя_файла
```

## Показать только совпадающие строки:

```
grep -o 'текст' имя_файла
```

## Поиск с отображением контекста (строки до и после совпадения):

```
grep -C 3 'текст' имя_файла  # 3 строки контекста
```

## Поиск только в определенной колонке (например, в первом):

```
grep -P '^[текст]' имя_файла
```

## Поиск в файлах с определенным расширением:

```
grep 'текст' *.расширение
```

## Сохранение результатов поиска в файл:

```
grep 'текст' имя_файла > результаты.txt
```