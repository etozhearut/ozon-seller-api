# Полигоны

_Тег: `PolygonAPI` · операций: 7_

## Создайте полигон доставки

`POST /v1/polygon/create`

Operation ID: `PolygonAPI_CreatePolygon`

Вы можете добавить полигон к методу доставки. Создайте полигон, получив его координаты на https://geojson.io : отметьте на карте минимум 3 точки и соедините их линиями. Сервис geojson.io возвращает координаты в формате [[[long lat]]] . Поменяйте местами широту и долготу в запросе метода.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `coordinates required` — string Координаты полигона доставки в формате [[[lat long]]] .

Пример запроса:

```json
{
  "coordinates": "[[[30.149574279785153,59.86550435303646],[30.21205902099609,59.846884387977326],[30.255661010742184,59.86240174913176],[30.149574279785153,59.86550435303646]]]"
}
```

### Ответы

- 200 Полигон создан
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `polygon_id` — integer <int64> Идентификатор полигона.

---

## Связать метод доставки с полигоном

`POST /v2/polygon/bind`

Operation ID: `PolygonBind`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `polygon_id required` — integer <int64> Идентификатор полигона.
- `time required` — integer <int64> Enum: 15 30 45 60 90 120 150 Время доставки в минутах.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "polygon_id": 0,
  "time": 15,
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

Пример ответа:

```json
{
  "code": 0,
  "details": [
    {
      "typeUrl": "string",
      "value": "string"
    }
  ],
  "message": "string"
}
```

---

## Свяжите метод доставки с полигоном доставки

`POST /v1/polygon/bind`

Operation ID: `PolygonAPI_BindPolygon`

Метод устаревает и будет отключён в будущем. Переключитесь на /v2/polygon/bind .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int32> Идентификатор метода доставки.
- `polygons required` — Array of objects Список полигонов.
- `warehouse_location required` — object Расположение склада.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "polygons": [
    {
      "polygon_id": "1323",
      "time": "30"
    }
  ],
  "warehouse_location": {
    "lat": "58.52391272075821",
    "lon": "31.236791610717773"
  }
}
```

### Ответы

- 200 Успешно
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Удалить полигон из области доставки

`POST /v1/polygon/delete`

Operation ID: `PolygonDelete`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `polygon_id required` — integer <int64> Идентификатор полигона.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "polygon_id": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

Пример ответа:

```json
{
  "code": 0,
  "details": [
    {
      "typeUrl": "string",
      "value": "string"
    }
  ],
  "message": "string"
}
```

---

## Получить список установленных полигонов на метод доставки

`POST /v1/polygon/list`

Operation ID: `PolygonList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Список полигонов

#### Схема ответа (200)

- `polygons` — Array of objects Список полигонов.
  - `coordinates` — string Координаты полигона доставки в формате [[[lat,long],[lat,long]]] .
  - `polygon_id` — integer <int64> Идентификатор полигона.
  - `time` — integer <int64> Время доставки в минутах.

Пример ответа:

```json
{
  "polygons": [
    {
      "coordinates": "string",
      "polygon_id": 0,
      "time": 0
    }
  ]
}
```

---

## Обновить координаты полигона доставки

`POST /v1/polygon/time/coordinates/update`

Operation ID: `PolygonTimeCoordinatesUpdate`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `coordinates required` — string Новые координаты полигона доставки в формате [[[lat,long],[lat,long]]] .
- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `polygon_id required` — integer <int64> Идентификатор полигона.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "coordinates": "string",
  "delivery_method_id": 0,
  "polygon_id": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

Пример ответа:

```json
{
  "code": 0,
  "details": [
    {
      "typeUrl": "string",
      "value": "string"
    }
  ],
  "message": "string"
}
```

---

## Установить новое время доставки в полигоне

`POST /v1/polygon/time/set`

Operation ID: `PolygonTimeSet`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `current_time required` — integer <int64> Enum: 15 30 45 60 90 120 150 Текущее время доставки в минутах.
- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `new_time required` — integer <int64> Enum: 15 30 45 60 90 120 150 Новое время доставки в минутах.
- `polygon_id required` — integer <int64> Идентификатор полигона.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "current_time": 15,
  "delivery_method_id": 0,
  "new_time": 15,
  "polygon_id": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

Пример ответа:

```json
{
  "code": 0,
  "details": [
    {
      "typeUrl": "string",
      "value": "string"
    }
  ],
  "message": "string"
}
```

---
