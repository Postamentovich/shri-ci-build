# School CI Server

## Установка и запуск

Сервер `cd server && npm ci && npm start` <br>
Агент `cd agent && npm ci && npm start` <br>

Одновременно сервер и агент можно запустить в докере командой `npm run docker-compose` <br>

Версия Node.js - 12.16 <br/>

Репозиторий с клиентом - https://github.com/Postamentovich/shri-homework-1

## Сервер

### Описание

Ходит за заданиями на сборку в базу (API) и передает задание на сборку одному из агентов. Потом принимает результаты сборки и записывает их в БД (API).

### Конфигурация

В папку `/server` нужно положить файл конфигурации server-conf.json в формате: <br/>
apiToken - токен авторизации https://hw.shri.yandex/</br>

```json
{
  "port": 8080,
  "apiBaseUrl": "https://hw.shri.yandex/api/",
  "apiToken": "YOUR_API_TOKEN"
}
```

### API

POST - `/notify-agent` - зарегистрировать агента. <br>
Модель :

```js
{
    port: number, // Порт на котором запущен агент
    host: string // Хост на котором запущен агент
}
```

POST - `/notify-build-result` - сохранить результаты сборки.<br>
Модель :

```js
{
    buildId: string, // ID cборки
    log: string, // Лог сборки
    status: string // Статус сборки
}
```

## Агент

### Описание

При запуске регистрируется в сервере, принимает задания на сборку, отсылает результаты на сервер.

### Конфигурация

В папку `/agent` нужно положить файл конфигурации agent-conf.json в формате:

```json
{
  "port": 8001,
  "serverHost": "127.0.0.1",
  "serverPort": 8080
}
```

### API

GET - `/health` - статус агента (для проверки существует агент или нет)<br>

POST - `/build` - запустить сборку.<br>
Модель :

```js
{
    buildId: string, // ID cборки
    repoName: string, // Адрес репозитория
    commitHash: string, // Хэш коммита
    buildCommand: string // Команда билда
}
```
