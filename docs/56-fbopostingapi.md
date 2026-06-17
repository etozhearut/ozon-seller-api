# Отправления

_Тег: `FboPostingAPI` · операций: 3_

## Отменить отправление из заказа

`POST /v1/posting/cancel`

Operation ID: `PostingAPI_PostingCancel`

Отменяет отправление из заказа. Используйте идентификатор причины отмены reasons.id из метода /v1/cancel-reason/list-by-posting .

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.
- `reason_id required` — integer <int32> Идентификатор причины отмены.
- `reason_message` — string Дополнительная информация по отмене.

Пример запроса:

```json
{
  "posting_number": "string",
  "reason_id": 0,
  "reason_message": "string"
}
```

### Ответы

- 200 Сообщение со статусом отмены

#### Схема ответа (200)

- `message` — string Текст сообщения.

---

## Проверить статус отмены отправления

`POST /v1/posting/cancel/status`

Operation ID: `PostingAPI_PostingCancelStatus`

### Тело запроса (application/json)

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

### Тело запроса (application/json)

- `posting_numbers` — Array of strings Идентификаторы отправлений.

Пример запроса:

```json
{
  "posting_numbers": [
    "string"
  ]
}
```

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
