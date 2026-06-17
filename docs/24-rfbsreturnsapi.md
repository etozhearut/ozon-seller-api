# Возвраты товаров rFBS

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `RFBSReturnsAPI` · методов: 8_

## Список заявок на возврат

`POST /v2/returns/rfbs/list`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsListV2`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "offer_id": "test-offer-123456",
    "posting_number": "789456123-0002-3",
    "group_state": [
      "New",
      "Approved"
    ],
    "created_at": {
      "from": "2026-02-01T00:00:00Z",
      "to": "2026-03-01T23:59:59Z"
    }
  },
  "last_id": 0,
  "limit": 1000
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтр.
- `last_id` — integer <int32> Идентификатор последнего значения на странице — return_id . Оставьте это поле пустым при выполнении первого запроса.
- `limit required` — integer <int32> Количество значений в ответе.

### Ответы

- 200 Список заявок на возврат
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `returns` — object Данные о заявках.
  - `client_name` — string Deprecated Имя покупателя.
  - `created_at` — string <date-time> Дата создания заявки.
  - `order_number` — string Номер заказа.
  - `posting_number` — string Номер отправления.
  - `product` — object Данные о товаре.
  - `return_id` — integer <int64> Идентификатор заявки на возврат.
  - `return_number` — string Номер заявки на возврат.
  - `state` — object Статусы заявки и возврата денег.

Пример ответа:
```json
{
  "returns": [
    {
      "return_id": 8000123456,
      "return_number": "RET-2026-00123",
      "posting_number": "789456123-0002-3",
      "order_number": "123456789",
      "created_at": "2026-02-15T10:30:00Z",
      "product": {
        "sku": 1000123456,
        "offer_id": "test-offer-123456",
        "name": "Тестовый товар 1",
        "price": 2999,
        "currency_code": "RUB"
      },
      "state": {
        "group_state": "New",
        "state": "ON_APPROVAL",
        "state_name": "На проверке"
      }
    }
  ]
}
```

---

## Информация о заявке на возврат

`POST /v2/returns/rfbs/get`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsGetV2`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `return_id required` — integer <int64> Идентификатор заявки на возврат. Получите методом /v2/returns/rfbs/list .

### Ответы

- 200 Информация о заявке
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `returns` — object Данные о заявке.
  - `available_actions` — Array of objects Данные о доступных действиях с заявкой.
  - `client_name` — string Deprecated Имя покупателя.
  - `client_photo` — Array of strings Ссылки на фотографии товара.
  - `client_return_method_type` — object Данные о способе возврата.
  - `comment` — string Комментарий покупателя.
  - `created_at` — string <date-time> Дата создания заявки.
  - `order_number` — string Номер заказа.
  - `posting_number` — string Номер отправления.
  - `product` — object Данные о товаре.
  - `rejection_comment` — string Комментарий об отклонении заявки.
  - `rejection_reason` — Array of objects Данные о причине отклонения заявки.
  - `return_method_description` — string Способ возврата товара.
  - `return_number` — string Номер заявки на возврат.
  - `return_reason` — object Данные о причине возврата.
  - `ru_post_tracking_number` — string Трек-номер почтового отправления.
  - `state` — object Данные о статусе возврата.
  - `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:
```json
{
  "returns": {
    "available_actions": [
      {
        "action": {
          "id": 0,
          "name": "string"
        }
      }
    ],
    "client_name": "string",
    "client_photo": [
      "string"
    ],
    "client_return_method_type": {
      "id": 0,
      "name": "string"
    },
    "comment": "string",
    "created_at": "2025-09-04T13:49:20.340Z",
    "order_number": "string",
    "posting_number": "string",
    "product": {
      "currency_code": "string",
      "name": "string",
      "offer_id": "string",
      "price": 0,
      "sku": 0
    },
    "rejection_comment": "string",
    "rejection_reason": [
      {
        "hint": "string",
        "id": 0,
        "is_comment_required": true,
        "name": "string"
      }
    ],
    "return_method_description": "string",
    "return_number": "string",
    "return_reason": {
      "id": 0,
      "is_defect": true,
      "name": "string"
    },
    "ru_post_tracking_number": "string",
    "state": {
      "state": "string",
      "state_name": "string"
    },
    "warehouse_id": 0
  }
}
```

---

## Отклонить заявку на возврат

`POST /v2/returns/rfbs/reject`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsRejectV2`

В будущем метод будет отключён. Переключитесь на /v1/returns/rfbs/action/set . Метод позволяет отклонить заявку на возврат rFBS-заказа. Вы можете объяснить своё решение в параметре comment .

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/reject" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "return_id": 0,
  "comment": "string",
  "rejection_reason_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `return_id required` — integer <int64> Идентификатор заявки на возврат.
- `comment` — string Комментарий. Передайте комментарий, если в ответе метода /v2/returns/rfbs/get параметр rejection_reason.is_comment_required — true .
- `rejection_reason_id required` — integer <int64> Идентификатор причины отмены. Передайте идентификатор из списка причин, полученного в ответе метода /v2/returns/rfbs/get в параметре rejection_reason .

### Ответы

- 200 Заявка отклонена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Вернуть часть стоимости товара

`POST /v2/returns/rfbs/compensate`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsCompensateV2`

В будущем метод будет отключён. Переключитесь на /v1/returns/rfbs/action/set . Метод для частичной компенсации стоимости товара: вы возвращаете часть денег покупателю, товар остаётся у него.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/compensate" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "compensation_amount": "string",
  "return_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `compensation_amount` — string Сумма компенсации.
- `return_id required` — integer <int64> Идентификатор заявки на возврат.

### Ответы

- 200 Частичная компенсация подтверждена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Одобрить заявку на возврат

`POST /v2/returns/rfbs/verify`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsVerifyV2`

В будущем метод будет отключён. Переключитесь на /v1/returns/rfbs/action/set . Метод позволяет одобрить заявку и согласиться на получение товара для проверки. Подтвердите получение товара с помощью метода /v2/returns/rfbs/receive-return .

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/verify" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "return_id": 0,
  "return_method_description": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `return_id required` — integer <int64> Идентификатор заявки на возврат.
- `return_method_description` — string Способ возврата товара.

### Ответы

- 200 Заявка одобрена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Подтвердить получение товара на проверку

`POST /v2/returns/rfbs/receive-return`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsReceiveReturnV2`

В будущем метод будет отключён. Переключитесь на /v1/returns/rfbs/action/set .

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/receive-return" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `return_id required` — integer <int64> Идентификатор заявки на возврат.

### Ответы

- 200 Получение подтверждено
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Вернуть деньги покупателю

`POST /v2/returns/rfbs/return-money`

Operation ID: `RFBSReturnsAPI_ReturnsRfbsReturnMoneyV2`

В будущем метод будет отключён. Переключитесь на /v1/returns/rfbs/action/set . Метод подтверждает возврат полной стоимости товара. Используйте метод, если согласны: сразу вернуть стоимость товара и оставить его покупателю; вернуть стоимость после получения и проверки товара. …

```bash
curl -X POST "https://api-seller.ozon.ru/v2/returns/rfbs/return-money" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "return_id": 0,
  "return_for_back_way": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `return_id required` — integer <int64> Идентификатор заявки на возврат.
- `return_for_back_way` — integer <int64> Сумма, возмещаемая покупателю за пересылку товара.

### Ответы

- 200 Возврат денег подтверждён
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Передать доступные действия для rFBS возвратов

`POST /v1/returns/rfbs/action/set`

Operation ID: `ReturnsAPI_ReturnsRfbsActionSet`

Метод для передачи действий для возврата rFBS.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/returns/rfbs/action/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "comment": "string",
  "compensation_amount": 0,
  "id": 0,
  "rejection_reason_id": 0,
  "return_for_back_way": 0,
  "return_id": 0
}'
```

### Тело запроса

- `comment` — string Комментарий продавца. Обязателен для id: -1 и id: -10 .
- `compensation_amount` — number <double> Сумма компенсации. Обязательна для id: 1020 .
- `id` — integer <int32> Идентификатор действия. Получите доступные действия returns.available_actions методом /v2/returns/rfbs/get .
- `rejection_reason_id` — integer <int32> Идентификатор причины отмены. Обязателен для id: -1 и id: -10 . Получите возможные причины отмены returns.rejection_reason методом /v2/returns/rfbs/get .
- `return_for_back_way` — number <double> Сумма, возмещаемая покупателю за пересылку товара. Отрицательные значения приравниваются к 0 .
- `return_id required` — integer <int64> Идентификатор заявки на возврат.

### Ответы

- 200 Действие передано

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
