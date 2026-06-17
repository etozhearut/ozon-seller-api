# Доставка rFBS

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `DeliveryrFBS` · методов: 7_

## Добавить трек-номера

`POST /v2/fbs/posting/tracking-number/set`

Operation ID: `PostingAPI_FbsPostingTrackingNumberSet`

Добавить трек-номера к отправлениям. Вы можете передать до 20 трек-номеров за раз.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/fbs/posting/tracking-number/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "tracking_numbers": [
    {
      "posting_number": "48173252-0033-2",
      "tracking_number": "123123123"
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `tracking_numbers required` — Array of objects Массив с парами идентификатор отправления — трек-номер.

### Ответы

- 200 Трек-номер добавлен
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `error` — string Ошибка при обработке запроса.
  - `posting_number` — string Номер отправления.
  - `result` — boolean Если запрос выполнен без ошибок — true .

Пример ответа:
```json
{
  "result": [
    {
      "error": "",
      "posting_number": "48173252-0033-2",
      "result": true
    }
  ]
}
```

---

## Изменить статус на «Доставляется»

`POST /v2/fbs/posting/delivering`

Operation ID: `PostingAPI_FbsPostingDelivering`

Перед изменением статуса проверьте текущий статус отправления методом /v3/posting/fbs/get . Изменение статуса происходит асинхронно. Перевести отправление в статус «Доставляется», если используется сторонняя служба доставки.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/fbs/posting/delivering" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": [
    "33920157-0018-1"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — Array of strings Идентификатор отправления.

### Ответы

- 200 Статус изменён
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `error` — string Ошибка при обработке запроса.
  - `posting_number` — string Номер отправления.
  - `result` — boolean Если запрос выполнен без ошибок — true .

Пример ответа:
```json
{
  "result": [
    {
      "error": [],
      "posting_number": "33920157-0018-1",
      "result": true
    }
  ]
}
```

---

## Изменить статус на «Последняя миля»

`POST /v2/fbs/posting/last-mile`

Operation ID: `PostingAPI_FbsPostingLastMile`

Перед изменением статуса проверьте текущий статус отправления методом /v3/posting/fbs/get . Изменение статуса происходит асинхронно. Перевести отправление в статус «Последняя миля», если используется сторонняя служба доставки.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/fbs/posting/last-mile" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": [
    "48173252-0033-2"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — Array of strings Идентификатор отправления.

### Ответы

- 200 Статус изменён
- 400 Invalid parameter
- 403 Access denied
- 404 Response not found
- 409 Request conflict
- 500 Internal server error

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `error` — string Ошибка при обработке запроса.
  - `posting_number` — string Номер отправления.
  - `result` — boolean Если запрос выполнен без ошибок — true .

Пример ответа:
```json
{
  "result": [
    {
      "error": [],
      "posting_number": "48173252-0033-2",
      "result": true
    }
  ]
}
```

---

## Изменить статус на «Доставлено»

`POST /v2/fbs/posting/delivered`

Operation ID: `PostingAPI_FbsPostingDelivered`

Перед изменением статуса проверьте текущий статус отправления методом /v3/posting/fbs/get . Изменение статуса происходит асинхронно. Перевести отправление в статус «Доставлено», если используется сторонняя служба доставки.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/fbs/posting/delivered" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": [
    "48173252-0033-2"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — Array of strings Идентификатор отправления.

### Ответы

- 200 Статус изменён
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `error` — string Ошибка при обработке запроса.
  - `posting_number` — string Номер отправления.
  - `result` — boolean Если запрос выполнен без ошибок — true .

Пример ответа:
```json
{
  "result": [
    {
      "error": [],
      "posting_number": "48173252-0033-2",
      "result": true
    }
  ]
}
```

---

## Доступные даты для переноса доставки

`POST /v1/posting/fbs/timeslot/change-restrictions`

Operation ID: `PostingAPI_PostingTimeslotChangeRestrictions`

Метод для получения доступных дат для переноса доставки и количества доступных переносов.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/fbs/timeslot/change-restrictions" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Доступные даты и количество
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `delivery_interval` — object Период дат, доступных для переноса.
- `remaining_changes_count` — integer <int64> Количество оставшихся переносов.

Пример ответа:
```json
{
  "delivery_interval": {
    "begin": "2023-03-27T08:43:05.658Z",
    "end": "2023-03-27T08:43:05.658Z"
  },
  "remaining_changes_count": 0
}
```

---

## Перенести дату доставки

`POST /v1/posting/fbs/timeslot/set`

Operation ID: `PostingAPI_SetPostingTimeslot`

Вы можете изменить дату доставки отправления не больше двух раз.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/fbs/timeslot/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "789456123-0002-3",
  "new_timeslot": {
    "from": "2026-03-16T14:00:00Z",
    "to": "2026-03-16T18:00:00Z"
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `new_timeslot required` — object Новый период для даты доставки.
- `posting_number required` — string Номер отправления.

### Ответы

- 200 Результат запроса
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean true , если дата изменена.

---

## Уточнить дату отгрузки отправления

`POST /v1/posting/cutoff/set`

Operation ID: `PostingAPI_SetPostingCutoff`

Метод для отправлений, которые доставляет продавец или неинтегрированный перевозчик.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/cutoff/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "789456123-0002-3",
  "new_cutoff_date": "2026-03-16T10:00:00Z"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `new_cutoff_date required` — string <date-time> Новая дата отгрузки.
- `posting_number required` — string Номер отправления.

### Ответы

- 200 Результат уточнения даты
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean true , если установлена новая дата.

---
