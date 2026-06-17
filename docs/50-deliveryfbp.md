# Работа с созданной поставкой FBP

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `DeliveryFBP` · методов: 11_

## Сгенерировать акт приёмки

`POST /v1/fbp/act-from/create`

Operation ID: `FbpAPI_FbpCreateAct`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/act-from/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `errors` — Array of strings Default: "CREATE_ACT_ERROR_REASON_UNSPECIFIED" Items Enum: "CREATE_ACT_ERROR_REASON_UNSPECIFIED" "INVALID_ORDER_TYPE" Причина ошибки: CREATE_ACT_ERROR_REASON_UNSPECIFIED — не определена; INVALID_ORDER_TYPE — нельзя создать акт для указанного идентификатора поставки.
- `file_uuid` — string Идентификатор акта приёмки.
- `is_success` — boolean true , если в запросе нет ошибок.

Пример ответа:
```json
{
  "errors": [
    "CREATE_ACT_ERROR_REASON_UNSPECIFIED"
  ],
  "file_uuid": "string",
  "is_success": true
}
```

---

## Получить статус генерации акта приёмки

`POST /v1/fbp/act-from/get`

Operation ID: `FbpAPI_FbpCheckActState`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/act-from/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `file_uuid required` — string Идентификатор акта приёмки.

### Ответы

- 200 Статус генерации акта приёмки

#### Схема ответа (200)

- `cdn_url` — string Ссылка на акт приёмки.
- `error` — string Default: "ERROR_REASON_UNSPECIFIED" Enum: "ERROR_REASON_UNSPECIFIED" "INVALID_COMPANY" "FILE_NOT_FOUND" "GENERATE_TIMEOUT_REACHED" "GENERATION_ERROR" Ошибка генерации: ERROR_REASON_UNSPECIFIED — не определена; INVALID_COMPANY — неверная компания; FILE_NOT_FOUND — файл не найден; GENERATE_TIMEOUT_REACHED — превышено время генерации; GENERATION_ERROR — ошибка во время генерации.
- `status` — string Default: "STATUS_UNSPECIFIED" Enum: "STATUS_UNSPECIFIED" "NOT_EXIST" "PROCESSING" "EXIST" "ERROR" Статус генерации: STATUS_UNSPECIFIED — не определён; NOT_EXIST — не существует; PROCESSING — в процессе; EXIST — завершена; ERROR — ошибка.

Пример ответа:
```json
{
  "cdn_url": "string",
  "error": "ERROR_REASON_UNSPECIFIED",
  "status": "STATUS_UNSPECIFIED"
}
```

---

## Сгенерировать транспортную накладную

`POST /v1/fbp/act-to/create`

Operation ID: `FbpAPI_FbpCreateConsignmentNote`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/act-to/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `code` — string Идентификатор транспортной накладной.

---

## Получить статус генерации транспортной накладной

`POST /v1/fbp/act-to/get`

Operation ID: `FbpAPI_FbpCheckConsignmentNoteState`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/act-to/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "code": "string",
  "supply_id": "string"
}'
```

### Тело запроса

- `code required` — string Идентификатор транспортной накладной.
- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Статус генерации транспортной накладной

#### Схема ответа (200)

- `error_message` — string Описание ошибки.
- `label_url` — string Ссылка на этикетки для поставки.
- `state` — string Default: "STATE_TYPE_UNSPECIFIED" Enum: "STATE_TYPE_UNSPECIFIED" "IN_PROGRESS" "FINISHED" "FAILED" Статус генерации: STATE_TYPE_UNSPECIFIED — не определён; IN_PROGRESS — в процессе; FINISHED — завершилась успешно; FAILED — ошибка.

Пример ответа:
```json
{
  "error_message": "string",
  "label_url": "string",
  "state": "STATE_TYPE_UNSPECIFIED"
}
```

---

## Получить информацию о завершённой поставке

`POST /v1/fbp/archive/get`

Operation ID: `FbpAPI_FbpArchiveGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/archive/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Информация о завершённой поставке

#### Схема ответа (200)

- `act_file_uuid` — string Идентификатор акта приёмки.
- `bundle_id` — string Идентификатор провалидированного списка товаров.
- `bundle_sku_summary` — object Сводная информация по товарам в поставке.
- `business_flow_type_id` — integer <int64> Идентификатор типа поставки.
- `created_date` — string <date-time> Дата и время создания заявки на поставку.
- `decline_reason` — object Причина отклонения поставки.
- `delivery_details` — object Детали доставки.
- `has_act` — boolean true , если был сформирован акт приёмки.
- `has_label` — boolean true , если были сформированы этикетки.
- `id` — integer <int64> Номер записи в архиве.
- `order_draft_id` — integer <int64> Идентификатор черновика поставки.
- `order_number` — string Идентификатор завершённой поставки.
- `package_units_count` — integer <int32> Количество грузомест.
- `receive_date` — string <date-time> Дата и время принятия поставки.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `status` — string Default: "ARCHIVE_STATUS_UNSPECIFIED" Enum: "ARCHIVE_STATUS_UNSPECIFIED" "COMPLETED" "REJECTED_AT_SUPPLY_WAREHOUSE" "CANCELLED_BY_SELLER" Статус завершённой поставки: ARCHIVE_STATUS_UNSPECIFIED — не определён; COMPLETED — завершена; REJECTED_AT_SUPPLY_WAREHOUSE — отклонена складом; CANCELLED_BY_SELLER — отменена продавцом.
- `supply_id` — string Идентификатор поставки.
- `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:
```json
{
  "act_file_uuid": "string",
  "bundle_id": "string",
  "bundle_sku_summary": {
    "rounded_total_volume_in_litres": 0,
    "total_items_count": 0,
    "total_quantity": 0
  },
  "business_flow_type_id": 0,
  "created_date": "2019-08-24T14:15:22Z",
  "decline_reason": {
    "code": "DECLINE_REASON_CODE_UNSPECIFIED",
    "message": "string"
  },
  "delivery_details": {
    "direct_details": {
      "by_seller_details": {
        "driver_name": "string",
        "vehicle_registration_number": "string",
        "vehicle_type": "string"
      },
      "by_tpl_details": {
        "tracking_number": "string",
        "transport_company_name": "string"
      },
      "timeslot_details": {
        "timeslot": {
          "timeslot_end": "2019-08-24T14:15:22Z",
          "timeslot_start": "2019-08-24T14:15:22Z"
        },
        "timeslot_reservation_id": "string"
      }
    },
    "drop_off_point": {
      "id": 0,
      "province_uuid": "string",
      "timeslot": {
        "timeslot_end": "2019-08-24T14:15:22Z",
        "timeslot_start": "2019-08-24T14:15:22Z"
      }
    },
    "pickup_details": {
      "address": "string",
      "comment": "string",
      "date": "2019-08-24T14:15:22Z",
      "sender_name": "string",
      "sender_phone": "string"
    },
    "supply_type": "SUPPLY_TYPE_UNSPECIFIED"
  },
  "has_act": true,
  "has_label": true,
  "id": 0,
  "order_draft_id": 0,
  "order_number": "string",
  "package_units_count": 0,
  "receive_date": "2019-08-24T14:15:22Z",
  "row_version": 0,
  "status": "ARCHIVE_STATUS_UNSPECIFIED",
  "supply_id": "string",
  "warehouse_id": 0
}
```

---

## Получить список завершённых поставок

`POST /v1/fbp/archive/list`

Operation ID: `FbpAPI_FbpArchiveList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/archive/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "count": "string",
  "last_id": "string"
}'
```

### Тело запроса

- `count required` — string <int32> Количество элементов в ответе.
- `last_id` — string <int64> Идентификатор последнего значения на странице. Оставьте это поле пустым при выполнении первого запроса. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.

### Ответы

- 200 Список завершённых поставок

#### Схема ответа (200)

- `has_next` — boolean true , если в ответе вернулись не все значения.
- `items` — Array of objects Завершённые поставки.
- `last_id` — integer <int64> Идентификатор последнего значения на странице.

Пример ответа:
```json
{
  "has_next": true,
  "items": [
    {
      "act_file_uuid": "string",
      "bundle_id": "string",
      "bundle_sku_summary": {
        "rounded_total_volume_in_litres": 0,
        "total_items_count": 0,
        "total_quantity": 0
      },
      "created_date": "2019-08-24T14:15:22Z",
      "decline_reason": {
        "code": "DECLINE_REASON_CODE_UNSPECIFIED",
        "message": "string"
      },
      "delivery_details": {
        "direct_details": {
          "by_seller_details": {
            "driver_name": "string",
            "vehicle_registration_number": "string",
            "vehicle_type": "string"
          },
          "by_tpl_details": {
            "tracking_number": "string",
            "transport_company_name": "string"
          },
          "timeslot_details": {
            "timeslot": {
              "timeslot_end": "2019-08-24T14:15:22Z",
              "timeslot_start": "2019-08-24T14:15:22Z"
            },
            "timeslot_reservation_id": "string"
          }
        },
        "drop_off_point": {
          "id": 0,
          "province_uuid": "string",
          "timeslot": {
            "timeslot_end": "2019-08-24T14:15:22Z",
            "timeslot_start": "2019-08-24T14:15:22Z"
          }
        },
        "pickup_details": {
          "address": "string",
          "comment": "string",
          "date": "2019-08-24T14:15:22Z",
          "sender_name": "string",
          "sender_phone": "string"
        },
        "supply_type": "SUPPLY_TYPE_UNSPECIFIED"
      },
      "external_order_id": "string",
      "has_act": true,
      "has_label": true,
      "order_draft_id": 0,
      "package_units_count": 0,
      "receive_date": "2019-08-24T14:15:22Z",
      "row_version": 0,
      "status": "ARCHIVE_STATUS_UNSPECIFIED",
      "supply_id": "string",
      "warehouse_id": 0,
      "whc_order_id": 0
    }
  ],
  "last_id": 0
}
```

---

## Cоздать задание на генерацию этикеток

`POST /v1/fbp/label/create`

Operation ID: `FbpAPI_FbpCreateLabel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/label/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `code` — string Идентификатор задания на генерацию этикеток.

---

## Получить статус задания на генерацию этикеток

`POST /v1/fbp/label/get`

Operation ID: `FbpAPI_FbpGetLabel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/label/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "code": "string",
  "supply_id": "string"
}'
```

### Тело запроса

- `code required` — string Идентификатор задания на генерацию этикеток.
- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `label_url` — string Ссылка на этикетки для поставки.
- `state` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "IN_PROGRESS" "FINISHED" "FAILED" Статус задания на генерацию этикеток: UNSPECIFIED — не определён; IN_PROGRESS — в процессе генерации; FINISHED — генерация завершилась успешно; FAILED — генерация завершилась с ошибкой.

Пример ответа:
```json
{
  "label_url": "string",
  "state": "UNSPECIFIED"
}
```

---

## Получить информацию о конкретной поставке

`POST /v1/fbp/order/get`

Operation ID: `FbpAPI_FbpOrderGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `supply_id required` — string Идентификатор поставки.

### Ответы

- 200 Детали поставки

#### Схема ответа (200)

- `attention_reasons` — Array of strings Default: "ORDER_ATTENTION_TYPE_UNSPECIFIED" Items Enum: "ORDER_ATTENTION_TYPE_UNSPECIFIED" "OLD" "TIME_SLOT_EXPIRED" Причины предупреждения: ORDER_ATTENTION_TYPE_UNSPECIFIED — не определена; OLD — устаревшая заявка; TIME_SLOT_EXPIRED — таймслот просрочен.
- `bundle_uuid` — string Идентификатор товарного состава.
- `can_be_cancelled` — boolean true , если заявку можно отменить.
- `cancellation_state` — object Статус отмены.
- `created_date` — string <date-time> Дата создания поставки.
- `delivery_details` — object Детали доставки.
- `draft_id` — integer <int64> Идентификатор черновика.
- `has_consignment_note` — boolean true , если есть подписанные документы.
- `has_label` — boolean true , если есть этикетки.
- `id` — integer <int64> Идентификатор заявки на поставку.
- `locked` — boolean true , если нельзя редактировать поставку.
- `order_number` — string Номер поставки.
- `package_units_count` — integer <int32> Количество грузомест.
- `receive_date` — string <date-time> Дата и время принятия поставки.
- `row_version` — integer <int64> Идентификатор актуальной версии черновика.
- `status` — string Default: "ORDER_STATUS_UNSPECIFIED" Enum: "ORDER_STATUS_UNSPECIFIED" "READY_TO_SUPPLY" "FILLING_DELIVERY_DETAILS" "COURIER_ASSIGNED" "COURIER_PICKED_UP" "ACCEPTANCE_AT_DROP_OFF_POINT" "IN_TRANSIT_TO_STORAGE_WAREHOUSE" "ACCEPTANCE_AT_STORAGE_WAREHOUSE" "CANCELLED" Статус заказа: ORDER_STATUS_UNSPECIFIED — не определён; READY_TO_SUPPLY — готов к отгрузке; FILLING_DELIVERY_DETAILS — заполнение данных поставки; COURIER_ASSIGNED — курьер назначен; COURIER_PICKED_UP — курьер забрал поставку; ACCEPTANCE_AT_DROP_OFF_POINT — принято на drop-off пункте; IN_TRANSIT_TO_STORAGE_WAREHOUSE — в пути на склад размещения; ACCEPTANCE_AT_STORAGE_WAREHOUSE — приёмка на складе; CANCELLED — заявка отменена.
- `supply_id` — string Идентификатор поставки.
- `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:
```json
{
  "attention_reasons": "ORDER_ATTENTION_TYPE_UNSPECIFIED",
  "bundle_uuid": "string",
  "can_be_cancelled": true,
  "cancellation_state": {
    "cancellation_error": {
      "error_code": "CODE_UNSPECIFIED",
      "message": "string"
    },
    "cancellation_status": "STATUS_UNSPECIFIED"
  },
  "created_date": "2019-08-24T14:15:22Z",
  "delivery_details": {
    "direct_details": {
      "by_seller_details": {
        "driver_name": "string",
        "vehicle_registration_number": "string",
        "vehicle_type": "string"
      },
      "by_tpl_details": {
        "tracking_number": "string",
        "transport_company_name": "string"
      },
      "timeslot_details": {
        "timeslot": {
          "timeslot_end": "2019-08-24T14:15:22Z",
          "timeslot_start": "2019-08-24T14:15:22Z"
        },
        "timeslot_reservation_id": "string"
      }
    },
    "drop_off_point": {
      "id": 0,
      "province_uuid": "string",
      "timeslot": {
        "timeslot_end": "2019-08-24T14:15:22Z",
        "timeslot_start": "2019-08-24T14:15:22Z"
      }
    },
    "pickup_details": {
      "address": "string",
      "comment": "string",
      "date": "2019-08-24T14:15:22Z",
      "sender_name": "string",
      "sender_phone": "string"
    },
    "supply_type": "SUPPLY_TYPE_UNSPECIFIED"
  },
  "draft_id": 0,
  "has_consignment_note": true,
  "has_label": true,
  "id": 0,
  "locked": true,
  "order_number": "string",
  "package_units_count": 0,
  "receive_date": "2019-08-24T14:15:22Z",
  "row_version": 0,
  "status": "ORDER_STATUS_UNSPECIFIED",
  "supply_id": "string",
  "warehouse_id": 0
}
```

---

## Получить список поставок

`POST /v1/fbp/order/list`

Operation ID: `FbpAPI_FbpOrderList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/fbp/order/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `count required` — integer <int32> Количество поставок в ответе.
- `last_id` — integer <int64> Идентификатор последней поставки на странице. Для первого запроса оставьте это поле пустым. Чтобы получить следующие значения, укажите id последней поставки из ответа предыдущего запроса.

### Ответы

- 200 Список поставок

#### Схема ответа (200)

- `has_next` — boolean true , если в ответе вернули не все поставки.
- `items` — Array of objects Поставки.
- `last_id` — integer <int64> Идентификатор последней поставки на странице.

Пример ответа:
```json
{
  "has_next": true,
  "items": [
    {
      "attention_reasons": "ORDER_ATTENTION_TYPE_UNSPECIFIED",
      "bundle_summary": {
        "rounded_total_volume_in_litres": 0,
        "total_item_count": 0,
        "total_quantity": 0
      },
      "can_be_cancelled": true,
      "cancellation_state": {
        "cancellation_error": {
          "error_code": "CODE_UNSPECIFIED",
          "message": "string"
        },
        "cancellation_status": "STATUS_UNSPECIFIED"
      },
      "created_date": "2019-08-24T14:15:22Z",
      "delivery_details": {
        "direct_details": {
          "by_seller_details": {
            "driver_name": "string",
            "vehicle_registration_number": "string",
            "vehicle_type": "string"
          },
          "by_tpl_details": {
            "tracking_number": "string",
            "transport_company_name": "string"
          },
          "timeslot_details": {
            "timeslot": {
              "timeslot_end": "2019-08-24T14:15:22Z",
              "timeslot_start": "2019-08-24T14:15:22Z"
            },
            "timeslot_reservation_id": "string"
          }
        },
        "drop_off_point": {
          "id": 0,
          "province_uuid": "string",
          "timeslot": {
            "timeslot_end": "2019-08-24T14:15:22Z",
            "timeslot_start": "2019-08-24T14:15:22Z"
          }
        },
        "pickup_details": {
          "address": "string",
          "comment": "string",
          "date": "2019-08-24T14:15:22Z",
          "sender_name": "string",
          "sender_phone": "string"
        },
        "supply_type": "SUPPLY_TYPE_UNSPECIFIED"
      },
      "has_consignment_note": true,
      "has_label": true,
      "id": 0,
      "locked": true,
      "order_number": "string",
      "package_units_count": 0,
      "receive_date": "2019-08-24T14:15:22Z",
      "status": "ORDER_STATUS_UNSPECIFIED",
      "supply_id": "string",
      "warehouse_id": 0
    }
  ],
  "last_id": 0
}
```

---

## Получить список отправлений

`POST /v1/posting/fbp/list`

Operation ID: `PostingFbpList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/posting/fbp/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cursor": "string",
  "filter": {
    "name": "string",
    "offer_id": "string",
    "posting_numbers": [
      "string"
    ],
    "since": "2019-08-24T14:15:22Z",
    "statuses": [
      "string"
    ],
    "to": "2019-08-24T14:15:22Z"
  },
  "limit": 1,
  "sort_by": "string",
  "sort_dir": "ASC"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр для поиска отправлений.
- `limit` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_by` — string Параметр, по которому сортируются отправления: last_change_status_date — по дате последнего изменения статуса; in_process_at — по дате начала обработки.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `postings` — Array of objects Список отправлений.

Пример ответа:
```json
{
  "cursor": "string",
  "postings": [
    {
      "financial_data": {
        "cluster_from": "string",
        "cluster_to": "string",
        "delivery_amount": 0,
        "products": [
          {
            "actions": [
              {
                "action_id": "string",
                "date_from": "2019-08-24T14:15:22Z",
                "date_to": "2019-08-24T14:15:22Z",
                "discount_percent": 0,
                "discount_value": 0,
                "is_from_seller": true,
                "description": "string"
              }
            ],
            "commissions_currency_code": "string",
            "old_price": 0,
            "price": 0,
            "product_id": 0,
            "quantity": 0,
            "total_discount_percent": 0,
            "total_discount_value": 0
          }
        ]
      },
      "in_process_at": "2019-08-24T14:15:22Z",
      "order_date": "2019-08-24T14:15:22Z",
      "order_id": 0,
      "order_number": "string",
      "posting_number": "string",
      "products": [
        {
          "customer_price": {
            "amount": "string",
            "currency": "string"
          },
          "name": "string",
          "offer_id": "string",
          "price": {
            "amount": "string",
            "currency": "string"
          },
          "quantity": 0,
          "seller_price": {
            "amount": "string",
            "currency": "string"
          },
          "sku": 0
        }
      ],
      "provider_id": 0,
      "status": "string"
    }
  ]
}
```

---
