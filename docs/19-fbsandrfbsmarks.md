# Управление кодами маркировки и сборкой заказов для FBS/rFBS

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FBSandrFBSMarks` · методов: 7_

Управление кодами маркировки и сборкой заказов для FBS/rFBS Коды маркировки товара в системе «Честный ЗНАК» передаются с криптохвостом. Проверьте, что в запросе коды маркировки сериализованы в соответствии с пунктом 7 стандарта RFC 8259 . Символ <GS> должен кодироваться как последовательность \u001d . Двойное экранирование в виде \\u001d не допускается. …

## Собрать заказ (версия 4)

`POST /v4/posting/fbs/ship`

Operation ID: `PostingAPI_ShipFbsPostingV4`

Ответ с кодом 200 не гарантирует успешную сборку заказа. Используйте метод /v3/posting/fbs/get , чтобы проверить, что заказ собран. Если в ответе указан result.substatus = ship_failed , повторите сборку заказа. Делит заказ на отправления и переводит его в статус awaiting_deliver . …

```bash
curl -X POST "https://api-seller.ozon.ru/v4/posting/fbs/ship" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "packages": [
    {
      "products": [
        {
          "product_id": 185479045,
          "quantity": 1
        }
      ]
    }
  ],
  "posting_number": "89491381-0072-1",
  "with": {
    "additional_data": true
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `packages required` — Array of objects Список упаковок. Каждая упаковка содержит список отправлений, на которые делится заказ.
  - `products required` — Array of objects Список товаров в отправлении.
    - `product_id required` — integer <int64> Идентификатор товара в системе Ozon — SKU.
    - `quantity required` — integer <int32> Количество экземпляров.
- `posting_number required` — string Номер отправления.
- `with` — object Дополнительная информация.
  - `additional_data` — boolean Чтобы получить дополнительную информацию, передайте true .

### Ответы

- 200 Результат сборки заказа

#### Схема ответа (200)

- `additional_data` — Array of objects Дополнительная информация об отправлениях.
- `result` — Array of strings Результат сборки отправлений.

Пример ответа:
```json
{
  "additional_data": [
    {
      "posting_number": "89491381-0072-1",
      "products": [
        {
          "currency_code": "RUB",
          "mandatory_mark": [
            "123"
          ],
          "name": "string",
          "offer_id": "17125",
          "price": "2000",
          "quantity": 1,
          "sku": 149618972
        }
      ]
    }
  ],
  "result": [
    "89491381-0072-1"
  ]
}
```

---

## Частичная сборка отправления (версия 4)

`POST /v4/posting/fbs/ship/package`

Operation ID: `PostingAPI_ShipFbsPostingPackage`

Ответ с кодом 200 не гарантирует успешную сборку отправления. Используйте метод /v3/posting/fbs/get , чтобы проверить, что отправление собрано. Если в ответе указан result.substatus = ship_failed , повторите сборку отправления. …

```bash
curl -X POST "https://api-seller.ozon.ru/v4/posting/fbs/ship/package" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "string",
  "products": [
    {
      "exemplarsIds": [
        "string"
      ],
      "product_id": 0,
      "quantity": 0
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.
- `products` — Array of objects Список товаров в отправлении.
  - `exemplarsIds` — Array of strings <int64> Идентификаторы экземпляров товара.
  - `product_id required` — integer <int64> Идентификатор товара в системе продавца — SKU.
  - `quantity required` — integer <int32> Количество экземпляров.

### Ответы

- 200 Результат сборки отправления

#### Схема ответа (200)

- `result` — string Номера отправлений, сформированные после сборки.

---

## Проверить и сохранить данные экземпляров

`POST /v6/fbs/posting/product/exemplar/set`

Operation ID: `PostingAPI_FbsPostingProductExemplarSetV6`

Асинхронный метод: для проверки наличия экземпляров в обороте в системе «Честный ЗНАК»; для сохранения данных экземпляров. Чтобы получить результаты проверок, используйте метод /v5/fbs/posting/product/exemplar/status . …

```bash
curl -X POST "https://api-seller.ozon.ru/v6/fbs/posting/product/exemplar/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "multi_box_qty": 0,
  "posting_number": "string",
  "products": [
    {
      "exemplars": [
        {
          "exemplar_id": 0,
          "gtd": "string",
          "is_gtd_absent": true,
          "is_rnpt_absent": true,
          "marks": [
            {
              "mark": "string",
              "mark_type": "string"
            }
          ],
          "rnpt": "string",
          "weight": 0
        }
      ],
      "product_id": 0
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `multi_box_qty` — integer <int32> Количество коробок, в которые упакован товар.
- `posting_number required` — string Номер отправления.
- `products required` — Array of objects Список товаров.

### Ответы

- 200 Запрос обработан

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

## Получить данные созданных экземпляров

`POST /v6/fbs/posting/product/exemplar/create-or-get`

Operation ID: `PostingAPI_FbsPostingProductExemplarCreateOrGetV6`

Метод для получения информации по экземплярам товаров из отправления, переданных в методе /v6/fbs/posting/product/exemplar/set . Используйте метод для получения exemplar_id .

```bash
curl -X POST "https://api-seller.ozon.ru/v6/fbs/posting/product/exemplar/create-or-get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Данные экземпляров

#### Схема ответа (200)

- `multi_box_qty` — integer <int32> Количество коробок, в которые упакован товар.
- `posting_number` — string Номер отправления.
- `products` — Array of objects Список товаров.

Пример ответа:
```json
{
  "multi_box_qty": 0,
  "posting_number": "string",
  "products": [
    {
      "exemplars": [
        {
          "exemplar_id": 0,
          "gtd": "string",
          "is_gtd_absent": true,
          "is_rnpt_absent": true,
          "marks": [
            {
              "mark": "string",
              "mark_type": "string"
            }
          ],
          "rnpt": "string",
          "weight": 0
        }
      ],
      "has_imei": true,
      "is_gtd_needed": true,
      "is_jw_uin_needed": true,
      "is_mandatory_mark_needed": true,
      "is_mandatory_mark_possible": true,
      "is_rnpt_needed": true,
      "product_id": 0,
      "quantity": 0,
      "is_weight_needed": true,
      "weight_max": 0,
      "weight_min": 0
    }
  ]
}
```

---

## Получить статус добавления экземпляров

`POST /v5/fbs/posting/product/exemplar/status`

Operation ID: `PostingAPI_FbsPostingProductExemplarStatusV5`

Метод для получения статусов добавления экземпляров, переданных в методе /v6/fbs/posting/product/exemplar/set . Также возвращает данные по этим экземплярам.

```bash
curl -X POST "https://api-seller.ozon.ru/v5/fbs/posting/product/exemplar/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Статусы проверки экземпляров

#### Схема ответа (200)

- `posting_number` — string Номер отправления.
- `products` — Array of objects Список товаров.
- `status` — string Статус проверки всех экземпляров и доступности сборки: ship_available — сборка доступна; ship_not_available — сборка недоступна; validation_in_process — экземпляры на проверке; update_available — редактирование информации об экземплярах доступно; update_not_available — редактирование информации об экземплярах недоступно.

Пример ответа:
```json
{
  "posting_number": "string",
  "products": [
    {
      "exemplars": [
        {
          "exemplar_id": 0,
          "gtd": "string",
          "gtd_check_status": "string",
          "gtd_error_codes": [
            "string"
          ],
          "is_gtd_absent": true,
          "is_rnpt_absent": true,
          "marks": [
            {
              "check_status": "string",
              "error_codes": [
                "string"
              ],
              "mark": "string",
              "mark_type": "string"
            }
          ],
          "rnpt": "string",
          "rnpt_check_status": "string",
          "rnpt_error_codes": [
            "string"
          ],
          "weight": 0,
          "weight_check_status": "string",
          "weight_error_codes": [
            "string"
          ]
        }
      ],
      "product_id": 0
    }
  ],
  "status": "string"
}
```

---

## Валидация кодов маркировки

`POST /v5/fbs/posting/product/exemplar/validate`

Operation ID: `PostingAPI_FbsPostingProductExemplarValidateV5`

Метод для проверки кодов на соответствие требованиям системы «Честный ЗНАК» по количеству и составу символов, а также других маркировок. Если у вас нет номера грузовой таможенной декларации (ГТД), вы можете его не указывать.

```bash
curl -X POST "https://api-seller.ozon.ru/v5/fbs/posting/product/exemplar/validate" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "string",
  "products": [
    {
      "exemplars": [
        {
          "gtd": "string",
          "marks": [
            {
              "mark": "string",
              "mark_type": "string"
            }
          ],
          "rnpt": "string",
          "weight": 0
        }
      ],
      "product_id": 0
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.
- `products required` — Array of objects Список товаров.

### Ответы

- 200 Результат валидации

#### Схема ответа (200)

- `products` — Array of objects Список товаров.
  - `error` — string Код ошибки.
  - `exemplars` — Array of objects Информация об экземплярах.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `valid` — boolean Результат прохождения проверки. true , если коды всех экземпляров соответствуют требованиям.

Пример ответа:
```json
{
  "products": [
    {
      "error": "string",
      "exemplars": [
        {
          "errors": [
            "string"
          ],
          "gtd": "string",
          "marks": [
            {
              "errors": [
                "string"
              ],
              "mark": "string",
              "mark_type": "string",
              "valid": true
            }
          ],
          "rnpt": "string",
          "valid": true,
          "weight": 0
        }
      ],
      "product_id": 0,
      "valid": true
    }
  ]
}
```

---

## Обновить данные экземпляров

`POST /v1/fbs/posting/product/exemplar/update`

Operation ID: `PostingAPI_FbsPostingProductExemplarUpdate`

Используйте метод после передачи информации по экземплярам методом /v6/fbs/posting/product/exemplar/set , чтобы сохранить обновлённые данные по экземплярам для отправлений в статусе «Ожидает отгрузки».

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbs/posting/product/exemplar/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Данные обновлены

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
