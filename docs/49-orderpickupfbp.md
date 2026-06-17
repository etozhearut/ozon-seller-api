# Работа с FBP-поставками с доставкой pick-up

_Тег: `OrderPickupFBP` · операций: 2_

## Отменить pick-up поставку

`POST /v1/fbp/order/pick-up/cancel`

Operation ID: `FbpAPI_FbpOrderPickUpCancel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Статус отмены

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error": {
    "order_errors": "ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---

## Изменить данные о точке забора

`POST /v1/fbp/order/pick-up/dlv/edit`

Operation ID: `FbpAPI_FbpOrderPickUpDlvEdit`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `pickup_details required` — object Детали отправителя.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.

Пример запроса:

```json
{
  "pickup_details": {
    "sender_name": "string",
    "sender_phone": "string"
  },
  "row_version": 0,
  "supply_id": "string"
}
```

### Ответы

- 200 Статус изменения

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error": {
    "order_errors": "ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---
