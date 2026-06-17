# Отправления

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FboPostingAPI` · методов: 3_

## Отменить отправление из заказа

`POST /v1/posting/cancel`

Operation ID: `PostingAPI_PostingCancel`

Отменяет отправление из заказа. Используйте идентификатор причины отмены reasons.id из метода /v1/cancel-reason/list-by-posting .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "string",
  "reason_id": 0,
  "reason_message": "string"
}'
```

### Тело запроса

- `posting_number required` — string Номер отправления.
- `reason_id required` — integer <int32> Идентификатор причины отмены.
- `reason_message` — string Дополнительная информация по отмене.

### Ответы

- 200 Сообщение со статусом отмены

#### Схема ответа (200)

- `message` — string Текст сообщения.

---

## Проверить статус отмены отправления

`POST /v1/posting/cancel/status`

Operation ID: `PostingAPI_PostingCancelStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/cancel/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `posting_number` — string Идентификатор отправления.

### Ответы

- 200 Статус отмены отправления

#### Схема ответа (200)

- `order_number` — string Номер заказа.
- `posting_number` — Array of strings Идентификатор отправления.
- `state` — string Статус отмены отправления.

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

## Получить маркировки экземпляров из отправления

`POST /v1/posting/marks`

Operation ID: `PostingAPI_PostingMarks`

Возвращает статусы выдачи экземпляров и коды маркировки «Честный ЗНАК» для каждого отправления. Укажите в чеке и выведите из оборота маркировки экземпляров из параметра issued_exemplars в ответе.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/marks" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_numbers": [
    "string"
  ]
}'
```

### Тело запроса

- `posting_numbers` — Array of strings Идентификаторы отправлений.

### Ответы

- 200 Список экземпляров отправления с маркировками

#### Схема ответа (200)

- `invalid_postings` — Array of strings Список неверных идентификаторов отправлений.
- `issued_exemplars` — Array of objects Список выданных покупателям экземпляров товаров.
- `non_issued_exemplars` — Array of objects Список не выданных покупателям экземпляров товаров.

Пример ответа:
```json
{
  "invalid_postings": [
    "string"
  ],
  "issued_exemplars": [
    {
      "exemplar_id": 0,
      "mandatory_marks": [
        "string"
      ],
      "posting_number": "string",
      "sku": 0
    }
  ],
  "non_issued_exemplars": [
    {
      "exemplar_id": 0,
      "posting_number": "string",
      "sku": 0
    }
  ]
}
```

---
