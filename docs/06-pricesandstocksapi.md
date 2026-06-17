# Цены и остатки товаров

_Тег: `PricesandStocksAPI` · операций: 11_

## Обновить количество товаров на складах

`POST /v2/products/stocks`

Operation ID: `ProductAPI_ProductsStocksV2`

Позволяет изменить информацию о количестве товара в наличии. Переданный остаток — количество товара в наличии без учёта зарезервированных товаров. Перед обновлением остатков проверьте количество зарезервированных товаров с помощью метода /v2/product/info/stocks-by-warehouse/fbs . За один запрос можно изменить наличие для 100 пар товар-склад. С одного аккаунта продавца можно отправить до 80 запросов в минуту. Обновлять остатки у одной пары товар-склад можно только 1 раз в 30 секунд, иначе в параметре result.errors в ответе будет ошибка TOO_MANY_REQUESTS . Вы можете задать наличие товара только после того, как его статус сменится на price_sent . Остатки крупногабаритных товаров можно обновлять только на предназначенных для них складах. Если запрос содержит оба параметра — offer_id и product_id , изменения применятся к товару с offer_id . Для избежания неоднозначности используйте только один из параметров.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `stocks required` — Array of objects Информация о товарах на складах.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `product_id required` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `stock required` — integer <int64> Количество товара в наличии без учёта зарезервированных товаров.
  - `warehouse_id required` — integer <int64> Идентификатор склада, полученный из метода /v1/warehouse/list .

Пример запроса:

```json
{
  "stocks": [
    {
      "offer_id": "PH11042",
      "product_id": 313455276,
      "stock": 100,
      "warehouse_id": 22142605386000
    }
  ]
}
```

### Ответы

- 200 Количество товаров обновлено
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects
  - `errors` — Array of objects Массив ошибок, которые возникли при обработке запроса.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `updated` — boolean Если запрос выполнен успешно и остатки обновлены — true .
  - `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:

```json
{
  "result": [
    {
      "warehouse_id": 22142605386000,
      "product_id": 118597312,
      "offer_id": "PH11042",
      "updated": true,
      "errors": []
    }
  ]
}
```

---

## Информация о количестве товаров

`POST /v4/product/info/stocks`

Operation ID: `ProductAPI_GetProductInfoStocks`

Возвращает информацию о ĸоличестве товаров по схемам FBS, rFBS и FBP: сĸольĸо единиц есть в наличии, сĸольĸо зарезервировано поĸупателями. Чтобы получить информацию об остатках по схеме FBO, используйте метод /v1/analytics/stocks .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр по товарам.
  - `offer_id` — Array of strings Фильтр по параметру offer_id . Можно передавать список значений.
  - `product_id` — Array of strings <int64> Фильтр по параметру product_id . Можно передавать список значений.
  - `visibility` — string Default: "ALL" Enum: "ALL" "VISIBLE" "INVISIBLE" "EMPTY_STOCK" "NOT_MODERATED" "MODERATED" "DISABLED" "STATE_FAILED" "READY_TO_SUPPLY" "VALIDATION_STATE_PENDING" "VALIDATION_STATE_FAIL" "VALIDATION_STATE_SUCCESS" "TO_SUPPLY" "IN_SALE" "REMOVED_FROM_SALE" "BANNED" "OVERPRICED" "CRITICALLY_OVERPRICED" "EMPTY_BARCODE" "BARCODE_EXISTS" "QUARANTINE" "ARCHIVED" "OVERPRICED_WITH_STOCK" "PARTIAL_APPROVED" "AUTO_ARCHIVED" "MANUAL_ARCHIVED" "SEASONAL_AUTO_ARCHIVED" "VISIBLE_WITH_FBO_STOCK" Фильтр по видимости товара: ALL — все товары, кроме архивных; VISIBLE — товары, которые видны покупателям; INVISIBLE — товары, которые не видны покупателям; EMPTY_STOCK — товары, у которых не указано наличие; NOT_MODERATED — товары, которые не прошли модерацию; MODERATED — товары, которые прошли модерацию; DISABLED — товары, которые видны покупателям, но недоступны к покупке; STATE_FAILED — товары, создание которых завершилось ошибкой; READY_TO_SUPPLY — товары, готовые к поставке; VALIDATION_STATE_PENDING — товары, которые проходят проверку валидатором на премодерации; VALIDATION_STATE_FAIL — товары, которые не прошли проверку валидатором на премодерации; VALIDATION_STATE_SUCCESS — товары, которые прошли проверку валидатором на премодерации; TO_SUPPLY — товары, готовые к продаже; IN_SALE — товары в продаже; REMOVED_FROM_SALE — товары, скрытые от покупателей; BANNED — заблокированные товары; OVERPRICED — товары с завышенной ценой; CRITICALLY_OVERPRICED — товары со слишком завышенной ценой; EMPTY_BARCODE — товары без штрихкода; BARCODE_EXISTS — товары со штрихкодом; QUARANTINE — товары на карантине после изменения цены более чем на 50%; ARCHIVED — товары в архиве; OVERPRICED_WITH_STOCK — товары в продаже со стоимостью выше, чем у конкурентов; PARTIAL_APPROVED — товары в продаже с пустым или неполным описанием; AUTO_ARCHIVED — товары, которые система перенесла в архив автоматически; MANUAL_ARCHIVED — товары, которые продавец перенёс в архив вручную; SEASONAL_AUTO_ARCHIVED — сезонные товары, которые система перенесла в архив автоматически; VISIBLE_WITH_FBO_STOCK — товары с остатками на FBO, которые видят покупатели.
  - `with_quant` — object Товары по тарифу «Эконом».
    - `created` — boolean Активные эконом-товары.
    - `exists` — boolean Эконом-товары во всех статусах.
- `limit required` — integer <int32> Количество значений на странице. Минимум — 1, максимум — 1000.

Пример запроса:

```json
{
  "cursor": "",
  "filter": {
    "offer_id": [
      "1233213232"
    ],
    "product_id": [
      "313455276"
    ],
    "visibility": "ALL",
    "with_quant": {
      "created": true,
      "exists": true
    }
  },
  "limit": 100
}
```

### Ответы

- 200 Количество товара

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `items` — Array of objects Информация о товарах.
- `total` — integer <int32> Количество уникальных товаров, для которых выводится информация об остатках.

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

## Получить информацию по остаткам на складе FBS и rFBS

`POST /v1/product/info/warehouse/stocks`

Operation ID: `ProductInfoWarehouseStocks`

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `limit required` — integer <int64> [ 1 .. 1000 ] Количество значений на странице.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "cursor": "",
  "limit": 10,
  "warehouse_id": 1020003080073000
}
```

### Ответы

- 200 Количество товара на складе FBS и rFBS

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.
- `has_next` — boolean Признак, что в ответе вернули не все товары: true — сделайте повторный запрос с другим значением cursor , чтобы получить остальные значения; false — ответ содержит все значения.
- `stocks` — Array of objects Информация об остатках товара.

Пример ответа:

```json
{
  "stocks": [
    {
      "sku": 147035011,
      "product_id": 28743,
      "offer_id": "02105020-35",
      "warehouse_id": 1020003080073000,
      "present": 1000,
      "reserved": 0,
      "free_stock": 1000,
      "updated_at": "2025-09-15T10:36:24.417498Z"
    }
  ],
  "has_next": false,
  "cursor": "147035011"
}
```

---

## Получить информацию об остатках на складах продавца

`POST /v2/product/info/stocks-by-warehouse/fbs`

Operation ID: `ProductAPI_GetProductInfoStocksByWarehouseFbsV2`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `limit required` — integer <uint64> <= 1000 Количество значений в ответе.
- `offer_id` — Array of strings <= 1000 items Идентификаторы товаров в системе продавца — артикул.
- `sku required` — Array of strings <int64> <= 1000 items Идентификаторы товаров в системе Ozon — SKU.

Пример запроса:

```json
{
  "cursor": "string",
  "limit": 0,
  "offer_id": [
    "string"
  ],
  "sku": [
    "string"
  ]
}
```

### Ответы

- 200 Количество товаров на складах
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернули не все товары.
- `products` — Array of objects Остатки товаров.

Пример ответа:

```json
{
  "cursor": "string",
  "has_next": true,
  "products": [
    {
      "free_stock": 0,
      "offer_id": "string",
      "present": 0,
      "product_id": 0,
      "reserved": 0,
      "sku": 0,
      "warehouse_id": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Информация об остатках на складах продавца (FBS и rFBS)

`POST /v1/product/info/stocks-by-warehouse/fbs`

Operation ID: `ProductAPI_ProductStocksByWarehouseFbs`

Метод устаревает и будет отключён 7 апреля 2026 года. Переключитесь на /v2/product/info/stocks-by-warehouse/fbs . Передайте в запросе offer_id или sku . Если укажете оба, будет использован только sku .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `sku required` — Array of strings <int64> Идентификатор товара в системе Ozon — SKU.
- `offer_id` — Array of strings <int64> Идентификатор товара в системе продавца — артикул.

Пример запроса:

```json
{
  "sku": [
    "string"
  ],
  "offer_id": [
    "string"
  ]
}
```

### Ответы

- 200 Количество товаров на складах FBS и rFBS
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `offer_id` — string <int64> Идентификатор товара в системе продавца — артикул.
  - `present` — integer <int64> Общее количество товара на складе.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — артикул.
  - `reserved` — integer <int64> Количество зарезервированных товаров на складе.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `warehouse_name` — string Название склада.

Пример ответа:

```json
{
  "result": [
    {
      "sku": 0,
      "offer_id": "string",
      "present": 0,
      "product_id": 0,
      "reserved": 0,
      "warehouse_id": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Обновить цену

`POST /v1/product/import/prices`

Operation ID: `ProductAPI_ImportProductsPrices`

Позволяет изменить цену одного или нескольких товаров. Цену каждого товара можно обновлять не больше 10 раз в час. Чтобы сбросить old_price , поставьте 0 у этого параметра. Если у товара установлена минимальная цена и включено автоприменение в акции, отключите его и обновите минимальную цену. Иначе вернётся ошибка action_price_enabled_min_price_missing . Если запрос содержит оба параметра — offer_id и product_id , изменения применятся к товару с offer_id . Для избежания неоднозначности используйте только один из параметров.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `prices` — Array of objects <= 1000 items Информация о ценах товаров.

Пример запроса:

```json
{
  "prices": [
    {
      "auto_action_enabled": "UNKNOWN",
      "auto_add_to_ozon_actions_list_enabled": "UNKNOWN",
      "currency_code": "RUB",
      "manage_elastic_boosting_through_price": true,
      "min_price": "800",
      "min_price_for_auto_actions_enabled": true,
      "net_price": "650",
      "offer_id": "",
      "old_price": "0",
      "price": "1448",
      "price_strategy_enabled": "UNKNOWN",
      "product_id": 1386,
      "quant_size": 1,
      "vat": "0.1"
    }
  ]
}
```

### Ответы

- 200 Цена обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результаты запроса.
  - `errors` — Array of objects Массив ошибок, которые возникли при обработке запроса.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `updated` — boolean Если информации о товаре успешно обновлена — true .

Пример ответа:

```json
{
  "result": [
    {
      "product_id": 1386,
      "offer_id": "PH8865",
      "updated": true,
      "errors": []
    }
  ]
}
```

---

## Обновление таймера актуальности минимальной цены

`POST /v1/product/action/timer/update`

Operation ID: `ProductAPI_ActionTimerUpdate`

Минимальная цена действует 30 дней после установки. После этого настройка выключается. Вы можете продлить её: вызовите метод повторно и укажите product_ids .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `product_ids` — Array of strings <int64> <= 1000 items Список идентификаторов товаров в системе Ozon — product_id .

### Ответы

- 200 Обновлено

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

## Получить статус установленного таймера

`POST /v1/product/action/timer/status`

Operation ID: `ProductAPI_ActionTimerStatus`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `product_ids` — number <= 1000 Список идентификаторов товаров в системе Ozon — product_id .

### Ответы

- 200 Статусы

#### Схема ответа (200)

- `statuses` — Array of objects
  - `expired_at` — string <date-time> Время окончания таймера. Если параметр пустой, активного таймера нет.
  - `min_price_for_auto_actions_enabled` — boolean true , если Ozon учитывает минимальную цену при добавлении в акции.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .

Пример ответа:

```json
{
  "statuses": [
    {
      "expired_at": "2019-08-24T14:15:22Z",
      "min_price_for_auto_actions_enabled": true,
      "product_id": 0
    }
  ]
}
```

---

## Получить информацию о цене товара

`POST /v5/product/info/prices`

Operation ID: `ProductAPI_GetProductInfoPrices`

Вы можете посмотреть историю обновления цен только в личном кабинете продавца. Подробнее об истории обновления цен в Базе знаний продавца

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр по товарам.
- `limit required` — integer <int32> [ 1 .. 1000 ] Количество значений на странице.

Пример запроса:

```json
{
  "cursor": "",
  "filter": {
    "offer_id": [
      "356792"
    ],
    "product_id": [
      "243686911"
    ],
    "visibility": "ALL"
  },
  "limit": 100
}
```

### Ответы

- 200 Информация о цене товара
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `items` — Array of objects Список товаров.
- `total` — integer <int32> Количество товаров в списке.

Пример ответа:

```json
{
  "items": [
    {
      "product_id": 1000123456,
      "offer_id": "test-offer-123456",
      "price": {
        "price": 2999.99,
        "old_price": 3499.99,
        "min_price": 2799.99,
        "net_price": 2000,
        "currency_code": "RUB",
        "vat": 0.2,
        "auto_action_enabled": true,
        "auto_add_to_ozon_actions_list_enabled": true
      },
      "commissions": {
        "sales_percent_fbo": 15,
        "sales_percent_fbs": 12,
        "fbo_deliv_to_customer_amount": 14.75,
        "fbs_deliv_to_customer_amount": 60,
        "fbo_return_flow_amount": 50,
        "fbs_return_flow_amount": 40
      },
      "price_indexes": {
        "color_index": "GREEN",
        "ozon_index_data": {
          "min_price": 2899.99,
          "min_price_currency": "RUB",
          "price_index_value": 0.95
        },
        "external_index_data": {
          "min_price": 2799.99,
          "min_price_currency": "RUB",
          "price_index_value": 0.93
        }
      },
      "marketing_actions": {
        "ozon_actions_exist": true,
        "current_period_from": "2026-03-01T00:00:00Z",
        "current_period_to": "2026-03-31T23:59:59Z",
        "actions": [
          {
            "title": "Скидка 15% на электронику",
            "value": 15,
            "date_from": "2026-03-01T00:00:00Z",
            "date_to": "2026-03-31T23:59:59Z"
          }
        ]
      },
      "acquiring": 1.5,
      "volume_weight": 0.5
    },
    {
      "product_id": 1000123457,
      "offer_id": "test-offer-123457",
      "price": {
        "price": 5999.99,
        "old_price": 6999.99,
        "min_price": 5499.99,
        "net_price": 4000,
        "currency_code": "RUB",
        "vat": 0.2,
        "auto_action_enabled": false,
        "auto_add_to_ozon_actions_list_enabled": false
      },
      "commissions": {
        "sales_percent_fbo": 10,
        "sales_percent_fbs": 8,
        "fbo_deliv_to_customer_amount": 25.5,
        "fbs_deliv_to_customer_amount": 75,
        "fbo_return_flow_amount": 60,
        "fbs_return_flow_amount": 50
      },
      "price_indexes": {
        "color_index": "YELLOW",
        "ozon_index_data": {
          "min_price": 5899.99,
          "min_price_currency": "RUB",
          "price_index_value": 0.98
        }
      },
      "marketing_actions": {
        "ozon_actions_exist": false
      },
      "acquiring": 1.8,
      "volume_weight": 0.8
    }
  ],
  "cursor": "",
  "total": 2
}
```

---

## Узнать информацию об уценке и основном товаре по SKU уценённого товара

`POST /v1/product/info/discounted`

Operation ID: `ProductAPI_GetProductInfoDiscounted`

Метод для получения информации о состоянии и дефектах уценённого товара по его SKU. Работает только с уценёнными товарами по схеме FBO. Также метод возвращает SKU основного товара.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `discounted_skus required` — Array of strings <int64> Список SKU уценённых товаров.

Пример запроса:

```json
{
  "discounted_skus": [
    "635548518"
  ]
}
```

### Ответы

- 200 Информация об уценке и основном товаре
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `items` — Array of objects Информация об уценке и основном товаре.
  - `comment_reason_damaged` — string Комментарий к причине повреждения.
  - `condition` — string Состояние товара — новый или Б/У.
  - `condition_estimation` — string Состояние товара по шкале от 1 до 7: 1 — удовлетворительное, 2 — хорошее, 3 — очень хорошее, 4 — отличное, 5–7 — как новый.
  - `defects` — string Дефекты товара.
  - `discounted_sku` — integer <int64> SKU уценённого товара.
  - `mechanical_damage` — string Описание механического повреждения.
  - `package_damage` — string Описание повреждения упаковки.
  - `packaging_violation` — string Признак нарушения целостности упаковки.
  - `reason_damaged` — string Причина повреждения.
  - `repair` — string Признак, что товар отремонтирован.
  - `shortage` — string Признак, что товар некомплектный.
  - `sku` — integer <int64> SKU основного товара.
  - `warranty_type` — string Наличие у товара действующей гарантии.

Пример ответа:

```json
{
  "items": [
    {
      "discounted_sku": 635548518,
      "sku": 320067758,
      "condition_estimation": "4",
      "packaging_violation": "",
      "warranty_type": "",
      "reason_damaged": "Механическое повреждение",
      "comment_reason_damaged": "повреждена заводская упаковка",
      "defects": "",
      "mechanical_damage": "",
      "package_damage": "",
      "shortage": "",
      "repair": "",
      "condition": ""
    }
  ]
}
```

---

## Установить скидку на уценённый товар

`POST /v1/product/update/discount`

Operation ID: `ProductAPI_ProductUpdateDiscount`

Метод для установки размера скидки на уценённые товары, продающиеся по схеме FBS.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `discount required` — integer <int32> Размер скидки: от 3 до 99 процентов.
- `product_id required` — integer <int64> Идентификатор товара в системе Ozon — product_id .

Пример запроса:

```json
{
  "discount": 10,
  "product_id": 876763232
}
```

### Ответы

- 200 Успешно
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат работы метода. true , если запрос выполнен без ошибок.

---
