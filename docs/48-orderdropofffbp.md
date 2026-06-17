# Работа с FBP-поставками с доставкой drop-off

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `OrderDropOffFBP` · методов: 3_

## Отменить поставку drop-off

`POST /v1/fbp/order/drop-off/cancel`

Operation ID: `FbpAPI_FbpOrderDropOffCancel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/drop-off/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Поставка отменена

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:
```json
{
  "error": {
    "order_errors": [
      "ERROR_TYPE_UNSPECIFIED"
    ]
  },
  "is_error": true,
  "row_version": 0
}
```

---

## Отредактировать информацию о поставке на drop-off пункт

`POST /v1/fbp/order/drop-off/dlv/edit`

Operation ID: `FbpAPI_FbpOrderDropOffDlvEdit`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/drop-off/dlv/edit" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "drop_off_date": "string",
  "row_version": 0,
  "supply_id": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `drop_off_date required` — string Дата прибытия поставки на drop-off пункт.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Информация передана

#### Схема ответа (200)

- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

---

## Получить график работы drop-off пункта

`POST /v1/fbp/order/drop-off/timetable`

Operation ID: `FbpAPI_FbpOrderDropOffTimetable`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/drop-off/timetable" \
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

- 200 График работы получен

#### Схема ответа (200)

- `calendar` — Array of objects Информация о графике работы drop-off пункта.
  - `calendar_item` — object Информация о дне.
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
