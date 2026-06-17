# Доставка FBO

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FBO` · методов: 15_

## Получить список отправлений

`POST /v3/posting/fbo/list`

Operation ID: `PostingFboList`

Возвращает список отправлений за указанный период времени. Если период больше года, вернётся ошибка PERIOD_IS_TOO_LONG . Дополнительно можно отфильтровать отправления по их статусу.

```bash
curl -X POST "https://api-seller.ozon.ru/v3/posting/fbo/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cursor": "",
  "filter": {
    "posting_numbers": [
      "9121072335-0187-1"
    ],
    "since": "2026-05-10T11:51:30.106Z",
    "statuses": [
      "awaiting_packaging",
      "awaiting_deliver",
      "delivering",
      "delivered",
      "cancelled"
    ],
    "to": "2026-05-15T11:51:30.106Z"
  },
  "limit": 100,
  "sort_dir": "ASC",
  "translit": false,
  "with": {
    "analytics_data": true,
    "financial_data": true,
    "legal_info": true
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр для поиска отправлений.
- `limit` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.
- `translit` — boolean true , чтобы включить транслитерацию адреса из кириллицы в латиницу.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернулись не все отправления.
- `postings` — Array of objects Список отправлений.

Пример ответа:
```json
{
  "has_next": false,
  "cursor": "",
  "postings": [
    {
      "posting_number": "9121072335-0187-1",
      "order_id": 39127012767,
      "order_number": "9121072335-0187",
      "status": "delivered",
      "substatus": "posting_received",
      "cancellation": null,
      "products": [
        {
          "is_marketplace_buyout": false,
          "offer_id": "SM-GALAXY-A54-128",
          "name": "Смартфон Samsung Galaxy A54",
          "sku": 9880276314,
          "quantity": 1,
          "price": {
            "amount": "28990",
            "currency": "RUB"
          },
          "digital_codes": []
        }
      ],
      "analytics_data": {
        "city": "",
        "delivery_type": "PVZ",
        "is_premium": false,
        "payment_type_group_name": "Система Быстрых Платежей",
        "warehouse_id": 1020001835658000,
        "warehouse_name": "ОМСК_РФЦ",
        "is_legal": false,
        "client_delivery_date_begin": null,
        "client_delivery_date_end": null
      },
      "financial_data": {
        "products": [
          {
            "payout": 0,
            "product_id": 9880276314,
            "old_price": 28990,
            "price": 28990,
            "total_discount_value": 0,
            "total_discount_percent": 0,
            "actions": [],
            "commission": {
              "amount": 0,
              "percent": 0,
              "currency": "RUB"
            }
          }
        ],
        "cluster_from": "Омск",
        "cluster_to": "Новосибирск"
      },
      "legal_info": null,
      "external_order": {
        "is_external": false,
        "platform_name": ""
      },
      "cancel_reason_id": 0,
      "created_at": "2026-05-10T23:59:51.653736Z",
      "in_process_at": "2026-05-11T00:00:07.716614Z",
      "additional_data": []
    }
  ]
}
```

---

## Список отправлений Deprecated

`POST /v2/posting/fbo/list`

Operation ID: `PostingAPI_GetFboPostingList`

С 1 июня 2026 года метод будет отключён. Переключитесь на /v3/posting/fbo/list . Возвращает список отправлений за указанный период времени. Если период больше года, вернётся ошибка PERIOD_IS_TOO_LONG . Дополнительно можно отфильтровать отправления по их статусу.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/posting/fbo/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "dir": "ASC",
  "filter": {
    "since": "2021-09-01T00:00:00.000Z",
    "status": "",
    "to": "2021-11-17T10:44:12.828Z"
  },
  "limit": 5,
  "offset": 0,
  "translit": true,
  "with": {
    "analytics_data": true,
    "financial_data": true,
    "legal_info": false
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `dir` — string Направление сортировки: ASC — по возрастанию, DESC — по убыванию.
- `filter required` — object Фильтр для поиска отправлений.
- `limit required` — integer <int64> Количество значений в ответе: максимум — 1000, минимум — 1.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента. Максимальное значение — 20000.
- `translit` — boolean Если включена транслитерация адреса из кириллицы в латиницу — true .
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Массив отправлений.
  - `additional_data` — Array of objects
  - `analytics_data` — object Данные аналитики.
  - `cancel_reason_id` — integer <int64> Идентификатор причины отмены отправления.
  - `created_at` — string <date-time> Дата и время создания отправления.
  - `financial_data` — object Финансовые данные.
  - `in_process_at` — string <date-time> Дата и время начала обработки отправления.
  - `legal_info` — object Юридическая информация о покупателе.
  - `order_id` — integer <int64> Идентификатор заказа, к которому относится отправление.
  - `order_number` — string Номер заказа, к которому относится отправление.
  - `posting_number` — string Номер отправления.
  - `products` — Array of objects Список товаров в отправлении.
  - `status` — string Статус отправления: awaiting_packaging — ожидает упаковки, awaiting_deliver — ожидает отгрузки, delivering — доставляется, delivered — доставлено, cancelled — отменено.
  - `substatus` — string Подстатус отправления: posting_split_pending , posting_created — создано; posting_packing — на упаковке; posting_transferring_to_delivery — передаётся в доставку; posting_on_way_to_city — на пути в город доставки; posting_returned_to_warehouse — возвращено на склад; posting_transferred_to_courier_service — передаётся в службу доставки; posting_in_courier_service — курьер в пути; posting_on_way_to_pickup_point — в пути в пункт выдачи; posting_in_pickup_point — в пункте выдачи; posting_delivered — доставлено курьером; posting_received — получено в пункте выдачи; posting_canceled — отменено.

Пример ответа:
```json
{
  "result": [
    {
      "order_id": 354680487,
      "order_number": "16965409-0014",
      "posting_number": "16965409-0014-1",
      "status": "delivered",
      "substatus": "posting_delivered",
      "cancel_reason_id": 7,
      "created_at": "2021-09-01T00:23:45.607000Z",
      "in_process_at": "2021-09-01T00:25:30.120000Z",
      "legal_info": {
        "company_name": "ООО Книжный Мир",
        "inn": "77123456789",
        "kpp": "777777777"
      },
      "products": [
        {
          "sku": 160249683,
          "name": "Так говорил Омар Хайям. Жизнеописание. Афоризмы и рубайят. Классика в словах и картинках",
          "quantity": 1,
          "offer_id": "978-5-906864-56-7",
          "price": "81.00",
          "is_marketplace_buyout": true,
          "digital_codes": [],
          "currency_code": "RUB"
        }
      ],
      "analytics_data": {
        "city": "Ростов-на-Дону",
        "delivery_type": "PVZ",
        "is_premium": false,
        "payment_type_group_name": "Карты оплаты",
        "warehouse_id": 17717042026000,
        "warehouse_name": "РОСТОВ-НА-ДОНУ_РФЦ",
        "is_legal": false,
        "client_delivery_date_begin": "2025-11-24T13:32:40.336Z",
        "client_delivery_date_end": "2025-11-24T18:32:40.336Z"
      },
      "financial_data": {
        "products": [
          {
            "commission_amount": 12.15,
            "commission_percent": 15,
            "payout": 68.85,
            "product_id": 160249683,
            "currency_code": "RUB",
            "old_price": 115,
            "price": 81,
            "total_discount_value": 34,
            "total_discount_percent": 29.57,
            "actions": [
              "Системная виртуальная скидка селлера"
            ],
            "quantity": 1
          }
        ]
      },
      "additional_data": [
        {
          "delivery_track_number": "RX123456789RU",
          "courier_name": "Иванов П.С.",
          "delivery_time": "2025-11-24T15:20:00.000Z"
        }
      ]
    }
  ]
}
```

---

## Информация об отправлении

`POST /v2/posting/fbo/get`

Operation ID: `PostingAPI_GetFboPosting`

Возвращает информацию об отправлении по его идентификатору.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/posting/fbo/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "50520644-0012-7",
  "translit": true,
  "with": {
    "analytics_data": true,
    "financial_data": true,
    "legal_info": false
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.
- `translit` — boolean Если включена транслитерация адреса из кириллицы в латиницу — true .
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

### Ответы

- 200 Информация об отправлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `additional_data` — Array of objects
  - `analytics_data` — object Данные аналитики.
  - `cancel_reason_id` — integer <int64> Идентификатор причины отмены отправления.
  - `created_at` — string <date-time> Дата и время создания отправления.
  - `fact_delivery_date` — string <date-time> Дата фактической передачи отправления в доставку.
  - `financial_data` — object Финансовые данные.
  - `in_process_at` — string <date-time> Дата и время начала обработки отправления.
  - `legal_info` — object Юридическая информация о покупателе.
  - `order_id` — integer <int64> Идентификатор заказа, к которому относится отправление.
  - `order_number` — string Номер заказа, к которому относится отправление.
  - `posting_number` — string Номер отправления.
  - `products` — Array of objects Список товаров в отправлении.
  - `status` — string Статус отправления: awaiting_packaging — ожидает упаковки, awaiting_deliver — ожидает отгрузки, delivering — доставляется, delivered — доставлено, cancelled — отменено.
  - `substatus` — string Подстатус отправления: posting_split_pending , posting_created — создано; posting_packing — на упаковке; posting_transferring_to_delivery — передаётся в доставку; posting_on_way_to_city — на пути в город доставки; posting_returned_to_warehouse — возвращено на склад; posting_transferred_to_courier_service — передаётся в службу доставки; posting_in_courier_service — курьер в пути; posting_on_way_to_pickup_point — в пути в пункт выдачи; posting_in_pickup_point — в пункте выдачи; posting_delivered — доставлено курьером; posting_received — получено в пункте выдачи; posting_canceled — отменено.

Пример ответа:
```json
{
  "result": {
    "order_id": 354679434,
    "order_number": "50520644-0012",
    "posting_number": "50520644-0012-7",
    "status": "delivered",
    "substatus": "posting_delivered",
    "cancel_reason_id": 0,
    "created_at": "2021-09-01T00:34:56.563Z",
    "fact_delivery_date": "string",
    "in_process_at": "2021-09-01T00:34:56.103Z",
    "legal_info": {
      "company_name": "string",
      "inn": "string",
      "kpp": "string"
    },
    "products": [
      {
        "sku": 254665483,
        "name": "Мочалка натуральная из люфы с деревянной ручкой",
        "quantity": 1,
        "offer_id": "PS1033",
        "price": "137.00",
        "is_marketplace_buyout": true,
        "digital_codes": [],
        "currency_code": "RUB"
      }
    ],
    "analytics_data": {
      "city": "",
      "delivery_type": "Courier",
      "is_premium": false,
      "payment_type_group_name": "Карты оплаты",
      "warehouse_id": 15431806189000,
      "warehouse_name": "ХОРУГВИНО_РФЦ",
      "is_legal": false,
      "client_delivery_date_begin": "2025-11-24T13:32:40.336Z",
      "client_delivery_date_end": "2025-11-24T13:32:40.336Z"
    },
    "financial_data": {
      "products": [
        {
          "commission_amount": 13.7,
          "commission_percent": 10,
          "payout": 123.3,
          "product_id": 254665483,
          "currency_code": "RUB",
          "customer_currency_code": "RUB",
          "old_price": 198,
          "price": 137,
          "total_discount_value": 61,
          "total_discount_percent": 30.81,
          "actions": [
            "Системная виртуальная скидка селлера"
          ],
          "quantity": 0
        }
      ]
    },
    "additional_data": []
  }
}
```

---

## Причины отмены отправлений по схеме FBO

`POST /v1/posting/fbo/cancel-reason/list`

Operation ID: `PostingAPI_GetPostingFboCancelReasonList`

Возвращает список причин отмены для всех FBO-отправлений.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/fbo/cancel-reason/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Причины отмены отправлений

#### Схема ответа (200)

- `reasons` — Array of objects Информация о причинах отмены.
  - `id` — integer <int64> Идентификатор причины отмены.
  - `name` — string Причина отмены.

Пример ответа:
```json
{
  "result": [
    {
      "id": 352,
      "title": "Товар закончился на складе продавца",
      "type_id": "seller",
      "is_available_for_cancellation": true
    },
    {
      "id": 401,
      "title": "Продавец отклонил арбитраж",
      "type_id": "seller",
      "is_available_for_cancellation": false
    },
    {
      "id": 402,
      "title": "Другое (вина продавца)",
      "type_id": "seller",
      "is_available_for_cancellation": true
    },
    {
      "id": 666,
      "title": "Возврат из службы доставки: нет доставки в указанный регион",
      "type_id": "seller",
      "is_available_for_cancellation": false
    }
  ]
}
```

---

## Количество заявок по статусам

`POST /v1/supply-order/status/counter`

Operation ID: `SupplyOrderAPI_SupplyOrderStatusCounter`

Возвращает количество заявок в конкретном статусе.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/status/counter" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Статус заявки и количество заявок в этом статусе

#### Схема ответа (200)

- `items` — Array of objects
  - `count` — integer <int32> Количество заявок в статусе.
  - `order_state` — string Default: "ORDER_STATE_UNSPECIFIED" Enum: "ORDER_STATE_UNSPECIFIED" "ORDER_STATE_DATA_FILLING" "ORDER_STATE_READY_TO_SUPPLY" "ORDER_STATE_ACCEPTED_AT_SUPPLY_WAREHOUSE" "ORDER_STATE_IN_TRANSIT" "ORDER_STATE_ACCEPTANCE_AT_STORAGE_WAREHOUSE" "ORDER_STATE_REPORTS_CONFIRMATION_AWAITING" "ORDER_STATE_REPORT_REJECTED" "ORDER_STATE_COMPLETED" "ORDER_STATE_REJECTED_AT_SUPPLY_WAREHOUSE" "ORDER_STATE_CANCELLED" Статус поставки: UNSPECIFIED — статус не указан; DATA_FILLING — заполнение данных; READY_TO_SUPPLY — готова к отгрузке; ACCEPTED_AT_SUPPLY_WAREHOUSE — принята на точке отгрузки; IN_TRANSIT — в пути; ACCEPTANCE_AT_STORAGE_WAREHOUSE — приёмка на складе; REPORTS_CONFIRMATION_AWAITING — согласование актов; REPORT_REJECTED — спор; COMPLETED — завершена; REJECTED_AT_SUPPLY_WAREHOUSE — отказано в приёмке; CANCELLED — отменена.

Пример ответа:
```json
{
  "items": [
    {
      "count": 1,
      "order_state": "ORDER_STATE_UNSPECIFIED"
    }
  ]
}
```

---

## Состав поставки или заявки на поставку

`POST /v1/supply-order/bundle`

Operation ID: `SupplyOrderBundle`

Используйте метод, чтобы получить товарный состав поставки или черновика заявки на поставку. Одним вызовом метода можно получить состав одной поставки или черновика заявки.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/bundle" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "bundle_ids": [
    "bundle_123456789",
    "bundle_987654321"
  ],
  "is_asc": true,
  "item_tags_calculation": {
    "dropoff_warehouse_id": 17717042026000,
    "storage_warehouse_ids": [
      "17717042026001",
      "17717042026002"
    ]
  },
  "last_id": "item_555123456789",
  "limit": 100,
  "query": "книга Хайям",
  "sort_field": "NAME"
}'
```

### Тело запроса

- `bundle_ids required` — Array of strings [ 1 .. 100 ] items Идентификаторы товарного состава поставки. Можно получить в методе /v3/supply-order/get .
- `is_asc` — boolean true , чтобы сортировать по возрастанию.
- `item_tags_calculation` — object Список складов для расчёта товарных тегов.
- `last_id` — string Идентификатор последнего значения SKU на странице.
- `limit required` — integer <int32> [ 1 .. 100 ] Количество товаров на странице.
- `query` — string Поисковый запрос, например: по названию, артикулу или SKU.
- `sort_field` — string Enum: "SKU" "NAME" "QUANTITY" "TOTAL_VOLUME_IN_LITRES" Сортировка по параметрам: SKU — SKU; NAME — названию товара; QUANTITY — количеству; TOTAL_VOLUME_IN_LITRES — объёму в литрах.

### Ответы

- 200 Состав поставки

#### Схема ответа (200)

- `items` — Array of objects Список товаров в заявке на поставку.
- `total_count` — integer <int32> Количество товаров в заявке.
- `has_next` — boolean Признак, что в ответе вернули не все товары: true — сделайте повторный запрос с другим значением last_id , чтобы получить остальные значения; false — ответ содержит все значения характеристики.
- `last_id` — string Идентификатор последнего значения на странице.

Пример ответа:
```json
{
  "items": [
    {
      "icon_path": "https://example.com/icons/book_icon_12345.png",
      "sku": 160249683,
      "name": "Так говорил Омар Хайям. Жизнеописание. Афоризмы и рубайят",
      "offer_id": "978-5-906864-56-7",
      "quantity": 5,
      "barcode": "9785906864567",
      "product_id": 160249683,
      "quant": 5,
      "is_quant_editable": true,
      "volume_in_litres": 0.5,
      "total_volume_in_litres": 2.5,
      "contractor_item_code": "CONTRACT_56789",
      "sfbo_attribute": "ITEM_SFBO_ATTRIBUTE_STANDARD",
      "shipment_type": "BUNDLE_ITEM_SHIPMENT_TYPE_PICKUP",
      "tags": [
        "EVSD_REQUIRED",
        "FRAGILE"
      ],
      "placement_zone": "ZONE_A"
    },
    {
      "icon_path": "https://example.com/icons/toy_icon_67890.png",
      "sku": 123456789,
      "name": "Конструктор LEGO Classic 11019",
      "offer_id": "LEGO_11019",
      "quantity": 10,
      "barcode": "5702017115689",
      "product_id": 123456789,
      "quant": 10,
      "is_quant_editable": true,
      "volume_in_litres": 1.2,
      "total_volume_in_litres": 12,
      "contractor_item_code": "CONTRACT_12345",
      "sfbo_attribute": "ITEM_SFBO_ATTRIBUTE_LARGE",
      "shipment_type": "BUNDLE_ITEM_SHIPMENT_TYPE_DELIVERY",
      "tags": [
        "EVSD_REQUIRED"
      ],
      "placement_zone": "SORT"
    }
  ],
  "total_count": 2,
  "has_next": false,
  "last_id": "item_123456789"
}
```

---

## Список заявок на поставку на склад Ozon

`POST /v3/supply-order/list`

Operation ID: `SupplyOrderList`

Учитываются заявки с поставкой на конкретный склад и через виртуальный распределительный центр (вРЦ) .

```bash
curl -X POST "https://api-seller.ozon.ru/v3/supply-order/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "states": [
      "COMPLETED"
    ]
  },
  "last_id": "null",
  "limit": 1,
  "sort_by": "ORDER_CREATION",
  "sort_dir": "DESC"
}'
```

### Тело запроса

- `filter required` — object Фильтр.
- `last_id` — string Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit required` — integer <int32> [ 1 .. 100 ] Количество значений на странице.
- `sort_by required` — string Enum: "ORDER_CREATION" "ORDER_STATE_UPDATED_AT" "TIMESLOT_FROM_UTC" "TIMESLOT_FROM_LOCAL" Параметр, по которому заявки на поставку будут отсортированы: ORDER_CREATION — по дате создания заявки; ORDER_STATE_UPDATED_AT — по обновлению статуса заявки; TIMESLOT_FROM_UTC — по таймслоту в UTC; TIMESLOT_FROM_LOCAL — по таймслоту в локальном времени.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.

### Ответы

- 200 Список заявок на поставку

#### Схема ответа (200)

- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, укажите полученное значение в следующем запросе в параметре last_id .
- `order_ids` — Array of strings <int64> Идентификаторы заявок на поставку.

Пример ответа:
```json
{
  "order_ids": [
    3424231244
  ],
  "last_id": "string"
}
```

---

## Информация о заявке на поставку

`POST /v3/supply-order/get`

Operation ID: `SupplyOrderGet`

Учитываются заявки с поставкой на конкретный склад и через виртуальный распределительный центр (вРЦ) .

```bash
curl -X POST "https://api-seller.ozon.ru/v3/supply-order/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `order_ids required` — Array of strings <int64> <= 50 items Идентификаторы заявок на поставку.

### Ответы

- 200 Информация о заявке

#### Схема ответа (200)

- `orders` — Array of objects Список заявок на поставку.
  - `created_date` — string <date-time> Дата создания заявки на поставку.
  - `data_filling_deadline_utc` — string <date-time> Время в секундах, оставшееся на заполнение данных по поставке. Только для заявок с вРЦ.
  - `dropoff_warehouse` — object Информация о пункте отгрузки.
  - `order_id` — integer <int64> Идентификатор заявки на поставку.
  - `order_number` — string Номер заявки на поставку.
  - `order_tags` — object Метки заявки на поставку.
  - `state` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "DATA_FILLING" "READY_TO_SUPPLY" "ACCEPTED_AT_SUPPLY_WAREHOUSE" "IN_TRANSIT" "ACCEPTANCE_AT_STORAGE_WAREHOUSE" "REPORTS_CONFIRMATION_AWAITING" "REPORT_REJECTED" "COMPLETED" "REJECTED_AT_SUPPLY_WAREHOUSE" "CANCELLED" "OVERDUE" Статус заявки на поставку: UNSPECIFIED — не определён; DATA_FILLING — заполнение данных; READY_TO_SUPPLY — готова к отгрузке; ACCEPTED_AT_SUPPLY_WAREHOUSE — принята на точке отгрузки; IN_TRANSIT — в пути; ACCEPTANCE_AT_STORAGE_WAREHOUSE — приёмка на складе; REPORTS_CONFIRMATION_AWAITING — согласование актов; REPORT_REJECTED — спор; COMPLETED — завершена; REJECTED_AT_SUPPLY_WAREHOUSE — отказано в приёмке; CANCELLED — отменена; OVERDUE — просрочена.
  - `state_updated_date` — string <date-time> Дата обновления статуса заявки на поставку.
  - `supplies` — Array of objects Информация о поставках.
  - `timeslot` — object Информация о таймслоте.

Пример ответа:
```json
{
  "orders": [
    {
      "order_id": 84882285,
      "order_number": "2111140905880",
      "created_date": "2026-01-23T00:00:00Z",
      "state": "READY_TO_SUPPLY",
      "state_updated_date": "2026-02-03T11:32:41.613489Z",
      "data_filling_deadline_utc": "2026-02-14T23:59:59Z",
      "dropoff_warehouse": {
        "warehouse_id": 23843917228000,
        "address": "г. Москва, ул. Летняя, д. 12, стр. 5",
        "name": "Центральный логистический комплекс"
      },
      "order_tags": {
        "is_econom": false,
        "is_virtual": false,
        "original_supply_id": 2111140905880,
        "is_super_fbo": false,
        "product_super_fbo": false,
        "is_quant": false
      },
      "timeslot": {
        "timeslot": {
          "from": "2026-02-15T09:00:00Z",
          "to": "2026-02-15T10:00:00Z"
        },
        "timezone_info": {
          "offset": "+03:00",
          "iana_name": "Europe/Moscow"
        }
      },
      "supplies": [
        {
          "supply_id": 2111140905880,
          "storage_warehouse": {
            "warehouse_id": 23843917228000,
            "arrival_date": "2026-02-15T00:00:00Z",
            "address": "г. Москва, ул. Летняя, д. 12, стр. 5",
            "name": "Центральный логистический комплекс"
          },
          "supply_tags": {
            "is_ettn_required": false,
            "is_evsd_required": false,
            "is_jewelry": false,
            "is_marking_possible": true,
            "is_marking_required": false,
            "is_utd": false,
            "freeze_stock_for_marking": false
          },
          "is_crossdock": false,
          "bundle_id": "019c5755-8768-7a00-9582-7c1f2c81681f",
          "state": "READY_TO_SUPPLY"
        }
      ]
    }
  ]
}
```

---

## Интервалы поставки

`POST /v1/supply-order/timeslot/get`

Operation ID: `SupplyOrderAPI_GetSupplyOrderTimeslots`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/timeslot/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_order_id required` — integer <int64> Идентификатор заявки на поставку.

### Ответы

- 200 Список интервалов поставки

#### Схема ответа (200)

- `timeslots` — Array of objects Интервалы поставки.
- `timezone` — Array of objects Часовой пояс.

Пример ответа:
```json
{
  "timeslots": [
    {
      "from": "2019-08-24T14:15:22Z",
      "to": "2019-08-24T14:15:22Z"
    }
  ],
  "timezone": [
    {
      "iana_name": "MSK",
      "offset": "+3"
    }
  ]
}
```

---

## Обновить интервал поставки

`POST /v1/supply-order/timeslot/update`

Operation ID: `SupplyOrderAPI_UpdateSupplyOrderTimeslot`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/timeslot/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "supply_order_id": 84882285,
  "timeslot": {
    "from": "2019-08-24T14:15:22Z",
    "to": "2019-08-24T14:15:22Z"
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_order_id required` — integer <int64> Идентификатор заявки на поставку.
- `timeslot required` — object Время интервала поставки.

### Ответы

- 200 Интервал обновлён

#### Схема ответа (200)

- `errors` — Array of strings Items Enum: "UPDATE_TIMESLOT_ERROR_UNSPECIFIED" "UPDATE_TIMESLOT_ERROR_INVALID_ORDER_STATE" "UPDATE_TIMESLOT_ERROR_INCOMPATIBLE_ORDER_FLOW" "UPDATE_TIMESLOT_ERROR_SET_TIMESLOT_DEADLINE_EXCEED" "UPDATE_TIMESLOT_ERROR_OUT_OF_ALLOWED_RANGE" "UPDATE_TIMESLOT_ERROR_ORDER_NOT_BELONG_CONTRACTOR" "UPDATE_TIMESLOT_ERROR_ORDER_NOT_BELONG_COMPANY" "UPDATE_TIMESLOT_ERROR_PICKUP_ORDER_LIMIT_EXCEEDED" Возможные ошибки: UNSPECIFIED — статус не указан; INVALID_ORDER_STATE — неверный статус заказа; INCOMPATIBLE_ORDER_FLOW — неверный статус интервала поставки; SET_TIMESLOT_DEADLINE_EXCEED — заявка на поставку просрочена; OUT_OF_ALLOWED_RANGE — вы ввели некорректное значение интервала поставки; ORDER_NOT_BELONG_CONTRACTOR — заявка создана другим юридическим лицом, работать с ней не получится; ORDER_NOT_BELONG_COMPANY — заявка не принадлежит вашему кабинету, работать с ней не получится; UPDATE_TIMESLOT_ERROR_PICKUP_ORDER_LIMIT_EXCEEDED — превышен суточный лимит на создание заявок на поставку курьером.
- `operation_id` — string Идентификатор операции.

Пример ответа:
```json
{
  "errors": [
    "UPDATE_TIMESLOT_ERROR_UNSPECIFIED"
  ],
  "operation_id": "89ta-98sk123451"
}
```

---

## Статус интервала поставки

`POST /v1/supply-order/timeslot/status`

Operation ID: `SupplyOrderAPI_GetSupplyOrderTimeslotStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/timeslot/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "operation_id": "89ta-98sk123451"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Статус данных

#### Схема ответа (200)

- `errors` — Array of strings Items Enum: "UPDATE_TIMESLOT_ERROR_UNSPECIFIED" "UPDATE_TIMESLOT_ERROR_INVALID_ORDER_STATE" "UPDATE_TIMESLOT_ERROR_INCOMPATIBLE_ORDER_FLOW" "UPDATE_TIMESLOT_ERROR_SET_TIMESLOT_DEADLINE_EXCEED" "UPDATE_TIMESLOT_ERROR_OUT_OF_ALLOWED_RANGE" "UPDATE_TIMESLOT_ERROR_ORDER_NOT_BELONG_CONTRACTOR" "UPDATE_TIMESLOT_ERROR_ORDER_NOT_BELONG_COMPANY" "UPDATE_TIMESLOT_ERROR_PICKUP_ORDER_LIMIT_EXCEEDED" Возможные ошибки: UNSPECIFIED — статус не указан; INVALID_ORDER_STATE — неверный статус заказа; INCOMPATIBLE_ORDER_FLOW — неверный статус интервала поставки; SET_TIMESLOT_DEADLINE_EXCEED — заявка на поставку просрочена; OUT_OF_ALLOWED_RANGE — вы ввели некорректное значение интервала поставки; ORDER_NOT_BELONG_CONTRACTOR — заявка создана другим юридическом лицом, работать с ней не получится; ORDER_NOT_BELONG_COMPANY — заявка не принадлежит вашему кабинету, работать с ней не получится; UPDATE_TIMESLOT_ERROR_PICKUP_ORDER_LIMIT_EXCEEDED — превышен суточный лимит на создание заявок на поставку курьером.
- `status` — string Default: "STATUS_UNSPECIFIED" Enum: "STATUS_UNSPECIFIED" "STATUS_ERROR" "STATUS_IN_PROGRESS" "STATUS_SUCCESS" Статус данных: UNSPECIFIED — не указан; ERROR — ошибка; IN_PROGRESS — устанавливается; SUCCESS — установлен.

Пример ответа:
```json
{
  "errors": [
    "UPDATE_TIMESLOT_ERROR_UNSPECIFIED"
  ],
  "status": "STATUS_UNSPECIFIED"
}
```

---

## Указать данные о водителе и автомобиле

`POST /v1/supply-order/pass/create`

Operation ID: `SupplyOrderAPI_SupplyOrderPassCreate`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/pass/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "supply_order_id": 1001,
  "vehicle": {
    "driver_name": "Иван Петров",
    "driver_phone": "+7 (999) 123-45-67",
    "vehicle_model": "ГАЗель NEXT",
    "vehicle_number": "А 123 ВВ 799"
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_order_id required` — integer <int64> Идентификатор заявки на поставку.
- `vehicle required` — object Информация о водителе и автомобиле.

### Ответы

- 200 Данные указаны

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "SET_VEHICLE_ERROR_UNSPECIFIED" "SET_VEHICLE_ERROR_INVALID_ORDER_STATE" "SET_VEHICLE_ERROR_VEHICLE_NOT_REQUIRED" "SET_VEHICLE_ERROR_ORDER_NOT_BELONG_CONTRACTOR" "SET_VEHICLE_ERROR_ORDER_NOT_BELONG_COMPANY" Причина ошибки: UNSPECIFIED — статус заявки не указан; INVALID_ORDER_STATE — неверный статус заявки; VEHICLE_NOT_REQUIRED — указывать данные автомобиля необязательно; ORDER_NOT_BELONG_CONTRACTOR — заявка создана другим юридическом лицом, работать с ней не получится; ORDER_NOT_BELONG_COMPANY — заявка не принадлежит вашему кабинету, работать с ней не получится.
- `operation_id` — string Идентификатор операции.

Пример ответа:
```json
{
  "error_reasons": [
    "SET_VEHICLE_ERROR_UNSPECIFIED"
  ],
  "operation_id": "550e8400-e29b-41d4-a716-446655440000"
}
```

---

## Статус ввода данных о водителе и автомобиле

`POST /v1/supply-order/pass/status`

Operation ID: `SupplyOrderAPI_SupplyOrderPassStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/pass/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "operation_id": "550e8400-e29b-41d4-a716-446655440000"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Статус

#### Схема ответа (200)

- `errors` — Array of strings Items Enum: "SET_VEHICLE_ERROR_UNSPECIFIED" "SET_VEHICLE_ERROR_INVALID_ORDER_STATE" "SET_VEHICLE_ERROR_VEHICLE_NOT_REQUIRED" "SET_VEHICLE_ERROR_ORDER_NOT_BELONG_CONTRACTOR" "SET_VEHICLE_ERROR_ORDER_NOT_BELONG_COMPANY" Причина ошибки: UNSPECIFIED — статус не указан; INVALID_ORDER_STATE — неверный статус заявки; VEHICLE_NOT_REQUIRED — указывать данные автомобиля необязательно; ORDER_NOT_BELONG_CONTRACTOR — заявка создана другим юридическом лицом, работать с ней не получится; ORDER_NOT_BELONG_COMPANY — заявка не принадлежит вашему кабинету, работать с ней не получится.
- `result` — string Default: "Unknown" Enum: "Unknown" "Success" "InProgress" "Failed" Статус ввода данных о водителе и автомобиле: Unknown — статус неизвестен; Success — данные указаны; InProgress — данные обрабатываются; Failed — не удалось обработать данные.

Пример ответа:
```json
{
  "errors": [
    "SET_VEHICLE_ERROR_UNSPECIFIED"
  ],
  "result": "Unknown"
}
```

---

## Получить подробную информацию о заявке на поставку

`POST /v1/supply-order/details`

Operation ID: `SupplyOrderAPI_SupplyOrderDetails`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/details" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `order_id required` — integer <int64> Идентификатор заявки на поставку.

### Ответы

- 200 Подробная информация о заявке
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `created_date` — string <date-time> Дата создания заявки на поставку.
- `data_filling_deadline_utc` — string <date-time> Время в секундах, оставшееся на заполнение данных по поставке. Только для заявок с вРЦ.
- `dropoff_warehouse_id` — integer <int64> Идентификатор пункта отгрузки.
- `order_id` — integer <int64> Идентификатор заявки на поставку.
- `order_number` — string Номер заявки на поставку.
- `order_tags` — object Метки заявки на поставку.
- `state` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "DATA_FILLING" "READY_TO_SUPPLY" "ACCEPTED_AT_SUPPLY_WAREHOUSE" "IN_TRANSIT" "ACCEPTANCE_AT_STORAGE_WAREHOUSE" "REPORTS_CONFIRMATION_AWAITING" "REPORT_REJECTED" "COMPLETED" "REJECTED_AT_SUPPLY_WAREHOUSE" "CANCELLED" "OVERDUE" Статус заявки на поставку: UNSPECIFIED — не определён; DATA_FILLING — заполнение данных; READY_TO_SUPPLY — готова к отгрузке; ACCEPTED_AT_SUPPLY_WAREHOUSE — принята на точке отгрузки; IN_TRANSIT — в пути; ACCEPTANCE_AT_STORAGE_WAREHOUSE — приёмка на складе; REPORTS_CONFIRMATION_AWAITING — согласование актов; REPORT_REJECTED — спор; COMPLETED — завершена; REJECTED_AT_SUPPLY_WAREHOUSE — отказано в приемке; CANCELLED — отменена; OVERDUE — просрочена.
- `state_updated_date` — string <date-time> Дата обновления статуса заявки на поставку.
- `supplies` — Array of objects Информация о поставках.
- `timeslot` — object Информация о таймслоте.
- `vehicle` — object Информация о водителе и машине.

Пример ответа:
```json
{
  "created_date": "2019-08-24T14:15:22Z",
  "data_filling_deadline_utc": "2019-08-24T14:15:22Z",
  "dropoff_warehouse_id": 0,
  "order_id": 0,
  "order_number": "string",
  "order_tags": {
    "is_econom": true,
    "is_super_fbo": true,
    "is_virtual": true,
    "original_supply_id": 0,
    "product_super_fbo": true
  },
  "state": "UNSPECIFIED",
  "state_updated_date": "2019-08-24T14:15:22Z",
  "supplies": [
    {
      "cancellation_allowability": {
        "can_not_set_reasons": [
          "UNSPECIFIED"
        ],
        "can_set": true
      },
      "content": {
        "bundle_id": "string",
        "can_not_set_reasons": [
          "UNSPECIFIED"
        ],
        "can_set": true
      },
      "ettn_info": {
        "contains_valid": true,
        "is_required": true,
        "is_uploaded": true
      },
      "is_crossdock": true,
      "overdue_reason": "UNSPECIFIED",
      "storage_warehouse": {
        "address": "string",
        "arrival_date": "2019-08-24T14:15:22Z",
        "name": "string",
        "warehouse_id": 0
      },
      "macrolocal_cluster_id": 0,
      "supply_id": 0,
      "supply_state": "UNSPECIFIED",
      "supply_tags": {
        "is_ettn_required": true,
        "is_evsd_required": true,
        "is_jewelry": true,
        "is_marking_possible": true,
        "is_marking_required": true,
        "is_utd": true
      }
    }
  ],
  "timeslot": {
    "can_not_set_reasons": [
      "UNSPECIFIED"
    ],
    "can_set": true,
    "value": {
      "timeslot": {
        "from": "2019-08-24T14:15:22Z",
        "to": "2019-08-24T14:15:22Z"
      },
      "timezone_info": {
        "iana_name": "string",
        "offset": "string"
      }
    }
  },
  "vehicle": {
    "can_not_set_reasons": [
      "UNSPECIFIED"
    ],
    "can_set": true,
    "value": {
      "driver_is_deleted": true,
      "driver_name": "string",
      "driver_phone": "string",
      "vehicle_is_deleted": true,
      "vehicle_model": "string",
      "vehicle_number": "string"
    }
  }
}
```

---

## Загруженность складов Ozon

`GET /v1/supplier/available_warehouses`

Operation ID: `SupplierAPI_SupplierAvailableWarehouses`

Метод возвращает список активных складов Ozon с информацией об их средней загруженности на ближайшее время.

```bash
curl -X GET "https://api-seller.ozon.ru/v1/supplier/available_warehouses" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Информация о загруженности складов

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `schedule` — object Загруженность.
  - `warehouse` — object Склад.

Пример ответа:
```json
{
  "result": [
    {
      "schedule": {
        "capacity": [
          {
            "start": "2019-08-24T14:15:22Z",
            "end": "2019-08-24T14:15:22Z",
            "value": 0
          }
        ],
        "date": "2019-08-24T14:15:22Z"
      },
      "warehouse": {
        "id": "2000030394123",
        "name": "ХОРУГВИНО-РФЦ"
      }
    }
  ]
}
```

---
