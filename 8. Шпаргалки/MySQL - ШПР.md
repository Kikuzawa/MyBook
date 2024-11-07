## Создание базы данных:

```
CREATE DATABASE имя_базы;
```

## Удаление базы данных:

```
DROP DATABASE имя_базы;
```

## Создание таблицы в базе данных:

```
CREATE TABLE имя_таблицы (
    колонка1 тип данных,
    колока2 тип данных,
    ...
);
```

## Удаление таблицы:

```
DROP TABLE имя_таблицы;
```

## Добавление данных в таблицу:

```
INSERT INTO tablename (column1, column2, ...)
VALUES (value1, value2, ...);
```

## Выборка данных из таблицы:

```
SELECT column1, column2, ...
FROM tablename
WHERE condition;
```

## Обновление данных в таблице:

```
UPDATE tablename
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

## Удаление данных из таблицы:

```
DELETE FROM tablename
WHERE condition;
```

## Добавление столбца в таблицу:

```
ALTER TABLE tablename
ADD column_name datatype;
```

## Удаление столбца из таблицы:

```
ALTER TABLE tablename
DROP COLUMN column_name;
```

## Изменение типа данных столбца:

```
ALTER TABLE tablename
MODIFY COLUMN column_name new_datatype;
```

## Изменение имени столбца:

```
ALTER TABLE tablename
CHANGE old_column_name new_column_name datatype;
```