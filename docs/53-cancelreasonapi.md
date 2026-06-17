# Причины отмены

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `CancelReasonAPI` · методов: 3_

## Причины отмены отправлений

`POST /v1/cancel-reason/list`

Operation ID: `CancelReasonList`

Возвращает возможные причины отмены отправлений и заказов.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cancel-reason/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Ответы

- 200 Причины отмены отправлений

#### Схема ответа (200)

- `reasons` — Array of objects Информация о причинах отмены.
  - `id` — integer <int64> Идентификатор причины отмены: 501 — «Ozon перенёс срок доставки»; 502 — «Отменили часть товаров из заказа»; 503 — «Не применилась скидка или промокод»; 504 — «Хочу изменить заказ и оформить заново»; 505 — «Слишком долго ждать»; 506 — «Нашёл дешевле»; 508 — «Не нашёл нужную причину»; 710 — «Указал неверный адрес».
  - `name` — string Причина отмены.

Пример ответа:
```json
{
  "reasons": [
    {
      "id": 0,
      "name": "string"
    }
  ]
}
```

---

## Причины отмены заказа

`POST /v1/cancel-reason/list-by-order`

Operation ID: `CancelReasonListByOrder`

Возвращает возможные причины отмены для заказа.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cancel-reason/list-by-order" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `order_number required` — string Номер заказа.

### Ответы

- 200 Причины отмены заказа

#### Схема ответа (200)

- `reasons` — Array of objects Информация о причинах отмены.
  - `id` — integer <int64> Идентификатор причины отмены: 501 — «Ozon перенёс срок доставки»; 502 — «Отменили часть товаров из заказа»; 503 — «Не применилась скидка или промокод»; 504 — «Хочу изменить заказ и оформить заново»; 505 — «Слишком долго ждать»; 506 — «Нашёл дешевле»; 508 — «Не нашёл нужную причину»; 710 — «Указал неверный адрес».
  - `name` — string Причина отмены.

Пример ответа:
```json
{
  "reasons": [
    {
      "id": 0,
      "name": "string"
    }
  ]
}
```

---

## Причины отмены отправления

`POST /v1/cancel-reason/list-by-posting`

Operation ID: `CancelReasonAPI_CancelReasonListByPosting`

Возвращает возможные причины отмены для отправления.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cancel-reason/list-by-posting" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Причины отмены отправления

#### Схема ответа (200)

- `reasons` — Array of objects Информация о причинах отмены.
  - `id` — integer <int64> Идентификатор причины отмены: 501 — «Ozon перенёс срок доставки»; 502 — «Отменили часть товаров из заказа»; 503 — «Не применилась скидка или промокод»; 504 — «Хочу изменить заказ и оформить заново»; 505 — «Слишком долго ждать»; 506 — «Нашёл дешевле»; 508 — «Не нашёл нужную причину»; 710 — «Указал неверный адрес».
  - `name` — string Причина отмены.

Пример ответа:
```json
{
  "reasons": [
    {
      "id": 0,
      "name": "string"
    }
  ]
}
```

---
