# Работа с цифровыми товарами

Работа с цифровыми товарами Подробнее о цифровых товарах в Базе знаний

_Тег: `Digital` · операций: 3_

## Загрузить коды цифровых товаров для отправления

`POST /v1/posting/digital/codes/upload`

Operation ID: `UploadPostingCodes`

Метод доступен только продавцам, работающим с цифровыми товарами. Вы можете загрузить коды цифровых товаров в течение 24 часов с момента получения заказа. Передайте все коды цифровых товаров к каждому товару в заказе за один запрос. Если передадите не все коды, запрос вернётся с ошибкой.

### Тело запроса (application/json)

- `exemplars_by_sku` — Array of objects Данные о кодах цифрового товара по SKU.
- `posting_number` — string Номер отправления.

Пример запроса:

```json
{
  "exemplars_by_sku": [
    {
      "exemplar_qty": 3,
      "not_available_exemplar_qty": 0,
      "sku": "6605735423",
      "exemplar_keys": [
        "4235234234",
        "431234234",
        "912382368"
      ]
    }
  ],
  "posting_number": "33920151-0719-1"
}
```

### Ответы

- 200 Коды цифровых товаров загружены

#### Схема ответа (200)

- `exemplars_by_sku` — Array of objects Данные о кодах цифрового товара по SKU.
  - `failed_exemplars` — Array of objects Список кодов цифровых товаров с ошибками.
  - `received_qty` — integer <int32> Количество кодов цифрового товара, которые были приняты.
  - `rejected_qty` — integer <int32> Количество кодов цифровых товаров, которые не были приняты или переданы.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример ответа:

```json
{
  "exemplars_by_sku": [
    {
      "sku": 123456,
      "received_qty": 3,
      "rejected_qty": 1,
      "failed_exemplars": [
        {
          "key": "23434252345",
          "message": "Некорректный формат кода"
        }
      ]
    }
  ]
}
```

---

## Получить список отправлений Deprecated

`POST /v1/posting/digital/list`

Operation ID: `ListPostingCodes`

Метод устаревает. Переключитесь на /v2/posting/digital/list . Возвращает список отправлений, по которым нужно загрузить коды цифровых товаров. Метод доступен только продавцам, работающим с цифровыми товарами. Чтобы получить список отправлений в любом статусе, воспользуйтесь методом /v2/posting/fbo/list .

### Тело запроса (application/json)

- `dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию, DESC — по убыванию.
- `filter` — object Фильтр для поиска отправлений.
- `limit` — integer <int64> Количество значений в ответе: максимум — 1000, минимум — 1.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента. Максимальное значение — 20000.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

Пример запроса:

```json
{
  "dir": "ASC",
  "filter": {
    "since": "2021-09-01T00:00:00.000Z",
    "posting_number": [
      "string"
    ],
    "to": "2021-11-17T10:44:12.828Z"
  },
  "limit": 5,
  "offset": 0,
  "with": {
    "analytics_data": true,
    "financial_data": true,
    "legal_info": false
  }
}
```

### Ответы

- 200 Список отправлений

#### Схема ответа (200)

- `result` — Array of objects Список отправлений.
  - `additional_data` — Array of objects Дополнительные параметры.
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
  - `status` — string Статус отправления: awaiting_packaging — ожидает упаковки.
  - `waiting_deadline_for_digital_code` — string <date-time> Время, до которого нужно передать коды цифровых товаров. Передайте коды цифровых товаров с помощью метода /v1/posting/digital/codes/upload .

Пример ответа:

```json
{
  "result": [
    {
      "order_id": 354680487,
      "order_number": "16965409-0014",
      "posting_number": "16965409-0014-1",
      "status": "awaiting_packaging",
      "cancel_reason_id": 0,
      "created_at": "2021-09-01T00:23:45.607000Z",
      "in_process_at": "2021-09-01T00:25:30.120000Z",
      "waiting_deadline_for_digital_code": "2025-06-16T10:12:00.664Z",
      "legal_info": {
        "company_name": "string",
        "inn": "string",
        "kpp": "string"
      },
      "products": [
        {
          "sku": 160249683,
          "name": "Так говорил Омар Хайям. Жизнеописание. Афоризмы и рубайят. Классика в словах и картинках",
          "offer_id": "978-5-906864-56-7",
          "price": "81.00",
          "required_qty_for_digital_code": 3,
          "currency_code": "RUB"
        }
      ],
      "analytics_data": {
        "city": "",
        "delivery_type": "PVZ",
        "is_premium": false,
        "payment_type_group_name": "Карты оплаты",
        "warehouse_id": 17717042026000,
        "warehouse_name": "РОСТОВ-НА-ДОНУ_РФЦ",
        "is_legal": false
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
            ]
          }
        ]
      },
      "additional_data": []
    }
  ]
}
```

---

## Обновить количество цифровых товаров

`POST /v1/product/digital/stocks/import`

Operation ID: `DigitalProductAPI_StocksImport`

Метод доступен только продавцам, работающим с цифровыми товарами. Используйте метод, чтобы изменить информацию о количестве товара в наличии.

### Тело запроса (application/json)

- `stocks` — Array of objects Данные об остатках.

Пример запроса:

```json
{
  "stocks": [
    {
      "stock": "2",
      "offer_id": "Мяч желтый 543561"
    }
  ]
}
```

### Ответы

- 200 Количество товаров обновлено

#### Схема ответа (200)

- `status` — Array of objects Информация о товарах.
  - `errors` — Array of objects Ошибки, которые возникли при обработке запроса.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `updated` — boolean true , если запрос выполнен успешно и остатки обновлены.

Пример ответа:

```json
{
  "status": [
    {
      "errors": [],
      "offer_id": "Мяч желтый 543561",
      "updated": true,
      "product_id": "29235761",
      "sku": "425291243"
    }
  ]
}
```

---
