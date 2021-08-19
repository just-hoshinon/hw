## Описание

WebAPI для работы с погодой.

## Запуск апи

Перед работой установите все библиотеки через `npm install`

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Конфиг

Конфиг находится в src/config.ts, в нём:
 
 - firstProvider - первый провайдер
 - secondProvider - второй провайдер
 - thirdProvider - третий провайдер
 - logType - тип логирования

Провайдерами могут быть:

 - openweathermap.org
 - weatherapi.com
 - tomorrow.io
 - stormglass.io

Типами логирования могут быть:

 - stdout
 - file

## Паттерны проектирования

Из паттернов проектирования я попытался создать фабрику, абстрактную фабрику, цепочку обязанностей, адаптер

## SOLID
 
Постарался сделать так, чтобы классы выполняли только одну функцию (SRP); в классах, которые имплементируют интерфейс, возвращаемым значением был класс, который имплементирует интерфейс в интерфейсе класса (LSP); интерфейсы были маленькими (ISP); не было зависимости в добавлении новых классов и рефакторинга другого кода (OCP)

## Логика работы

Используется Nest.JS (который является более обширным фреймворком над работой с запросами) и TypeScript (для работы с типами данных). В файле app.module.ts происходит DI, а также присоединяется контроллер погоды. Запрос, по которому можно получить информацию о погоде:

```bash
localhost:3000/weather/:city/
```

В контроллере вызывается сервис по погоде, логирование и аналитика по количеству запросов. Сервис по погоде работает с цепочкой обязанностей, в которую соответственно через менеджер контейнеров добавляются в нужном порядке провайдеры погоды. Логирование сделано в двух типах, так же присутствует абстракция и возможность добавлять другие виды логирования, имеется фабрика. Аналитика по количеству запросов сделана на основе файла `analytics.json`, где хранится информация о количестве запросов. Так же есть интерфейс, фабрики нету (так как соответственно только одна база данных). Названия методов сделаны под заточку на MySQL (почти). Есть так же сервис перевода из метров/секунд в километр/час (так как некоторые провайдеры по дефолту дают м/с), получения координат (некоторые провайдеры не принимают названия городов).
