# Заказы

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `OrderAPI` · методов: 4_

## Отменить заказ

`POST /v1/order/cancel`

Operation ID: `OrderAPI_OrderCancel`

Отменяет заказ со всеми отправлениями. Используйте идентификатор причины отмены reasons.id из метода /v1/cancel-reason/list-by-order .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/order/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "order_number": "string",
  "reason_id": 0,
  "reason_message": "string"
}'
```

### Тело запроса

- `order_number required` — string Номер заказа.
- `reason_id required` — integer <int32> Идентификатор причины отмены заказа.
- `reason_message` — string Причина отмены заказа.

### Ответы

- 200 Заказ отменён

#### Схема ответа (200)

- `message` — string Статус обработки отмены.

---

## Проверить возможность отмены заказа

`POST /v1/order/cancel/check`

Operation ID: `OrderAPI_OrderCancelCheck`

Возвращает возможность отмены заказа для покупателя.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/order/cancel/check" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `order_number required` — string Номер заказа.

### Ответы

- 200 Результат проверки

#### Схема ответа (200)

- `cancellable` — boolean true , если заказ можно отменить.
- `order_number` — string Номер заказа.
- `posting_groups` — Array of objects Группы отправлений.
- `postings` — Array of objects Информация о возможности отмены отправлений.

Пример ответа:
```json
{
  "cancellable": true,
  "order_number": "string",
  "posting_groups": [
    {
      "posting_numbers": [
        "string"
      ]
    }
  ],
  "postings": [
    {
      "cancellable": true,
      "posting_number": "string",
      "why_not_cancellable": "string"
    }
  ]
}
```

---

## Получить статус отмены заказа

`POST /v1/order/cancel/status`

Operation ID: `OrderAPI_OrderCancelStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/order/cancel/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `order_number required` — string Номер заказа.

### Ответы

- 200 Статус отмены заказа

#### Схема ответа (200)

- `order_number` — string Номер заказа.
- `posting_number` — Array of strings Список отправлений в заказе.
- `state` — string Статус отмены заказа.

Пример ответа:
```json
{
  "order_number": "string",
  "posting_number": [
    "string"
  ],
  "state": "string"
}
```

---

## Создать заказ

`POST /v2/order/create`

Operation ID: `OrderAPI_OrderCreate`

Создаёт заказ для покупателя и получателя в системе Ozon. Передайте вариант доставки из ответа метода /v2/delivery/checkout . В ответе могут быть не все отправления. Получите список всех отправлений по номеру заказа order_number методом: /v2/posting/fbo/list — для схемы FBO; /v3/posting/fbs/list — для схемы FBS. …

```bash
curl -X POST "https://api-seller.ozon.ru/v2/order/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "buyer": {
    "first_name": "string",
    "last_name": "string",
    "middle_name": "string",
    "phone": "string"
  },
  "delivery": {},
  "delivery_schema": "MIX",
  "recipient": {
    "recipient_first_name": "string",
    "recipient_last_name": "string",
    "recipient_middle_name": "string",
    "recipient_phone": "string"
  },
  "splits": [
    {
      "delivery_method": {
        "delivery_method_id": 0,
        "delivery_type": "COURIER",
        "logistic_date_range": {
          "from": "2019-08-24T14:15:22Z",
          "to": "2019-08-24T14:15:22Z"
        },
        "price": {
          "currency_code": "string",
          "nanos": 0,
          "units": 0
        },
        "timeslot_id": 0
      },
      "items": [
        {
          "offer_id": "string",
          "price": {
            "currency_code": "string",
            "nanos": 0,
            "units": 0
          },
          "quantity": 0,
          "sku": 0
        }
      ],
      "warehouse_id": 0
    }
  ]
}'
```

### Тело запроса

- `buyer required` — object Информация о покупателе.
- `delivery required` — courier (object) or pick_up (object) Информация о доставке.
- `delivery_schema required` — string Default: "MIX" Enum: "MIX" "FBO" "FBS" Схема доставки: MIX — на выбор Ozon; FBO — FBO; FBS — FBS.
- `recipient required` — object Информация о получателе.
- `splits required` — Array of objects Информация об отправлениях в заказе.

### Ответы

- 200 Заказ создан

#### Схема ответа (200)

- `order_number` — string Номер заказа.
- `postings` — Array of strings Отправления.

Пример ответа:
```json
{
  "order_number": "string",
  "postings": [
    "string"
  ]
}
```

---
