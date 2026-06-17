# Работа с FBP-черновиками c доставкой drop-off

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `DraftDropOffFBP` · методов: 8_

## Создать черновик для доставки в drop-off пункт

`POST /v1/fbp/draft/drop-off/create`

Operation ID: `FbpDraftDropOffCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "bundle_id": "string",
  "delivery_details": {
    "drop_off_date": "string",
    "drop_off_point_id": 0,
    "drop_off_province_uuid": "string"
  },
  "package_units_count": 0,
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `bundle_id required` — string Идентификатор провалидированного списка товаров.
- `delivery_details required` — object Детали доставки.
- `package_units_count required` — integer <int32> Количество грузомест.
- `warehouse_id required` — integer <int64> Идентификатор склада продавца.

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id` — string Идентификатор заявки на поставку.

Пример ответа:
```json
{
  "draft_id": 0,
  "row_version": 0,
  "supply_id": "string"
}
```

---

## Удалить черновик для доставки в drop-off пункт

`POST /v1/fbp/draft/drop-off/delete`

Operation ID: `FbpDraftDropOffDelete`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_id required` — string Идентификатор заявки на поставку.

### Ответы

- 200 Черновик удалён

#### Схема ответа (200)

- `cancellation_state` — object Статус отмены.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:
```json
{
  "cancellation_state": {
    "cancellation_error": {
      "error_code": "NO_RESPONSE_FROM_3PF",
      "message": "string"
    },
    "cancellation_status": "CONFIRMATION"
  },
  "row_version": 0
}
```

---

## Отредактировать детали доставки для drop-off черновика

`POST /v1/fbp/draft/drop-off/dlv/edit`

Operation ID: `FbpDraftDropOffDlvEdit`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/dlv/edit" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "drop_off_date": "string",
  "drop_off_point_id": 0,
  "drop_off_province_uuid": "string",
  "row_version": 0,
  "supply_id": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `drop_off_date required` — string Дата доставки.
- `drop_off_point_id required` — integer <int64> Идентификатор drop-off пункта.
- `drop_off_province_uuid required` — string Уникальный идентификатор провинции.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор заявки на поставку.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

---

## Перевести черновик в действующую поставку

`POST /v1/fbp/draft/drop-off/registrate`

Operation ID: `FbpDraftDropOffRegistrate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/registrate" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "row_version": 0,
  "supply_id": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор заявки на поставку.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `error` — object Ошибка.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:
```json
{
  "error": {
    "bundle_errors": [
      {
        "errors": [
          "BUNDLE_ITEM_ERROR_UNSPECIFIED"
        ],
        "sku": 0
      }
    ],
    "order_error": "ORDER_ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---

## Получить список провинций

`POST /v1/fbp/draft/drop-off/province/list`

Operation ID: `FbpDraftDropOffProvinceList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/province/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Список провинций

#### Схема ответа (200)

- `provinces` — Array of objects Список провинций.
  - `name` — string Название провинции.
  - `points_count` — integer <int32> Количество пунктов на карте.
  - `province_uuid` — string Уникальный идентификатор провинции.

Пример ответа:
```json
{
  "provinces": [
    {
      "name": "string",
      "points_count": 0,
      "province_uuid": "string"
    }
  ]
}
```

---

## Получить список drop-off пунктов в провинции

`POST /v1/fbp/draft/drop-off/point/list`

Operation ID: `FbpDraftDropOffPointList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/point/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "next_page_number": 0,
  "page_size": 0,
  "province_uuid": "string",
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `next_page_number` — integer <int32> Следующий номер страницы.
- `page_size required` — integer <int32> Количество элементов на странице.
- `province_uuid required` — string Уникальный идентификатор провинции.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Список drop-off пунктов

#### Схема ответа (200)

- `drop_off_points` — Array of objects Список drop-off пунктов.
  - `city` — string Город.
  - `drop_off_point_id` — integer <int64> Идентификатор drop-off пункта.
  - `nearest_drop_off_date` — string <date-time> Ближайшая дата отгрузки.
  - `point_address` — string Адрес drop-off пункта.
  - `province_uuid` — string Уникальный идентификатор провинции.

Пример ответа:
```json
{
  "drop_off_points": [
    {
      "city": "string",
      "drop_off_point_id": 0,
      "nearest_drop_off_date": "2019-08-24T14:15:22Z",
      "point_address": "string",
      "province_uuid": "string"
    }
  ]
}
```

---

## Получить расписание работы drop-off пункта

`POST /v1/fbp/draft/drop-off/point/timetable`

Operation ID: `FbpDraftDropOffPointTimetable`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/point/timetable" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "drop_off_point_id": 0,
  "province_uuid": "string",
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `drop_off_point_id required` — integer <int64> Идентификатор drop-off пункта.
- `province_uuid required` — string Уникальный идентификатор провинции.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Расписание работы

#### Схема ответа (200)

- `calendar` — Array of objects Расписание работы drop-off пункта.
  - `calendar_item` — object Расписание работы.
  - `day_of_week` — string Default: "DAY_OF_WEEK_UNSPECIFIED" Enum: "DAY_OF_WEEK_UNSPECIFIED" "MONDAY" "TUESDAY" "WEDNESDAY" "THURSDAY" "FRIDAY" "SATURDAY" "SUNDAY" Дни недели: DAY_OF_WEEK_UNSPECIFIED — не определён, MONDAY — понедельник, TUESDAY — вторник, WEDNESDAY — среда, THURSDAY — четверг, FRIDAY — пятница, SATURDAY — суббота, SUNDAY — воскресенье.

Пример ответа:
```json
{
  "calendar": [
    {
      "calendar_item": {
        "break_hours": {
          "timeslot_end": "string",
          "timeslot_start": "string"
        },
        "is_holiday": true,
        "opening_hours": {
          "timeslot_end": "string",
          "timeslot_start": "string"
        }
      },
      "day_of_week": "DAY_OF_WEEK_UNSPECIFIED"
    }
  ]
}
```

---

## Проверить список товаров, которые склад партнёра может принять

`POST /v1/fbp/draft/drop-off/product/validate`

Operation ID: `FbpDraftDropOffProductValidate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/draft/drop-off/product/validate" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "skus": [
    {
      "count": 0,
      "sku": 0
    }
  ],
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `skus required` — Array of objects Идентификаторы товаров в системе Ozon — SKU.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Результат проверки

#### Схема ответа (200)

- `approved_items` — Array of objects Принятые товары.
- `bundle_generated` — boolean true , если создан товарный состав.
- `bundle_id` — string Идентификатор провалидированного списка товаров.
- `rejected_items` — Array of objects Отклонённые товары.

Пример ответа:
```json
{
  "approved_items": [
    {
      "barcode": "string",
      "icon_name": "string",
      "name": "string",
      "offer_id": "string",
      "quantity": 0,
      "sku": 0,
      "volume": 0
    }
  ],
  "bundle_generated": true,
  "bundle_id": "string",
  "rejected_items": [
    {
      "barcode": "string",
      "icon_name": "string",
      "name": "string",
      "offer_id": "string",
      "quantity": 0,
      "rejection_reasons": [
        "BUNDLE_ITEM_ERROR_UNSPECIFIED"
      ],
      "sku": 0,
      "volume": 0
    }
  ]
}
```

---
