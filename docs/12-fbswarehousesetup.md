# Создание FBS-складов и управление ими

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FBSWarehouseSetup` · методов: 17_

## Получить список drop-off пунктов для создания склада

`POST /v1/warehouse/fbs/create/drop-off/list`

Operation ID: `WarehouseAPI_ListDropOffPointsForCreateFBSWarehouse`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/create/drop-off/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "country_code": "RU",
  "is_kgt": false,
  "coordinates": {
    "latitude": 55,
    "longitude": 37
  },
  "search": {
    "address": "москва",
    "types": [
      "PPZ"
    ]
  }
}'
```

### Тело запроса

- `coordinates` — object Координаты.
- `country_code required` — string Код страны в формате ISO 2.
- `is_kgt required` — boolean true , если товар крупногабаритный.
- `search` — object Параметры поиска.

### Ответы

- 200 Список получен

#### Схема ответа (200)

- `points` — Array of objects Список пунктов.
  - `address` — string Адрес drop-off пункта.
  - `coordinates` — object Координаты drop-off пункта.
  - `discount_percent` — number <float> Процент скидки за передачу отправления.
  - `id` — string Идентификатор drop-off пункта.
  - `last_transit_time_local` — object Время, до которого нужно передать отправления, чтобы получить скидку за отгрузку.
  - `type` — string Enum: "PVZ" "PPZ" "SC" Тип drop-off пункта: PVZ — пункт выдачи заказов; PPZ — пункт приёма заказов; SC — сортировочный центр.

Пример ответа:
```json
{
  "points": [
    {
      "address": "Россия, Москва, Москва, Россия, г. Москва, Никольская улица, 7-9 строение 4",
      "discount_percent": 1,
      "id": "1020002487458000",
      "last_transit_time_local": {
        "hours": 12,
        "minutes": 0,
        "nanos": 0,
        "seconds": 0
      },
      "coordinates": {
        "latitude": 55.756107,
        "longitude": 37.620426
      },
      "type": "PVZ"
    }
  ]
}
```

---

## Получить список drop-off пунктов для изменения информации склада

`POST /v1/warehouse/fbs/update/drop-off/list`

Operation ID: `WarehouseAPI_ListDropOffPointsForUpdateFBSWarehouse`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/update/drop-off/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "search": {
    "address": "москва",
    "types": [
      "PPZ"
    ]
  },
  "warehouse_id": 0
}'
```

### Тело запроса

- `search` — object Параметры поиска.
- `warehouse_id required` — integer <int64> Фильтр по существующему FBS-складу.

### Ответы

- 200 Список получен

#### Схема ответа (200)

- `points` — Array of objects Список пунктов.
  - `address` — string Адрес drop-off пункта.
  - `coordinates` — object Координаты drop-off пункта.
  - `discount_percent` — number <float> Процент скидки за передачу отправления.
  - `id` — string Идентификатор drop-off пункта.
  - `last_transit_time_local` — object Время, до которого нужно передать отправления, чтобы получить скидку за отгрузку.
  - `type` — string Enum: "PVZ" "PPZ" "SC" Тип drop-off пункта: PVZ — пункт выдачи заказов; PPZ — пункт приёма заказов; SC — сортировочный центр.

Пример ответа:
```json
{
  "points": [
    {
      "address": "Россия, Москва, Москва, Россия, г. Москва, Никольская улица, 7-9 строение 4",
      "discount_percent": 1,
      "id": "1020002487458000",
      "last_transit_time_local": {
        "hours": 12,
        "minutes": 0,
        "nanos": 0,
        "seconds": 0
      },
      "coordinates": {
        "latitude": 55.756107,
        "longitude": 37.620426
      },
      "type": "PVZ"
    }
  ]
}
```

---

## Получить список таймслотов для создания склада с отгрузкой drop-off

`POST /v1/warehouse/fbs/create/drop-off/timeslot/list`

Operation ID: `WarehouseFbsCreateDropOffTimeslotList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/create/drop-off/timeslot/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `drop_off_point_id required` — integer <int64> Идентификатор drop-off пункта.

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `timeslots` — Array of objects Список таймслотов.
  - `acceptance_end_time_local` — string Местное время окончания приёма заказа.
  - `acceptance_start_time_local` — string Местное время начала приёма заказа.
  - `from` — string Время начала таймслота.
  - `id` — integer <int64> Идентификатор таймслота.
  - `to` — string Время окончания таймслота.

Пример ответа:
```json
{
  "timeslots": [
    {
      "acceptance_end_time_local": "string",
      "acceptance_start_time_local": "string",
      "from": "string",
      "id": 0,
      "to": "string"
    }
  ]
}
```

---

## Получить список таймслотов для обновления склада с отгрузкой drop-off

`POST /v1/warehouse/fbs/update/drop-off/timeslot/list`

Operation ID: `WarehouseFbsUpdateDropOffTimeslotList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/update/drop-off/timeslot/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "drop_off_point_id": 0,
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `drop_off_point_id required` — integer <int64> Идентификатор drop-off пункта.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `timeslots` — Array of objects Список таймслотов.
  - `acceptance_end_time_local` — string Местное время окончания приёма заказа.
  - `acceptance_start_time_local` — string Местное время начала приёма заказа.
  - `from` — string Время начала таймслота.
  - `id` — integer <int64> Идентификатор таймслота.
  - `to` — string Время окончания таймслота.

Пример ответа:
```json
{
  "timeslots": [
    {
      "acceptance_end_time_local": "string",
      "acceptance_start_time_local": "string",
      "from": "string",
      "id": 0,
      "to": "string"
    }
  ]
}
```

---

## Получить список таймслотов для создания склада с отгрузкой pick-up

`POST /v1/warehouse/fbs/create/pick-up/timeslot/list`

Operation ID: `WarehouseFbsCreatePickUpTimeslotList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/create/pick-up/timeslot/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "is_kgt": true,
  "address_coordinates": {
    "latitude": 55.7558,
    "longitude": 37.6173
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `address_coordinates required` — object Координаты склада.
- `is_kgt required` — boolean Признак крупногабаритного товара.

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `is_pickup_supported` — boolean Признак поддержки отгрузки pick-up.
- `timeslots` — Array of objects Список таймслотов.

Пример ответа:
```json
{
  "is_pickup_supported": true,
  "timeslots": [
    {
      "id": 123456789,
      "from": "00:00",
      "to": "00:00"
    },
    {
      "id": 987654321,
      "from": "00:00",
      "to": "00:00"
    }
  ]
}
```

---

## Получить список таймслотов для обновления склада с отгрузкой pick-up

`POST /v1/warehouse/fbs/update/pick-up/timeslot/list`

Operation ID: `WarehouseFbsUpdatePickUpTimeslotList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/update/pick-up/timeslot/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `timeslots` — Array of objects Список таймслотов.
  - `from` — string Время начала таймслота.
  - `id` — integer <int64> Идентификатор таймслота.
  - `to` — string Время окончания таймслота.

Пример ответа:
```json
{
  "timeslots": [
    {
      "id": 1001,
      "from": "00:00",
      "to": "00:00"
    },
    {
      "id": 1002,
      "from": "00:00",
      "to": "00:00"
    }
  ]
}
```

---

## Создать склад

`POST /v1/warehouse/fbs/create`

Operation ID: `WarehouseAPI_CreateWarehouseFBS`

Если создаёте склад с доставкой в drop-off пункт, используйте метод /v1/warehouse/fbs/create/drop-off/list , чтобы получить точки.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "address_coordinates": {
    "latitude": 55.69626,
    "longitude": 37.42686
  },
  "first_mile_type": "DROP_OFF",
  "drop_off_point_id": 1020002487458000,
  "cut_in_time": 0,
  "timeslot_id": 0,
  "return_point_id": 0,
  "is_kgt": false,
  "name": "Склад",
  "options": {
    "comment": "Комментарий",
    "courier_phones": [
      "+7(999)999-99-99"
    ],
    "is_auto_assembly": true,
    "is_waybill_enabled": true
  },
  "phone": "+7(999)999-99-99",
  "working_days": [
    "MONDAY",
    "TUESDAY",
    "WEDNESDAY",
    "THURSDAY",
    "FRIDAY"
  ]
}'
```

### Тело запроса

- `address_coordinates required` — object Координаты адреса склада.
- `cut_in_time required` — integer <int64> Время на приём заказов в минутах. Например, если вы передадите 3000 , приём заказов будет завершён через 50 часов с момента передачи.
- `drop_off_point_id` — integer <int64> Идентификатор drop-off пункта.
- `first_mile_type required` — string Enum: "PICK_UP" "DROP_OFF" Тип первой мили: PICK_UP — отгрузка заказов курьеру; DROP_OFF — отгрузка заказов в пункт приёма.
- `is_kgt required` — boolean true , если товар крупногабаритный.
- `name required` — string Название склада.
- `options` — object Параметры склада.
- `phone required` — string Номер телефона склада. Укажите в формате +7(XXX)XXX-XX-XX.
- `timeslot_id required` — integer <int64> Идентификатор таймслота.
- `return_point_id` — integer <int64> Идентификатор пункта возврата. Получите значение параметра методом /v1/warehouse/fbs/create/return-point/list .
- `working_days` — Array of strings <= 7 items Items Enum: "MONDAY" "TUESDAY" "WEDNESDAY" "THURSDAY" "FRIDAY" "SATURDAY" "SUNDAY" Рабочие дни склада: MONDAY — понедельник, TUESDAY — вторник, WEDNESDAY — среда, THURSDAY — четверг, FRIDAY — пятница, SATURDAY — суббота, SUNDAY — воскресенье.

### Ответы

- 200 Склад создан

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции на создание FBS-склада. Чтобы получить статус операции, используйте метод /v1/warehouse/operation/status .

Пример ответа:
```json
{
  "operation_id": "a0cfefee-9a5a-4580-bc32-2f9a6c7973e3"
}
```

---

## Обновить склад

`POST /v1/warehouse/fbs/update`

Operation ID: `UpdateWarehouseFBS`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "address_coordinates": {
    "latitude": 55.69626,
    "longitude": 37.42686
  },
  "name": "Склад Балашиха",
  "options": {
    "comment": "Заезд на склад через главные ворота",
    "courier_phones": [
      "+7(999)999-99-99"
    ],
    "is_auto_assembly": true,
    "is_waybill_enabled": true
  },
  "phone": "+7(XXX)XXX-XX-XX",
  "warehouse_id": 1020002929332000,
  "working_days": [
    "MONDAY",
    "TUESDAY",
    "WEDNESDAY",
    "THURSDAY",
    "FRIDAY"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `address_coordinates required` — object Координаты склада.
- `name` — string >= 100 characters Название склада.
- `options` — object Параметры склада.
- `phone` — string +7(XXX)XXX-XX-XX Номер телефона склада.
- `warehouse_id required` — integer <int64> Идентификатор склада.
- `working_days` — Array of strings [ 5 .. 7 ] Items Enum: "MONDAY" "TUESDAY" "WEDNESDAY" "THURSDAY" "FRIDAY" "SATURDAY" "SUNDAY" Рабочие дни склада: MONDAY — понедельник; TUESDAY — вторник; WEDNESDAY — среда; THURSDAY — четверг; FRIDAY — пятница; SATURDAY — суббота; SUNDAY — воскресенье.

### Ответы

- 200 Склад обновлён

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Обновить первую милю

`POST /v1/warehouse/fbs/first-mile/update`

Operation ID: `UpdateWarehouseFBSFirstMile`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/first-mile/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "first_mile_type": "DROP_OFF",
  "drop_off_point_id": 0,
  "cut_in_time": 0,
  "timeslot_id": 0,
  "return_point_id": 0,
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cut_in_time required` — integer <int64> Время на приём заказов в минутах. Например, если вы передадите 3000 , приём заказов будет завершён через 50 часов с момента передачи.
- `drop_off_point_id` — integer <int64> Идентификатор drop-off пункта. Если first_mile_type = DROP_OFF , параметр обязательный.
- `first_mile_type required` — string Enum: "PICK_UP" "DROP_OFF" Тип первой мили: PICK_UP — отгрузка заказов курьеру; DROP_OFF — отгрузка заказов в пункт приёма.
- `timeslot_id required` — integer <int64> Идентификатор таймслота.
- `return_point_id` — integer <int64> Идентификатор пункта возврата. Получите значение параметра методом /v1/warehouse/fbs/update/return-point/list .
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Первая миля обновлена

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Получить список пунктов возврата для создания склада

`POST /v1/warehouse/fbs/create/return-point/list`

Operation ID: `WarehouseFBSCreateReturnPointList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/create/return-point/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "coordinates": {
    "latitude": 0,
    "longitude": 0
  },
  "country_code": "string",
  "last_id": 0,
  "limit": 1,
  "search": {
    "address": "string",
    "types": [
      "PVZ"
    ]
  },
  "selected_dropoff_point_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `coordinates required` — object Координаты пункта возврата.
- `country_code required` — string Код страны в формате ISO 2.
- `last_id` — integer <int64> Идентификатор последнего значения на странице.
- `limit required` — integer <int32> [ 1 .. 500 ] Количество значений в ответе.
- `search` — object Параметры поиска.
- `selected_dropoff_point_id` — integer <int64> Идентификатор выбранной точки отгрузки на складе.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернули не все пункты возврата.
- `is_selected_point_available` — boolean Признак доступности пункта возврата.
- `last_id` — integer <int64> Идентификатор последнего значения на странице.
- `points` — Array of objects Список пунктов возврата.

Пример ответа:
```json
{
  "has_next": true,
  "is_selected_point_available": true,
  "last_id": 0,
  "points": [
    {
      "address": "string",
      "coordinates": {
        "latitude": 0,
        "longitude": 0
      },
      "id": 0,
      "name": "string",
      "type": "UNSPECIFIED",
      "utc_offset": 0,
      "working_days": [
        {
          "day": "UNSPECIFIED",
          "from": "string",
          "to": "string"
        }
      ]
    }
  ]
}
```

---

## Получить список пунктов возврата для обновления склада

`POST /v1/warehouse/fbs/update/return-point/list`

Operation ID: `WarehouseFBSUpdateReturnPointList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/update/return-point/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "current_dropoff_point_id": 0,
  "current_return_point_id": 0,
  "last_id": 0,
  "limit": 1,
  "search": {
    "address": "string",
    "types": [
      "PVZ"
    ]
  },
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `current_dropoff_point_id` — integer <int64> Идентификатор выбранной точки отгрузки на складе.
- `current_return_point_id` — integer <int64> Установленный пункт возврата. Получите значение параметра методом /v1/warehouse/fbs/return-mile/info .
- `last_id` — integer <int64> Идентификатор последнего значения на странице.
- `limit required` — integer <int32> [ 1 .. 500 ] Количество значений в ответе.
- `search` — object Параметры поиска.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернули не все пункты возврата.
- `is_selected_point_available` — boolean Признак доступности пункта возврата для выбора.
- `last_id` — integer <int64> Идентификатор последнего значения на странице.
- `points` — Array of objects Список пунктов возврата.

Пример ответа:
```json
{
  "has_next": true,
  "is_selected_point_available": true,
  "last_id": 0,
  "points": [
    {
      "address": "string",
      "coordinates": {
        "latitude": 0,
        "longitude": 0
      },
      "id": 0,
      "name": "string",
      "type": "UNSPECIFIED",
      "utc_offset": 0,
      "working_days": [
        {
          "day": "UNSPECIFIED",
          "from": "string",
          "to": "string"
        }
      ]
    }
  ]
}
```

---

## Получить информацию о возвратной миле

`POST /v1/warehouse/fbs/return-mile/info`

Operation ID: `WarehouseFBSReturnMileInfo`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/return-mile/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_ids required` — Array of strings <int64> [ 1 .. 1000 ] Идентификаторы складов.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `return_mile_settings` — Array of objects Информация о возвратной миле на складе.
  - `is_return_mile_required` — boolean Признак, что необходимо установить пункт возврата.
  - `return_point` — object Информация о пункте возврата.
  - `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:
```json
{
  "return_mile_settings": [
    {
      "is_return_mile_required": true,
      "return_point": {
        "address": "string",
        "coordinates": {
          "latitude": 0,
          "longitude": 0
        },
        "id": 0,
        "name": "string",
        "type": "UNSPECIFIED",
        "utc_offset": 0,
        "working_days": [
          {
            "day": "UNSPECIFIED",
            "from": "string",
            "to": "string"
          }
        ]
      },
      "warehouse_id": 0
    }
  ]
}
```

---

## Проверить необходимость установки возвратной мили на склад

`POST /v1/warehouse/fbs/return-mile/check`

Operation ID: `WarehouseFbsReturnMileCheck`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/return-mile/check" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "country_code": "string",
  "first_mile_type": "PICK_UP",
  "is_kgt": true,
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `country_code required` — string Код страны в формате ISO 2.
- `first_mile_type required` — string Enum: "PICK_UP" "DROP_OFF" Тип первой мили: PICK_UP — отгрузка заказов курьеру; DROP_OFF — отгрузка заказов в пункт приёма.
- `is_kgt required` — boolean Признак крупногабаритного товара.
- `warehouse_id` — integer <int64> Идентификатор склада.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `should_set_return_mile` — boolean Признак, что необходимо установить возвратную милю.
- `unavailability_reasons` — Array of strings Причины, по которым нельзя установить возвратную милю.

Пример ответа:
```json
{
  "should_set_return_mile": true,
  "unavailability_reasons": [
    "string"
  ]
}
```

---

## Создать вызов курьера на забор отгрузки pick-up

`POST /v1/warehouse/fbs/pickup/courier/create`

Operation ID: `WarehouseFbsPickUpCourierCreate`

Метод позволяет запланировать приезд курьера для отгрузки ему отправлений. Подробнее об отгрузках курьеру на FBS в Базе знаний

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/pickup/courier/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_id required` — integer <int64> Идентификатор склада. Чтобы получить список складов для планирования выездов, используйте /v1/warehouse/fbs/pickup/planning/list .

### Ответы

- 200 Вызов создан

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

## Отменить вызов курьера на забор отгрузки pick-up

`POST /v1/warehouse/fbs/pickup/courier/cancel`

Operation ID: `WarehouseFbsPickUpCourierCancel`

Метод позволяет отменить запланированный приезд курьера. Подробнее об отгрузках курьеру на FBS в Базе знаний

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/pickup/courier/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Вызов отменён

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

## Получить историю отгрузок курьерам

`POST /v1/warehouse/fbs/pickup/history/list`

Operation ID: `WarehouseFbsPickUpHistoryList`

Подробнее об отгрузках курьеру в Базе знаний продавца

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/pickup/history/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cursor": "string",
  "filter": {
    "planned_date": "string",
    "warehouse_id": [
      "string"
    ],
    "was_planned": true
  },
  "limit": 1
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр.
- `limit required` — integer <int64> [ 1 .. 1000 ] Количество значений на странице.

### Ответы

- 200 История отгрузок
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат метода.
  - `cursor` — string Указатель для выборки следующих данных.
  - `history` — Array of objects История отгрузок.

Пример ответа:
```json
{
  "result": {
    "cursor": "string",
    "history": [
      {
        "planned_date": "string",
        "status": "string",
        "updated_at": "2019-08-24T14:15:22Z",
        "warehouse_id": 0,
        "warehouse_name": "string",
        "was_planned": true
      }
    ]
  }
}
```

---

## Получить список складов для планирования отгрузок курьеру

`POST /v1/warehouse/fbs/pickup/planning/list`

Operation ID: `WarehouseFbsPickUpPlanningList`

Чтобы создать отгрузку, используйте метод /v1/warehouse/fbs/pickup/courier/create . Подробнее об отгрузках курьеру на FBS в Базе знаний

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbs/pickup/planning/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список складов

#### Схема ответа (200)

- `result` — object Список складов.
  - `warehouses` — Array of objects Информация о складах.

Пример ответа:
```json
{
  "result": {
    "warehouses": [
      {
        "can_modify_pickup_plan": true,
        "has_postings_to_be_planned": true,
        "is_pickup_planned": true,
        "last_pickup_plan_date_at": "2019-08-24T14:15:22Z",
        "warehouse_id": 0,
        "warehouse_name": "string"
      }
    ]
  }
}
```

---
