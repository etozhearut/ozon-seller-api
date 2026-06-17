# Возвратные отгрузки

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `ReturnAPI` · методов: 8_

## Количество возвратов FBS

`POST /v1/returns/company/fbs/info`

Operation ID: `returnsCompanyFBSInfo`

Метод для получения информации о возвратах FBS и их количестве.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/returns/company/fbs/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "place_id": 0
  },
  "pagination": {
    "last_id": 0,
    "limit": 500
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтры.
- `pagination required` — object Разделение ответа метода.

### Ответы

- 200 Количество возвратов FBS

#### Схема ответа (200)

- `drop_off_points` — Array of objects Информация о drop-off пунктах.
- `has_next` — boolean Признак, есть ли ещё пункты, где продавца ожидают возвраты.

Пример ответа:
```json
{
  "drop_off_points": [
    {
      "address": "string",
      "box_count": 0,
      "id": 0,
      "name": "string",
      "pass_info": {
        "count": 0,
        "is_required": true
      },
      "place_id": 0,
      "returns_count": 0,
      "utc_offset": "string",
      "warehouses_ids": [
        "string"
      ]
    }
  ],
  "has_next": true
}
```

---

## Проверить возможность получения возвратных отгрузок по штрихкоду

`POST /v1/return/giveout/is-enabled`

Operation ID: `ReturnAPI_GiveoutIsEnabled`

Если у вас есть доступ, в параметре enabled будет указано значение true .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/is-enabled" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Результат проверки

#### Схема ответа (200)

- `enabled` — boolean true , если вы можете получить возвратную отгрузку по штрихкоду.

---

## Список возвратных отгрузок

`POST /v1/return/giveout/list`

Operation ID: `ReturnAPI_GiveoutList`

Метод для получения списка активных возвратов. Возвратная отгрузка становится активной после сканирования штрихкода. После сканирования штрихкода второй раз активная выдача переходит в статус неактивной.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `last_id` — integer <int64> Идентификатор последнего значения на странице.
- `limit required` — integer <int64> Количество элементов в ответе.

### Ответы

- 200 Список возвратных отгрузок

#### Схема ответа (200)

- `giveouts` — Array of objects Идентификатор отгрузки.
  - `approved_articles_count` — integer <int32> Количество товаров в отгрузке.
  - `created_at` — string <date-time> Дата и время.
  - `giveout_id` — integer <int64> Идентификатор отгрузки.
  - `giveout_status` — string Статусы возвратной отгрузки: GIVEOUT_STATUS_UNSPECIFIED — не определён, напишите в поддержку. GIVEOUT_STATUS_CREATED — создана. GIVEOUT_STATUS_APPROVED — одобрена. GIVEOUT_STATUS_COMPLETED — завершена. GIVEOUT_STATUS_CANCELLED — отменена.
  - `total_articles_count` — integer <int32> Общее количество товаров, которые нужно забрать со склада.
  - `warehouse_address` — string Адрес склада.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `warehouse_name` — string Название склада.

Пример ответа:
```json
{
  "giveouts": [
    {
      "approved_articles_count": 0,
      "created_at": "2019-08-24T14:15:22Z",
      "giveout_id": 0,
      "giveout_status": "string",
      "total_articles_count": 0,
      "warehouse_address": "string",
      "warehouse_id": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Информация о возвратной отгрузке

`POST /v1/return/giveout/info`

Operation ID: `ReturnAPI_GiveoutInfo`

Метод для получения информации о возвратной отгрузке. В параметр giveout_id передаётся значение, полученное в методе /v1/return/giveout/list .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `giveout_id required` — integer <int64> Идентификатор отгрузки.

### Ответы

- 200 Информация о возвратной отгрузке

#### Схема ответа (200)

- `articles` — Array of objects Артикулы товаров.
- `giveout_id` — integer <int64> Идентификатор отгрузки.
- `giveout_status` — string Статусы возвратной отгрузки: GIVEOUT_STATUS_UNSPECIFIED — не определён, напишите в поддержку. GIVEOUT_STATUS_CREATED — создана. GIVEOUT_STATUS_APPROVED — одобрена. GIVEOUT_STATUS_COMPLETED — завершена. GIVEOUT_STATUS_CANCELLED — отменена.
- `warehouse_address` — string Адрес склада.
- `warehouse_name` — string Название склада.

Пример ответа:
```json
{
  "articles": [
    {
      "approved": true,
      "delivery_schema": "string",
      "name": "string",
      "seller_id": 0
    }
  ],
  "giveout_id": 0,
  "giveout_status": "string",
  "warehouse_address": "string",
  "warehouse_name": "string"
}
```

---

## Значение штрихкода для возвратных отгрузок

`POST /v1/return/giveout/barcode`

Operation ID: `ReturnAPI_GiveoutGetBarcode`

Используйте этот метод, чтобы получить штрихкод из ответа методов /v1/return/giveout/get-png и /v1/return/giveout/get-pdf в текстовом виде.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/barcode" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Значение штрихкода

#### Схема ответа (200)

- `barcode` — string Значение штрихкода в текстовом виде.

---

## Штрихкод для получения возвратной отгрузки в формате PDF

`POST /v1/return/giveout/get-pdf`

Operation ID: `ReturnAPI_GiveoutGetPDF`

Возвращает PDF-файл со штрихкодом. Метод работает только для схемы FBS.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/get-pdf" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Штрихкод для возвратной отгрузки

#### Схема ответа (200)

- `file_content` — string PDF-файл со штрихкодом в кодировке Base64.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:
```json
{
  "content_type": "application/pdf",
  "file_name": "string",
  "file_content": "string"
}
```

---

## Штрихкод для получения возвратной отгрузки в формате PNG

`POST /v1/return/giveout/get-png`

Operation ID: `ReturnAPI_GiveoutGetPNG`

Возвращает PNG-файл со штрихкодом.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/get-png" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Штрихкод для возвратной отгрузки

#### Схема ответа (200)

- `file_content` — string PNG-файл со штрихкодом в кодировке Base64.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:
```json
{
  "content_type": "image/png",
  "file_name": "string",
  "file_content": "string"
}
```

---

## Сгенерировать новый штрихкод

`POST /v1/return/giveout/barcode-reset`

Operation ID: `ReturnAPI_GiveoutBarcodeReset`

Используйте метод, если ваш штрихкод попал в посторонние руки. Метод возвращает PNG-файл с новым штрихкодом. После использования метода вы не сможете получить возвратную отгрузку по старым штрихкодам. Чтобы получить новый штрихкод в PDF-формате, запросите его методом /v1/return/giveout/get-pdf .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/return/giveout/barcode-reset" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Новый штрихкод

#### Схема ответа (200)

- `file_content` — string Изображение со штрихкодом в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:
```json
{
  "content_type": "image/png",
  "file_name": "string",
  "file_content": "string"
}
```

---
