## Создание индекса
Создание индекса с названием `index_name`.
```http
PUT <ELASTICSEARCH_URL>/index_name
```

## Создание типа
```http
PUT <ELASTICSEARCH_URL>/index_name/type_name
```

В последних версиях Elasticsearch рекомендуется *не создавать тип*, а использовать *тип по умолчанию* `_doc`.

## Удаление индекса
```http
DELETE <ELASTICSEARCH_URL>/index_name
```

## Создание документа
Создание документа в типе `type_name` индекса `index_name`.
```http
POST <ELASTICSEARCH_URL>/index_name/type_name
Content-Type: application/json

{
  "name": "Alen Stone",
  "job": "Full-stack Enginer"
}
```

## Обновление документа по ID
Обновление документа типа `_doc` в индексе `users` по id.
```http
PUT <ELASTICSEARCH_URL>/users/_doc/H3tVi3ABpFL-9AlTbAgj
Content-Type: application/json

{
  "name": "Richard Stone",
  "job": "Full-stack Enginer"
}
```

## Удаление документа по ID

Удаление документа типа `_doc` по id.
```http
DELETE <ELASTICSEARCH_URL>/users/_doc/H3tVi3ABpFL-9AlTbAgj
```

## Группировка нескольких запросов в один (bulk)

Elasticasearch предоставляет возможность группировки нескольких изменяющих данные запросов в один при помощи специального POST-запроса `/_bulk`.

При помощи этого запроса можно манипулировать данными в нескольких индексах и их типах одновременно.

Запрос и его тело выглядят примерно следующим образом.
```http
POST <ELASTICSEARCH_URL>/_bulk

{ "index" : { "_index" : "test", "_id" : "1" } }
{ "field1" : "value1" }
{ "create" : { "_index" : "test", "_id" : "3" } }
{ "field1" : "value3" }
{ "delete" : { "_index" : "test", "_id" : "3" } }
{ "update" : { "_index" : "test", "_id" : "1" } }
{ "doc" : { "field2" : "value2" } }

```
Возможные операции
* `create` - создание документа. Выдаёт ошибку, если документ с указанным `_id` уже существует.
* `index` - создание документа (аналогично `create`, но без ошибки, а с замещением существующего).
* `update` - обновление части документа, указанной в переданном свойстве `doc`.
* `delete` - удаление документа.

Указание `_index` является обязательным, указание `_id` - нет (это поле может быть сгенерировано автоматически).

Можно также указать `type`, но поскольку он не указан, все документы индексируются в `_doc`.

В конце тела запроса обязателен переход на новую строку.

### Пример вставки двух пользователей
```http
POST <ELASTICSEARCH_URL>/_bulk
Content-Type: application/json

{ "index": { "_index": "users", "_id" : "1" } }
{ "name": "Harry Smith", "job": "Dev Ops" }
{ "index": { "_index": "users", "_id" : "2" } }
{ "name":" Sam Brave", "job": "QA" }
   
```
