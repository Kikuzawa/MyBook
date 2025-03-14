[[Docker - ШПР]]
# [[Контейнеры и контейнеризация]]

# Docker

## [[Архитектурный дизайн|Архитектура]] Docker


*Docker* использует *клиент-серверную архитектуру*. 

С клиента, который называется **Docker-клиент** (Docker client), поступают CLI-команды. 
```js
/* примеры CLI-команд */
docker build
docker ps
docker run
```

Клиент при помощи REST API передаёт команды серверу, который называется **Docker-демон** (Docker daemon).

*Docker-демон* собирает, запускает и раздаёт (distribute) контейнеры.

## Этапы докеризации приложения

- [Создание Dockerfile](#создание-dockerfile)
- [Построение образа](#построение-образа)
- [Создание и запуск контейнера](#создание-и-запуск-контейнера)
- [Композиция нескольких контейнеров](#композиция-нескольких-контейнеров)

### Создание Dockerfile

**Dockerfile** содержит **инструкции** (instructions) — последовательность действий, которые нужно выполнить, чтобы [построить образ](#построение-образа). 

Простой пример Dockerfile для NodeJS-приложения.
```Dockerfile
# Dockerfile
FROM node:latest
EXPOSE 3001

WORKDIR /app

COPY ./package.json .
COPY ./src ./src

RUN npm install
RUN npm run build

CMD npm run start
```

Конкретные инструкции Dockerfile разобраны [здесь](#dockerfile). 

### Построение образа

**Образ** (Image) — доступный только для чтения шаблон с инструкциями о том, как запустить какой-то Docker-контейнер, содержащий внутри себя всё необходимое для выполнения этих инструкций. 

Один образ может расширять другой.

**Базовый образ** (Base Image) — образ, который *не имеет родительского образа*.

Для *построения образа* используется команда `docker build`, которая принимает **контекст** (context) — путь к папке, с которой будет происходить работа в Dockerfile.

```js
/* узнать текущую папку консоли */
ls

/* "." означает, что текущая папка консоли взята в качестве контекста */
docker build . 
```

Образ строится на основании Dockerfile, который по умолчанию берётся из корня контекста.

Каждая инструкция в Dockerfile создаёт новый слой (layer) в образе. 

<!-- Когда Dockerfile изменяется и образ пересобирается (rebuild), пересобираются только изменённые слои. -->

Если Dockerfile лежит не в корне контекста, то можно явно указать путь к файлу.
```cmd
docker build -f /path/to/a/Dockerfile .
```
Можно задать явное название образа.
```cmd
docker build -t your_image_name .
```

Найти созданный образ можно среди других образов при помощи команды `docker images`.
```js
docker build -t test .
docker images

/*
  REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
  test                   latest              9468e6677939        22 seconds ago      730MB
  mongo                  3.4                 aeaac14e1ffb        5 months ago        429MB
  redis                  4.0                 04c446bf216f        5 months ago        89.2MB
  node                   10.15.3             5a401340b79f        10 months ago       899MB
*/
```

Образы хранятся в Docker-реестре (Docker registry). 

Одним из публичных реестров является Docker Hub. Он используется по умолчанию.
```js
// загрузить image из реестра
docker pull <image>
// загрузить image в реестр
docker push <image>
```

### Создание и запуск контейнера

**Контейнер** (Container) — *запускаемый экземпляр образа*. 

Для создания и последующего запуска контейнера по образу можно спользовать команду `docker run`.
```js
docker run -d image_name
```
Флаг `-d` используется для запуска контейнера в фоновом режиме (in background), таким текущая консоль не будет занята контейнером и можно будет вводить в неё другие команды.

Можно также задать явное имя контейнеру при создании.
```js
docker run -d --name container_name image_name
```

Команда `docker run` объединяет в себе две команды: `docker create` и `docker start`.
```js
/* создание контейнера */
docker create image_name
/* запуск ещё не запущенного контейнера */
docker start container_id
```

Для просмотра списка всех запущенных контейнеров и информации о них используется команда `docker ps`.
```js
docker ps

/*
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
b56a528cbe78        test                "docker-entrypoint.s…"   2 days ago          Up About a minute   3001/tcp           fervent_brattain
*/
```
Для просмотра всех контейнеров (в том числе и незапущенных) используется флаг `-a`.
```js
docker ps -a
```

Если есть необходимость посмотреть, что лежит внутри запущенного контейнера, можно зайти в него при помощи команды `docker exec`.
```js
docker exec -i -t container_id bash
/* осуществляется переход в интерактивный режим */
```
Пример работы в интерактивном режиме.
```js
/* вывод названий файлов и папок в текущей директории "app" */
root:/app# ls

/* вывод файла "package.json" в консоль */
root:/app# cat package.json

/* переход в папку "src" */
root:/app# cd ./src

/* выход из интерактивного режима */
root:/app/src# exit 
```

Флаг `-i` отвечает за переход в интерактивный режим, флаг `-t` позволяет эмулировать терминал.

Для остановки контейнера используется команда `docker stop`.
```js
docker stop container_id
```

### Композиция нескольких контейнеров

Для написания приложения чаще всего не достаточно одного контейнера. 

Если есть необходимость иметь несколько контейнеров в одном приложении, то нужно каждый из них настроить, как указано в этапах выше.

Обычно контейнеры зависят друг от друга, поэтому их запуск должен осуществляться в строгой последовательности. Такой запуск называется композицией контейнеров.

Чтобы было проще запускать композицию контейнеров, её шаги описывается в отдельном файле при помощи [Docker Compose](#docker-compose).

## Хранение данных в Volume

**Volume** — предпочитительный механизм для хранения данных (persisting data), используемых в Docker-контейнере.

Volume даёт контейнеру доступ к какой-то локальной папке на хост-машине, на которой этот контейнер запущен. Файлы из Volume нельзя использовать на этапе сборки (build-time), то есть в Dockerfile нельзя использовать файлы из Volume, они доступны лишь во время выполнения (run-time).

### Почему следует использовать Volume

* Volume хранится вне контейнера, поэтому он не увеличивает размер контейнера и не подвержен влиянию жизненного цикла контейнера.


# [[Dockerfile]]

# [[Docker Compose]]
