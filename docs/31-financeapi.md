# Финансовые отчёты

Финансовые отчёты Больше методов в разделе Premium-методы .

_Тег: `FinanceAPI` · операций: 10_

## Отчёт о реализации товаров (версия 2)

`POST /v2/finance/realization`

Operation ID: `FinanceAPI_GetRealizationReportV2`

Метод недоступен для продавцов, которые заключили договор с ТОО «ОЗОН Маркетплейс Казахстан». Метод позволяет получить отчёт за период не раньше августа 2023 года. Отчёты за более ранние периоды доступны в личном кабинете. Отчёт о реализации доставленных и возвращённых товаров за месяц. Отмены и невыкупы не включаются. Соответствует разделу Финансы → Документы → Отчёты о реализации → Отчёт о реализации товара в личном кабинете. Отчёт придёт не позднее 5-го числа следующего месяца. Подробнее об отчёте в Базе знаний продавца

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `month required` — integer <int32> Месяц.
- `year required` — integer <int32> Год.

### Ответы

- 200 Отчёт о реализации
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `header` — object Титульный лист отчёта.
  - `rows` — Array of objects Таблица отчёта.

Пример ответа:

```json
{
  "result": {
    "header": {
      "contract_date": "string",
      "contract_number": "string",
      "currency_sys_name": "string",
      "doc_date": "string",
      "number": "string",
      "payer_inn": "string",
      "payer_kpp": "string",
      "payer_name": "string",
      "receiver_inn": "string",
      "receiver_kpp": "string",
      "receiver_name": "string",
      "start_date": "string",
      "stop_date": "string"
    },
    "rows": [
      {
        "commission_ratio": 0,
        "delivery_commission": {
          "amount": 0,
          "bonus": 0,
          "commission": 0,
          "compensation": 0,
          "price_per_instance": 0,
          "quantity": 0,
          "standard_fee": 0,
          "bank_coinvestment": 0,
          "stars": 0,
          "pick_up_point_coinvestment": 0,
          "total": 0
        },
        "item": {
          "barcode": "string",
          "name": "string",
          "offer_id": "string",
          "sku": 0
        },
        "return_commission": {
          "amount": 0,
          "bonus": 0,
          "commission": 0,
          "compensation": 0,
          "price_per_instance": 0,
          "quantity": 0,
          "standard_fee": 0,
          "bank_coinvestment": 0,
          "stars": 0,
          "pick_up_point_coinvestment": 0,
          "total": 0
        },
        "rowNumber": 0,
        "seller_price_per_instance": 0
      }
    ]
  }
}
```

---

## Позаказный отчёт о реализации товаров

`POST /v1/finance/realization/posting`

Operation ID: `FinanceAPI_GetRealizationReportV1`

Метод недоступен для продавцов, которые заключили договор с ТОО «ОЗОН Маркетплейс Казахстан». Отчёт о реализации доставленных и возвращённых товаров с детализацией по каждому заказу. Отмены и невыкупы не включаются. Отчёт доступен с настоящего времени по август 2023 года включительно.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `month required` — integer <int32> Месяц.
- `year required` — integer <int32> Год.

### Ответы

- 200 Позаказный отчёт о реализации
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `header` — object Титульный лист отчёта.
- `rows` — Array of objects Таблица отчёта.

Пример ответа:

```json
{
  "header": {
    "contract_date": "string",
    "contract_number": "string",
    "currency_sys_name": "string",
    "doc_date": "string",
    "number": "string",
    "payer_inn": "string",
    "payer_kpp": "string",
    "payer_name": "string",
    "receiver_inn": "string",
    "receiver_kpp": "string",
    "receiver_name": "string",
    "start_date": "string",
    "stop_date": "string"
  },
  "rows": [
    {
      "commission_ratio": 0,
      "delivery_commission": {
        "amount": 0,
        "bonus": 0,
        "commission": 0,
        "compensation": 0,
        "price_per_instance": 0,
        "quantity": 0,
        "standard_fee": 0,
        "bank_coinvestment": 0,
        "stars": 0,
        "pick_up_point_coinvestment": 0,
        "total": 0
      },
      "item": {
        "barcode": "string",
        "name": "string",
        "offer_id": "string",
        "sku": 0
      },
      "return_commission": {
        "amount": 0,
        "bonus": 0,
        "commission": 0,
        "compensation": 0,
        "price_per_instance": 0,
        "quantity": 0,
        "standard_fee": 0,
        "bank_coinvestment": 0,
        "stars": 0,
        "pick_up_point_coinvestment": 0,
        "total": 0
      },
      "row_number": 0,
      "seller_price_per_instance": 0,
      "order": {
        "posting_number": "string",
        "created_date": "string"
      },
      "legal_entity_document": {
        "number": "string",
        "sale_date": "string"
      }
    }
  ]
}
```

---

## Список транзакций

`POST /v3/finance/transaction/list`

Operation ID: `FinanceAPI_FinanceTransactionListV3`

Метод устаревает и будет отключён 6 июля 2026 года. Переключитесь на /v1/finance/accrual/postings , /v1/finance/accrual/types , /v1/finance/accrual/by-day . Используйте метод с последовательной отправкой запросов. Данные могут не соответствовать информации в личном кабинете. Возвращает подробную информацию по всем начислениям. Максимальный период, за который можно получить информацию в одном запросе — 1 месяц. Если в запросе не указывать posting_number , то в ответе будут все отправления за указанный период или отправления определённого типа.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter` — posting_number (object) or date (object) Фильтр.
- `page required` — integer <int64> Номер страницы, возвращаемой в запросе.
- `page_size required` — integer <int64> <= 1000 Количество элементов на странице.

Пример запроса:

```json
{
  "filter": {
    "date": {
      "from": "2021-11-01T00:00:00.000Z",
      "to": "2021-11-02T00:00:00.000Z"
    },
    "operation_type": [],
    "posting_number": "",
    "transaction_type": "all"
  },
  "page": 1,
  "page_size": 1000
}
```

### Ответы

- 200 Список транзакций
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `operations` — Array of objects Информация об операциях.
  - `page_count` — integer <int64> Количество страниц. Если 0, страниц больше нет.
  - `row_count` — integer <int64> Количество транзакций на всех страницах. Если 0, транзакций больше нет.

Пример ответа:

```json
{
  "result": {
    "operations": [
      {
        "operation_id": 11401182187840,
        "operation_type": "MarketplaceMarketingActionCostOperation",
        "operation_date": "2021-11-01 00:00:00",
        "operation_type_name": "Услуги продвижения товаров",
        "delivery_charge": 0,
        "return_delivery_charge": 0,
        "accruals_for_sale": 0,
        "sale_commission": 0,
        "amount": -6.46,
        "type": "services",
        "posting": {
          "delivery_schema": "",
          "order_date": "",
          "posting_number": "",
          "warehouse_id": 0
        },
        "items": [],
        "services": []
      }
    ],
    "page_count": 1,
    "row_count": 355
  }
}
```

---

## Суммы транзакций

`POST /v3/finance/transaction/totals`

Operation ID: `FinanceAPI_FinanceTransactionTotalV3`

Метод устаревает и будет отключён 6 июля 2026 года. Переключитесь на /v1/finance/accrual/postings , /v1/finance/accrual/types , /v1/finance/accrual/by-day . Данные могут не соответствовать информации в личном кабинете. Возвращает итоговые суммы по транзакциям за указанный период. Если вы неправильно заполните номера отправлений, в ответе вернутся нулевые значения.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date` — object Фильтр по дате.
- `posting_number required` — string Номер отправления.
- `transaction_type` — string Тип операции: all — все, orders — заказы, returns — возвраты и отмены, services — сервисные сборы, compensation — компенсация, transferDelivery — стоимость доставки, other — прочее.

Пример запроса:

```json
{
  "date": {
    "from": "2021-11-01T00:00:00.000Z",
    "to": "2021-11-02T00:00:00.000Z"
  },
  "posting_number": "",
  "transaction_type": "all"
}
```

### Ответы

- 200 Суммы транзакций
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `accruals_for_sale` — number <double> Общая стоимость товаров и возвратов в заданный период.
  - `compensation_amount` — number <double> Компенсации.
  - `money_transfer` — number <double> Начисления за доставку и возвраты при работе по схеме «Доставка по выбору продавца».
  - `others_amount` — number <double> Прочие начисления.
  - `processing_and_delivery` — number <double> Стоимость услуг обработки отправлений, сборки заказов, магистрали и последней мили, а также доставки до введения новых комиссий и тарифов с 1 февраля 2021 года. Магистраль — доставка товаров между кластерами. Последняя миля — доставка товаров покупателю в пункт выдачи заказов, постамат или курьером.
  - `refunds_and_cancellations` — number <double> Стоимость обратной магистрали, обработки возвратов, отмен и невыкупа товара, а также возвратов до введения новых комиссий и тарифов с 1 февраля 2021 года. Магистраль — доставка товаров между кластерами. Последняя миля — доставка товаров покупателю в пункт выдачи заказов, постамат или курьером.
  - `sale_commission` — number <double> Сумма комиссии, которая была удержана при продаже товара и возвращена при его возврате.
  - `services_amount` — number <double> Стоимость дополнительных услуг, не связанных напрямую с доставками и возвратами товаров. Например, продвижения или размещения товаров.

Пример ответа:

```json
{
  "result": {
    "accruals_for_sale": 96647.58,
    "sale_commission": -11456.65,
    "processing_and_delivery": -24405.68,
    "refunds_and_cancellations": -330,
    "services_amount": -1307.57,
    "compensation_amount": 0,
    "money_transfer": 0,
    "others_amount": 113.05
  }
}
```

---

## Реестр продаж юридическим лицам

`POST /v1/finance/document-b2b-sales`

Operation ID: `ReportAPI_CreateDocumentB2BSalesReport`

Используйте метод, чтобы получить отчёт по продажам юридическим лицам. Соответствует разделу Финансы → Документы → Реестр продаж юр. лицам в личном кабинете.

### Тело запроса (application/json)

- `date required` — string Отчётный период в формате YYYY-MM .
- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.

Пример запроса:

```json
{
  "date": "string",
  "language": "DEFAULT"
}
```

### Ответы

- 200 Результат запроса

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---

## Реестр продаж юридическим лицам в JSON-формате

`POST /v1/finance/document-b2b-sales/json`

Operation ID: `ReportAPI_CreateDocumentB2BSalesJSONReport`

Используйте метод, чтобы получить отчёт по продажам юридическим лицам в JSON-формате. Соответствует разделу Финансы → Документы → Реестр продаж юр. лицам в личном кабинете.

### Тело запроса (application/json)

- `date required` — string Отчётный период в формате YYYY-MM . Отчёт доступен до января 2019 включительно.

### Ответы

- 200 Отчёт в JSON-формате
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `date_from` — string Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to` — string Дата окончания отчётного периода в формате YYYY-MM-DD .
- `invoices` — Array of objects Список счетов-фактур.
- `seller_info` — object Информация о продавце.

Пример ответа:

```json
{
  "date_from": "string",
  "date_to": "string",
  "invoices": [
    {
      "buyer_info": {
        "name": "string",
        "address": "string",
        "inn": "string",
        "kpp": "string"
      },
      "currency": "string",
      "currency_code": 0,
      "info": {
        "date": "string",
        "number": "string",
        "status": "string",
        "type": "UPD"
      },
      "offer_id": "string",
      "operations": [
        {
          "amount": 0,
          "cost_without_vat": 0,
          "date": "string",
          "gtd_number": "string",
          "origin_country": "string",
          "posting_number": "string",
          "price": 0,
          "quantity": 0,
          "rnpt_number": "string",
          "type": "DELIVERY",
          "vat_amount": 0,
          "vat_rate": 0
        }
      ],
      "product_name": "string",
      "sku": 0,
      "unit_code": 0,
      "unit_name": "string"
    }
  ],
  "seller_info": {
    "company_name": "string",
    "inn": "string",
    "kpp": "string"
  }
}
```

---

## Отчёт о взаиморасчётах

`POST /v1/finance/mutual-settlement`

Operation ID: `ReportAPI_CreateMutualSettlementReport`

Используйте метод, чтобы получить отчёт о взаиморасчетах. Соответствует разделу Финансы → Документы → Аналитические отчеты → Отчет о взаиморасчетах в личном кабинете.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date required` — string Отчётный период в формате YYYY-MM .
- `language` — string Default: "DEFAULT" Язык ответа: RU — русский, EN — английский.

Пример запроса:

```json
{
  "date": "string",
  "language": "DEFAULT"
}
```

### Ответы

- 200 Результат запроса

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `code` — string Уникальный идентификатор отчёта. По нему вы можете получить отчёт в течение 3 дней после запроса. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---

## Отчёт о выкупленных товарах

`POST /v1/finance/products/buyout`

Operation ID: `GetFinanceProductsBuyout`

Возвращает отчёт о товарах, которые выкупил Ozon для продажи в ЕАЭС и другие страны. Соответствует разделу Финансы → Документы → УПД по сделкам с юр. лицами → УПД по выкупленным товарам в личном кабинете. Подробнее о продаже товаров в ЕАЭС и другие страны в Базе знаний

### Тело запроса (application/json)

- `date_from required` — string Дата, с которой будут данные в отчёте.
- `date_to required` — string Дата, по которую будут данные в отчёте. Максимальный период — 31 день.

Пример запроса:

```json
{
  "date_from": "YYYY-MM-DD",
  "date_to": "YYYY-MM-DD"
}
```

### Ответы

- 200 Отчёт по выкупленным товарам

#### Схема ответа (200)

- `products` — Array of objects Список выкупленных товаров
  - `amount` — number <float> Сумма к начислению.
  - `buyout_price` — number <float> Цена выкупа товара с НДС.
  - `deduction_by_category_percent` — number <float> Скидка по категории в процентах.
  - `name` — string Название товара.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `posting_number` — string Номер отправления.
  - `quantity` — integer <int32> Количество товара.
  - `seller_price_per_instance` — number <float> Цена продавца с учётом скидки.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `vat_percent` — integer <int32> Ставка НДС для товара в процентах.

Пример ответа:

```json
{
  "products": [
    {
      "name": "Футболка Daniele Patrici",
      "offer_id": "y9604060-S",
      "sku": "985111329",
      "posting_number": "0208194185-0001-1",
      "seller_price_per_instance": 95,
      "deduction_by_category_percent": "3.13",
      "buyout_price": 92.3,
      "vat_percent": 20,
      "quantity": 1,
      "amount": 92.3
    },
    {
      "name": "Сабо T.TACCARDI",
      "offer_id": "W1496390-41",
      "sku": "1505091950",
      "posting_number": "84189625-0009-3",
      "seller_price_per_instance": 2086,
      "deduction_by_category_percent": "3.01",
      "buyout_price": 2023.3,
      "vat_percent": 20,
      "quantity": 1,
      "amount": 2023.3
    }
  ]
}
```

---

## Отчёт о компенсациях

`POST /v1/finance/compensation`

Operation ID: `ReportAPI_GetCompensationReport`

Метод для получения отчёта о компенсациях. Соответствует отчёту из раздела Финансы → Документы → Компенсации и прочие начисления в личном кабинете.

### Тело запроса (application/json)

- `date required` — string Отчётный период в формате YYYY-MM .
- `language` — string Default: "RU" Язык отчёта: RU — русский, EN — английский.

Пример запроса:

```json
{
  "date": "2023-09",
  "language": "RU"
}
```

### Ответы

- 200 Отчёт о компенсациях

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `code` — string Уникальный идентификатор отчёта. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---

## Отчёт о декомпенсациях

`POST /v1/finance/decompensation`

Operation ID: `ReportAPI_GetDecompensationReport`

Метод для получения отчёта о декомпенсациях. Соответствует отчёту из раздела Финансы → Документы → Компенсации и прочие начисления в личном кабинете.

### Тело запроса (application/json)

- `date required` — string Отчётный период в формате YYYY-MM .
- `language` — string Default: "RU" Язык отчёта: RU — русский, EN — английский.

Пример запроса:

```json
{
  "date": "2023-09",
  "language": "RU"
}
```

### Ответы

- 200 Отчёт о декомпенсациях

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `code` — string Уникальный идентификатор отчёта. Чтобы получить отчёт, передайте это значение в метод /v1/report/info .

---
