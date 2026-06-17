# Сертификаты качества

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `CertificationAPI` · методов: 15_

## Список типов соответствия требованиям (версия 1)

`GET /v1/product/certificate/accordance-types`

Operation ID: `ProductAPI_ProductCertificateAccordanceTypes`

```bash
curl -X GET "https://api-seller.ozon.ru/v1/product/certificate/accordance-types" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Cправочник типов соответствия требованиям
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Список типов и названий сертификатов.
  - `name` — string Название документа.
  - `value` — string Значение справочника.

Пример ответа:
```json
{
  "result": [
    {
      "name": "ГОСТ",
      "value": "gost"
    },
    {
      "name": "Технический регламент РФ",
      "value": "technical_regulations_rf"
    },
    {
      "name": "Технический регламент ТС",
      "value": "technical_regulations_cu"
    }
  ]
}
```

---

## Список типов соответствия требованиям (версия 2)

`GET /v2/product/certificate/accordance-types/list`

Operation ID: `CertificateAccordanceTypes`

```bash
curl -X GET "https://api-seller.ozon.ru/v2/product/certificate/accordance-types/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список типов соответствия требованиям

#### Схема ответа (200)

- `result` — object Список типов соответствия требованиям.
  - `base` — Array of objects Основные типы соответствия требованиям.
  - `hazard` — Array of objects Типов соответствия требованиям, относящиеся к опасным товарам.

Пример ответа:
```json
{
  "result": {
    "base": [
      {
        "title": "ГОСТ",
        "code": "gost"
      },
      {
        "title": "Технический регламент РФ",
        "code": "technical_regulations_rf"
      },
      {
        "title": "Технический регламент ТС",
        "code": "technical_regulations_cu"
      }
    ],
    "hazard": [
      {
        "title": "ПБ химической продукции",
        "code": "chemical_products"
      },
      {
        "title": "Safety Data Sheet (SDS)",
        "code": "safety_data_sheet"
      },
      {
        "title": "Отказное письмо",
        "code": "rejection_letter"
      }
    ]
  }
}
```

---

## Справочник типов документов

`GET /v1/product/certificate/types`

Operation ID: `ProductAPI_ProductCertificateTypes`

```bash
curl -X GET "https://api-seller.ozon.ru/v1/product/certificate/types" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Справочник типов документов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Список типов и названий сертификатов.
  - `name` — string Название документа.
  - `value` — string Значение справочника.

Пример ответа:
```json
{
  "result": [
    {
      "name": "Сертификат соответствия",
      "value": "certificate_of_conformity"
    },
    {
      "name": "Декларация о соответствии",
      "value": "declaration"
    },
    {
      "name": "Свидетельство о регистрации",
      "value": "certificate_of_registration"
    },
    {
      "name": "Регистрационное удостоверение",
      "value": "registration_certificate"
    },
    {
      "name": "Отказное письмо",
      "value": "refused_letter"
    },
    {
      "name": "Ветеринарное свидетельство",
      "value": "veterinary_cover_document"
    },
    {
      "name": "Паспорт безопасности",
      "value": "safety_data_sheet"
    }
  ]
}
```

---

## Список сертифицируемых категорий

`POST /v2/product/certification/list`

Operation ID: `ProductAPI_ProductCertificationList`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/product/certification/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `page required` — integer <int64> Номер страницы.
- `page_size required` — integer <int64> [ 1 .. 1000 ] Количество элементов на странице.

### Ответы

- 200 Список сертифицируемых категорий

#### Схема ответа (200)

- `certification` — Array of objects Информация о сертифицируемых категориях.
- `total` — integer <int64> Всего категорий.

Пример ответа:
```json
{
  "certification": [
    {
      "category_id": 582676,
      "category_name": "Витаминно-минеральные комплексы для взрослых",
      "is_required": true,
      "type_id": 113,
      "type_name": "products"
    }
  ],
  "total": 1
}
```

---

## Список сертифицируемых категорий

`POST /v1/product/certification/list`

Operation ID: `ProductAPI_V1ProductCertificationList`

14 апреля 2025 года метод будет отключён. Переключитесь на /v2/product/certification/list .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certification/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `page` — integer <int32> Номер страницы, возвращаемой в запросе.
- `page_size` — integer <int32> Количество элементов на странице.

### Ответы

- 200 Список сертифицируемых категорий
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `certification` — Array of objects Информация о сертифицируемых категориях.
  - `total` — integer <int64> Всего категорий.

Пример ответа:
```json
{
  "result": {
    "certification": [
      {
        "is_required": true,
        "category_name": "Витаминно-минеральные комплексы для взрослых"
      }
    ],
    "total": 1
  }
}
```

---

## Добавить сертификаты для товаров

`POST /v1/product/certificate/create`

Operation ID: `ProductAPI_ProductCertificateCreate`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `files required` — Array of file Массив сертификатов для товара. Допустимые расширения jpg, jpeg, png, pdf.
- `name required` — string Название сертификата. Максимум 100 символов.
- `number required` — string Номер сертификата. Максимум 100 символов.
- `type_code required` — string Enum: "certificate_of_conformity" "declaration" "certificate_of_registration" "registration_certificate" "refused_letter" "veterinary_cover_document" "safety_data_sheet" Тип сертификата. Чтобы получить доступные типы, используйте метод GET /v1/product/certificate/types .
- `accordance_type_code` — string Enum: "technical_regulations_rf" "technical_regulations_cu" "gost" Тип соответствия требованиям. Чтобы получить доступные типы, используйте метод GET /v1/product/certificate/accordance-types . Параметр обязательный, если type_code = declaration , certificate_of_conformity или safety_data_sheet .
- `issue_date required` — string <date-time> Default: "2021-04-30T11:31:26Z" Дата начала действия сертификата.
- `expire_date` — string <date-time> Дата окончания действия сертификата. Может быть пустым для бессрочных сертификатов. Формат: 2021-04-30T11:31:26Z .

### Ответы

- 200 Идентификатор загруженного сертификата
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Привязать сертификат к товару

`POST /v1/product/certificate/bind`

Operation ID: `ProductAPI_ProductCertificateBind`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/bind" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "certificate_id": 50058,
  "product_id": [
    290
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `certificate_id required` — integer <int64> Идентификатор сертификата, который был присвоен при его загрузке.
- `product_id required` — Array of integers <int64> Массив идентификаторов товаров, к которым относится этот сертификат.

### Ответы

- 200 Сертификат привязан к товару
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнен без ошибок.

---

## Удалить сертификат

`POST /v1/product/certificate/delete`

Operation ID: `CertificateDelete`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `certificate_id required` — integer <int32> Идентификатор сертификата.

### Ответы

- 200 Результат удаления сертификата

#### Схема ответа (200)

- `result` — object Результат удаления сертификата.
  - `is_delete` — boolean Удалён ли сертификат: true — удалён, false — не удалён.
  - `error_message` — string Описание ошибок при удалении сертификата.

Пример ответа:
```json
{
  "result": {
    "is_delete": true,
    "error_message": "string"
  }
}
```

---

## Информация о сертификате

`POST /v1/product/certificate/info`

Operation ID: `CertificateInfo`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "certificate_number": "2312342134123"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `certificate_number required` — string Идентификатор сертификата.

### Ответы

- 200 Информация о сертификате

#### Схема ответа (200)

- `result` — object Информация о сертификате.
  - `certificate_id` — integer <int32> Идентификатор.
  - `certificate_number` — string Номер.
  - `certificate_name` — string Название.
  - `type_code` — string Тип.
  - `status_code` — string Статус.
  - `accordance_type_code` — string Тип соответствия требованиям.
  - `rejection_reason_code` — string Причина отклонения сертификата.
  - `verification_comment` — string Комментарий модератора.
  - `issue_date` — string <date-time> Дата создания.
  - `expire_date` — string <date-time> Дата окончания действия.
  - `products_count` — integer <int32> Количество товаров, привязанных к сертификату.

Пример ответа:
```json
{
  "result": {
    "certificate_id": 54321,
    "certificate_number": "CERT-2026-MSK-789456",
    "certificate_name": "Сертификат безопасности продукции",
    "type_code": "SAFETY_2026",
    "status_code": "PENDING",
    "accordance_type_code": "PARTIAL",
    "rejection_reason_code": "DOCS_MISSING",
    "verification_comment": "Требуются дополнительные документы от производителя для полного подтверждения",
    "issue_date": "2025-11-01T08:00:00Z",
    "expire_date": "2027-11-01T08:00:00Z",
    "products_count": 15
  }
}
```

---

## Список сертификатов

`POST /v1/product/certificate/list`

Operation ID: `CertificateList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "offer_id": "OFFER-2026-001",
  "status": "approved",
  "type": "certificate_of_conformity,declaration",
  "page": 1,
  "page_size": 50
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `offer_id` — string Идентификатор товара в системе продавца — артикул, привязанный к сертификату. Передайте параметр, если нужны сертификаты, к которым привязаны определённые товары.
- `status` — string Статус сертификата. Передайте параметр, если нужны сертификаты с определённым статусом.
- `type` — string Тип сертификата. Передайте параметр, если нужны сертификаты с определённым типом.
- `page required` — integer <int32> Страница, с которой следует выводить список. Минимальное значение — 1.
- `page_size required` — integer <int32> Количество объектов на странице. Значение — от 1 до 1000.

### Ответы

- 200 Список сертификатов

#### Схема ответа (200)

- `result` — object Список сертификатов.
  - `certificates` — Array of objects Информация о сертификате.
  - `page_count` — integer <int32> Количество страниц.

Пример ответа:
```json
{
  "result": {
    "certificates": [
      {
        "certificate_id": 1120593,
        "certificate_name": "Test comment DS. Post-script prod2stg",
        "certificate_number": "ЕАЭС RU С-CN.НВ46.В.00413/21",
        "type_code": "certificate_of_conformity",
        "status_code": "approved",
        "accordance_type_code": "technical_regulations_cu",
        "rejection_reason_code": "",
        "issue_date": "2021-08-12T03:00:00Z",
        "expire_date": "2024-08-11T03:00:00Z",
        "products_count": 0,
        "verification_comment": ""
      },
      {
        "certificate_id": 624047,
        "certificate_name": "Test comment DS. Post-script prod2stg",
        "certificate_number": "ЕАЭС N RU Д-CN.НВ25.В.11796/20",
        "type_code": "declaration",
        "status_code": "approved",
        "accordance_type_code": "technical_regulations_cu",
        "rejection_reason_code": "",
        "issue_date": "2020-07-03T03:00:00Z",
        "expire_date": "2023-07-02T03:00:00Z",
        "products_count": 1,
        "verification_comment": ""
      }
    ],
    "page_count": 7
  }
}
```

---

## Список возможных статусов товаров

`POST /v1/product/certificate/product_status/list`

Operation ID: `ProductStatusList`

Метод для получения списка возможных статусов товаров при их привязке к сертификату.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/product_status/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список статусов товаров

#### Схема ответа (200)

- `result` — Array of objects Список статусов товаров.
  - `code` — string Код статуса товара при привязке к сертификату.
  - `name` — string Описание статуса.

Пример ответа:
```json
{
  "result": [
    {
      "code": "approved",
      "name": "Одобрен"
    },
    {
      "code": "declined",
      "name": "Отклонён"
    },
    {
      "code": "awaiting_verification",
      "name": "На проверке"
    }
  ]
}
```

---

## Список товаров, привязанных к сертификату

`POST /v1/product/certificate/products/list`

Operation ID: `CertificateProductsList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/products/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "certificate_id": 624047,
  "product_status_code": "approved",
  "page": 1,
  "page_size": 50
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `certificate_id required` — integer <int32> Идентификатор сертификата.
- `product_status_code` — string Статус проверки товара при привязке к сертификату.
- `page required` — integer <int32> Номер страницы, с которой выводить список. Минимальное значение — 1.
- `page_size required` — integer <int32> Количество объектов на странице. Значение — от 1 до 1000.

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `result` — object Товары, привязанные к сертификату.
  - `items` — Array of objects Список товаров.
  - `count` — integer <int64> Количество найденных товаров.

Пример ответа:
```json
{
  "result": {
    "items": [
      {
        "product_id": 53110101,
        "product_status_code": "approved"
      }
    ],
    "count": 1
  }
}
```

---

## Отвязать товар от сертификата

`POST /v1/product/certificate/unbind`

Operation ID: `CertificateUnbind`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/unbind" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "certificate_id": 624047,
  "product_id": [
    "53110101"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `certificate_id required` — integer <int32> Идентификатор сертификата.
- `product_id required` — Array of strings <int64> Список идентификаторов товара, которые нужно отвязать от сертификата.

### Ответы

- 200 Товар отвязан от сертификата

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `error` — string Сообщение об ошибке при отвязывании товара.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `updated` — boolean Был ли отвязан товар от сертификата: true — отвязан, false — не отвязан.

Пример ответа:
```json
{
  "result": [
    {
      "product_id": 53110101,
      "updated": true,
      "error": ""
    }
  ]
}
```

---

## Возможные причины отклонения сертификата

`POST /v1/product/certificate/rejection_reasons/list`

Operation ID: `RejectionReasonsList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/rejection_reasons/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Причины отклонения сертификата

#### Схема ответа (200)

- `result` — Array of objects Причины отклонения сертификата.
  - `code` — string Код причины отклонения сертификата.
  - `name` — string Описание причины отклонения сертификата.

Пример ответа:
```json
{
  "result": [
    {
      "code": "incorrect_type",
      "name": "Необходим другой тип документа"
    },
    {
      "code": "not_all_pages",
      "name": "Предоставлены не все страницы документа"
    },
    {
      "code": "not_in_registry",
      "name": "Документа нет в едином реестре"
    },
    {
      "code": "not_match_information",
      "name": "Копия документа не соответствует указанной информации"
    },
    {
      "code": "not_signed",
      "name": "Документ не подписан или отсутствует печать"
    },
    {
      "code": "not_true",
      "name": "Информация в документе не соответствует действительности"
    },
    {
      "code": "not_valid_in_rf",
      "name": "Документ не действует на территории РФ"
    },
    {
      "code": "bad_quality",
      "name": "Качество копии не позволяет разобрать текст"
    },
    {
      "code": "annulled",
      "name": "Документ аннулирован в реестре"
    },
    {
      "code": "archive",
      "name": "Документ перенесен в архив"
    },
    {
      "code": "expired_document",
      "name": "Срок действия документа истек"
    }
  ]
}
```

---

## Возможные статусы сертификатов

`POST /v1/product/certificate/status/list`

Operation ID: `CertificateStatusList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/certificate/status/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Возможные статусы сертификатов

#### Схема ответа (200)

- `result` — Array of objects Список возможных статусов сертификатов.
  - `code` — string Код статуса сертификата.
  - `name` — string Описание статуса.

Пример ответа:
```json
{
  "result": [
    {
      "code": "approved",
      "name": "Одобрены"
    },
    {
      "code": "awaiting_verification",
      "name": "Ожидают проверки"
    },
    {
      "code": "declined",
      "name": "Отклонены"
    },
    {
      "code": "verification",
      "name": "На проверке"
    }
  ]
}
```

---
