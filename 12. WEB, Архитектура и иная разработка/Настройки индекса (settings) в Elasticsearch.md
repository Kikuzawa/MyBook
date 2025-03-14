

У каждого индекса при создании задаётся набор настроек.

## Получение текущих настроек индекса
Текущие настройки индекса можно получить по GET-запросу `_settings`.
```http
GET <ELASTICSEARCH_URL>/users/_settings
```

```js
/* response body */
{
  "users": {
    "settings": {
      "index": {
        "number_of_shards": "1",
        "provided_name": "users",
        "creation_date": "1582885295047",
        "number_of_replicas": "1",
        "uuid": "cNAP5avkRueTAkUGNxsHew",
        "version": {
          "created": "7030299"
        }
      }
    }
  }
}
```

*Количество шардов* указано в *настройках индекса* в *свойстве* `number_of_shards`.

Количество *реплик* указано в *настройках индекса* в *свойстве* `number_of_replicas`.

