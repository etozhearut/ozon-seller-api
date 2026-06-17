# Работа с квантами

_Тег: `Quants` · операций: 2_

## Список эконом-товаров

`POST /v1/product/quant/list`

Operation ID: `QuantProductList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `limit required` — integer <int64> Максимальное количество элементов в ответе.
- `visibility` — string Default: "ALL" Enum: "ALL" "VISIBLE" "INVISIBLE" "EMPTY_STOCK" "NOT_MODERATED" "MODERATED" "DISABLED" "STATE_FAILED" "READY_TO_SUPPLY" "VALIDATION_STATE_PENDING" "VALIDATION_STATE_FAIL" "VALIDATION_STATE_SUCCESS" "TO_SUPPLY" "IN_SALE" "REMOVED_FROM_SALE" "OVERPRICED" "CRITICALLY_OVERPRICED" "EMPTY_BARCODE" "BARCODE_EXISTS" "QUARANTINE" "ARCHIVED" "OVERPRICED_WITH_STOCK" "PARTIAL_APPROVED" Фильтр по видимости товара: ALL — все товары, кроме архивных. VISIBLE — товары, которые видны покупателям. INVISIBLE — товары, которые не видны покупателям. EMPTY_STOCK — товары, которых нет в наличии. NOT_MODERATED — товары, которые не прошли модерацию. MODERATED — товары, которые прошли модерацию. DISABLED — товары, которые видны покупателям, но недоступны к покупке. STATE_FAILED — товары, создание которых завершилось ошибкой. READY_TO_SUPPLY — товары, готовые к поставке. VALIDATION_STATE_PENDING — товары, которые проходят проверку валидатором на премодерации. VALIDATION_STATE_FAIL — товары, которые не прошли проверку валидатором на премодерации. VALIDATION_STATE_SUCCESS — товары, которые прошли проверку валидатором на премодерации. TO_SUPPLY — товары, готовые к продаже. IN_SALE — товары в продаже. REMOVED_FROM_SALE — товары, скрытые от покупателей. OVERPRICED — превышение цены. CRITICALLY_OVERPRICED — критическое превышение цены. EMPTY_BARCODE — пустой штрихкод. BARCODE_EXISTS — штрихкод указан. QUARANTINE — товар в карантине после изменения цены на 50% и больше. ARCHIVED — товары в архиве. OVERPRICED_WITH_STOCK — товары в продаже, цена которых выше, чем у конкурентов. PARTIAL_APPROVED — товары в продаже, у которых пустое или неполное описание.

Пример запроса:

```json
{
  "cursor": "",
  "limit": 1000,
  "visibility": "ALL"
}
```

### Ответы

- 200 Эконом-товары

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `products` — Array of objects Эконом-товары.
- `total_items` — integer <int32> Остаток на всех складах, шт.

Пример ответа:

```json
{
  "cursor": "string",
  "products": [
    {
      "offer_id": "string",
      "product_id": 0,
      "quants": [
        {
          "quant_code": "string",
          "quant_size": 0
        }
      ]
    }
  ],
  "total_items": 0
}
```

---

## Информация об эконом-товаре

`POST /v1/product/quant/info`

Operation ID: `QuantGetInfo`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `quant_code required` — Array of strings [ 1 .. 1000 ] items Список квантов с товарами.

### Ответы

- 200 Информация об эконом-товаре

#### Схема ответа (200)

- `items` — Array of objects Эконом-товары.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `quant_info` — object Информация о кванте.

Пример ответа:

```json
{
  "items": [
    {
      "offer_id": "string",
      "product_id": 0,
      "quant_info": {
        "quants": [
          {
            "barcodes_extended": [
              {
                "barcode": "string",
                "error": "string",
                "status": "string"
              }
            ],
            "dimensions": {
              "depth": 0,
              "height": 0,
              "weight": 0,
              "width": 0
            },
            "marketing_price": {
              "price": "string",
              "seller_price": "string"
            },
            "min_price": "string",
            "old_price": "string",
            "price": "string",
            "quant_code": "string",
            "quant_sice": 0,
            "shipment_type": "string",
            "sku": 0,
            "statuses": {
              "state_description": "string",
              "state_name": "string",
              "state_sys_name": "string",
              "state_tooltip": "string"
            }
          }
        ]
      }
    }
  ]
}
```

---
