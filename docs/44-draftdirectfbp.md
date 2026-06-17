# Работа с FBP-черновиками с доставкой direct

_Тег: `DraftDirectFBP` · операций: 10_

## Создать черновик с доставкой силами продавца

`POST /v1/fbp/draft/direct/seller-dlv/create`

Operation ID: `FbpDraftDirectSellerDlvCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `bundle_id required` — string Идентификатор провалидированного списка товаров.
- `delivery_details required` — object Детали доставки.
- `package_units_count required` — integer <int32> Количество грузомест.
- `warehouse_id required` — integer <int64> Идентификатор склада продавца.

Пример запроса:

```json
{
  "bundle_id": "string",
  "delivery_details": {
    "driver_name": "string",
    "timeslot_start": "2019-08-24T14:15:22Z",
    "vehicle_number": "string",
    "vehicle_type": "string"
  },
  "package_units_count": 0,
  "warehouse_id": 0
}
```

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

## Обновить информацию о доставке силами продавца в черновике

`POST /v1/fbp/draft/direct/seller-dlv/edit`

Operation ID: `FbpDraftDirectSellerDlvEdit`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `driver_name required` — string ФИО водителя.
- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор заявки на поставку.
- `vehicle_number required` — string Номер автомобиля.
- `vehicle_type required` — string Тип автомобиля.

Пример запроса:

```json
{
  "driver_name": "string",
  "row_version": 0,
  "supply_id": "string",
  "vehicle_number": "string",
  "vehicle_type": "string"
}
```

### Ответы

- 200 Черновик обновлён

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error": {
    "errors": "ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---

## Отредактировать таймслот в черновике

`POST /v1/fbp/draft/direct/timeslot/edit`

Operation ID: `FbpDraftDirectTimeslotEdit`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор заявки на поставку.
- `timeslot_start required` — string <date-time> Начало таймслота.

Пример запроса:

```json
{
  "row_version": 0,
  "supply_id": "string",
  "timeslot_start": "2019-08-24T14:15:22Z"
}
```

### Ответы

- 200 Таймслот отредактирован

#### Схема ответа (200)

- `error_reasons` — Array of strings Default: "RESERVE_FAILURE_TYPE_UNSPECIFIED" Items Enum: "RESERVE_FAILURE_TYPE_UNSPECIFIED" "REQUEST_VALIDATION" "INVALID_RESERVE" "LOGISTICS_REASON" "SCHEDULE_REASON" "NO_CAPACITY" Причина ошибки: RESERVE_FAILURE_TYPE_UNSPECIFIED — не определена; REQUEST_VALIDATION — в запросе указана дата резервирования в прошлом; INVALID_RESERVE — исходный резерв не найден, неактивен или уже содержит заявки, а его пытаются перезаписать; LOGISTICS_REASON — ошибка на стороне логистики; SCHEDULE_REASON — ошибка на стороне расписаний; NO_CAPACITY — нет доступных слотов для резервирования.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error_reasons": "RESERVE_FAILURE_TYPE_UNSPECIFIED",
  "row_version": 0
}
```

---

## Получить список таймслотов для прямой поставки

`POST /v1/fbp/draft/direct/timeslot/get`

Operation ID: `FbpDraftDirectGetTimeslot`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `bundle_id required` — string Идентификатор провалидированного списка товаров.
- `interval_end required` — string <date-time> Дата окончания нужного периода доступных таймслотов.
- `interval_start required` — string <date-time> Дата начала нужного периода доступных таймслотов.
- `warehouse_id required` — integer <int64> Идентификатор склада продавца.

Пример запроса:

```json
{
  "bundle_id": "string",
  "interval_end": "2019-08-24T14:15:22Z",
  "interval_start": "2019-08-24T14:15:22Z",
  "warehouse_id": 0
}
```

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `reasons` — Array of strings Default: "EMPTY_TIMESLOTS_REASON_UNSPECIFIED" Items Enum: "EMPTY_TIMESLOTS_REASON_UNSPECIFIED" "LOGISTICS_UNKNOWN" "NO_ROUTE" "NO_ROUTE_SCHEDULES" "NO_LOGISTICS_CAPACITY" "SCHEDULE_UNKNOWN" "NOT_ENOUGH_CAPACITY" "NOT_ENOUGH_TRUCKS" "LIMITS_NOT_AVAILABLE" "CROSS_DOCK_RESERVE_MISSING" "SCHEDULE_RESERVE_MISSING" Причины отсутствия таймслотов: EMPTY_TIMESLOTS_REASON_UNSPECIFIED — не определено; LOGISTICS_UNKNOWN — неизвестная ошибка на стороне логистики; NO_ROUTE — нет маршрута; NO_ROUTE_SCHEDULES — нет расписания на маршруте; NO_LOGISTICS_CAPACITY — недостаточно доступных слотов на маршруте; SCHEDULE_UNKNOWN — неизвестная ошибка на стороне расписаний; NOT_ENOUGH_CAPACITY — недостаточно доступных слотов на складе; NOT_ENOUGH_TRUCKS — недостаточно машиномест; LIMITS_NOT_AVAILABLE — не настроены лимиты на складе; CROSS_DOCK_RESERVE_MISSING — не забронирован кросс-докинговый резерв на складе; SCHEDULE_RESERVE_MISSING — отсутствует необходимый резерв по расписанию.
- `timeslots` — Array of objects Список доступных таймслотов.
- `warehouse_timezone_name` — string Часовой пояс склада продавца.

Пример ответа:

```json
{
  "reasons": "EMPTY_TIMESLOTS_REASON_UNSPECIFIED",
  "timeslots": [
    {
      "timeslot_end": "2019-08-24T14:15:22Z",
      "timeslot_start": "2019-08-24T14:15:22Z"
    }
  ],
  "warehouse_timezone_name": "string"
}
```

---

## Создать черновик заявки на поставку без указания способа доставки

`POST /v1/fbp/draft/direct/create`

Operation ID: `FbpDraftDirectCreate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `bundle_id required` — string Идентификатор провалидированного списка товаров. Чтобы получить, используйте метод /v1/fbp/draft/direct/product/validate .
- `delivery_details required` — object Детали доставки.
- `package_units_count required` — integer <int32> Количество единиц упаковки.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "bundle_id": "string",
  "delivery_details": {
    "timeslot_start": "2019-08-24T14:15:22Z"
  },
  "package_units_count": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id` — string Идентификатор поставки.

Пример ответа:

```json
{
  "draft_id": 0,
  "row_version": 0,
  "supply_id": "string"
}
```

---

## Удалить черновик заявки на поставку

`POST /v1/fbp/draft/direct/delete`

Operation ID: `FbpDraftDirectDelete`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `supply_id required` — string Идентификатор поставки.

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
      "error_code": "CODE_UNSPECIFIED",
      "message": "string"
    },
    "cancellation_status": "STATUS_UNSPECIFIED"
  },
  "row_version": 0
}
```

---

## Проверить список товаров для склада партнёра

`POST /v1/fbp/draft/direct/product/validate`

Operation ID: `FbpDraftDirectProductValidate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `skus required` — Array of objects Идентификаторы товаров в системе Ozon — SKU.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "skus": [
    {
      "count": 0,
      "sku": 0
    }
  ],
  "warehouse_id": 0
}
```

### Ответы

- 200 Результат проверки

#### Схема ответа (200)

- `approved_items` — Array of objects Подтверждённые товары.
- `bundle_generated` — boolean true , если провалидированный список товаров создан.
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
      "rejection_reasons": "BUNDLE_ITEM_ERROR_UNSPECIFIED",
      "sku": 0,
      "volume": 0
    }
  ]
}
```

---

## Перевести черновик в действующую поставку

`POST /v1/fbp/draft/direct/registrate`

Operation ID: `FbpDraftDirectRegistrate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.

Пример запроса:

```json
{
  "row_version": 0,
  "supply_id": "string"
}
```

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
        "errors": "BUNDLE_ITEM_ERROR_UNSPECIFIED",
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

## Создать черновик заявки на доставку сторонней транспортной компанией

`POST /v1/fbp/draft/direct/tpl-dlv/create`

Operation ID: `FbpAPI_FbpDraftDirectTplDlvCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `bundle_id required` — string Идентификатор комплекта.
- `delivery_details required` — object Детали доставки.
- `package_units_count required` — integer <int32> Количество грузомест.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "bundle_id": "string",
  "delivery_details": {
    "timeslot_start": "2019-08-24T14:15:22Z",
    "tracking_number": "string",
    "transport_company_name": "string"
  },
  "package_units_count": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Статус генерации

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id` — string Идентификатор поставки.

Пример ответа:

```json
{
  "draft_id": 0,
  "row_version": 0,
  "supply_id": "string"
}
```

---

## Редактировать черновик поставки со способом доставки сторонней транспортной компанией

`POST /v1/fbp/draft/direct/tpl-dlv/edit`

Operation ID: `FbpAPI_FbpDraftDirectTplDlvEdit`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `row_version required` — integer <int64> Идентификатор актуальной версии черновика.
- `supply_id required` — string Идентификатор поставки.
- `tracking_number required` — string Трек-номер отправления.
- `transport_company_name required` — string Название транспортной компании.

Пример запроса:

```json
{
  "row_version": 0,
  "supply_id": "string",
  "tracking_number": "string",
  "transport_company_name": "string"
}
```

### Ответы

- 200 Черновик изменён

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "errors": [
    "ERROR_TYPE_UNSPECIFIED"
  ],
  "is_error": "true",
  "row_version": 0
}
```

---
