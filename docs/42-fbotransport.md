# Работа с транспортными грузоместами FBO

_Тег: `FBOTransport` · операций: 14_

## Получить информацию о грузоместах

`POST /v2/cargoes/get`

Operation ID: `CargoesGetV2`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `supplies required` — Array of objects <= 100 items Информация о поставках.

Пример запроса:

```json
{
  "supplies": [
    {
      "cargo_ids": [
        "string"
      ],
      "supply_id": 0
    }
  ]
}
```

### Ответы

- 200 Информация о грузоместах
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `supplies` — Array of objects Информация о грузоместах и транспортных грузоместах.
  - `bundle_id` — string Идентификатор товарного состава.
  - `cargoes` — Array of objects Грузоместа.
  - `cargoes_bundle_id` — string Товарный состав грузомест.
  - `limits` — object Количество грузомест и товаров в поставке.
  - `supply_id` — integer <int64> Идентификатор поставки.
  - `transport_cargoes` — Array of objects Информация о транспортных грузоместах.

Пример ответа:

```json
{
  "supplies": [
    {
      "bundle_id": "string",
      "cargoes": [
        {
          "bundle_id": "string",
          "cargo_id": 0,
          "content_type": "UNSPECIFIED",
          "placement_zone_type": "UNSPECIFIED",
          "tracking_info": {
            "arrival_at": {
              "date": "2019-08-24T14:15:22Z",
              "timezone_info": {
                "iana_name": "string",
                "offset": 0
              }
            },
            "status": "UNSPECIFIED",
            "type": "UNSPECIFIED"
          },
          "transport_cargo_id": 0,
          "type": "UNSPECIFIED"
        }
      ],
      "cargoes_bundle_id": "string",
      "limits": {
        "max_box_count": 0,
        "max_box_sku_count": 0,
        "max_pallet_count": 0,
        "max_transport_pallet_count": 0
      },
      "supply_id": 0,
      "transport_cargoes": [
        {
          "box_count": 0,
          "summary_bundle_id": "string",
          "tracking_info": {
            "arrival_at": {
              "date": "2019-08-24T14:15:22Z",
              "timezone": {
                "iana_name": "string",
                "offset": 0
              }
            },
            "status": "UNSPECIFIED",
            "type": "UNSPECIFIED"
          },
          "transport_cargo_id": 0,
          "type": "UNSPECIFIED"
        }
      ]
    }
  ]
}
```

---

## Удалить грузоместа и транспортные грузоместа в заявке на поставку

`POST /v2/cargoes/delete`

Operation ID: `CargoesDeleteV2`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cargo_ids required` — Array of strings <int64> <= 1500 Список идентификаторов грузомест, которые нужно удалить.
- `supply_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .
- `transport_cargo_deletion_type required` — string Enum: "UNBIND_CONTAINED_CARGOES" "DELETE_CONTAINED_CARGOES" Тип удаления транспортного грузоместа: UNBIND_CONTAINED_CARGOES — удалить только транспортное грузоместо и отвязать от него все грузоместа; DELETE_CONTAINED_CARGOES — удалить транспортное грузоместо вместе с грузоместами.
- `transport_cargo_ids` — Array of strings <int64> <= 40 Список идентификаторов транспортных грузомест, которые нужно удалить.

Пример запроса:

```json
{
  "cargo_ids": [
    "string"
  ],
  "supply_id": 0,
  "transport_cargo_deletion_type": "UNBIND_CONTAINED_CARGOES",
  "transport_cargo_ids": [
    "string"
  ]
}
```

### Ответы

- 200 Грузоместа и транспортные грузоместа удалены
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `errors` — object Ошибки.
- `operation_id` — string Идентификатор операции. Получите статус операции методом /v2/cargoes/delete/status .

Пример ответа:

```json
{
  "errors": {
    "cargo_error_reasons": [
      {
        "cargo_id": 0,
        "error_reasons": [
          "UNSPECIFIED"
        ]
      }
    ],
    "supply_error_reasons": [
      "UNSPECIFIED"
    ],
    "transport_cargo_error_reasons": [
      {
        "error_reasons": [
          "UNSPECIFIED"
        ],
        "transport_cargo_id": 0
      }
    ]
  },
  "operation_id": "string"
}
```

---

## Получить информацию о статусе удаления грузомест и транспортных грузомест

`POST /v2/cargoes/delete/status`

Operation ID: `CargoesDeleteStatusV2`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id required` — string Идентификатор операции из метода /v2/cargoes/delete .

### Ответы

- 200 Статус удаления грузомест и транспортных грузомест
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `errors` — object Ошибки.
- `status` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "SUCCESS" "IN_PROGRESS" "FAILED" Статус удаления грузоместа: UNSPECIFIED — не определён; SUCCESS — грузоместо удалено; IN_PROGRESS — грузоместо в процессе удаления; ERROR — возникла ошибка при удалении грузоместа.

Пример ответа:

```json
{
  "errors": {
    "cargo_error_reasons": [
      {
        "cargo_id": 0,
        "error_reasons": [
          "UNSPECIFIED"
        ]
      }
    ],
    "supply_error_reasons": [
      "UNSPECIFIED"
    ],
    "transport_cargo_error_reasons": [
      {
        "error_reasons": [
          "UNSPECIFIED"
        ],
        "transport_cargo_id": 0
      }
    ]
  },
  "status": "UNSPECIFIED"
}
```

---

## Включить или отключить транспортные грузоместа в поставке

`POST /v1/cargoes/transport/activate`

Operation ID: `CargoesTransportActivate`

Если в поставке создано грузоместо, отключить транспортные грузоместа не получится. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `is_transport required` — boolean true , чтобы включить транспортные грузоместа.
- `supply_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .

Пример запроса:

```json
{
  "is_transport": true,
  "supply_id": 0
}
```

### Ответы

- 200 Транспортные грузоместа включены или отключены
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/cargoes/transport/activate/status .

---

## Получить статус включения или отключения транспортных грузомест

`POST /v1/cargoes/transport/activate/status`

Operation ID: `CargoesTransportActivateStatus`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id required` — string Идентификатор операции из метода /v1/cargoes/transport/activate .

### Ответы

- 200 Статус
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "OPERATION_NOT_FOUND" "SUPPLY_IS_FINALIZED" "CAN_NOT_EDIT_TAG" "UNDEFINED" Ошибки при включении или отключении транспортных грузомест: OPERATION_NOT_FOUND — операция не найдена; SUPPLY_IS_FINALIZED , CAN_NOT_EDIT_TAG — нельзя изменить поставку; UNDEFINED — неизвестная ошибка.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус включения или отключения транспортных грузомест: SUCCESS — успешно; IN_PROGRESS — в процессе; FAILED — ошибка.

Пример ответа:

```json
{
  "error_reasons": [
    "OPERATION_NOT_FOUND"
  ],
  "status": "SUCCESS"
}
```

---

## Создать транспортное грузоместо

`POST /v1/cargoes/transport/create`

Operation ID: `CargoesTransportCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `supply_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .
- `transport_cargoes required` — Array of objects Количество транспортных грузомест по типам.

Пример запроса:

```json
{
  "supply_id": 0,
  "transport_cargoes": [
    {
      "count": 0,
      "type": "PALLET"
    }
  ]
}
```

### Ответы

- 200 Транспортное грузоместо создано
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "SUPPLY_NOT_FOUND" "WAREHOUSE_LIMITS_EXCEED" "SUPPLY_DOES_NOT_BELONG_TO_THE_CONTRACTOR" "SUPPLY_DOES_NOT_BELONG_TO_THE_COMPANY" "PALLET_SUPPLY_CONTAINS_BOXES" "SUPPLY_CARGOES_IS_FINALIZED" "SUPPLY_CARGOES_LOCKED" "OPERATION_NOT_FOUND" "ETTN_IS_UPLOADED" "BOX_SUPPLY_CONTAINS_PALLETS" "UNDEFINED" Ошибки при создании транспортных грузомест: SUPPLY_NOT_FOUND — поставка не найдена; WAREHOUSE_LIMITS_EXCEED — превышены лимиты склада; SUPPLY_DOES_NOT_BELONG_TO_THE_CONTRACTOR — заявка на поставку не принадлежит юридическому лицу; SUPPLY_DOES_NOT_BELONG_TO_THE_COMPANY — заявка на поставку не принадлежит продавцу; PALLET_SUPPLY_CONTAINS_BOXES — палетная поставка не может содержать коробки; SUPPLY_CARGOES_IS_FINALIZED — нельзя редактировать заявку на поставку; SUPPLY_CARGOES_LOCKED — другой процесс блокирует редактирование грузомест поставки; OPERATION_NOT_FOUND — операция не найдена; ETTN_IS_UPLOADED — нельзя редактировать поставку с загруженной эТТН; BOX_SUPPLY_CONTAINS_PALLETS — поставка может содержать только коробки; UNDEFINED — неизвестная ошибка.
- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/cargoes/transport/create/status .

Пример ответа:

```json
{
  "error_reasons": [
    "SUPPLY_NOT_FOUND"
  ],
  "operation_id": "string"
}
```

---

## Получить статус создания транспортного грузоместа

`POST /v1/cargoes/transport/create/status`

Operation ID: `CargoesTransportCreateStatus`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id` — string Идентификатор операции из метода /v1/cargoes/transport/create .

### Ответы

- 200 Статус
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "SUPPLY_NOT_FOUND" "WAREHOUSE_LIMITS_EXCEED" "SUPPLY_DOES_NOT_BELONG_TO_THE_CONTRACTOR" "SUPPLY_DOES_NOT_BELONG_TO_THE_COMPANY" "PALLET_SUPPLY_CONTAINS_BOXES" "SUPPLY_CARGOES_IS_FINALIZED" "SUPPLY_CARGOES_LOCKED" "OPERATION_NOT_FOUND" "ETTN_IS_UPLOADED" "BOX_SUPPLY_CONTAINS_PALLETS" "UNDEFINED" Ошибки при создании транспортных грузомест: SUPPLY_NOT_FOUND — поставка не найдена; WAREHOUSE_LIMITS_EXCEED — превышены лимиты склада; SUPPLY_DOES_NOT_BELONG_TO_THE_CONTRACTOR — заявка на поставку не принадлежит юридическому лицу; SUPPLY_DOES_NOT_BELONG_TO_THE_COMPANY — заявка на поставку не принадлежит продавцу; PALLET_SUPPLY_CONTAINS_BOXES — палетная поставка не может содержать коробки; SUPPLY_CARGOES_IS_FINALIZED — нельзя редактировать заявку на поставку; SUPPLY_CARGOES_LOCKED — другой процесс блокирует редактирование грузомест поставки; OPERATION_NOT_FOUND — операция не найдена; ETTN_IS_UPLOADED — нельзя редактировать поставку с загруженной эТТН; BOX_SUPPLY_CONTAINS_PALLETS — поставка может содержать только коробки; UNDEFINED — неизвестная ошибка.
- `result` — object Созданные транспортные грузоместа.
  - `transport_cargoes` — Array of objects Информация о созданных транспортных грузоместах.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус создания транспортных грузомест: SUCCESS — успешно; IN_PROGRESS — в процессе; FAILED — ошибка.

Пример ответа:

```json
{
  "error_reasons": [
    "SUPPLY_NOT_FOUND"
  ],
  "result": {
    "transport_cargoes": [
      {
        "id": 0,
        "type": "PALLET"
      }
    ]
  },
  "status": "SUCCESS"
}
```

---

## Связать или отвязать грузоместа и транспортные грузоместа

`POST /v1/cargoes/transport/bind`

Operation ID: `CargoesTransportBind`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cargoes_unbind_transport_cargoes required` — Array of strings <int64> Идентификаторы транспортных грузомест, которые надо отвязать.
- `supply_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .
- `transport_cargo_bind` — Array of objects Идентификаторы грузомест и транспортных грузомест, которые надо связать.

Пример запроса:

```json
{
  "cargoes_unbind_transport_cargoes": [
    "string"
  ],
  "supply_id": 0,
  "transport_cargo_bind": [
    {
      "cargo_ids": [
        "string"
      ],
      "transport_cargo_id": 0
    }
  ]
}
```

### Ответы

- 200 Грузоместа связаны или отвязаны
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "OPERATION_NOT_FOUND" "OPERATION_FAILED" "SUPPLY_NOT_FOUND" "SUPPLY_DOES_NOT_BELONG_TO_COMPANY" "SUPPLY_DOES_NOT_BELONG_TO_CONTRACTOR" "TRANSPORT_CARGOES_NOT_ENABLED_FOR_SUPPLY" "INVALID_SUPPLY_STATE" "SUPPLY_IS_FINALIZED" "CARGO_IDS_NOT_FOUND" "TRANSPORT_CARGO_IDS_NOT_FOUND" "ETTN_IS_UPLOADED" Ошибки связывания или отвязывания грузомест: OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; SUPPLY_NOT_FOUND — поставка не найдена; SUPPLY_DOES_NOT_BELONG_TO_COMPANY — заявка на поставку не принадлежит продавцу; SUPPLY_DOES_NOT_BELONG_TO_CONTRACTOR — поставка не принадлежит юридическому лицу; TRANSPORT_CARGOES_NOT_ENABLED_FOR_SUPPLY — для поставки не включены транспортные грузоместа; INVALID_SUPPLY_STATE — неверный статус поставки; SUPPLY_IS_FINALIZED — нельзя редактировать поставку; CARGO_IDS_NOT_FOUND — грузоместа не найдены; TRANSPORT_CARGO_IDS_NOT_FOUND — траспортные грузоместа не найдены; ETTN_IS_UPLOADED — нельзя редактировать поставку с загруженной эТТН.
- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/cargoes/transport/bind/status .

Пример ответа:

```json
{
  "error_reasons": [
    "OPERATION_NOT_FOUND"
  ],
  "operation_id": "string"
}
```

---

## Получить статус связывания или отвязывания грузомест и транспортных грузомест

`POST /v1/cargoes/transport/bind/status`

Operation ID: `CargoesTransportBindStatus`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id required` — string Идентификатор операции из метода /v1/cargoes/transport/bind .

### Ответы

- 200 Статус
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "OPERATION_NOT_FOUND" "OPERATION_FAILED" "SUPPLY_NOT_FOUND" "SUPPLY_DOES_NOT_BELONG_TO_COMPANY" "SUPPLY_DOES_NOT_BELONG_TO_CONTRACTOR" "TRANSPORT_CARGOES_NOT_ENABLED_FOR_SUPPLY" "INVALID_SUPPLY_STATE" "SUPPLY_IS_FINALIZED" "CARGO_IDS_NOT_FOUND" "TRANSPORT_CARGO_IDS_NOT_FOUND" "ETTN_IS_UPLOADED" Ошибки связывания или отвязывания грузомест: OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; SUPPLY_NOT_FOUND — поставка не найдена; SUPPLY_DOES_NOT_BELONG_TO_COMPANY — заявка на поставку не принадлежит продавцу; SUPPLY_DOES_NOT_BELONG_TO_CONTRACTOR — поставка не принадлежит юридическому лицу; TRANSPORT_CARGOES_NOT_ENABLED_FOR_SUPPLY — для поставки не включены транспортные грузоместа; INVALID_SUPPLY_STATE — неверный статус поставки; SUPPLY_IS_FINALIZED — нельзя редактировать поставку; CARGO_IDS_NOT_FOUND — грузоместа не найдены; TRANSPORT_CARGO_IDS_NOT_FOUND — траспортные грузоместа не найдены; ETTN_IS_UPLOADED — нельзя редактировать поставку с загруженной эТТН.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус связывания или отвязывания грузомест: SUCCESS — успешно; IN_PROGRESS — в процессе; FAILED — ошибка.

Пример ответа:

```json
{
  "error_reasons": [
    "OPERATION_NOT_FOUND"
  ],
  "status": "SUCCESS"
}
```

---

## Получить информацию о грузоместах в поставках

`POST /v1/cargoes/supplies/get`

Operation ID: `CargoesSuppliesGet`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `supply_ids required` — Array of strings <int64> <= 50 items Список идентификаторов поставок.

### Ответы

- 200 Информация о грузоместах в поставках
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `not_found_supply_ids` — Array of strings <int64> Поставки, которые не удалось найти.
- `supplies_cargoes` — Array of objects Информация о грузоместах в поставках.

Пример ответа:

```json
{
  "not_found_supply_ids": [
    "string"
  ],
  "supplies_cargoes": [
    {
      "bundle_id": "string",
      "cargoes_without_transport_cargoes": [
        {
          "barcode": "string",
          "bundle_id": "string",
          "cargo_id": 0
        }
      ],
      "supply_id": 0,
      "transport_cargoes": [
        {
          "bundle_id": "string",
          "cargoes": [
            {
              "barcode": "string",
              "bundle_id": "string",
              "cargo_id": 0
            }
          ],
          "transport_cargo_id": 0,
          "type": "PALLET"
        }
      ]
    }
  ]
}
```

---

## Сгенерировать этикетки для транспортных грузомест по идентификатору поставки

`POST /v1/cargoes/label/transport-by-order/create`

Operation ID: `CargoesLabelTransportByOrderCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `order_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .

### Ответы

- 200 Этикетки сгенерированы
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "ORDER_NOT_FOUND" "OPERATION_NOT_FOUND" "OPERATION_FAILED" "ALL_SUPPLIES_SKIPPED" "LABELS_COUNT_EXCEED" "UNDEFINED" Ошибки при генерации этикеток транспортных грузомест: ORDER_NOT_FOUND — заявка не найдена; OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; ALL_SUPPLIES_SKIPPED — нет этикеток для поставок; LABELS_COUNT_EXCEED — превышено количество генерируемых этикеток; UNDEFINED — неизвестная ошибка.
- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/cargoes/label/transport-by-order/status .

Пример ответа:

```json
{
  "error_reasons": [
    "ORDER_NOT_FOUND"
  ],
  "operation_id": "string"
}
```

---

## Получить статус генерации этикеток для транспортных грузомеcт по идентификатору поставки

`POST /v1/cargoes/label/transport-by-order/status`

Operation ID: `CargoesLabelTransportByOrderStatus`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id required` — string Идентификатор операции из метода /v1/cargoes/label/transport-by-order/create .

### Ответы

- 200 Статус
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "ORDER_NOT_FOUND" "OPERATION_NOT_FOUND" "OPERATION_FAILED" "ALL_SUPPLIES_SKIPPED" "LABELS_COUNT_EXCEED" "UNDEFINED" Ошибки при генерации этикеток транспортных грузомест: ORDER_NOT_FOUND — заявка не найдена; OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; ALL_SUPPLIES_SKIPPED — нет этикеток для поставок; LABELS_COUNT_EXCEED — превышено количество генерируемых этикеток; UNDEFINED — неизвестная ошибка.
- `result` — object Этикетки транспортных грузомест.
  - `file_url` — string Ссылка на PDF-файл с этикетками.
  - `skipped_supplies_ids` — Array of strings <int64> Поставки, по которым не сгенерированы этикетки.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус генерации этикеток: SUCCESS — успешно; IN_PROGRESS — в процессе; FAILED — ошибка.

Пример ответа:

```json
{
  "error_reasons": [
    "ORDER_NOT_FOUND"
  ],
  "result": {
    "file_url": "string",
    "skipped_supplies_ids": [
      "string"
    ]
  },
  "status": "SUCCESS"
}
```

---

## Сгенерировать этикетки транспортных грузомест по идентификатору грузоместа

`POST /v1/cargoes/label/transport/create`

Operation ID: `CargoesLabelTransportCreate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `supply_id required` — integer <int64> Идентификатор поставки. Получите значение параметра методом /v3/supply-order/get .
- `transport_cargo_ids` — Array of strings <int64> <= 40 items Идентификаторы транспортных грузомест. Если ничего не передать, в ответе вернутся этикетки для всех транспортных грузомест в поставке.

Пример запроса:

```json
{
  "supply_id": 0,
  "transport_cargo_ids": [
    "string"
  ]
}
```

### Ответы

- 200 Этикетки сгенерированы
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "INVALID_STATE" "OPERATION_NOT_FOUND" "OPERATION_FAILED" "SUPPLY_NOT_BELONG_CONTRACTOR" "SUPPLY_NOT_BELONG_COMPANY" "SUPPLY_IS_EMPTY" "CARGOES_NOT_FOUND" Ошибки при генерации этикеток транспортных грузомест: INVALID_STATE — недопустимое состояние поставки; OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; SUPPLY_NOT_BELONG_CONTRACTOR — поставка не принадлежит юридическому лицу; SUPPLY_NOT_BELONG_COMPANY — заявка на поставку не принадлежит продавцу; SUPPLY_IS_EMPTY — в поставке нет грузомест; CARGOES_NOT_FOUND — грузоместа не найдены.
- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/cargoes/label/transport-by-order/status .

Пример ответа:

```json
{
  "error_reasons": [
    "INVALID_STATE"
  ],
  "operation_id": "string"
}
```

---

## Получить статус генерации этикеток транспортных грузомест по идентификатору грузоместа

`POST /v1/cargoes/label/transport/status`

Operation ID: `CargoesLabelTransportStatus`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev. Подробнее о работе с транспортными грузоместами

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `operation_id` — string Идентификатор операции из метода /v1/cargoes/label/transport/create .

### Ответы

- 200 Статус
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "INVALID_STATE" "OPERATION_NOT_FOUND" "OPERATION_FAILED" "SUPPLY_NOT_BELONG_CONTRACTOR" "SUPPLY_NOT_BELONG_COMPANY" "SUPPLY_IS_EMPTY" "CARGOES_NOT_FOUND" Ошибки при генерации этикеток транспортных грузомест: INVALID_STATE — недопустимое состояние поставки; OPERATION_NOT_FOUND — операция не найдена; OPERATION_FAILED — операция завершилась с ошибкой; SUPPLY_NOT_BELONG_CONTRACTOR — поставка не принадлежит юридическому лицу; SUPPLY_NOT_BELONG_COMPANY — заявка на поставку не принадлежит продавцу; SUPPLY_IS_EMPTY — в поставке нет грузомест; CARGOES_NOT_FOUND — грузоместа не найдены.
- `result` — object Этикетки транспортных грузомест.
  - `file_url` — string Ссылка на PDF-файл с этикетками.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус генерации этикеток: SUCCESS — успешно; IN_PROGRESS — в процессе; FAILED — ошибка.

Пример ответа:

```json
{
  "error_reasons": [
    "INVALID_STATE"
  ],
  "result": {
    "file_url": "string"
  },
  "status": "SUCCESS"
}
```

---
