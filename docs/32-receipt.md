# Чеки

Чеки Подробнее о чеках в Справке для продавцов Ozon Global .

_Тег: `Receipt` · операций: 3_

## Получить чек в формате PDF

`POST /v1/receipts/get`

Operation ID: `GetReceipt`

Метод доступен продавцам, которые заключили договор с ТОО «ОЗОН Маркетплейс Казахстан».

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `receipt_id required` — string Идентификатор чека. Получите значение параметра методом /v1/receipts/seller/list .

### Ответы

- 200 Чек

#### Схема ответа (200)

- `content` — string <byte> PDF-файл с чеком в бинарном виде.

---

## Получить список чеков продавца

`POST /v1/receipts/seller/list`

Operation ID: `ReceiptsSellerList`

Метод доступен продавцам, которые заключили договор с ТОО «ОЗОН Маркетплейс Казахстан».

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `page` — integer <int64> Default: 0 Количество страниц, которое нужно пропустить.
- `page_size` — integer <int64> <= 100 Default: 100 Количество элементов на странице.
- `posting_numbers` — Array of strings Фильтр по номерам отправлений.

Пример запроса:

```json
{
  "page": 0,
  "page_size": 100,
  "posting_numbers": [
    "string"
  ]
}
```

### Ответы

- 200 Список чеков продавца

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернулись не все записи: true — сделайте повторный запрос с новым параметром page , чтобы получить остальные значения; false — ответ содержит все записи с чеками.
- `receipts` — Array of objects Информация о чеках.

Пример ответа:

```json
{
  "has_next": true,
  "receipts": [
    {
      "created_at": "2019-08-24T14:15:22Z",
      "operation_type": "UNSPECIFIED",
      "order_id": 0,
      "parent_receipt_id": "string",
      "posting_numbers": [
        "string"
      ],
      "receipt_id": "string",
      "receipt_number": "string",
      "type": "UNSPECIFIED",
      "updated_at": "2019-08-24T14:15:22Z"
    }
  ]
}
```

---

## Загрузить чек

`POST /v1/receipts/upload`

Operation ID: `UploadReceipt`

Метод доступен продавцам, которые заключили договор с ТОО «ОЗОН Маркетплейс Казахстан».

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (multipart/form-data)

- `content required` — string <byte> Содержание файла в бинарном виде.
- `operation_type required` — string Тип операции. Получите значение параметра методом /v1/receipts/seller/list .
- `parent_receipt_id` — string Идентификатор родительского чека. Передайте параметр с идентификатором чека, который нужно изменить.
- `posting_numbers required` — Array of strings Номера отправлений.
- `receipt_number required` — string Номер чека.
- `type required` — string Enum: "INCOMING" "REFUND" Тип чека: INCOMING — чек реализации; REFUND — чек возврата.

### Ответы

- 200 Чек загружен

#### Схема ответа (200)

- `receipt_id` — string Идентификатор чека.

---
