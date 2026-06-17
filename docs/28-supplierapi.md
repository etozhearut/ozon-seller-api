# Накладные

_Тег: `SupplierAPI` · операций: 4_

## Создать или изменить счёт-фактуру

`POST /v2/invoice/create-or-update`

Operation ID: `InvoiceAPI_InvoiceCreateOrUpdateV2`

Создание или изменение таможенного счёта-фактуры для возврата НДС продавцам из Турции. Вы можете создать счёт-фактуру только для отправлений в статусах awaiting_approve или awaiting_packaging .

### Тело запроса (application/json)

- `date required` — string <date-time> Дата счёта-фактуры.
- `hs_codes` — Array of objects HS-коды товаров.
- `number` — string Номер счёта-фактуры. Номер может содержать буквы и цифры, максимальная длина — 50 символов.
- `posting_number required` — string Номер отправления.
- `price` — number <double> Стоимость, указанная в счёте-фактуре. Разделитель дробной части — точка, до двух знаков после точки.
- `price_currency` — string Валюта счёта-фактуры: USD — доллар, EUR — евро, TRY — турецкая лира, CNY — юань, RUB — рубль, GBP — фунт стерлингов. Значение по умолчанию — USD .
- `url required` — string Ссылка на счёт-фактуру. Чтобы создать ссылку, используйте метод v1/invoice/file/upload .

Пример запроса:

```json
{
  "HS_code": [
    {
      "sku": "SKU123",
      "code": "534758761999"
    },
    {
      "sku": "SKU456",
      "code": "534758761000"
    },
    {
      "sku": "SKU789",
      "code": "534758761777"
    }
  ],
  "date": "2023-08-01T12:08:44.342Z",
  "number": "424fdsf234",
  "posting_number": "33920146-0252-1",
  "price": 234.34,
  "price_currency": "RUB",
  "url": "https://cdn.ozone.ru/s3/ozon-disk-api/techdoc/seller-api/earsivfatura_1690960445.pdf"
}
```

### Ответы

- 200 Счёт-фактура создана или изменена

#### Схема ответа (200)

- `result` — boolean Результат работы метода.

---

## Загрузка счёта-фактуры

`POST /v1/invoice/file/upload`

Operation ID: `invoice_upload`

Доступные форматы: JPEG и PDF. Максимальный размер файла: 10 МБ.

### Тело запроса (application/json)

- `base64_content required` — string Счёт-фактура в кодировке Base64.
- `posting_number required` — string Номер отправления.

Пример запроса:

```json
{
  "base64_content": "string",
  "posting_number": "string"
}
```

### Ответы

- 200 Ссылка на счёт-фактуру

#### Схема ответа (200)

- `url` — string Ссылка на счёт-фактуру.

---

## Получить информацию о счёте-фактуре

`POST /v2/invoice/get`

Operation ID: `invoice_getV2`

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Информация о счёте-фактуре

#### Схема ответа (200)

- `result` — object Информация о счёте-фактуре.
  - `date` — string <date-time> Дата загрузки счёта-фактуры.
  - `file_url` — string Ссылка на счёт-фактуру.
  - `hs_codes` — Array of objects HS-коды товаров.
  - `number` — string Номер счёта-фактуры.
  - `price` — number <double> Стоимость, указанная в счёте-фактуре. Разделитель дробной части — точка, до двух знаков после точки. Пример: 199.99 .
  - `price_currency` — string Валюта счёта-фактуры: USD — доллар, EUR — евро, TRY — турецкая лира, CNY — юань, RUB — рубль, GBP — фунт стерлингов. Значение по умолчанию — USD .

Пример ответа:

```json
{
  "result": {
    "date": "2019-08-24T14:15:22Z",
    "file_url": "string",
    "hs_codes": [
      {
        "code": "string",
        "sku": "string"
      }
    ],
    "number": "string",
    "price": 0,
    "price_currency": "string"
  }
}
```

---

## Удалить ссылку на счёт-фактуру

`POST /v1/invoice/delete`

Operation ID: `invoice_delete`

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Ссылка удалена

#### Схема ответа (200)

- `result` — boolean Результат работы метода.

---
