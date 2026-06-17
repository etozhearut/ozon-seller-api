# Работа с FBP-черновиками с доставкой pick-up

_Тег: `DraftPickupFBP` · операций: 5_

## Создать черновик заявки на pick-up поставку

`POST /v1/fbp/draft/pick-up/create`

Operation ID: `FbpAPI_FbpDraftPickupCreate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `bundle_id required` — string Идентификатор состава поставки.
- `delivery_details required` — object Детали доставки.
- `package_units_count required` — integer <int32> Количество грузомест.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "bundle_id": "string",
  "delivery_details": {
    "address": "string",
    "comment": "string",
    "date": "2019-08-24T14:15:22Z",
    "sender_name": "string",
    "sender_phone": "string"
  },
  "package_units_count": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика заявки на поставку.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id` — string Идентификатор поставки.

Пример ответа:

```json
{
  "draft_id": 0,
  "row_version": 0,
  "supply_id": "string"
}
```

---

## Отменить черновик заявки на pick-up поставку

`POST /v1/fbp/draft/pick-up/delete`

Operation ID: `FbpAPI_FbpDraftPickUpDelete`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Черновик отменён

#### Схема ответа (200)

- `cancellation_state` — object Статус отмены.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "cancellation_state": {
    "cancellation_error": {
      "error_code": "CODE_UNSPECIFIED",
      "message": "string"
    },
    "cancellation_status": "STATUS_UNSPECIFIED"
  },
  "row_version": 0
}
```

---

## Изменить черновик заявки на pick-up поставку

`POST /v1/fbp/draft/pick-up/dlv/edit`

Operation ID: `FbpAPI_FbpDraftPickupDlvEdit`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `pickup_details required` — object Детали доставки.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.

Пример запроса:

```json
{
  "pickup_details": {
    "address": "string",
    "comment": "string",
    "date": "2019-08-24T14:15:22Z",
    "sender_name": "string",
    "sender_phone": "string"
  },
  "row_version": 0,
  "supply_id": "string"
}
```

### Ответы

- 200 Информация отредактирована

#### Схема ответа (200)

- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

---

## Провалидировать список товаров для pick-up поставки

`POST /v1/fbp/draft/pick-up/product/validate`

Operation ID: `FbpAPI_FbpDraftPickUpProductValidate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `skus required` — Array of objects Список идентификаторов товаров — SKU.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "skus": [
    {
      "count": 0,
      "sku": 0
    }
  ],
  "warehouse_id": 0
}
```

### Ответы

- 200 Список провалидирован

#### Схема ответа (200)

- `approved_items` — Array of objects Подтверждённые товары.
- `bundle_generated` — boolean true , если проверенный список товаров создан.
- `bundle_id` — string Идентификатор провалидированного списка товаров.
- `rejected_items` — Array of objects Отклонённые товары.

Пример ответа:

```json
{
  "approved_items": [
    {
      "barcode": "string",
      "icon_name": "string",
      "name": "string",
      "offer_id": "string",
      "quantity": 0,
      "sku": 0,
      "volume": 0
    }
  ],
  "bundle_generated": true,
  "bundle_id": "string",
  "rejected_items": [
    {
      "barcode": "string",
      "icon_name": "string",
      "name": "string",
      "offer_id": "string",
      "quantity": 0,
      "rejection_reasons": [
        "BUNDLE_ITEM_ERROR_UNSPECIFIED"
      ],
      "sku": 0,
      "volume": 0
    }
  ]
}
```

---

## Перевести черновик в действующую поставку

`POST /v1/fbp/draft/pick-up/registrate`

Operation ID: `FbpDraftPickUpRegistrate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор заявки на поставку.

Пример запроса:

```json
{
  "row_version": 0,
  "supply_id": "string"
}
```

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `error` — object Ошибка.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error": {
    "bundle_errors": [
      {
        "errors": [
          "BUNDLE_ITEM_ERROR_UNSPECIFIED"
        ],
        "sku": 0
      }
    ],
    "order_error": "ORDER_ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---
