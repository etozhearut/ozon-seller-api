# Работа со складами FBS и rFBS

_Тег: `WarehouseAPI` · операций: 10_

## Список складов

`POST /v2/warehouse/list`

Operation ID: `WarehouseListV2`

Метод возвращает список складов FBS и rFBS. Чтобы получить список складов FBO, используйте метод /v1/warehouse/fbo/list .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `limit required` — integer <= 200 Количество значений в ответе.
- `cursor` — string Указатель для выборки следующих данных.
- `warehouse_ids` — Array of strings <int64> <= 200 Идентификаторы складов.

Пример запроса:

```json
{
  "cursor": "string",
  "limit": 0,
  "warehouse_ids": [
    20605650762000
  ]
}
```

### Ответы

- 200 Список складов

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `warehouses` — Array of objects Список складов.
  - `address_info` — object Информация о расположении склада.
  - `carriage_label_type` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "BIG" "SMALL" Тип этикетки: UNSPECIFIED — неизвестный тип; BIG — большая этикетка; SMALL — маленькая этикетка.
  - `courier_comment` — string Комментарий для курьера.
  - `courier_phones` — Array of strings Номера телефонов для связи с курьером.
  - `created_at` — string <date-time> Дата и время создания склада.
  - `cut_in_time` — integer <int64> Время на отгрузку в минутах.
  - `first_mile` — object Первая миля.
  - `has_entrusted_acceptance` — boolean Признак подключения доверительной приемки.
  - `has_postings_limit` — boolean Признак наличия лимита минимального количества заказов. true , если лимит есть.
  - `is_auto_assembly` — boolean Признак включённой автосборки.
  - `is_comfort` — boolean Признак доставки comfort. Время доставки до покупателя от 60 минут.
  - `is_express` — boolean Признак доставки express. Время доставки до покупателя не больше 60 минут.
  - `is_kgt` — boolean Признак, что склад принимает крупногабаритные товары.
  - `is_rfbs` — boolean Признак работы склада по схеме rFBS.
  - `is_waybill_enabled` — boolean Признак включённой печати транспортной накладной.
  - `min_postings_limit` — integer <int32> Минимальное количество заказов, которое можно привезти в одной поставке.
  - `name` — string Название склада.
  - `pause_at` — string <date-time> Дата, когда продавец поставил склад на паузу. Если значение null , склад активный. Только для rFBS-склада.
  - `phone` — string Номер телефона склада.
  - `postings_limit` — integer <int32> Лимит заказов. -1 , если лимита нет.
  - `sla_cut_in` — integer <int64> Минимальное время на сборку заказа в минутах.
  - `status` — string Статус склада.
  - `timetable` — object Расписание работы склада.
  - `updated_at` — string <date-time> Дата и время последнего обновления данных склада.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `warehouse_type` — string Тип склада.
  - `with_item_list` — boolean Признак включённой печати листа подбора.
  - `working_days` — Array of strings Items Enum: "UNSPECIFIED" "MONDAY" "TUESDAY" "WEDNESDAY" "THURSDAY" "FRIDAY" "SATURDAY" "SUNDAY" Рабочие дни склада: UNSPECIFIED — значение не определено; MONDAY — понедельник; TUESDAY — вторник; WEDNESDAY — среда; THURSDAY — четверг; FRIDAY — пятница; SATURDAY — суббота; SUNDAY — воскресенье.
- `has_next` — boolean true , если в ответе вернулись не все значения.
- `code` — integer <int32> Код ошибки.
- `details` — Array of objects Дополнительная информация об ошибке.
- `message` — string Описание ошибки.

Пример ответа:

```json
{
  "cursor": "string",
  "warehouses": [
    {
      "address_info": {
        "address": "Россия, Московская Область, Софьино, промзона ССТ, к2, 2",
        "latitude": 55.495093,
        "longitude": 38.172731,
        "utc": "UTC+03:00"
      },
      "carriage_label_type": "BIG",
      "courier_comment": "",
      "courier_phones": [
        "+7(999)999-99-99"
      ],
      "created_at": "2025-03-11T11:57:51.811Z",
      "first_mile": {
        "type": "PICK_UP",
        "dropoff_point_id": "1020002075314000",
        "timeslot_from": "20:59",
        "timeslot_id": 287231,
        "timeslot_to": "21:00",
        "first_mile_is_changing": false
      },
      "has_entrusted_acceptance": true,
      "has_postings_limit": false,
      "is_auto_assembly": true,
      "is_kgt": true,
      "is_rfbs": true,
      "is_waybill_enabled": true,
      "min_postings_limit": 2,
      "is_comfort": true,
      "is_express": true,
      "warehouse_type": "string",
      "cut_in_time": 0,
      "name": "17023",
      "phone": "+7(999)999-99-99",
      "postings_limit": -1,
      "sla_cut_in": 2939,
      "status": "created",
      "timetable": {
        "timetable_from": "2025-03-11T11:57:51.811Z",
        "timetable_to": "2025-03-11T11:57:51.811Z",
        "working_hours": [
          {
            "time_from": "2025-03-11T11:57:51.811Z",
            "time_to": "2025-03-11T11:57:51.811Z"
          }
        ]
      },
      "updated_at": "2025-03-11T11:57:51.811Z",
      "warehouse_id": 20605650762000,
      "with_item_list": true,
      "working_days": [
        "MONDAY",
        "TUESDAY",
        "WEDNESDAY",
        "THURSDAY",
        "FRIDAY"
      ]
    }
  ],
  "has_next": "string"
}
```

---

## Список складов

`POST /v1/warehouse/list`

Operation ID: `WarehouseAPI_WarehouseList`

Метод устаревает и будет отключён 7 апреля 2026 года. Переключитесь на /v2/warehouse/list . Возвращает список складов FBS и rFBS. Чтобы получить список складов FBO, используйте метод /v1/cluster/list . Метод можно использовать 1 раз в минуту.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `limit required` — integer <int64> <= 200 Количество значений в ответе.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

Пример запроса:

```json
{
  "limit": 1,
  "offset": 1,
  "with": {
    "able_to_set_price": true
  }
}
```

### Ответы

- 200 Список складов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Список складов.
  - `has_entrusted_acceptance` — boolean Признак доверительной приёмки. true , если доверительная приёмка включена на складе.
  - `is_rfbs` — boolean Признак работы склада по схеме rFBS: true — склад работает по схеме rFBS; false — не работает по схеме rFBS.
  - `name` — string Название склада.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `can_print_act_in_advance` — boolean Возможность печати акта приёма-передачи заранее. true , если печатать заранее возможно.
  - `first_mile_type` — object Первая миля FBS.
  - `has_postings_limit` — boolean Признак наличия лимита минимального количества заказов. true , если лимит есть.
  - `is_karantin` — boolean Признак, что склад не работает из-за карантина.
  - `is_kgt` — boolean Признак, что склад принимает крупногабаритные товары.
  - `is_economy` — boolean true , если склад работает с эконом-товарами.
  - `is_able_to_set_price` — boolean true , если можно установить цену.
  - `is_presorted` — boolean true , если отгрузка с предсортировкой.
  - `is_timetable_editable` — boolean Признак, что можно менять расписание работы складов.
  - `min_postings_limit` — integer <int32> Минимальное значение лимита — количество заказов, которые можно привезти в одной поставке.
  - `postings_limit` — integer <int32> Значение лимита. -1 , если лимита нет.
  - `min_working_days` — integer <int64> Количество рабочих дней склада.
  - `status`
    - `new`
    - `created`
    - `disabled`
    - `blocked`
    - `disabled_due_to_limit`
    - `error`
  - `working_days` — Array of strings Items Enum: "1" "2" "3" "4" "5" "6" "7" Рабочие дни склада.

Пример ответа:

```json
{
  "result": [
    {
      "warehouse_id": 17777777788000,
      "name": "BlueMarketplace",
      "is_rfbs": false,
      "is_able_to_set_price": false,
      "has_entrusted_acceptance": true,
      "first_mile_type": {
        "dropoff_point_id": "1121111996024111",
        "dropoff_timeslot_id": 294144,
        "first_mile_is_changing": false,
        "first_mile_type": "DropOff"
      },
      "is_kgt": false,
      "can_print_act_in_advance": false,
      "min_working_days": 5,
      "is_karantin": false,
      "has_postings_limit": false,
      "postings_limit": -1,
      "working_days": [
        1,
        2,
        3,
        4,
        5,
        6,
        7
      ],
      "min_postings_limit": 2,
      "is_timetable_editable": false,
      "status": "created",
      "is_economy": false,
      "is_presorted": false
    }
  ]
}
```

---

## Список методов доставки realFBS-склада

`POST /v2/delivery-method/list`

Operation ID: `WarehouseAPI_DeliveryMethodListV2`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр для поиска методов доставки.
- `limit required` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.

Пример запроса:

```json
{
  "cursor": "string",
  "filter": {
    "delivery_method_ids": [
      "string"
    ],
    "provider_ids": [
      "string"
    ],
    "status": [
      "NEW"
    ],
    "warehouse_ids": [
      "string"
    ]
  },
  "limit": 1,
  "sort_dir": "ASC"
}
```

### Ответы

- 200 Список методов склада
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернули не все методы доставки.
- `delivery_methods` — Array of objects Методы доставки.

Пример ответа:

```json
{
  "cursor": "string",
  "has_next": true,
  "delivery_methods": [
    {
      "created_at": "2019-08-24T14:15:22Z",
      "cutoff": "string",
      "id": 0,
      "is_express": true,
      "name": "string",
      "provider_id": 0,
      "sla_cut_in": 0,
      "status": "NEW",
      "template_id": 0,
      "tpl_dropoff_point": {
        "address": "string",
        "address_coordinates": {
          "latitude": 0,
          "longitude": 0
        },
        "code": "string",
        "name": "string"
      },
      "tpl_integration_type": "string",
      "updated_at": "2019-08-24T14:15:22Z",
      "warehouse_id": 0
    }
  ]
}
```

---

## Список методов доставки склада

`POST /v1/delivery-method/list`

Operation ID: `WarehouseAPI_DeliveryMethodList`

Метод устаревает и будет отключён 7 апреля 2026 года. Переключитесь на /v2/delivery-method/list .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter` — object Фильтр для поиска методов доставки.
- `limit required` — integer <int64> Количество элементов в ответе. Максимум — 50, минимум — 1.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.

Пример запроса:

```json
{
  "filter": {
    "provider_id": 424,
    "status": "ACTIVE",
    "warehouse_id": 15588127982000
  },
  "limit": 100,
  "offset": 0
}
```

### Ответы

- 200 Список методов склада
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `has_next` — boolean Признак, что в запросе вернулась только часть методов доставки: true — сделайте повторный запрос с новым параметром offset для получения остальных методов; false — ответ содержит все методы доставки по запросу.
- `result` — Array of objects Результат запроса.
  - `company_id` — integer <int64> Идентификатор продавца.
  - `created_at` — string <date-time> Дата и время создания метода доставки.
  - `cutoff` — string Время, до которого продавцу нужно собрать заказ.
  - `id` — integer <int64> Идентификатор метода доставки.
  - `name` — string Название метода доставки.
  - `provider_id` — integer <int64> Идентификатор службы доставки.
  - `sla_cut_in` — integer <int64> Минимальное время на сборку заказа в минутах в соответствии с настройками склада.
  - `status` — string Статус метода доставки: NEW — создан, EDITED — редактируется, ACTIVE — активный, DISABLED — неактивный.
  - `template_id` — integer <int64> Идентификатор услуги по доставке заказа.
  - `updated_at` — string <date-time> Дата и время последнего обновления метода метода доставки.
  - `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:

```json
{
  "result": [
    {
      "id": 15588127982000,
      "company_id": 1,
      "name": "Ozon Логистика курьеру, Есипово",
      "status": "ACTIVE",
      "cutoff": "13:00",
      "provider_id": 24,
      "template_id": 0,
      "warehouse_id": 15588127982000,
      "created_at": "2019-04-04T15:22:31.048202Z",
      "updated_at": "2021-08-15T10:21:44.854209Z",
      "sla_cut_in": 1440
    }
  ],
  "has_next": false
}
```

---

## Получить информацию по возвратным настройкам rFBS и rFBS Express

`POST /v1/delivery-method/return/settings/get`

Operation ID: `GetDeliveryMethodReturnSettingsV1`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int64> Идентификатор способа доставки. Получите значение параметра методом /v2/delivery-method/list .

### Ответы

- 200 Информация получена

#### Схема ответа (200)

- `settings` — object Информация о возвратных настройках.
  - `courier_details` — object Настройки курьера.
  - `post_office_zipcode` — string Индекс отделения Почты России для «лёгкого возврата» .
  - `transport_company_details` — object Настройки транспортной компании.

Пример ответа:

```json
{
  "settings": {
    "courier_details": {
      "contact_days": 0
    },
    "post_office_zipcode": "string",
    "transport_company_details": {
      "transport_company_names": [
        "string"
      ],
      "zipcode": "string"
    }
  }
}
```

---

## Получить статус операции

`POST /v1/warehouse/operation/status`

Operation ID: `GetWarehouseFBSOperationStatus`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id required` — string Идентификатор операции.

Пример запроса:

```json
{
  "operation_id": "a0cfefee-9a5a-4580-bc32-2f9a6c7973e3"
}
```

### Ответы

- 200 Статус операции

#### Схема ответа (200)

- `error` — object Ошибка при обработке операции.
- `result` — object Результат операции.
  - `entity_id` — integer <int64> Идентификатор обрабатываемой сущности. Если операция CREATE_FBS_WAREHOUSE , вернётся идентификатор склада.
- `status` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "IN_PROGRESS" "SUCCESS" "ERROR" Статус операции: UNSPECIFIED — не определено; IN_PROGRESS — в процессе; SUCCESS — выполнена; ERROR — завершилась с ошибкой.
- `type` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "CREATE_FBS_WAREHOUSE" "UPDATE_FBS_WAREHOUSE" "SET_FIRST_MILE" "WAREHOUSE_ENABLE_DISABLE" "WAREHOUSE_PAUSE_UNPAUSE" Тип операции: UNSPECIFIED — не определено; CREATE_FBS_WAREHOUSE — создание FBS-склада; UPDATE_FBS_WAREHOUSE — обновление FBS-склада; SET_FIRST_MILE — установка первой мили; WAREHOUSE_ENABLE_DISABLE — архивация или разархивация FBS-склада; WAREHOUSE_PAUSE_UNPAUSE — включение или выключение паузы rFBS-склада.

Пример ответа:

```json
{
  "error": {
    "code": "string",
    "message": "string"
  },
  "result": {
    "warehouse_id": 1020005000219156
  },
  "status": "SUCCESS",
  "type": "CREATE_FBS_WAREHOUSE"
}
```

---

## Перенести склад в архив

`POST /v1/warehouse/archive`

Operation ID: `ArchiveWarehouseFBS`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `reason required` — string <= 200 characters Причина переноса склада в архив.
- `return_point_id` — integer <int64> Идентификатор пункта возврата. Получите значение параметра методом /v1/warehouse/fbs/update/return-point/list .
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "reason": "Тестовая причина",
  "warehouse_id": 1020002929332000,
  "return_point_id": 0
}
```

### Ответы

- 200 Склад перенесён в архив

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Перенести склад из архива

`POST /v1/warehouse/unarchive`

Operation ID: `UnarchiveWarehouseFBS`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `return_point_id` — integer <int64> Идентификатор пункта возврата. Получите значение параметра методом /v1/warehouse/fbs/update/return-point/list .
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "warehouse_id": 1020002929332000,
  "return_point_id": 0
}
```

### Ответы

- 200 Склад перенесён из архива

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Получить список товаров с ограничениями по доставке

`POST /v1/warehouse/invalid-products/get`

Operation ID: `WarehouseInvalidProductsGet`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `last_id` — integer <int64> Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `warehouse_id required` — integer <int64> Идентификатор склада. Получите значение параметра методом /v1/warehouse/warehouses-with-invalid-products .

Пример запроса:

```json
{
  "last_id": 0,
  "warehouse_id": 0
}
```

### Ответы

- 200 Список товаров с ограничениями

#### Схема ответа (200)

- `has_next` — boolean true , если в ответе вернулись не все товары.
- `last_id` — integer <int64> Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .
- `validation_results` — Array of objects Результат проверки.
- `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:

```json
{
  "has_next": true,
  "last_id": 0,
  "validation_results": [
    {
      "item": {
        "size": {
          "height_mm": 0,
          "length_mm": 0,
          "width_mm": 0
        },
        "sku": 0,
        "weight_g": 0
      },
      "state": "UNSPECIFIED",
      "validation_errors": [
        {
          "characteristic": "UNSPECIFIED",
          "restriction_price": {
            "currency": "string",
            "value": 0
          },
          "restriction_vwc": 0,
          "template_id": 0,
          "type": "UNSPECIFIED"
        }
      ]
    }
  ],
  "warehouse_id": 0
}
```

---

## Получить список складов с ограниченными для доставки товарами

`POST /v1/warehouse/warehouses-with-invalid-products`

Operation ID: `WarehouseWithInvalidProducts`

Возвращает идентификаторы складов, на которых находятся товары с ограничениями. Такие товары недоступны для доставки со склада.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список складов

#### Схема ответа (200)

- `warehouse_ids` — Array of strings <int64> Список идентификаторов складов, у которых есть хотя бы 1 товар, который недоступен для доставки со склада. Чтобы получить список товаров с ограничениями, используйте метод /v1/warehouse/invalid-products/get .

---
