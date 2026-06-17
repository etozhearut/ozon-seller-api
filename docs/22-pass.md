# Пропуски

_Тег: `Pass` · операций: 7_

## Список пропусков

`POST /v1/pass/list`

Operation ID: `PassList`

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтры.
- `limit required` — integer <int32> [ 1 .. 1000 ] Default: 1000 Ограничение по количеству записей в ответе.

Пример запроса:

```json
{
  "cursor": "",
  "filter": {
    "arrival_pass_ids": [
      "5000123456",
      "5000123457"
    ],
    "arrival_reason": "FBS_DELIVERY",
    "dropoff_point_ids": [
      "7000123456",
      "7000123457"
    ],
    "only_active_passes": true,
    "warehouse_ids": [
      "3000007863",
      "3000007864"
    ]
  },
  "limit": 1000
}
```

### Ответы

- 200 Список пропусков

#### Схема ответа (200)

- `arrival_passes` — Array of objects Список пропусков для перевозки.
- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.

Пример ответа:

```json
{
  "arrival_passes": [
    {
      "arrival_pass_id": 5000123456,
      "arrival_reasons": [
        "FBS_DELIVERY",
        "FBS_RETURN"
      ],
      "arrival_time": "2026-03-16T10:00:00Z",
      "driver_name": "Иванов Иван Иванович",
      "driver_phone": "+79991234567",
      "dropoff_point_id": 7000123456,
      "is_active": true,
      "vehicle_license_plate": "А123БВ777",
      "vehicle_model": "ГАЗель NEXT",
      "warehouse_id": 3000007863
    },
    {
      "arrival_pass_id": 5000123457,
      "arrival_reasons": [
        "FBS_RETURN"
      ],
      "arrival_time": "2026-03-16T14:30:00Z",
      "driver_name": "Петров Петр Петрович",
      "driver_phone": "+79997654321",
      "dropoff_point_id": 7000123457,
      "is_active": true,
      "vehicle_license_plate": "В456ГД178",
      "vehicle_model": "Ford Transit",
      "warehouse_id": 3000007864
    }
  ],
  "cursor": "next-page-cursor-12345"
}
```

---

## Создать пропуск

`POST /v1/carriage/pass/create`

Operation ID: `carriagePassCreate`

Идентификатор созданного пропуска добавится к перевозке.

### Тело запроса (application/json)

- `arrival_passes required` — Array of objects Список пропусков.
- `carriage_id required` — integer <int64> Идентификатор перевозки.

Пример запроса:

```json
{
  "carriage_id": 5000123456,
  "arrival_passes": [
    {
      "driver_name": "Иванов Иван Иванович",
      "driver_phone": "+79991234567",
      "vehicle_license_plate": "А123БВ777",
      "vehicle_model": "ГАЗель NEXT",
      "with_returns": true
    }
  ]
}
```

### Ответы

- 200 Пропуск создан

#### Схема ответа (200)

- `arrival_pass_ids` — Array of strings <int64> Идентификаторы пропусков.

Пример ответа:

```json
{
  "arrival_pass_ids": [
    6000123456
  ]
}
```

---

## Обновить пропуск

`POST /v1/carriage/pass/update`

Operation ID: `carriagePassUpdate`

### Тело запроса (application/json)

- `arrival_passes required` — Array of objects Список пропусков.
- `carriage_id required` — integer <int64> Идентификатор перевозки.

Пример запроса:

```json
{
  "carriage_id": 5000123456,
  "arrival_passes": [
    {
      "id": 6000123456,
      "driver_name": "Иванов Иван Иванович",
      "driver_phone": "+79991234567",
      "vehicle_license_plate": "А123БВ777",
      "vehicle_model": "ГАЗель NEXT",
      "with_returns": true
    }
  ]
}
```

### Ответы

- 200 Пропуск обновлён

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

## Удалить пропуск

`POST /v1/carriage/pass/delete`

Operation ID: `carriagePassDelete`

### Тело запроса (application/json)

- `arrival_pass_ids required` — Array of strings <int64> Идентификаторы пропусков.
- `carriage_id required` — integer <int64> Идентификатор перевозки.

Пример запроса:

```json
{
  "carriage_id": 5000123456,
  "arrival_pass_ids": [
    6000123456,
    6000123457
  ]
}
```

### Ответы

- 200 Пропуск удалён

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

## Создать пропуск для возврата

`POST /v1/return/pass/create`

Operation ID: `returnPassCreate`

### Тело запроса (application/json)

- `arrival_passes required` — Array of objects Список пропусков.

Пример запроса:

```json
{
  "arrival_passes": [
    {
      "arrival_time": "2026-03-17T10:00:00Z",
      "driver_name": "Иванов Иван Иванович",
      "driver_phone": "+79991234567",
      "dropoff_point_id": 7000123456,
      "vehicle_license_plate": "А123БВ777",
      "vehicle_model": "ГАЗель NEXT",
      "warehouse_id": 3000007863
    }
  ]
}
```

### Ответы

- 200 Пропуск создан

#### Схема ответа (200)

- `arrival_pass_ids` — Array of strings <int64> Идентификаторы пропусков.

Пример ответа:

```json
{
  "arrival_pass_ids": [
    6000123456
  ]
}
```

---

## Обновить пропуск для возврата

`POST /v1/return/pass/update`

Operation ID: `returnPassUpdate`

### Тело запроса (application/json)

- `arrival_passes required` — Array of objects Список пропусков.

Пример запроса:

```json
{
  "arrival_passes": [
    {
      "arrival_pass_id": 6000123456,
      "arrival_time": "2026-03-17T14:30:00Z",
      "driver_name": "Иванов Иван Иванович",
      "driver_phone": "+79991234567",
      "vehicle_license_plate": "А123БВ777",
      "vehicle_model": "ГАЗель NEXT"
    }
  ]
}
```

### Ответы

- 200 Пропуск обновлён

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

## Удалить пропуск для возврата

`POST /v1/return/pass/delete`

Operation ID: `returnPassDelete`

### Тело запроса (application/json)

- `arrival_pass_ids required` — Array of strings <int64> Идентификаторы пропусков.

Пример запроса:

```json
{
  "arrival_pass_ids": [
    6000123456,
    6000123457
  ]
}
```

### Ответы

- 200 Пропуск удалён

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
