# Обработка заказов FBS и rFBS

_Тег: `FBS` · операций: 23_

## Получить список необработанных отправлений

`POST /v4/posting/fbs/unfulfilled/list`

Operation ID: `PostingFbsUnfulfilledList`

Возвращает список необработанных отправлений за указанный период времени — он должен быть не больше одного года. Возможные статусы отправлений: awaiting_registration — ожидает регистрации; acceptance_in_progress — идёт приёмка; awaiting_approve — ожидает подтверждения; awaiting_packaging — ожидает упаковки; awaiting_deliver — ожидает отгрузки; arbitration — арбитраж; client_arbitration — клиентский арбитраж доставки; delivering — доставляется; driver_pickup — у водителя; cancelled — отменено; not_accepted — не принято на сортировочном центре. Чтобы получать актуальную дату отгрузки, регулярно обновляйте информацию об отправлениях или подключите пуш-уведомления .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр запроса. Используйте фильтр по времени сборки — cutoff или по дате передачи отправления в доставку — delivering_date . Если использовать их вместе, в ответе вернётся ошибка. Чтобы использовать фильтр по времени сборки, заполните поля cutoff_from и cutoff_to . Чтобы использовать фильтр по дате передачи отправления в доставку, заполните поля delivering_date_from и delivering_date_to .
  - `cutoff_from` — string <date-time> YYYY-MM-DDThh:mm:ss.mcsZ Время, до которого продавцу нужно собрать заказ. Начало периода.
  - `cutoff_to` — string <date-time> YYYY-MM-DDThh:mm:ss.mcsZ Время, до которого продавцу нужно собрать заказ. Конец периода.
  - `delivering_date_from` — string <date-time> Минимальная дата передачи отправления в доставку.
  - `delivering_date_to` — string <date-time> Максимальная дата передачи отправления в доставку.
  - `delivery_method_ids` — Array of strings <int64> <= 1000 items Идентификатор способа доставки. Можно получить с помощью метода /v1/delivery-method/list .
  - `last_changed_status_date` — object Период, в который последний раз изменялся статус отправления.
  - `provider_ids` — Array of strings <int64> <= 1000 items Идентификатор службы доставки. Можно получить с помощью метода /v1/delivery-method/list .
  - `statuses` — Array of strings Статус отправления: acceptance_in_progress — идёт приёмка; awaiting_approve — ожидает подтверждения; awaiting_packaging — ожидает упаковки; awaiting_registration — ожидает регистрации; awaiting_deliver — ожидает отгрузки; arbitration — арбитраж; client_arbitration — клиентский арбитраж доставки; delivering — доставляется; driver_pickup — у водителя; not_accepted — не принято на сортировочном центре.
  - `warehouse_ids` — Array of strings <int64> <= 1000 items Идентификатор склада. Можно получить с помощью метода /v1/warehouse/list .
- `limit` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.
- `translit` — boolean true , чтобы включить транслитерацию адреса из кириллицы в латиницу.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.
  - `analytics_data` — boolean true , чтобы добавить в ответ данные аналитики.
  - `barcodes` — boolean true , чтобы добавить в ответ штрихкоды отправления.
  - `financial_data` — boolean true , чтобы добавить в ответ финансовые данные.
  - `legal_info` — boolean true , чтобы добавить в ответ юридическую информацию.

Пример запроса:

```json
{
  "sort_dir": "asc",
  "limit": 100,
  "filter": {
    "cutoff_from": "2026-01-21T07:13:56.042Z",
    "cutoff_to": "2026-01-21T07:13:56.042Z",
    "delivery_method_ids": [
      123456
    ],
    "statuses": [
      "awaiting_packaging",
      "awaiting_deliver",
      "delivering"
    ],
    "warehouse_ids": [
      3092889984
    ],
    "provider_ids": [
      57
    ],
    "last_change_status_date": {
      "from": "2026-01-21T07:13:56.042Z",
      "to": "2026-01-21T07:13:56.042Z"
    }
  },
  "cursor": "",
  "with": {
    "barcodes": true,
    "analytics_data": true,
    "financial_data": true,
    "legal_info": true
  }
}
```

### Ответы

- 200 Список необработанных отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `count` — integer <int64> Количество отправлений в ответе.
- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернулись не все отправления.
- `postings` — Array of objects Список отправлений.

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

## Список необработанных отправлений Deprecated

`POST /v3/posting/fbs/unfulfilled/list`

Operation ID: `PostingAPI_GetFbsPostingUnfulfilledList`

С 1 июня 2026 года метод будет отключён. Переключитесь на /v4/posting/fbs/unfulfilled/list . Возвращает список необработанных отправлений за указанный период времени — он должен быть не больше одного года. Возможные статусы отправлений: awaiting_registration — ожидает регистрации, acceptance_in_progress — идёт приёмка, awaiting_approve — ожидает подтверждения, awaiting_packaging — ожидает упаковки, awaiting_deliver — ожидает отгрузки, arbitration — арбитраж, client_arbitration — клиентский арбитраж доставки, delivering — доставляется, driver_pickup — у водителя, cancelled — отменено, not_accepted — не принят на сортировочном центре. Чтобы получать актуальную дату отгрузки, регулярно обновляйте информацию об отправлениях или подключите пуш-уведомления .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `dir` — string Направление сортировки: asc — по возрастанию, desc — по убыванию.
- `filter required` — object Фильтр запроса. Используйте фильтр либо по времени сборки — cutoff , либо по дате передачи отправления в доставку — delivering_date . Если использовать их вместе, в ответе вернётся ошибка. Чтобы использовать фильтр по времени сборки, заполните поля cutoff_from и cutoff_to . Чтобы использовать фильтр по дате передачи отправления в доставку, заполните поля delivering_date_from и delivering_date_to .
- `limit required` — integer <int64> Количество значений в ответе: максимум — 1000, минимум — 1.
- `offset required` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

Пример запроса:

```json
{
  "dir": "ASC",
  "filter": {
    "cutoff_from": "2021-08-24T14:15:22Z",
    "cutoff_to": "2021-08-31T14:15:22Z",
    "delivery_method_id": [],
    "is_quantum": false,
    "provider_id": [],
    "status": "awaiting_packaging",
    "warehouse_id": []
  },
  "limit": 100,
  "offset": 0,
  "with": {
    "analytics_data": true,
    "barcodes": true,
    "financial_data": true,
    "legal_info": false,
    "translit": true
  }
}
```

### Ответы

- 200 Список необработанных отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `count` — integer <int64> Счётчик элементов в ответе.
  - `postings` — Array of objects Список отправлений и подробная информация по каждому.

Пример ответа:

```json
{
  "has_next": true,
  "cursor": "eyJmaWVsZHMiOlt7Im5hbWUiOiJvc3AuY3V0b2ZmIiwiZGlyIjoiYXNjIiwidmFsIjoiMjAyNi0wMy0zMVQwOTowMDowMFoifSx7Im5hbWUiOiJvc3AuaW5fcHJvY2Vzc19hdCIsImRpciI6ImFzYyIsInZhbCI6IjIwMjYtMDMtMzBUMTE6MjY6MDhaIn0seyJuYW1lIjoib3NwLmlkIiwiZGlyIjoiYXNjIiwidmFsIjoiMTIwOTM3NTA2MiJ9LHsibmFtZSI6Im9zcC5pZCIsImRpciI6ImFzYyIsInZhbCI6IjEyMDkzNzUwNjIifV19",
  "postings": [
    {
      "posting_number": "0132112277-0102-1",
      "order_id": 35566808085,
      "order_number": "0132112277-0102",
      "pickup_code_verified_at": null,
      "status": "awaiting_packaging",
      "substatus": "posting_created",
      "delivery_method": {
        "id": 1020005003107121,
        "name": "Доставка Ozon самостоятельно, Ногинск",
        "warehouse_id": 1020005003107121,
        "warehouse": "FBS LikeSK",
        "tpl_provider_id": 24,
        "tpl_provider": "Доставка Ozon"
      },
      "delivery_schema": "fbs",
      "tracking_number": "",
      "tpl_integration_type": "ozon",
      "in_process_at": "2026-03-30T11:26:08Z",
      "shipment_date": "2026-03-31T09:00:00Z",
      "shipment_date_without_delay": "2026-04-04T16:59:00Z",
      "optional": {
        "products_with_possible_mandatory_mark": []
      },
      "cancellation": {
        "cancel_reason_id": 0,
        "cancel_reason": "",
        "cancellation_type": "",
        "cancelled_after_ship": false,
        "affect_cancellation_rating": false,
        "cancellation_initiator": ""
      },
      "container": {
        "cargo_type": "BOX",
        "container_date": "string",
        "container_id": 0,
        "container_number": 0
      },
      "container_sort_type": "string",
      "customer": null,
      "products": [
        {
          "is_blr_traceable": false,
          "is_marketplace_buyout": false,
          "offer_id": "SK-58 (малиновый)",
          "name": "Профессиональная плойка Cronier cr-2013 для афро кудрей 9 мм",
          "sku": 827098843,
          "quantity": 1,
          "imei": [],
          "weight": 0.55,
          "product_color": "малиновый",
          "price": {
            "amount": "1530",
            "currency": "RUB"
          }
        }
      ],
      "addressee": null,
      "barcodes": null,
      "analytics_data": null,
      "destination_place_id": 0,
      "destination_place_name": "",
      "financial_data": null,
      "is_express": false,
      "legal_info": null,
      "quantum_id": 0,
      "require_blr_traceable_attrs": false,
      "requirements": {
        "products_requiring_gtd": [],
        "products_requiring_country": [],
        "products_requiring_mandatory_mark": [],
        "products_requiring_rnpt": [],
        "products_requiring_jw_uin": [],
        "products_requiring_change_country": [],
        "products_requiring_imei": [],
        "products_requiring_weight": []
      },
      "tariffication": {
        "current_tariff_rate": 0,
        "current_tariff_type": "no_discount",
        "current_tariff_charge": {
          "amount": "0",
          "currency": "RUB"
        },
        "current_tariff_min_charge": null,
        "next_tariff_rate": 1,
        "next_tariff_type": "commission",
        "next_tariff_charge": {
          "amount": "50",
          "currency": "RUB"
        },
        "next_tariff_min_charge": {
          "amount": "50",
          "currency": "RUB"
        },
        "next_tariff_starts_at": "2026-03-31T09:00:01Z"
      },
      "external_order": {
        "is_external": false,
        "platform_name": ""
      },
      "volume_weight": 0,
      "is_click_and_collect": false,
      "delivering_date": null,
      "is_multibox": false,
      "multi_box_qty": 1,
      "is_presortable": false,
      "prr_option": "",
      "parent_posting_number": "",
      "available_actions": [
        "cancel",
        "has_barcode_for_printing",
        "product_cancel",
        "ship",
        "ship_async"
      ],
      "tariffication_steps": [
        {
          "min_charge": null,
          "tariff_charge": {
            "amount": "0",
            "currency": "RUB"
          },
          "tariff_deadline_at": "2026-03-31T09:00:00Z",
          "tariff_rate": 0,
          "tariff_type": "no_discount"
        },
        {
          "min_charge": {
            "amount": "50",
            "currency": "RUB"
          },
          "tariff_charge": {
            "amount": "50",
            "currency": "RUB"
          },
          "tariff_deadline_at": "2026-03-31T21:00:00Z",
          "tariff_rate": 1,
          "tariff_type": "commission"
        },
        {
          "min_charge": {
            "amount": "50",
            "currency": "RUB"
          },
          "tariff_charge": {
            "amount": "50",
            "currency": "RUB"
          },
          "tariff_deadline_at": "2026-04-01T09:00:00Z",
          "tariff_rate": 1,
          "tariff_type": "commission"
        },
        {
          "min_charge": {
            "amount": "100",
            "currency": "RUB"
          },
          "tariff_charge": {
            "amount": "100",
            "currency": "RUB"
          },
          "tariff_deadline_at": "2026-04-01T21:00:00Z",
          "tariff_rate": 2,
          "tariff_type": "commission"
        },
        {
          "min_charge": {
            "amount": "100",
            "currency": "RUB"
          },
          "tariff_charge": {
            "amount": "100",
            "currency": "RUB"
          },
          "tariff_deadline_at": "2026-04-02T09:00:00Z",
          "tariff_rate": 2,
          "tariff_type": "commission"
        },
        {
          "min_charge": {
            "amount": "150",
            "currency": "RUB"
          },
          "tariff_charge": {
            "amount": "150",
            "currency": "RUB"
          },
          "tariff_deadline_at": "9999-12-31T23:59:59.999999900Z",
          "tariff_rate": 3,
          "tariff_type": "commission"
        }
      ]
    }
  ],
  "count": 38
}
```

---

## Получить список отправлений

`POST /v4/posting/fbs/list`

Operation ID: `PostingFbsList`

Возвращает список отправлений за указанный период времени — он должен быть не больше одного года. Чтобы получать актуальную дату отгрузки, регулярно обновляйте информацию об отправлениях или подключите пуш-уведомления .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр.
  - `delivery_method_ids` — Array of strings <int64> <= 1000 items Идентификатор способа доставки. Можно получить с помощью метода /v1/delivery-method/list .
  - `is_blr_traceable` — boolean true , если товар отслеживаемый.
  - `last_changed_status_date` — object Период, в который последний раз изменялся статус у отправлений.
  - `order_id` — integer <int64> Идентификатор заказа.
  - `order_numbers` — Array of strings <= 100 items Номера заказов, к которым относятся отправления.
  - `provider_ids` — Array of strings <int64> <= 1000 items Идентификатор службы доставки. Можно получить с помощью метода /v1/delivery-method/list .
  - `since required` — string <date-time> YYYY-MM-DDThh:mm:ssZ Дата начала периода, за который нужно получить список отправлений.
  - `statuses` — Array of strings Статус отправления: awaiting_registration — ожидает регистрации; acceptance_in_progress — идёт приёмка; awaiting_approve — ожидает подтверждения; awaiting_packaging — ожидает упаковки; awaiting_deliver — ожидает отгрузки; arbitration — арбитраж; client_arbitration — клиентский арбитраж доставки; delivering — доставляется; driver_pickup — у водителя; delivered — доставлено; cancelled — отменено; not_accepted — не принято на сортировочном центре; sent_by_seller – отправлено продавцом.
  - `to required` — string <date-time> YYYY-MM-DDThh:mm:ssZ Дата конца периода, за который нужно получить список отправлений.
  - `warehouse_ids` — Array of strings <int64> <= 1000 items Идентификатор склада. Можно получить с помощью метода /v1/warehouse/list .
- `limit required` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.
- `translit` — boolean true , чтобы включить транслитерацию адреса из кириллицы в латиницу.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.
  - `analytics_data` — boolean true , чтобы добавить в ответ данные аналитики.
  - `barcodes` — boolean true , чтобы добавить в ответ штрихкоды отправления.
  - `financial_data` — boolean true , чтобы добавить в ответ финансовые данные.
  - `legal_info` — boolean true , чтобы добавить в ответ юридическую информацию.

Пример запроса:

```json
{
  "sort_dir": "asc",
  "filter": {
    "order_numbers": [
      "12345-1"
    ],
    "delivery_method_ids": [
      123456789
    ],
    "is_blr_traceable": false,
    "last_changed_status_date": {
      "from": "2026-04-01T19:47:39.878Z",
      "to": "2026-04-02T11:47:39.878Z"
    },
    "order_id": 123456,
    "since": "2026-04-01T19:47:39.878Z",
    "to": "2026-04-02T11:47:39.878Z",
    "statuses": [
      "awaiting_packaging",
      "awaiting_deliver",
      "delivering",
      "delivered"
    ],
    "provider_ids": [
      123
    ],
    "warehouse_ids": [
      3092889984
    ]
  },
  "limit": 100,
  "cursor": "",
  "with": {
    "analytics_data": true,
    "barcodes": true,
    "financial_data": true,
    "legal_info": true,
    "translit": true
  }
}
```

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

## Список отправлений Deprecated

`POST /v3/posting/fbs/list`

Operation ID: `PostingAPI_GetFbsPostingListV3`

С 1 июня 2026 года метод будет отключён. Переключитесь на /v4/posting/fbs/list . Возвращает список отправлений за указанный период времени — он должен быть не больше одного года. has_next = true в ответе может значить, что вернули не весь массив отправлений. Чтобы получить информацию об остальных отправлениях, сделайте новый запрос с другим значением offset . Чтобы получать актуальную дату отгрузки, регулярно обновляйте информацию об отправлениях или подключите пуш-уведомления .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `dir` — string Направление сортировки: asc — по возрастанию, desc — по убыванию.
- `filter required` — object Фильтр.
- `limit required` — integer <int64> Количество значений в ответе: максимум — 1000, минимум — 1.
- `offset required` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

Пример запроса:

```json
{
  "dir": "ASC",
  "filter": {
    "delivery_method_id": [
      "21321684811000"
    ],
    "is_blr_traceable": true,
    "is_quantum": false,
    "last_changed_status_date": {
      "from": "2023-11-03T11:47:39.878Z",
      "to": "2023-11-03T11:47:39.878Z"
    },
    "order_id": 0,
    "provider_id": [
      "24"
    ],
    "since": "2023-11-03T11:47:39.878Z",
    "status": "awaiting_packaging",
    "to": "2023-11-03T11:47:39.878Z",
    "warehouse_id": [
      "21321684811000"
    ]
  },
  "limit": 100,
  "offset": 0,
  "with": {
    "analytics_data": true,
    "barcodes": true,
    "financial_data": true,
    "translit": true
  }
}
```

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Массив отправлений.
  - `has_next` — boolean Признак, что в ответе вернули не весь массив отправлений: true — необходимо сделать новый запрос с другим значением offset , чтобы получить информацию об остальных отправлениях; false — в ответе вернули весь массив отправлений для фильтра, который был задан в запросе.
  - `postings` — Array of objects Информация об отправлении.

Пример ответа:

```json
{
  "result": {
    "postings": [
      {
        "posting_number": "0132112277-0101-1",
        "order_id": 35566798085,
        "order_number": "0132112277-0101",
        "status": "awaiting_packaging",
        "delivery_method": {
          "id": 1020005003107121,
          "name": "Доставка Ozon самостоятельно, Ногинск",
          "warehouse_id": 1020005003107121,
          "warehouse": "FBS LikeSK",
          "tpl_provider_id": 24,
          "tpl_provider": "Доставка Ozon"
        },
        "tracking_number": "",
        "tpl_integration_type": "ozon",
        "in_process_at": "2026-03-30T11:26:00Z",
        "shipment_date": "2026-03-31T09:00:00Z",
        "delivering_date": null,
        "cancellation": {
          "cancel_reason_id": 0,
          "cancel_reason": "",
          "cancellation_type": "",
          "cancelled_after_ship": false,
          "affect_cancellation_rating": false,
          "cancellation_initiator": ""
        },
        "container": {
          "cargo_type": "BOX",
          "container_date": "string",
          "container_id": 0,
          "container_number": 0
        },
        "container_sort_type": "string",
        "customer": null,
        "products": [
          {
            "price": "1530.0000",
            "offer_id": "SK-58 (малиновый)",
            "name": "Профессиональная плойка Cronier cr-2013 для афро кудрей 9 мм",
            "sku": 827098843,
            "quantity": 1,
            "currency_code": "RUB",
            "is_blr_traceable": false,
            "is_marketplace_buyout": false,
            "imei": []
          }
        ],
        "addressee": null,
        "barcodes": {
          "upper_barcode": "711008270053000",
          "lower_barcode": "711008270053000"
        },
        "analytics_data": {
          "region": "",
          "city": "",
          "delivery_type": "PVZ",
          "is_premium": false,
          "payment_type_group_name": "Ozon Банк",
          "warehouse_id": 1020005003107121,
          "warehouse": "FBS LikeSK",
          "tpl_provider_id": 24,
          "tpl_provider": "Доставка Ozon",
          "delivery_date_begin": "2026-04-11T15:00:00Z",
          "delivery_date_end": "2026-04-11T16:00:00Z",
          "is_legal": false,
          "client_delivery_date_begin": null,
          "client_delivery_date_end": null
        },
        "financial_data": {
          "products": [
            {
              "commission_amount": 0,
              "commission_percent": 0,
              "payout": 0,
              "product_id": 827098843,
              "old_price": 1530,
              "price": 1530,
              "total_discount_value": 0,
              "total_discount_percent": 0,
              "actions": [
                "Округление"
              ],
              "quantity": 1,
              "currency_code": "RUB",
              "customer_currency_code": "RUB",
              "customer_price": 662.86
            }
          ],
          "cluster_from": "Moskva, MO i Dal`nie regiony`",
          "cluster_to": "Rostov"
        },
        "is_express": false,
        "requirements": {
          "products_requiring_gtd": [],
          "products_requiring_country": [],
          "products_requiring_mandatory_mark": [],
          "products_requiring_rnpt": [],
          "products_requiring_jw_uin": [],
          "products_requiring_change_country": [],
          "products_requiring_imei": [],
          "products_requiring_weight": []
        },
        "parent_posting_number": "",
        "available_actions": [
          "cancel",
          "has_barcode_for_printing",
          "product_cancel",
          "ship",
          "ship_async"
        ],
        "multi_box_qty": 1,
        "is_multibox": false,
        "substatus": "posting_created",
        "prr_option": "",
        "quantum_id": 0,
        "tariffication": {
          "current_tariff_rate": 0,
          "current_tariff_type": "no_discount",
          "current_tariff_charge": "",
          "current_tariff_charge_currency_code": "RUB",
          "next_tariff_rate": 0,
          "next_tariff_type": "commission",
          "next_tariff_charge": "50",
          "next_tariff_starts_at": "2026-03-31T09:00:01Z",
          "next_tariff_charge_currency_code": "RUB"
        },
        "destination_place_id": 0,
        "destination_place_name": "",
        "is_presortable": false,
        "pickup_code_verified_at": null,
        "optional": {
          "products_with_possible_mandatory_mark": []
        },
        "legal_info": {
          "company_name": "",
          "inn": "",
          "kpp": ""
        },
        "shipment_date_without_delay": "2026-04-04T16:59:00Z",
        "require_blr_traceable_attrs": false,
        "is_click_and_collect": false,
        "tariffication_steps": [
          {
            "min_charge": null,
            "tariff_charge": {
              "amount": "0",
              "currency": "RUB"
            },
            "tariff_deadline_at": "2026-03-31T09:00:00Z",
            "tariff_rate": 0,
            "tariff_type": "no_discount"
          },
          {
            "min_charge": {
              "amount": "50",
              "currency": "RUB"
            },
            "tariff_charge": {
              "amount": "50",
              "currency": "RUB"
            },
            "tariff_deadline_at": "2026-03-31T21:00:00Z",
            "tariff_rate": 1,
            "tariff_type": "commission"
          },
          {
            "min_charge": {
              "amount": "50",
              "currency": "RUB"
            },
            "tariff_charge": {
              "amount": "50",
              "currency": "RUB"
            },
            "tariff_deadline_at": "2026-04-01T09:00:00Z",
            "tariff_rate": 1,
            "tariff_type": "commission"
          },
          {
            "min_charge": {
              "amount": "100",
              "currency": "RUB"
            },
            "tariff_charge": {
              "amount": "100",
              "currency": "RUB"
            },
            "tariff_deadline_at": "2026-04-01T21:00:00Z",
            "tariff_rate": 2,
            "tariff_type": "commission"
          },
          {
            "min_charge": {
              "amount": "100",
              "currency": "RUB"
            },
            "tariff_charge": {
              "amount": "100",
              "currency": "RUB"
            },
            "tariff_deadline_at": "2026-04-02T09:00:00Z",
            "tariff_rate": 2,
            "tariff_type": "commission"
          },
          {
            "min_charge": {
              "amount": "150",
              "currency": "RUB"
            },
            "tariff_charge": {
              "amount": "150",
              "currency": "RUB"
            },
            "tariff_deadline_at": "9999-12-31T23:59:59.999999900Z",
            "tariff_rate": 3,
            "tariff_type": "commission"
          }
        ]
      }
    ],
    "has_next": false
  }
}
```

---

## Получить информацию об отправлении по идентификатору

`POST /v3/posting/fbs/get`

Operation ID: `PostingAPI_GetFbsPostingV3`

Чтобы получать актуальную дату отгрузки, регулярно обновляйте информацию об отправлениях или подключите пуш-уведомления .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — string Идентификатор отправления.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.
  - `analytics_data` — boolean Добавить в ответ данные аналитики.
  - `barcodes` — boolean Добавить в ответ штрихкоды отправления.
  - `financial_data` — boolean Добавить в ответ финансовые данные.
  - `legal_info` — boolean Добавить в ответ юридическую информацию.
  - `product_exemplars` — boolean Добавить в ответ данные о продуктах и их экземплярах.
  - `related_postings` — boolean Добавить в ответ номера связанных отправлений. Связанные отправления — те, на которое было разделено родительское отправление при сборке.
  - `translit` — boolean Выполнить транслитерацию возвращаемых значений.

Пример запроса:

```json
{
  "posting_number": "57195475-0050-3",
  "with": {
    "analytics_data": false,
    "barcodes": false,
    "financial_data": false,
    "legal_info": false,
    "product_exemplars": false,
    "related_postings": true,
    "translit": false
  }
}
```

### Ответы

- 200 Информация об отправлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Информация об отправлении.
  - `additional_data` — Array of objects
  - `addressee` — object Контактные данные получателя.
  - `analytics_data` — object Данные аналитики.
  - `available_actions` — Array of strings Доступные действия и информация об отправлении: arbitration — открыть спор; awaiting_delivery — перевести в статус «Ожидает отгрузки»; can_create_chat — начать чат с покупателем; cancel — отменить отправление; click_track_number — просмотреть по трек-номеру историю изменения статусов в личном кабинете; customer_phone_available — телефон покупателя; has_weight_products — весовые товары в отправлении; hide_region_and_city — скрыть регион и город покупателя в отчёте; invoice_get — получить информацию из счёта-фактуры; invoice_send — создать счёт-фактуру; invoice_update — отредактировать счёт-фактуру; label_download_big — скачать большую этикетку; label_download_small — скачать маленькую этикетку; label_download — скачать этикетку; non_int_delivered — перевести в статус «Условно доставлен»; non_int_delivering — перевести в статус «Доставляется»; non_int_last_mile — перевести в статус «Курьер в пути»; product_cancel — отменить часть товаров в отправлении; set_cutoff — необходимо указать дату отгрузки, воспользуйтесь методом /v1/posting/cutoff/set ; set_timeslot — изменить время доставки покупателю; set_track_number — указать или изменить трек-номер; ship_async_in_process — отправление собирается; ship_async_retry — собрать отправление повторно после ошибки сборки; ship_async — собрать отправление; ship_with_additional_info — необходимо заполнить дополнительную информацию; ship — собрать отправление; update_cis — изменить дополнительную информацию.
  - `barcodes` — object Штрихкоды отправления.
  - `cancellation` — object Информация об отмене.
  - `courier` — object Данные о курьере.
  - `customer` — object Данные о покупателе.
  - `container` — object Информация о грузоместе.
  - `container_sort_type` — string Тип сортировки грузоместа: SORT — сортируемый; NON-SORT — несортируемый.
  - `delivering_date` — string <date-time> Дата передачи отправления в доставку.
  - `delivery_method` — object Метод доставки.
  - `delivery_price` — string Стоимость доставки.
  - `fact_delivery_date` — string <date-time> Дата фактической передачи отправления в доставку.
  - `financial_data` — object Данные о стоимости товара, размере скидки, выплате и комиссии.
  - `in_process_at` — string <date-time> Дата и время начала обработки отправления.
  - `is_express` — boolean Если использовалась быстрая доставка Ozon Express — true .
  - `is_multibox` — boolean Признак, что в отправлении есть многокоробочный товар и нужно передать количество коробок для него: true — до сборки передайте количество коробок через метод /v3/posting/multiboxqty/set . false — отправление собрано с указанием количества коробок в параметре multi_box_qty или в отправлении нет многокоробочного товара.
  - `legal_info` — object Юридическая информация о покупателе.
  - `multi_box_qty` — integer <int32> Количество коробок, в которые упакован товар.
  - `optional` — object Список товаров с дополнительными характеристиками.
  - `order_id` — integer <int64> Идентификатор заказа, к которому относится отправление.
  - `order_number` — string Номер заказа, к которому относится отправление.
  - `parent_posting_number` — string Номер родительского отправления, в результате разделения которого появилось текущее.
  - `pickup_code_verified_at` — string <date-time> Дата и время успешной валидации кода курьера. Чтобы проверить код курьера, воспользуйтесь методом /v1/posting/fbs/pick-up-code/verify .
  - `posting_number` — string Номер отправления.
  - `product_exemplars` — object Информация по продуктам и их экземплярам. Ответ содержит поле product_exemplars , если в запросе передан признак with.product_exemplars = true .
  - `products` — Array of objects Массив товаров в отправлении.
  - `provider_status` — string Статус службы доставки.
  - `prr_option` — object Информация об услуге погрузочно-разгрузочных работ. Актуально для КГТ-отправлений с доставкой силами продавца или интегрированной службой.
  - `related_postings` — object Связанные отправления.
  - `related_weight_postings` — Array of strings Список номеров связанных весовых отправлений.
  - `require_blr_traceable_attrs` — boolean true , если нужно заполнить атрибуты прослеживаемости.
  - `requirements` — object Cписок продуктов, для которых нужно передать страну-изготовителя, номер грузовой таможенной декларации (ГТД), регистрационный номер партии товара (РНПТ), маркировку «Честный ЗНАК», другие маркировки или вес, чтобы перевести отправление в следующий статус.
  - `shipment_date` — string <date-time> Дата и время, до которой необходимо собрать отправление. Показываем рекомендованное время отгрузки. По истечении этого времени начнёт применяться новый тариф, информацию о нём уточняйте в поле tariffication .
  - `shipment_date_without_delay` — string <date-time> Дата и время отгрузки без просрочки.
  - `status` — string Статус отправления: acceptance_in_progress — идёт приёмка, arbitration — арбитраж, awaiting_approve — ожидает подтверждения, awaiting_deliver — ожидает отгрузки, awaiting_packaging — ожидает упаковки, awaiting_registration — ожидает регистрации, awaiting_verification — создано, cancelled — отменено, cancelled_from_split_pending — отменён из-за разделения отправления, client_arbitration — клиентский арбитраж доставки, delivered — доставлено, delivering — доставляется, driver_pickup — у водителя, not_accepted — не принят на сортировочном центре,
  - `substatus` — string Подстатус отправления: posting_acceptance_in_progress — идёт приёмка, posting_in_arbitration — арбитраж, posting_created — создано, posting_in_carriage — в перевозке, posting_not_in_carriage — не добавлено в перевозку, posting_registered — зарегистрировано, posting_transferring_to_delivery ( status=awaiting_deliver ) — передаётся в доставку, posting_awaiting_passport_data — ожидает паспортных данных, posting_created — создано, posting_awaiting_registration — ожидает регистрации, posting_registration_error — ошибка регистрации, posting_transferring_to_delivery ( status=awaiting_registration ) — передаётся курьеру, posting_split_pending — создано, posting_canceled — отменено, posting_in_client_arbitration — клиентский арбитраж доставки, posting_delivered — доставлено, posting_received — получено, posting_conditionally_delivered — условно доставлено, posting_in_courier_service — курьер в пути, posting_in_pickup_point — в пункте выдачи, posting_on_way_to_city — в пути в ваш город, posting_on_way_to_pickup_point — в пути в пункт выдачи, posting_returned_to_warehouse — возвращено на склад, posting_transferred_to_courier_service — передаётся в службу доставки, posting_driver_pick_up — у водителя, posting_not_in_sort_center — не принято на сортировочном центре, ship_failed — сборка не удалась.
  - `previous_substatus` — string Предыдущий подстатус отправления. Возможные значения: posting_acceptance_in_progress — идёт приёмка, posting_in_arbitration — арбитраж, posting_created — создано, posting_in_carriage — в перевозке, posting_not_in_carriage — не добавлено в перевозку, posting_registered — зарегистрировано, posting_transferring_to_delivery ( status=awaiting_deliver ) — передаётся в доставку, posting_awaiting_passport_data — ожидает паспортных данных, posting_created — создано, posting_awaiting_registration — ожидает регистрации, posting_registration_error — ошибка регистрации, posting_transferring_to_delivery ( status=awaiting_registration ) — передаётся курьеру, posting_split_pending — создано, posting_canceled — отменено, posting_in_client_arbitration — клиентский арбитраж доставки, posting_delivered — доставлено, posting_received — получено, posting_conditionally_delivered — условно доставлено, posting_in_courier_service — курьер в пути, posting_in_pickup_point — в пункте выдачи, posting_on_way_to_city — в пути в ваш город, posting_on_way_to_pickup_point — в пути в пункт выдачи, posting_returned_to_warehouse — возвращено на склад, posting_transferred_to_courier_service — передаётся в службу доставки, posting_driver_pick_up — у водителя, posting_not_in_sort_center — не принято на сортировочном центре.
  - `tpl_integration_type` — string Тип интеграции со службой доставки: ozon — доставка через Ozon логистику. aggregator — доставка внешней службой, Ozon регистрирует заказ. 3pl_tracking — доставка внешней службой, продавец регистрирует заказ. non_integrated — доставка силами продавца.
  - `tracking_number` — string Трек-номер отправления.
  - `tariffication` — object Информация по тарификации отгрузки.
  - `tariffication_steps` — Array of objects Этапы тарификации.

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

## Получить информацию об отправлении по штрихкоду

`POST /v2/posting/fbs/get-by-barcode`

Operation ID: `PostingAPI_GetFbsPostingByBarcode`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `barcode required` — string Штрихкод отправления. Можно получить с помощью методов: /v3/posting/fbs/get , /v3/posting/fbs/list и /v3/posting/fbs/unfulfilled/list в массиве barcodes .

### Ответы

- 200 Информация об отправлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `barcodes` — object Штрихкоды отправления.
  - `cancel_reason_id` — integer <int64> Идентификатор причины отмены отправления.
  - `created_at` — string <date-time> Дата и время создания отправления.
  - `in_process_at` — string <date-time> Дата и время начала обработки отправления.
  - `order_id` — integer <int64> Идентификатор заказа, к которому относится отправление.
  - `order_number` — string Номер заказа, к которому относится отправление.
  - `posting_number` — string Номер отправления.
  - `products` — Array of objects Список товаров в отправлении.
  - `shipment_date` — string <date-time> Дата и время, до которой необходимо собрать отправление. Если отправление не собрать к этой дате — оно автоматически отменится.
  - `status` — string Статус отправления.

Пример ответа:

```json
{
  "result": {
    "order_id": 47558522075,
    "order_number": "2130415463-0013",
    "posting_number": "2130415463-0013-1",
    "status": "delivered",
    "cancel_reason_id": 0,
    "created_at": "2025-01-29T08:58:07Z",
    "in_process_at": "2025-01-29T08:59:40Z",
    "shipment_date": "2025-01-29T18:00:00Z",
    "products": [
      {
        "sku": 498274975,
        "name": "Стульчик для кормления ребенка",
        "quantity": 1,
        "offer_id": "6460551001",
        "price": "2300.0000"
      }
    ],
    "barcodes": {
      "upper_barcode": "%101%10293145035",
      "lower_barcode": "201864523528000"
    }
  }
}
```

---

## Указать количество коробок для многокоробочных отправлений

`POST /v3/posting/multiboxqty/set`

Operation ID: `PostingAPI_PostingMultiBoxQtySetV3`

Метод для передачи количества коробок для отправлений, в которых есть многокоробочные товары. Используйте метод при работе по схеме rFBS Агрегатор — c доставкой партнёрами Ozon.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — string Идентификатор многокоробочного отправления.
- `multi_box_qty required` — integer <int64> Количество коробок, в которые упакован товар.

Пример запроса:

```json
{
  "multi_box_qty": 321231,
  "posting_number": "228245774-2229-1"
}
```

### Ответы

- 200 Количество коробок указано
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат передачи количества коробок.
  - `result` — boolean Возможные значения: true — значение передано успешно. false — при передаче произошла ошибка. Попробуйте снова.

---

## Список доступных стран-изготовителей

`POST /v2/posting/fbs/product/country/list`

Operation ID: `PostingAPI_ListCountryProductFbsPostingV2`

Метод для получения списка доступных стран-изготовителей и их ISO кодов.

### Тело запроса (application/json)

- `name_search` — string Фильтрация по строке.

### Ответы

- 200 Список доступных стран-изготовителей

#### Схема ответа (200)

- `result` — Array of objects Список стран-изготовителей и ISO коды.
  - `name` — string Название страны на русском языке.
  - `country_iso_code` — string ISO код страны.

Пример ответа:

```json
{
  "result": [
    {
      "name": "Алжир",
      "country_iso_code": "DZ"
    }
  ]
}
```

---

## Добавить информацию о стране-изготовителе товара

`POST /v2/posting/fbs/product/country/set`

Operation ID: `PostingAPI_SetCountryProductFbsPostingV2`

Метод для добавления на продукт атрибута «Страна-изготовитель», если он не был указан.

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.
- `product_id required` — integer <int64> Идентификатор товара в системе Ozon — product_id .
- `country_iso_code required` — string Двухбуквенный код добавляемой страны по стандарту ISO_3166-1. Список доступных стран-изготовителей и их ISO коды можно получить с помощью метода /v2/posting/fbs/product/country/list .

Пример запроса:

```json
{
  "country_iso_code": "NO",
  "posting_number": "57195475-0050-3",
  "product_id": 180550365
}
```

### Ответы

- 200 Страна-изготовитель добавлена

#### Схема ответа (200)

- `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
- `is_gtd_needed` — boolean Признак того, что необходимо передать номер грузовой таможенной декларации (ГТД) для продукта и отправления.

Пример ответа:

```json
{
  "product_id": 180550365,
  "is_gtd_needed": true
}
```

---

## Получить ограничения пункта приёма

`POST /v1/posting/fbs/restrictions`

Operation ID: `PostingAPI_GetRestrictions`

Метод для получения габаритных, весовых и прочих ограничений пункта приёма по номеру отправления. Метод применим только для работы по схеме FBS.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления, для которого нужно определить ограничения.

Пример запроса:

```json
{
  "posting_number": "76673629-0020-1"
}
```

### Ответы

- 200 Ограничения пункта приёма
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object
  - `posting_number` — string Номер отправления.
  - `max_posting_weight` — number <double> Ограничение по максимальному весу в граммах.
  - `min_posting_weight` — number <double> Ограничение по минимальному весу в граммах.
  - `width` — number <double> Ограничение по ширине в сантиметрах.
  - `length` — number <double> Ограничение по длине в сантиметрах.
  - `height` — number <double> Ограничение по высоте в сантиметрах.
  - `max_posting_price` — number <double> Ограничение по максимальной стоимости отправления в рублях.
  - `min_posting_price` — number <double> Ограничение по минимальной стоимости отправления в рублях.

Пример ответа:

```json
{
  "result": {
    "posting_number": "76673629-0020-1",
    "max_posting_weight": 40000,
    "min_posting_weight": 0,
    "width": 500,
    "height": 500,
    "length": 500,
    "max_posting_price": 500000,
    "min_posting_price": 0
  }
}
```

---

## Напечатать этикетку

`POST /v2/posting/fbs/package-label`

Operation ID: `PostingAPI_PostingFBSPackageLabel`

Если вы работаете по схеме rFBS или rFBS Express, изучите процесс печати этикетки в Базе знаний продавца . Генерирует PDF-файл с этикетками для указанных отправлений в статусе «Ожидает отгрузки» — awaiting_deliver . В одном запросе можно передать не больше 20 идентификаторов. Если хотя бы для одного отправления возникнет ошибка, этикетки не будут подготовлены для всех отправлений в запросе. Рекомендуем запрашивать этикетки через 45–60 секунд после сборки заказа. Ошибка The next postings aren't ready означает, что этикетки ещё не готовы, повторите запрос позднее.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — Array of strings Идентификатор отправления.

Пример запроса:

```json
{
  "posting_number": [
    "48173252-0034-4"
  ]
}
```

### Ответы

- 200 Маркировка напечатана
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `file_content` — string <byte> Содержание файла в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:

```json
{
  "content_type": "application/pdf",
  "file_name": "ticket-170660-2023-07-13T13:17:06Z.pdf",
  "file_content": "%PDF-1.7\n%âãÏÓ\n53 0 obj\n<</MarkInfo<</Marked true/Type/MarkInfo>>/Pages 9 0 R/StructTreeRoot 10 0 R/Type/Catalog>>\nendobj\n8 0 obj\n<</Filter/FlateDecode/Length 2888>>\nstream\nxå[[ݶ\u0011~?¿BÏ\u0005Bs\u001c^\u0000Àwí5ú\u0010 m\u0016Èsà¦)\n;hÒ\u0014èÏïG\u0014)<{äµ] ]?¬¬oIÎ}¤F±óϤñï\u001bÕü×X­´OÏï?^~¹$<ø¨È9q\u0013Y\u0012åñì§_¼|ÿégü\t+\u0012\u001bxª}Æxҿ¿¼_º¼xg¦þ5OkuÌ3ýíògüûå\"Ni\u0016C\u0001°\u000fA9g'r¢\"\u0013YóĪ\u0018NÑ{\u001dÕóZ¬\\Ô\""
}
```

---

## Создать задание на формирование этикеток

`POST /v2/posting/fbs/package-label/create`

Operation ID: `PostingAPI_CreateLabelBatchV2`

Если вы работаете по схеме rFBS или rFBS Express, изучите процесс печати этикетки в Базе знаний продавца . Метод для создания задания на асинхронное формирование этикеток для отправлений в статусе «Ожидает отгрузки» — awaiting_deliver . Метод может вернуть несколько заданий: на формирование маленькой и большой этикетки. Рекомендуем запрашивать этикетки через 45–60 секунд после сборки заказа. Чтобы получить созданные этикетки, используйте /v1/posting/fbs/package-label/get .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — Array of strings Номера отправлений, для которых нужны этикетки.

Пример запроса:

```json
{
  "posting_number": [
    "4708216109137",
    "3697105098026"
  ]
}
```

### Ответы

- 200 Задания на формирование этикеток
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `tasks` — Array of objects Список заданий.

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

## Создать задание на выгрузку этикеток

`POST /v1/posting/fbs/package-label/create`

Operation ID: `PostingAPI_CreateLabelBatch`

В будущем метод будет отключён. Мы предупредим вас об этом за месяц. Переключитесь на /v2/posting/fbs/package-label/create . Метод для создания задания на асинхронное формирование этикеток. Для получения этикеток, созданных в результате вызова метода, используйте /v1/posting/fbs/package-label/get .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — Array of strings Номера отправлений, для которых нужны этикетки.

Пример запроса:

```json
{
  "posting_number": [
    "228245774-2229-1"
  ]
}
```

### Ответы

- 200 Идентификатор задания на формирование этикеток
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `task_id` — integer <int64> Идентификатор задания на формирование этикеток.

Пример ответа:

```json
{
  "result": {
    "task_id": 5819327210249
  }
}
```

---

## Получить файл с этикетками

`POST /v1/posting/fbs/package-label/get`

Operation ID: `PostingAPI_GetLabelBatch`

Метод для получения этикеток после вызова /v1/posting/fbs/package-label/create .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `task_id required` — integer <int64> Номер задания на формирование этикеток из ответа метода /v1/posting/fbs/package-label/create .

### Ответы

- 200 Статус формирования этикеток или файл с ними
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `error` — string Код ошибки.
  - `file_url` — string Ссылка на файл с этикетками.
  - `printed_postings_count` — integer <int32> Количество напечатанных этикеток.
  - `status` — string Статус формирования этикеток: pending — задание в очереди. in_progress — формируются. completed — файл с этикетками готов. error — ошибка при создании файла.
  - `unprinted_postings` — Array of objects Информация об ошибках, из-за которых не получилось напечатать этикетки.
  - `unprinted_postings_count` — integer <int32> Количество этикеток, которые не получилось напечатать.

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

## Причины отмены отправления

`POST /v1/posting/fbs/cancel-reason`

Operation ID: `PostingAPI_GetPostingFbsCancelReasonV1`

Возвращает список причин отмены для конкретных отправлений.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `related_posting_numbers required` — Array of strings Номера отправлений.

Пример запроса:

```json
{
  "related_posting_numbers": [
    "73837363-0010-3"
  ]
}
```

### Ответы

- 200 Причины отмены отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат запроса.
  - `posting_number` — string Номер отправления.
  - `reasons` — Array of objects Информация о причинах отмены.

Пример ответа:

```json
{
  "result": [
    {
      "posting_number": "73837363-0010-3",
      "reasons": [
        {
          "id": 352,
          "title": "Товар закончился на складе продавца",
          "type_id": "seller"
        },
        {
          "id": 400,
          "title": "Остался только бракованный товар",
          "type_id": "seller"
        },
        {
          "id": 402,
          "title": "Другое (вина продавца)",
          "type_id": "seller"
        }
      ]
    }
  ]
}
```

---

## Причины отмены отправлений

`POST /v2/posting/fbs/cancel-reason/list`

Operation ID: `PostingAPI_GetPostingFbsCancelReasonList`

Возвращает список причин отмены для всех отправлений.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Причины отмены отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `id` — integer <int64> Идентификатор причины отмены.
  - `is_available_for_cancellation` — boolean Результат отмены отправления. true , если запрос доступен для отмены.
  - `title` — string Название категории.
  - `type_id` — string Инициатор отмены отправления: buyer — покупатель, seller — продавец.

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

## Отменить отправку некоторых товаров в отправлении

`POST /v2/posting/fbs/product/cancel`

Operation ID: `PostingAPI_CancelFbsPostingProduct`

Используйте метод, если вы не можете отправить часть продуктов из отправления. Чтобы получить идентификаторы причин отмены cancel_reason_id при работе по схемам FBS или rFBS, используйте метод /v2/posting/fbs/cancel-reason/list . Условно-доставленные отправления отменить нельзя.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cancel_reason_id required` — integer <int64> Идентификатор причины отмены отправления товара.
- `cancel_reason_message required` — string Обязательное поле. Дополнительная информация по отмене.
- `items required` — Array of objects Информация о товарах.
- `posting_number required` — string Идентификатор отправления.

Пример запроса:

```json
{
  "cancel_reason_id": 352,
  "cancel_reason_message": "Product is out of stock",
  "items": [
    {
      "quantity": 5,
      "sku": 150587396
    }
  ],
  "posting_number": "33920113-1231-1"
}
```

### Ответы

- 200 Отправка отменена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — string Номер отправления.

---

## Отменить отправление

`POST /v2/posting/fbs/cancel`

Operation ID: `PostingAPI_CancelFbsPosting`

Меняет статус отправления на cancelled . Перед началом работы проверьте причины отмены для конкретного отправления методом /v1/posting/fbs/cancel-reason . Условно-доставленные отправления отменить нельзя. Если значение параметра cancel_reason_id — 402, заполните поле cancel_reason_message .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cancel_reason_id required` — integer <int64> Идентификатор причины отмены отправления.
- `cancel_reason_message` — string Дополнительная информация по отмене. Если cancel_reason_id = 402 , параметр обязательный.
- `posting_number required` — string Идентификатор отправления.

Пример запроса:

```json
{
  "cancel_reason_id": 352,
  "cancel_reason_message": "Product is out of stock",
  "posting_number": "33920113-1231-1"
}
```

### Ответы

- 200 Отправление отменено
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнился без ошибок.

---

## Открыть спор по отправлению

`POST /v2/posting/fbs/arbitration`

Operation ID: `PostingAPI_MoveFbsPostingToArbitration`

Если отправление передано в доставку, но не просканировано в сортировочном центре, можно открыть спор. Открытый спор переведёт отправление в статус arbitration .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — Array of strings Идентификатор отправления.

Пример запроса:

```json
{
  "posting_number": [
    "33920143-1195-1"
  ]
}
```

### Ответы

- 200 Открыт спор по отправлению
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнился без ошибок.

---

## Передать отправление к отгрузке

`POST /v2/posting/fbs/awaiting-delivery`

Operation ID: `PostingAPI_MoveFbsPostingToAwaitingDelivery`

Передает спорные заказы к отгрузке. Статус отправления изменится на awaiting_deliver .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — Array of strings Идентификатор отправления. Максимальное количество в одном запросе — 100.

Пример запроса:

```json
{
  "posting_number": [
    "33920143-1195-1"
  ]
}
```

### Ответы

- 200 Отправление передано к отгрузке
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнился без ошибок.

---

## Проверить код курьера

`POST /v1/posting/fbs/pick-up-code/verify`

Operation ID: `PostingAPI_PostingFBSPickupCodeVerify`

Метод позволяет проверить код курьера при передаче отправлений realFBS Express. Подробнее о передаче отправлений в Базе знаний продавца .

### Тело запроса (application/json)

- `pickup_code required` — string Код курьера.
- `posting_number required` — string Номер отправления.

Пример запроса:

```json
{
  "pickup_code": "342341212312",
  "posting_number": "228245774-2229-1"
}
```

### Ответы

- 200 Результат проверки

#### Схема ответа (200)

- `valid` — boolean true , если код корректный.

---

## Таможенные декларации ETGB

`POST /v1/posting/global/etgb`

Operation ID: `PostingAPI_GetEtgb`

Метод для получения таможенных деклараций Elektronik Ticaret Gümrük Beyannamesi (ETGB) для продавцов из Турции.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date required` — object Фильтр по периоду создания деклараций.

Пример запроса:

```json
{
  "date": {
    "from": "2023-02-13T12:13:16.818Z",
    "to": "2023-02-13T12:13:16.818Z"
  }
}
```

### Ответы

- 200 Информация о декларациях
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат запроса.
  - `posting_number` — string Номер отправления.
  - `etgb` — object Информация о декларации.

Пример ответа:

```json
{
  "result": [
    {
      "posting_number": "228245774-2229-1",
      "etgb": {
        "number": "1",
        "date": "12-12-2026",
        "url": "htttp://testurl/posting/global.ru"
      }
    }
  ]
}
```

---

## Список неоплаченных товаров, заказанных юридическими лицами

`POST /v1/posting/unpaid-legal/product/list`

Operation ID: `PostingAPI_UnpaidLegalProductList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `limit required` — integer <int32> [ 1 .. 1000 ] Количество значений в ответе.

### Ответы

- 200 Список неоплаченных товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `products` — Array of objects Список неоплаченных товаров.
- `cursor` — string Указатель для выборки следующих данных.

Пример ответа:

```json
{
  "products": [
    {
      "product_id": 145123054,
      "offer_id": "10032",
      "quantity": 1,
      "name": "Телевизор LG",
      "image_url": "https://url"
    }
  ],
  "cursor": "hCGiPPopcBFMgMErdzaCEpzQfinuPyEhUoSmBMADuoFAhBjXeA=="
}
```

---
