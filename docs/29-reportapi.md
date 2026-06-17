# Отчёты

_Тег: `ReportAPI` · операций: 11_

## Информация об отчёте

`POST /v1/report/info`

Operation ID: `ReportAPI_ReportInfo`

Возвращает информацию о созданном ранее отчёте по его идентификатору.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `code required` — string Уникальный идентификатор отчёта.

Пример запроса:

```json
{
  "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69"
}
```

### Ответы

- 200 Информация об отчёте
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Информация об отчёте.
  - `code` — string Уникальный идентификатор отчёта.
  - `created_at` — string <date-time> Дата создания отчёта.
  - `error` — string Код ошибки при генерации отчёта.
  - `expires_at` — string <date-time> Дата и время, до которых отчёт доступен по ссылке. Поле вернётся пустым, если отчёт сформирован до 14 октября 2025.
  - `file` — string Ссылка на XLSX-файл. Для отчёта с типом SELLER_RETURNS ссылка доступна 5 минут после выполнения запроса.
  - `params` — object Массив с фильтрами, указанными при создании отчёта продавцом.
  - `report_type` — string Тип отчёта: SELLER_PRODUCTS — отчёт по товарам; SELLER_STOCK — отчёт об остатках товаров; SELLER_RETURNS — отчёт о возвратах; SELLER_POSTINGS — отчёт об отправлениях; SELLER_DISCOUNTED — отчёт об уценённых товарах; MUTUAL_SETTLEMENT — отчёт о взаиморасчётах; DOCUMENT_B2B_SALES — отчёт о продажах юридическим лицам; COMPENSATION_REPORT — отчёт о компенсациях; DECOMPENSATION_REPORT — отчёт о декомпенсациях; MARKED_PRODUCTS_SALES — отчёт по продажам маркированных товаров; SELLER_PLACEMENT_BY_PRODUCTS — отчёт о стоимости размещения по товарам; SELLER_PLACEMENT_BY_SUPPLIES — отчёт о стоимости размещения по поставкам.
  - `status` — string Статус генерации отчёта: waiting — в очереди на обработку, processing — обрабатывается, success — отчёт успешно создан, failed — ошибка при создании отчёта.

Пример ответа:

```json
{
  "result": {
    "code": "REPORT_seller_products_924336_1720170405_a9ea2f27",
    "status": "success",
    "error": "",
    "expires_at": "2025-11-10T11:16:00.267Z",
    "file": "https://cdn1.ozone.ru/s3/item-picture-6/f3/ce/f4ceae54b323213d3e61e59c323bd8e5.csv",
    "report_type": "seller_products",
    "params": {},
    "created_at": "2021-11-25T14:54:55.688260Z"
  }
}
```

---

## Список отчётов

`POST /v1/report/list`

Operation ID: `ReportAPI_ReportList`

Возвращает список отчётов, которые были сформированы раньше.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `page required` — integer <int32> Номер страницы.
- `page_size required` — integer <int32> Количество значений на странице: по умолчанию — 100, маĸсимальное значение — 1000.
- `report_type` — string Default: "ALL" Тип отчёта: ALL — все отчёты; SELLER_PRODUCTS — отчёт по товарам; SELLER_STOCK — отчёт об остатках товаров; SELLER_RETURNS — отчёт о возвратах; SELLER_POSTINGS — отчёт об отправлениях; SELLER_DISCOUNTED — отчёт об уценённых товарах; MUTUAL_SETTLEMENT — отчёт о взаиморасчётах; DOCUMENT_B2B_SALES — отчёт о продажах юридическим лицам; COMPENSATION_REPORT — отчёт о компенсациях; DECOMPENSATION_REPORT — отчёт о декомпенсациях; MARKED_PRODUCTS_SALES — отчёт по продажам маркированных товаров; SELLER_PLACEMENT_BY_PRODUCTS — отчёт о стоимости размещения по товарам; SELLER_PLACEMENT_BY_SUPPLIES — отчёт о стоимости размещения по поставкам.

Пример запроса:

```json
{
  "page": 1,
  "page_size": 1000,
  "report_type": "ALL"
}
```

### Ответы

- 200 Список отчётов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `reports` — Array of objects Массив со всеми сгенерированными отчётами.
  - `total` — integer <int32> Суммарное количество отчётов.

Пример ответа:

```json
{
  "result": {
    "reports": [
      {
        "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69",
        "status": "success",
        "error": "",
        "expires_at": "2025-11-10T11:35:10.028Z",
        "file": "https://cdn1.ozone.ru/s3/item-picture-6/f3/ce/f4ceae54b323213d3e61e59c323bd8e5.csvv",
        "report_type": "seller_products",
        "params": {
          "visibility": "3"
        },
        "created_at": "2019-02-06T12:09:47.258062Z"
      },
      {
        "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69",
        "status": "success",
        "error": "",
        "file": "https://cdn1.ozone.ru/s3/item-picture-6/f3/ce/f4ceae54b323213d3e61e59c323bd8e5.csv",
        "report_type": "seller_products",
        "params": {
          "visibility": "3"
        },
        "created_at": "2019-02-15T08:34:32.267178Z"
      }
    ],
    "total": 2
  }
}
```

---

## Отчёт по товарам

`POST /v1/report/products/create`

Operation ID: `ReportAPI_CreateCompanyProductsReport`

Метод для получения отчёта с данными о товарах. Например, Ozon ID, количества товаров, цен, статуса. Соответствует разделу/действию Товары и цены → Список товаров → Скачать → Товары CSV в личном кабинете. Пояснения к некоторым полям: Ozon Product ID — идентификатор товара в нашей системе. Например, если вы продаёте товар со склада Ozon и со своего склада, Ozon Product ID будет для них одинаковым. FBO Ozon SKU ID — идентификатор товара, который продаётся со склада Ozon. FBS Ozon SKU ID — идентификатор товара, который продаётся с вашего склада. CrossBorder Ozon SKU — идентификатор товара, который продаётся из-за границы. Barcode — штрихкод товара, который печатается на маркировке. Статус товара — можно ли купить товар на Ozon. Если статус «Готов к продаже», товар купить нельзя. Доступно на складе Ozon, шт — сколько штук товара на складе доступно для продажи. Это количество не включает зарезервированные товары. Зарезервировано, шт — сколько штук товара со статусом «Зарезервировано». Товар зарезервирован с момента получения заказа на Ozon и до упаковки для передачи покупателю. Текущая цена с учётом скидки, руб. — цена, по которой товар продаётся сейчас (на момент загрузки отчёта, с учётом скидки). Если товар участвует в акции, указана цена без её учёта. Базовая цена (цена до скидок), руб. — цена без учёта скидки. Цена Premium, руб. — цена для покупателей с подпиской Ozon Premium. Рекомендованная цена, руб. — минимальная цена на товар на другой торговой площадке. Актуальная ссылка на рекомендованную цену — ссылка на товар с рекомендованной ценой на другой торговой площадке.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.
- `offer_id` — Array of strings Идентификатор товара в системе продавца — артикул.
- `search` — string Поиск по содержанию записи, проверяет наличие.
- `sku` — Array of integers <int64> Идентификатор товара в системе Ozon — SKU.
- `visibility` — string Default: "ALL" Enum: "ALL" "VALIDATION_STATE_FAIL" "TO_SUPPLY" "IN_SALE" "REMOVED_FROM_SALE" "PARTIAL_APPROVED" "IMAGE_ABSENT" "ARCHIVED" "AUTO_ARCHIVED" "MANUAL_ARCHIVED" Фильтр по видимости товара: ALL — все товары, кроме архивных; VALIDATION_STATE_FAIL — товары, которые не прошли проверку валидатором на премодерации; TO_SUPPLY — товары, готовые к продаже; IN_SALE — товары в продаже; REMOVED_FROM_SALE — товары, скрытые от покупателей; PARTIAL_APPROVED — товары с предупреждениями, требуют доработки; IMAGE_ABSENT — товары без фото; ARCHIVED — товары в архиве; AUTO_ARCHIVED — товары, архивированные автоматически; MANUAL_ARCHIVED — товары, архивированные вручную.

Пример запроса:

```json
{
  "language": "DEFAULT",
  "offer_id": [],
  "search": "",
  "sku": [],
  "visibility": "ALL"
}
```

### Ответы

- 200 Отчёт по товарам
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

Пример ответа:

```json
{
  "result": {
    "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69"
  }
}
```

---

## Отчёт о возвратах

`POST /v2/report/returns/create`

Operation ID: `ReportAPI_ReportReturnsCreate`

Метод для получения отчёта о возвратах FBO и FBS.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter required` — object Фильтр.
- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.

Пример запроса:

```json
{
  "filter": {
    "delivery_schema": "fbs",
    "date_from": "2024-09-16T00:00:00.000Z",
    "date_to": "2024-09-19T23:59:59.999Z",
    "status": "MovingToSeller"
  },
  "language": "DEFAULT"
}
```

### Ответы

- 200 Отчёт о возвратах FBO и FBS
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

Пример ответа:

```json
{
  "result": {
    "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69"
  }
}
```

---

## Отчёт об отправлениях

`POST /v1/report/postings/create`

Operation ID: `ReportAPI_CreateCompanyPostingsReport`

Отчёт об отправлениях с информацией по заказам: статусы заказов, дата начала обработки, номера заказов, номера отправлений, стоимость отправлений, содержимое отправлений. Соответствует разделу FBO → Заказы со склада Ozon и FBS → Заказы с моих складов → CSV в личном кабинете.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter required` — object Фильтр.
- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

Пример запроса:

```json
{
  "filter": {
    "processed_at_from": "2021-09-02T17:10:54.861Z",
    "processed_at_to": "2021-11-02T17:10:54.861Z",
    "delivery_schema": [
      "fbo"
    ],
    "is_express": true,
    "sku": [],
    "cancel_reason_id": [],
    "offer_id": "",
    "status_alias": [],
    "statuses": [],
    "title": ""
  },
  "language": "DEFAULT",
  "with": {
    "additional_data": false,
    "analytics_data": false,
    "customer_data": false,
    "jewelry_codes": false
  }
}
```

### Ответы

- 200 Отчёт об отправлениях
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

Пример ответа:

```json
{
  "result": {
    "code": "REPORT_seller_postings_514893_1722847571_32a3508c-6b53-408c-a212-6c97138d23ed"
  }
}
```

---

## Финансовый отчёт

`POST /v1/finance/cash-flow-statement/list`

Operation ID: `FinanceAPI_FinanceCashFlowStatementList`

Метод для получения финансового отчёта за периоды с 01 по 15 и с 16 по 31. Запросить отчёт за отдельные дни не получится. Соответствует разделу Финансы → Баланс → Доходы и расходы в личном кабинете.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date required` — object Период формирования отчёта.
- `page required` — integer <int32> Номер страницы, возвращаемой в запросе.
- `with_details` — boolean true , если нужно добавить дополнительные параметры в ответ.
- `page_size required` — integer <int32> Количество элементов на странице.

Пример запроса:

```json
{
  "date": {
    "from": "2022-01-01T00:00:00.000Z",
    "to": "2022-12-31T00:00:00.000Z"
  },
  "with_details": true,
  "page": 1,
  "page_size": 1
}
```

### Ответы

- 200 Финансовый отчёт
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `cash_flows` — Array of objects Список отчётов.
  - `details` — object Детализированная информация.
  - `page_count` — integer <int64> Количество страниц с отчётами.

Пример ответа:

```json
{
  "result": {
    "cash_flows": [
      {
        "commission_amount": 1437,
        "currency_code": "string",
        "item_delivery_and_return_amount": 1991,
        "orders_amount": 1000,
        "period": {
          "begin": "2023-04-03T09:12:10.239Z",
          "end": "2023-04-03T09:12:10.239Z",
          "id": 11567022278500
        },
        "returns_amount": -3000,
        "services_amount": 8471.28
      }
    ],
    "details": {
      "period": {
        "begin": "2023-04-03T09:12:10.239Z",
        "end": "2023-04-03T09:12:10.239Z",
        "id": 11567022278500
      },
      "payments": [
        {
          "payment": 0,
          "currency_code": "string"
        }
      ],
      "begin_balance_amount": 0,
      "delivery": {
        "total": 0,
        "amount": 0,
        "delivery_services": {
          "total": 0,
          "items": [
            {
              "name": "string",
              "price": 0
            }
          ]
        }
      },
      "return": {
        "total": 0,
        "amount": 0,
        "return_services": {
          "total": 0,
          "items": [
            {
              "name": "string",
              "price": 0
            }
          ]
        }
      },
      "loan": 0,
      "invoice_transfer": 0,
      "rfbs": {
        "total": 0,
        "transfer_delivery": 0,
        "transfer_delivery_return": 0,
        "compensation_delivery_return": 0,
        "partial_compensation": 0,
        "partial_compensation_return": 0
      },
      "services": {
        "total": 0,
        "items": [
          {
            "name": "string",
            "price": 0
          }
        ]
      },
      "others": {
        "total": 0,
        "items": [
          {
            "name": "string",
            "price": 0
          }
        ]
      },
      "end_balance_amount": 0
    }
  },
  "page_count": 15
}
```

---

## Отчёт об уценённых товарах

`POST /v1/report/discounted/create`

Operation ID: `ReportAPI_CreateDiscountedReport`

Запускает генерацию отчёта по уценённым товарам на складе Ozon. Ozon может сам уценить товар, например, при повреждении. В результате запроса будет не сам отчёт, а его уникальный идентификатор. Чтобы получить отчёт, отправьте идентификатор в запросе метода /v1/report/info . С одного аккаунта продавца можно отправить 1 запрос в минуту. Соответствует разделу Аналитика → Отчёты → Продажи со склада Ozon → Товары, уценённые Ozon в личном кабинете.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Результат запроса
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `code` — string Уникальный идентификатор отчёта. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

Пример ответа:

```json
{
  "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69"
}
```

---

## Отчёт об остатках на FBS-складе

`POST /v1/report/warehouse/stock`

Operation ID: `ReportAPI_CreateStockByWarehouseReport`

Отчёт с информацией о количестве доступных и зарезервированных единиц товара на складе. Соответствует разделу FBS → Управление логистикой → Управление остатками → Скачать в XLS в личном кабинете. В результате запроса будет не сам отчёт, а его уникальный идентификатор. Чтобы получить отчёт, отправьте идентификатор в запросе метода /v1/report/info .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.
- `warehouseId required` — Array of strings <int64> <= 50 characters Идентификаторы складов. Ограничение значений в запросе. Максимум — 50.

Пример запроса:

```json
{
  "language": "DEFAULT",
  "warehouseId": [
    "1020002425123000"
  ]
}
```

### Ответы

- 200 Результат запроса
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

Пример ответа:

```json
{
  "code": "REPORT_seller_products_924336_1720170405_a9ea2f27-a473-4b13-99f9-d0cfcb5b1a69"
}
```

---

## Получить отчёт о стоимости размещения по товарам

`POST /v1/report/placement/by-products/create`

Operation ID: `CreatePlacementByProductsReport`

Соответствует разделу FBO → Стоимость размещения в личном кабинете. Отчёт можно получить не больше 5 раз в день.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_from required` — string Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to required` — string Дата окончания отчётного периода в формате YYYY-MM-DD . Максимальный период — 31 день.

Пример запроса:

```json
{
  "date_from": "string",
  "date_to": "string"
}
```

### Ответы

- 200 Отчёт о стоимости размещения

#### Схема ответа (200)

- `code` — string Уникальный идентификатор отчёта. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---

## Получить отчёт о стоимости размещения по поставкам

`POST /v1/report/placement/by-supplies/create`

Operation ID: `CreatePlacementBySuppliesReport`

Соответствует разделу FBO → Стоимость размещения в личном кабинете. Отчёт можно получить не больше 5 раз в день.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_from required` — string Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to required` — string Дата окончания отчётного периода в формате YYYY-MM-DD . Максимальный период — 31 день.

Пример запроса:

```json
{
  "date_from": "string",
  "date_to": "string"
}
```

### Ответы

- 200 Отчёт о стоимости размещения

#### Схема ответа (200)

- `code` — string Уникальный идентификатор отчёта. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---

## Сгенерировать отчёт по продажам товаров с маркировкой

`POST /v1/report/marked-products-sales/create`

Operation ID: `CreateCompanyMarkedProductsSalesReport`

В одном отчёте вы можете получить не больше 50 000 кодов маркировки. Чтобы получить остальные данные, уменьшите период формирования отчёта.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date` — object Период формирования отчёта.

Пример запроса:

```json
{
  "date": {
    "from": "string",
    "to": "string"
  }
}
```

### Ответы

- 200 Результат запроса

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---
