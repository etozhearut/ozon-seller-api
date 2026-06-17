# Штрихкоды товаров

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `BarcodeAPI` · методов: 2_

## Привязать штрихкод к товару

`POST /v1/barcode/add`

Operation ID: `add-barcode`

Если у товара есть штрихкод, который не указан в системе Ozon, привяжите его с помощью этого метода. Чтобы сгенерировать штрихкод, используйте метод /v1/barcode/generate . Подробнее о требованиях к штрихкодам в Базе знаний продавца На одном товаре может быть до 100 штрихкодов. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/barcode/add" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "barcodes": [
    {
      "barcode": "112772873170",
      "sku": "99887766"
    }
  ]
}'
```

### Тело запроса

- `barcodes required` — Array of objects <= 100 items Список штрихкодов и товаров.

### Ответы

- 200 Штрихкод привязан

#### Схема ответа (200)

- `errors` — Array of objects Список ошибок.
  - `code` — string Код ошибки.
  - `error` — string Описание ошибки.
  - `barcode` — string Штрихкод, который не удалось привязать.
  - `sku` — integer <int64> Идентификатор товара, к которому не удалось привязать штрихкод.

Пример ответа:
```json
{
  "errors": [
    {
      "code": "123-123",
      "error": "does not exist",
      "barcode": "112772873170",
      "sku": 99887766
    }
  ]
}
```

---

## Создать штрихкод для товара

`POST /v1/barcode/generate`

Operation ID: `generate-barcode`

Если у товара нет штрихкода, вы можете создать его с помощью этого метода. Если штрихкод уже есть, но он не указан в системе Ozon, вы можете привязать его через метод /v1/barcode/add . За один запрос вы можете создать штрихкоды не больше чем для 100 товаров. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/barcode/generate" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `product_ids required` — Array of strings <int64> Идентификаторы товаров, для которых нужно создать штрихкод.

### Ответы

- 200 Штрихкод создан

#### Схема ответа (200)

- `errors` — Array of objects Ошибки при создании штрихкода.
  - `code` — string Код ошибки.
  - `error` — string Описание ошибки.
  - `barcode` — string Штрихкод, при создании которого произошла ошибка.
  - `product_id` — integer <int64> Идентификатор товара, для которого не удалось создать штрихкод.

Пример ответа:
```json
{
  "errors": [
    {
      "code": "123-123",
      "error": "does not exist",
      "barcode": "112772873170",
      "product_id": 9988776612
    }
  ]
}
```

---
