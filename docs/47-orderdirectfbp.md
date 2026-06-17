# Работа с FBP-поставками с доставкой direct

_Тег: `OrderDirectFBP` · операций: 4_

## Отменить поставку

`POST /v1/fbp/order/direct/cancel`

Operation ID: `FbpAPI_FbpOrderDirectCancel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `supply_id required` — string Идентификатор заявки на поставку.

### Ответы

- 200 Результат отмены

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

## Обновить информацию о доставке силами продавца

`POST /v1/fbp/order/direct/seller-dlv/edit`

Operation ID: `FbpAPI_FbpOrderDirectSellerDlvEdit`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

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

- 200 Информация обновлена

#### Схема ответа (200)

- `error` — object Информация об ошибке.
- `is_error` — boolean true , если есть ошибка.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error": {
    "order_errors": "ERROR_TYPE_UNSPECIFIED"
  },
  "is_error": true,
  "row_version": 0
}
```

---

## Отредактировать таймслот в заявке на поставку

`POST /v1/fbp/order/direct/timeslot/edit`

Operation ID: `FbpAPI_FbpEditTimeslot`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

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

- `error_reasons` — Array of strings Items Enum: "RESERVE_FAILURE_TYPE_UNSPECIFIED" "REQUEST_VALIDATION" "INVALID_RESERVE" "LOGISTICS_REASON" "SCHEDULE_REASON" Причина ошибки: RESERVE_FAILURE_TYPE_UNSPECIFIED — не определена; REQUEST_VALIDATION — в запросе указана дата резервирования в прошлом; INVALID_RESERVE — исходный резерв не найден, неактивен или уже содержит заявки, а его пытаются перезаписать; LOGISTICS_REASON — ошибка на стороне логистики; SCHEDULE_REASON — ошибка на стороне расписаний.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.

Пример ответа:

```json
{
  "error_reasons": [
    "RESERVE_FAILURE_TYPE_UNSPECIFIED"
  ],
  "row_version": 0
}
```

---

## Получить список таймслотов для поставки

`POST /v1/fbp/order/direct/timeslot/list`

Operation ID: `FbpAPI_FbpAvailableTimeslotList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Тело запроса (application/json)

- `interval_end required` — string <date-time> Дата окончания нужного периода доступных таймслотов.
- `interval_start required` — string <date-time> Дата начала нужного периода доступных таймслотов.
- `supply_id required` — string Идентификатор заявки на поставку.

Пример запроса:

```json
{
  "interval_end": "2019-08-24T14:15:22Z",
  "interval_start": "2019-08-24T14:15:22Z",
  "supply_id": "string"
}
```

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `reasons` — Array of strings Items Enum: "EMPTY_TIMESLOTS_REASON_UNSPECIFIED" "LOGISTICS_UNKNOWN" "NO_ROUTE" "NO_ROUTE_SCHEDULES" "NO_LOGISTICS_CAPACITY" "SCHEDULE_UNKNOWN" "NOT_ENOUGH_CAPACITY" "NOT_ENOUGH_TRUCKS" "LIMITS_NOT_AVAILABLE" "CROSS_DOCK_RESERVE_MISSING" "SCHEDULE_RESERVE_MISSING" Причины отсутствия таймслотов: EMPTY_TIMESLOTS_REASON_UNSPECIFIED — не определено; LOGISTICS_UNKNOWN — неизвестная ошибка на стороне логистики; NO_ROUTE — нет маршрута; NO_ROUTE_SCHEDULES — нет расписания на маршруте; NO_LOGISTICS_CAPACITY — недостаточно доступных слотов на маршруте; SCHEDULE_UNKNOWN — неизвестная ошибка на стороне расписаний; NOT_ENOUGH_CAPACITY — недостаточно доступных слотов на складе; NOT_ENOUGH_TRUCKS — недостаточно машиномест; LIMITS_NOT_AVAILABLE — не настроены лимиты на складе; CROSS_DOCK_RESERVE_MISSING — не забронирован кросс-докинговый резерв на складе; SCHEDULE_RESERVE_MISSING — отсутствует необходимый резерв по расписанию.
- `timeslots` — Array of objects Список доступных таймслотов.
- `warehouse_timezone_name` — string Часовой пояс склада продавца.

Пример ответа:

```json
{
  "reasons": [
    "EMPTY_TIMESLOTS_REASON_UNSPECIFIED"
  ],
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
