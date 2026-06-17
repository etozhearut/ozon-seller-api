# Работа с FBP-поставками с доставкой pick-up

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `OrderPickupFBP` · методов: 2_

## Отменить pick-up поставку

`POST /v1/fbp/order/pick-up/cancel`

Operation ID: `FbpAPI_FbpOrderPickUpCancel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/pick-up/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

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

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/pick-up/dlv/edit" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "pickup_details": {
    "sender_name": "string",
    "sender_phone": "string"
  },
  "row_version": 0,
  "supply_id": "string"
}'
```

### Тело запроса

- `pickup_details required` — object Детали отправителя.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.

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
