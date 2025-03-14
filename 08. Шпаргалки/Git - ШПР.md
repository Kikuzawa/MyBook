## Инициализация репозитория:

```
git init
```

## Клонирование репозитория:

```
git clone url_репозитория
```

## Проверка состояния репозитория:

```
git status
```

## Добавление файлов в индекс (стейджинг):

```
git add имя_файла
```

## Добавление всех изменений в индекс:

```
git add .
```

## Коммит изменений:

```
git commit -m "Сообщение о коммите"
```

## Коммит изменений с пропуском стадии индексации (стейджинга):

```
git commit -a -m "Сообщение о коммите"
```

## Просмотр истории коммитов:

```
git log
```

## Просмотр истории с кратким выводом:

```
git log --oneline
```

## Просмотр изменений в файлах:

```
git diff
```

## Просмотр изменений в конкретном файле:

```
git diff имя_файла
```

## Отмена изменений в файле (возврат к последнему коммиту):

```
git checkout -- имя_файла
```

## Удаление файла из индекса (но не из рабочей директории):

```
git reset имя_файла
```

## Отмена последнего коммита (с сохранением изменений в рабочей директории):

```
git reset --soft HEAD~
```

## Полная отмена последнего коммита (удаление изменений):

```
git reset --hard HEAD~
```

## Создание новой ветки:

```
git branch имя_ветки
```

## Переключение на другую ветку:

```
git checkout имя_ветки
```

## Создание новой ветки и переключение на неё:

```
git checkout -b имя_ветки
```

## Слияние ветки:

```
git merge имя_ветки
```

## Удаление ветки:

```
git branch -d имя_ветки
```

## Список всех веток:

```
git branch
```

## Публикация изменений в удалённый репозиторий:

```
git push origin имя_ветки
```

## Загрузка изменений из удалённого репозитория:

```
git pull origin имя_ветки
```

## Привязка к удалённому репозиторию:

```
git remote add origin url_репозитория
```

## Просмотр удалённых репозиториев:

```
git remote -v
```

## Синхронизация ветки с удалённым репозиторием:

```
git fetch
```

## Перебазирование (rebase) ветки:

```
git rebase имя_ветки
```

## Исправление последнего коммита (без создания нового):

```
git commit --amend -m "Новое сообщение"
```

## Восстановление удалённой ветки:

```
git checkout -b имя_ветки origin/имя_ветки
```

## Сброс изменений в рабочей директории к состоянию удалённого репозитория:

```
git reset --hard origin/имя_ветки
```