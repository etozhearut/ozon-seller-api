# Создание и управление заявками на поставку FBO

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FboSupplyRequest` · методов: 30_

## Получить информацию о макролокальных кластерах

`POST /v2/cluster/list`

Operation ID: `DraftClusterList`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/cluster/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Макролокальные кластеры
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Список кластеров.
  - `data` — object Кластер.
  - `macrolocal_cluster_id` — integer <int64> Идентификатор кластера.

Пример ответа:
```json
{
  "result": [
    {
      "data": {
        "fulfillments": [
          {
            "name": "string",
            "warehouse_id": 0
          }
        ],
        "macrolocal_cluster": {
          "country": {
            "name": "string",
            "uid": "string"
          },
          "name": "string"
        }
      },
      "macrolocal_cluster_id": 0
    }
  ]
}
```

---

## Информация о кластерах и их складах

`POST /v1/cluster/list`

Operation ID: `SupplyDraftAPI_DraftClusterList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cluster/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cluster_type": "CLUSTER_TYPE_OZON"
}'
```

### Тело запроса

- `cluster_ids` — Array of strings <int64> Идентификаторы кластеров.
- `cluster_type required` — string Enum: "CLUSTER_TYPE_OZON" "CLUSTER_TYPE_CIS" Тип кластера: CLUSTER_TYPE_OZON — кластер в России, CLUSTER_TYPE_CIS — кластер в СНГ.

### Ответы

- 200 Информация о кластерах

#### Схема ответа (200)

- `clusters` — Array of objects Кластеры.
  - `id` — integer <int64> Идентификатор кластера.
  - `logistic_clusters` — Array of objects Информация о складах кластера.
  - `macrolocal_cluster_id` — integer <int64> Идентификатор кластера размещения.
  - `name` — string Название кластера.
  - `type` — string Enum: "CLUSTER_TYPE_OZON" "CLUSTER_TYPE_CIS" Тип кластера: CLUSTER_TYPE_OZON — кластер в России, CLUSTER_TYPE_CIS — кластер в СНГ.

Пример ответа:
```json
{
  "clusters": [
    {
      "id": 0,
      "logistic_clusters": [
        {
          "warehouses": [
            {
              "name": "string",
              "type": "FULL_FILLMENT",
              "warehouse_id": 0
            }
          ]
        }
      ],
      "macrolocal_cluster_id": 0,
      "name": "string",
      "type": "CLUSTER_TYPE_OZON"
    }
  ]
}
```

---

## Поиск точек для отгрузки поставки

`POST /v1/warehouse/fbo/list`

Operation ID: `SupplyDraftAPI_DraftGetWarehouseFboList`

Используйте метод, чтобы найти точки отгрузки для кросс-докинга и прямых поставок. Вы можете посмотреть адреса всех точек на карте и в виде таблицы в Базе знаний .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbo/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter_by_supply_type": [
    "CREATE_TYPE_CROSSDOCK"
  ],
  "search": "string"
}'
```

### Тело запроса

- `filter_by_supply_type required` — Array of strings Items Enum: "CREATE_TYPE_CROSSDOCK" "CREATE_TYPE_DIRECT" Тип поставки: CREATE_TYPE_CROSSDOCK — кросс-докинг, CREATE_TYPE_DIRECT — прямая.
- `search required` — string >= 4 characters Поиск по названию склада. Для поиска пунктов выдачи заказов укажите полное название.

### Ответы

- 200 Информация о складах

#### Схема ответа (200)

- `search` — Array of objects Результат поиска складов.
  - `address` — string Адрес склада.
  - `coordinates` — object Координаты склада.
  - `name` — string Название склада.
  - `warehouse_id` — integer <int64> Идентификатор склада, пункта выдачи заказов или сортировочного центра.
  - `warehouse_type` — string Enum: "WAREHOUSE_TYPE_DELIVERY_POINT" "WAREHOUSE_TYPE_ORDERS_RECEIVING_POINT" "WAREHOUSE_TYPE_SORTING_CENTER" "WAREHOUSE_TYPE_FULL_FILLMENT" "WAREHOUSE_TYPE_CROSS_DOCK" Тип склада, пункта выдачи заказов или сортировочного центра: WAREHOUSE_TYPE_DELIVERY_POINT — пункт выдачи заказов, WAREHOUSE_TYPE_ORDERS_RECEIVING_POINT — пункт приёма заказов, WAREHOUSE_TYPE_SORTING_CENTER — сортировочный центр, WAREHOUSE_TYPE_FULL_FILLMENT — фулфилмент, WAREHOUSE_TYPE_CROSS_DOCK — кросс-докинг.

Пример ответа:
```json
{
  "search": [
    {
      "address": "string",
      "coordinates": {
        "latitude": 0,
        "longitude": 0
      },
      "name": "string",
      "warehouse_id": 0,
      "warehouse_type": "WAREHOUSE_TYPE_DELIVERY_POINT"
    }
  ]
}
```

---

## Создать черновик заявки на поставку

`POST /v1/draft/create`

Operation ID: `SupplyDraftAPI_DraftCreate`

Метод устаревает и будет отключён 16 марта 2026 года. Используйте /v1/draft/crossdock/create , /v1/draft/direct/create или /v1/draft/multi-cluster/create . Черновик заявки на поставку доступен 30 минут. Вы можете создавать черновики заявки на поставку 2 раза в минуту и 50 раз в час. Максимум — 500 черновиков в день. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cluster_ids": [
    "string"
  ],
  "drop_off_point_warehouse_id": 0,
  "items": [
    {
      "quantity": 0,
      "sku": 0
    }
  ],
  "type": "CREATE_TYPE_CROSSDOCK"
}'
```

### Тело запроса

- `cluster_ids` — Array of strings <int64> Идентификаторы кластеров для поставки. Получите методом /v1/cluster/list .
- `drop_off_point_warehouse_id` — integer <int64> Идентификатор точки отгрузки — пункта выдачи заказов или сортировочного центра. Можно получить с помощью метода /v1/warehouse/fbo/list . Только для типа поставки type = CREATE_TYPE_CROSSDOCK .
- `items required` — Array of objects <= 5000 items Товары.
- `type required` — string Enum: "CREATE_TYPE_CROSSDOCK" "CREATE_TYPE_DIRECT" Тип поставки: CREATE_TYPE_CROSSDOCK — кросс-докинг, CREATE_TYPE_DIRECT — прямая.

### Ответы

- 200 Черновик заявки создан

#### Схема ответа (200)

- `operation_id` — string Идентификатор черновика заявки на поставку.

---

## Создать черновик заявки на поставку кросс-докингом

`POST /v1/draft/crossdock/create`

Operation ID: `DraftCrossdockCreate`

Черновик заявки на поставку не отображается в личном кабинете продавца и доступен 30 минут. Вы можете создавать черновики заявки на поставку: 2 раза в минуту; 50 раз в час; 500 раз в день. Если превысите лимит, вернётся ошибка 429.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/crossdock/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cluster_info": {
    "items": [
      {
        "quantity": 0,
        "sku": 0
      }
    ],
    "macrolocal_cluster_id": 0
  },
  "deletion_sku_mode": "PARTIAL",
  "delivery_info": {
    "drop_off_warehouse": {
      "warehouse_id": 0,
      "warehouse_type": "DELIVERY_POINT"
    },
    "seller_warehouse_id": 0,
    "type": "DROPOFF"
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cluster_info required` — object Информация о кластере.
- `deletion_sku_mode` — string Default: "PARTIAL" Enum: "PARTIAL" "FULL" Режим удаления SKU, которые не попали в поставку. Возможные значения: PARTIAL — система удалит только те единицы SKU, которые не прошли проверку; FULL — система удалит все единицы SKU, если хотя бы 1 единица этого SKU не прошла проверку.
- `delivery_info required` — object Информация о доставке.

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика. Используйте в методе /v2/draft/create/info .
- `errors` — Array of objects Ошибки.

Пример ответа:
```json
{
  "draft_id": 0,
  "errors": [
    {
      "error_message": "UNSPECIFIED",
      "error_reasons": [
        "UNSPECIFIED"
      ],
      "items_validation": [
        {
          "macrolocal_cluster_id": 0,
          "rejected_items": [
            {
              "reasons": [
                "UNSPECIFIED"
              ],
              "sku": 0
            }
          ]
        }
      ],
      "macrolocal_cluster_ids": [
        "string"
      ],
      "message": "string",
      "skus": [
        "string"
      ]
    }
  ]
}
```

---

## Создать черновик заявки на прямую поставку

`POST /v1/draft/direct/create`

Operation ID: `DraftDirectCreate`

Черновик заявки на поставку не отображается в личном кабинете продавца и доступен 30 минут. Вы можете создавать черновики заявки на поставку: 2 раза в минуту; 50 раз в час; 500 раз в день. Если превысите лимит, вернётся ошибка 429.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/direct/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cluster_info": {
    "items": [
      {
        "quantity": 0,
        "sku": 0
      }
    ],
    "macrolocal_cluster_id": 0
  },
  "deletion_sku_mode": "PARTIAL"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cluster_info required` — object Информация о кластере.
- `deletion_sku_mode` — string Default: "PARTIAL" Enum: "PARTIAL" "FULL" Режим удаления SKU, которые не попали в поставку. Возможные значения: PARTIAL — система удалит только те единицы SKU, которые не прошли проверку; FULL — система удалит все единицы SKU, если хотя бы 1 единица этого SKU не прошла проверку.

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика. Используйте в методе /v2/draft/create/info .
- `errors` — Array of objects Ошибки.

Пример ответа:
```json
{
  "draft_id": 0,
  "errors": [
    {
      "error_message": "UNSPECIFIED",
      "error_reasons": [
        "UNSPECIFIED"
      ],
      "items_validation": [
        {
          "macrolocal_cluster_id": 0,
          "rejected_items": [
            {
              "reasons": [
                "UNSPECIFIED"
              ],
              "sku": 0
            }
          ]
        }
      ],
      "macrolocal_cluster_ids": [
        "string"
      ],
      "message": "string",
      "skus": [
        "string"
      ]
    }
  ]
}
```

---

## Создать черновик заявки на поставку для нескольких кластеров

`POST /v1/draft/multi-cluster/create`

Operation ID: `DraftMultiClusterCreate`

Черновик заявки на поставку не отображается в личном кабинете продавца и доступен 30 минут. Вы можете создавать черновики заявки на поставку: 2 раза в минуту; 50 раз в час; 500 раз в день. Если превысите лимит, вернётся ошибка 429.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/multi-cluster/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "clusters_info": [
    {
      "items": [
        {
          "quantity": 0,
          "sku": 0
        }
      ],
      "macrolocal_cluster_id": 0
    }
  ],
  "deletion_sku_mode": "PARTIAL",
  "delivery_info": {
    "drop_off_warehouse": {
      "warehouse_id": 0,
      "warehouse_type": "DELIVERY_POINT"
    },
    "seller_warehouse_id": 0,
    "type": "DROPOFF"
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `clusters_info required` — Array of objects <= 20 items Информация о кластерах.
- `deletion_sku_mode` — string Default: "PARTIAL" Enum: "PARTIAL" "FULL" Режим удаления SKU, которые не попали в поставку. Возможные значения: PARTIAL — система удалит только те единицы SKU, которые не прошли проверку; FULL — система удалит все единицы SKU, если хотя бы 1 единица этого SKU не прошла проверку.
- `delivery_info required` — object Информация о доставке.

### Ответы

- 200 Черновик создан

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика. Используйте в методе /v2/draft/create/info .
- `errors` — Array of objects Ошибки.

Пример ответа:
```json
{
  "draft_id": 0,
  "errors": [
    {
      "error_message": "UNSPECIFIED",
      "error_reasons": [
        "UNSPECIFIED"
      ],
      "items_validation": [
        {
          "macrolocal_cluster_id": 0,
          "rejected_items": [
            {
              "reasons": [
                "UNSPECIFIED"
              ],
              "sku": 0
            }
          ]
        }
      ],
      "macrolocal_cluster_ids": [
        "string"
      ],
      "message": "string",
      "skus": [
        "string"
      ]
    }
  ]
}
```

---

## Получить информацию о черновике заявки на поставку

`POST /v2/draft/create/info`

Operation ID: `DraftCreateInfo`

Вы можете создавать черновики заявки на поставку 2 раза в минуту и 50 раз в час. Если превысите лимит, вернётся ошибка 429.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/draft/create/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `draft_id required` — integer <int64> Идентификатор черновика из методов /v1/draft/crossdock/create , /v1/draft/direct/create или /v1/draft/multi-cluster/create .

### Ответы

- 200 Информация о черновике

#### Схема ответа (200)

- `clusters` — Array of objects Кластеры.
- `errors` — Array of objects Ошибки.
- `status` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "SUCCESS" "IN_PROGRESS" "FAILED" Статус создания черновика заявки на поставку: UNSPECIFIED — не определён, SUCCESS — создан, IN_PROGRESS — создаётся, FAILED — не удалось создать.

Пример ответа:
```json
{
  "clusters": [
    {
      "cluster_name": "string",
      "macrolocal_cluster_id": 0,
      "supply_type": "CROSSDOCK",
      "warehouses": [
        {
          "availability_status": {
            "invalid_reason": "UNSPECIFIED",
            "state": "UNSPECIFIED"
          },
          "bundle_id": "string",
          "restricted_bundle_id": "string",
          "storage_warehouse": {
            "address": "string",
            "name": "string",
            "warehouse_id": 0
          },
          "supply_tags": [
            "UNSPECIFIED"
          ],
          "total_rank": 0,
          "total_score": 0
        }
      ]
    }
  ],
  "errors": [
    {
      "error_message": "UNSPECIFIED",
      "error_reasons": [
        "UNSPECIFIED"
      ],
      "items_validation": [
        {
          "macrolocal_cluster_id": 0,
          "rejected_items": [
            {
              "reasons": [
                "UNSPECIFIED"
              ],
              "sku": 0
            }
          ]
        }
      ],
      "macrolocal_cluster_ids": [
        "string"
      ],
      "message": "string",
      "skus": [
        "string"
      ]
    }
  ],
  "status": "UNSPECIFIED"
}
```

---

## Информация о черновике заявки на поставку

`POST /v1/draft/create/info`

Operation ID: `SupplyDraftAPI_DraftCreateInfo`

Метод устаревает и будет отключён 16 марта 2026 года. Используйте /v2/draft/create/info . Вы можете создавать черновики заявки на поставку 2 раза в минуту и 50 раз в час. Если превысите лимит, вернётся ошибка 429. Возвращает информацию о созданном черновике заявки на поставку. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/create/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "operation_id": "0191c1c8-041c-754f-bba1-05de7741f9cb"
}'
```

### Тело запроса

- `operation_id required` — string Уникальный идентификатор генерации черновика заявки на поставку.

### Ответы

- 200 Информация о черновике заявки на поставку

#### Схема ответа (200)

- `clusters` — Array of objects Кластеры.
- `draft_id` — integer <int64> Идентификатор черновика заявки на поставку.
- `errors` — Array of objects Ошибки.
- `status` — string Default: "CALCULATION_STATUS_FAILED" Enum: "CALCULATION_STATUS_FAILED" "CALCULATION_STATUS_SUCCESS" "CALCULATION_STATUS_IN_PROGRESS" "CALCULATION_STATUS_EXPIRED" Статус создания черновика заявки на поставку: CALCULATION_STATUS_FAILED — не удалось создать черновик, CALCULATION_STATUS_SUCCESS — черновик создан, CALCULATION_STATUS_IN_PROGRESS — черновик создаётся, CALCULATION_STATUS_EXPIRED — истёк срок действия черновика.

Пример ответа:
```json
{
  "clusters": [
    {
      "cluster_id": 0,
      "cluster_name": "string",
      "warehouses": [
        {
          "bundle_ids": [
            {
              "bundle_id": "string",
              "is_docless": true
            }
          ],
          "restricted_bundle_id": "string",
          "status": {
            "invalid_reason": "WAREHOUSE_SCORING_INVALID_REASON_UNSPECIFIED",
            "is_available": true,
            "state": "WAREHOUSE_SCORING_STATUS_FULL_AVAILABLE"
          },
          "supply_warehouse": {
            "address": "string",
            "name": "string",
            "warehouse_id": 0
          },
          "total_rank": 0,
          "total_score": 0,
          "travel_time_days": 0
        }
      ]
    }
  ],
  "draft_id": 30957724,
  "errors": [
    {
      "error_message": "string",
      "items_validation": [
        {
          "reasons": [
            "string"
          ],
          "sku": 0
        }
      ],
      "unknown_cluster_ids": [
        "string"
      ]
    }
  ],
  "status": "CALCULATION_STATUS_FAILED"
}
```

---

## Получить список доступных таймслотов

`POST /v2/draft/timeslot/info`

Operation ID: `DraftTimeslotInfo`

Не используйте в запросе draft_id из метода /v1/draft/create/info , иначе вернётся ошибка.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/draft/timeslot/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "string",
  "date_to": "string",
  "draft_id": 0,
  "supply_type": "CROSSDOCK",
  "selected_cluster_warehouses": [
    {
      "macrolocal_cluster_id": 0,
      "storage_warehouse_id": 0
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string YYYY-MM-DD Дата начала периода доступных таймслотов.
- `date_to required` — string YYYY-MM-DD Дата окончания периода доступных таймслотов. Максимальный период — 28 дней с текущей даты.
- `draft_id required` — integer <int64> Идентификатор черновика из метода /v2/draft/create/info .
- `supply_type required` — string Enum: "CROSSDOCK" "DIRECT" "MULTI_CLUSTER" Тип поставки: CROSSDOCK — кросс-докинг; DIRECT — прямая; MULTI_CLUSTER — для нескольких кластеров.
- `selected_cluster_warehouses required` — Array of objects <= 20 items Информация о кластере и складах в нём. Можно передать один кластер для кросс-докинговой и прямой поставки или список всех кластеров для поставки в несколько кластеров.

### Ответы

- 200 Список таймслотов

#### Схема ответа (200)

- `error_reason` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "INVALID_CLUSTERS_COUNT" "REQUESTED_PERIOD_MORE_THAN_MAX" "UNDEFINED" Причина ошибки: UNSPECIFIED — не определена; INVALID_CLUSTERS_COUNT — переданы не все кластеры из расчёта; REQUESTED_PERIOD_MORE_THAN_MAX — превышен период; INVALID_REQUESTED_CLUSTER_IDS — переданы кластеры, которых нет в расчёте. UNDEFINED — неизвестная ошибка.
- `result` — object Информация о таймслотах.
  - `drop_off_warehouse_timeslots` — object Таймслоты складов.
  - `requested_date_from` — string Дата начала периода.
  - `requested_date_to` — string Дата окончания периода.

Пример ответа:
```json
{
  "error_reason": "UNSPECIFIED",
  "result": {
    "drop_off_warehouse_timeslots": {
      "current_time_in_timezone": "string",
      "days": [
        {
          "date_in_timezone": "string",
          "timeslots": [
            {
              "from_in_timezone": "string",
              "to_in_timezone": "string"
            }
          ]
        }
      ],
      "warehouse_timezone": "string"
    },
    "requested_date_from": "string",
    "requested_date_to": "string"
  }
}
```

---

## Доступные таймслоты

`POST /v1/draft/timeslot/info`

Operation ID: `SupplyDraftAPI_DraftTimeslotInfo`

Метод устаревает и будет отключён 16 марта 2026 года. Используйте /v2/draft/timeslot/info . Черновик заявки на поставку доступен 30 минут. Возвращает доступные таймслоты на конечных складах отгрузки. Для кросс-док поставок вернутся таймслоты склада отгрузки, который был передан при создании черновика.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/timeslot/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2019-08-24T14:15:22Z",
  "date_to": "2019-08-24T14:15:22Z",
  "draft_id": 0,
  "warehouse_ids": [
    "string"
  ]
}'
```

### Тело запроса

- `date_from required` — string <date-time> Дата начала нужного периода доступных таймслотов.
- `date_to required` — string <date-time> Дата окончания нужного периода доступных таймслотов. Максимальный период — 28 дней с текущей даты.
- `draft_id required` — integer <int64> Идентификатор черновика заявки на поставку. Получите методом /v1/draft/create/info .
- `warehouse_ids required` — Array of strings <int64> <= 10 items Идентификаторы складов размещения.

### Ответы

- 200 Таймслоты

#### Схема ответа (200)

- `drop_off_warehouse_timeslots` — Array of objects Таймслоты складов.
- `requested_date_from` — string <date-time> Дата начала интересующего периода.
- `requested_date_to` — string <date-time> Дата окончания интересующего периода.

Пример ответа:
```json
{
  "drop_off_warehouse_timeslots": [
    {
      "current_time_in_timezone": "2019-08-24T14:15:22Z",
      "days": [
        {
          "date_in_timezone": "2019-08-24T14:15:22Z",
          "timeslots": [
            {
              "from_in_timezone": "2019-08-24T14:15:22Z",
              "to_in_timezone": "2019-08-24T14:15:22Z"
            }
          ]
        }
      ],
      "drop_off_warehouse_id": 0,
      "warehouse_timezone": "string"
    }
  ],
  "requested_date_from": "2019-08-24T14:15:22Z",
  "requested_date_to": "2019-08-24T14:15:22Z"
}
```

---

## Создать заявку на поставку по черновику

`POST /v2/draft/supply/create`

Operation ID: `DraftSupplyCreate`

Вы можете создавать заявку на поставку по черновику: 2 раза в минуту; 50 раз в час; 500 раз в день. Если превысите лимит, вернётся ошибка 429.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/draft/supply/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "draft_id": 0,
  "selected_cluster_warehouses": [
    {
      "macrolocal_cluster_id": 0,
      "storage_warehouse_id": 0
    }
  ],
  "timeslot": {
    "from_in_timezone": "string",
    "to_in_timezone": "string"
  },
  "supply_type": "CROSSDOCK"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `draft_id required` — integer <int64> Идентификатор черновика из метода /v2/draft/create/info .
- `selected_cluster_warehouses required` — Array of objects Информация о кластере и складах в нём. Можно передать один кластер для кросс-докинговой и прямой поставки или список всех кластеров для поставки в несколько кластеров.
- `timeslot` — object Таймслот поставки.
- `supply_type required` — string Enum: "CROSSDOCK" "DIRECT" "MULTI_CLUSTER" Тип поставки: CROSSDOCK — кросс-докинг; DIRECT — прямая; MULTI_CLUSTER — для нескольких кластеров.

### Ответы

- 200 Заявка создана

#### Схема ответа (200)

- `draft_id` — integer <int64> Идентификатор черновика.
- `error_reasons` — Array of strings Items Enum: "UNSPECIFIED" "SOME_SERVICE_ERROR" "ORDER_SKU_LIMIT" "INVALID_QUANTITY_OR_QUANT" "ORDER_ALREADY_CREATED" "ORDER_CREATION_IN_PROGRESS" "DRAFT_DOES_NOT_EXIST" "CONTRACTOR_CAN_NOT_CREATE_ORDER" "INACTIVE_CONTRACT" "DRAFT_INCORRECT_STATE" "INVALID_VOLUME" "INVALID_ROUTE" "INVALID_STORAGE_WAREHOUSE" "INVALID_STORAGE_REGION" "INVALID_SPLITTING" "INVALID_SUPPLY_CONTENT" "TIMESLOT_NOT_AVAILABLE" "SKU_DISTRIBUTION_REQUIRED_BUT_NOT_POSSIBLE" "XDOCK_IN_DELIVERY_POINT_DISABLED_FOR_SELLER" "DRAFT_IS_LOCKED" "INVALID_PACKAGE_UNITS_COUNTS" "SELLER_CONVERSATION_DOES_NOT_EXIST" "USER_CAN_NOT_CREATE_SELLER_CONVERSATION" "SKU_WITH_ETTN_REQUIRED_TAG_NOT_ALLOWED_FOR_DROP_OFF_POINT" "INVALID_SELLER_WAREHOUSE" "PICKUP_ORDER_LIMIT_EXCEEDED" "MINIMUM_VOLUME_IN_LITRES_INVALID" "INVALID_CLUSTERS_COUNT" "CAN_NOT_CREATE_ORDER" "UNDEFINED" Причина ошибки: UNSPECIFIED — не определена; SOME_SERVICE_ERROR — ошибка при редактировании поставки; ORDER_SKU_LIMIT — количество товаров в поставке больше 5000; INVALID_QUANTITY_OR_QUANT — некорректное количество товара или грузомест; ORDER_ALREADY_CREATED — заказ уже создан; ORDER_CREATION_IN_PROGRESS — создание заказа в процессе; DRAFT_DOES_NOT_EXIST — черновик не существует; CONTRACTOR_CAN_NOT_CREATE_ORDER — контрагент не может создать заказ; INACTIVE_CONTRACT — нельзя редактировать состав поставки с неактивным договором; DRAFT_INCORRECT_STATE — некорректный статус черновика; INVALID_VOLUME — некорректный объём поставки; INVALID_ROUTE — некорректный маршрут; INVALID_STORAGE_WAREHOUSE — некорректный склад хранения; INVALID_STORAGE_REGION — некорректный регион хранения; INVALID_SPLITTING — некорректное разделение; INVALID_SUPPLY_CONTENT — некорректное содержимое поставки; TIMESLOT_NOT_AVAILABLE — нет доступных таймслотов; SKU_DISTRIBUTION_REQUIRED_BUT_NOT_POSSIBLE — требуется распределение SKU, но оно невозможно; XDOCK_IN_DELIVERY_POINT_DISABLED_FOR_SELLER — поставка кросс-докингом через пункт выдачи заказов недоступна для продавца; DRAFT_IS_LOCKED — черновик заблокирован; INVALID_PACKAGE_UNITS_COUNTS — некорректное количество грузомест; SELLER_CONVERSATION_DOES_NOT_EXIST — точка отгрузки с таким id не существует; USER_CAN_NOT_CREATE_SELLER_CONVERSATION — пользователь не может создать диалог с продавцом; SKU_WITH_ETTN_REQUIRED_TAG_NOT_ALLOWED_FOR_DROP_OFF_POINT — товар с меткой is_ettn_required не разрешён для точки отгрузки; INVALID_SELLER_WAREHOUSE — склад продавца недоступен; PICKUP_ORDER_LIMIT_EXCEEDED — превышен лимит заказов на самовывоз; MINIMUM_VOLUME_IN_LITRES_INVALID — некорректный минимальный объём в литрах; INVALID_CLUSTERS_COUNT — переданы не все кластеры из расчёта; CAN_NOT_CREATE_ORDER — не удалось создать заказ; UNDEFINED — неизвестная ошибка.

Пример ответа:
```json
{
  "draft_id": 0,
  "error_reasons": [
    "UNSPECIFIED"
  ]
}
```

---

## Создать заявку на поставку по черновику

`POST /v1/draft/supply/create`

Operation ID: `SupplyDraftAPI_DraftSupplyCreate`

Метод устаревает и будет отключён 16 марта 2026 года. Используйте /v2/draft/supply/create .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/supply/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "draft_id": 0,
  "timeslot": {
    "from_in_timezone": "2019-08-24T14:15:22Z",
    "to_in_timezone": "2019-08-24T14:15:22Z"
  },
  "warehouse_id": 0
}'
```

### Тело запроса

- `draft_id required` — integer <int64> Идентификатор черновика заявки на поставку.
- `timeslot` — object Таймслот поставки.
- `warehouse_id required` — integer <int64> Идентификатор склада размещения. Можно получить с помощью метода /v1/draft/create/info .

### Ответы

- 200 Заявка создана

#### Схема ответа (200)

- `operation_id` — string Идентификатор заявки на поставку.

---

## Получить информацию о создании заявки на поставку

`POST /v2/draft/supply/create/status`

Operation ID: `DraftSupplyCreateStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/draft/supply/create/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `draft_id required` — integer <int64> Идентификатор черновика. Получите значение параметра методом /v2/draft/supply/create .

### Ответы

- 200 Информация о создании заявки на поставку

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "UNSPECIFIED" "SOME_SERVICE_ERROR" "ORDER_SKU_LIMIT" "INVALID_QUANTITY_OR_QUANT" "ORDER_ALREADY_CREATED" "ORDER_CREATION_IN_PROGRESS" "DRAFT_DOES_NOT_EXIST" "CONTRACTOR_CAN_NOT_CREATE_ORDER" "INACTIVE_CONTRACT" "DRAFT_INCORRECT_STATE" "INVALID_VOLUME" "INVALID_ROUTE" "INVALID_STORAGE_WAREHOUSE" "INVALID_STORAGE_REGION" "INVALID_SPLITTING" "INVALID_SUPPLY_CONTENT" "TIMESLOT_NOT_AVAILABLE" "SKU_DISTRIBUTION_REQUIRED_BUT_NOT_POSSIBLE" "XDOCK_IN_DELIVERY_POINT_DISABLED_FOR_SELLER" "DRAFT_IS_LOCKED" "INVALID_PACKAGE_UNITS_COUNTS" "SELLER_CONVERSATION_DOES_NOT_EXIST" "USER_CAN_NOT_CREATE_SELLER_CONVERSATION" "SKU_WITH_ETTN_REQUIRED_TAG_NOT_ALLOWED_FOR_DROP_OFF_POINT" "INVALID_SELLER_WAREHOUSE" "PICKUP_ORDER_LIMIT_EXCEEDED" "INVALID_CLUSTERS_COUNT" "UNDEFINED" Причина ошибки: UNSPECIFIED — не определена; SOME_SERVICE_ERROR — ошибка при редактировании поставки; ORDER_SKU_LIMIT — количество товаров в поставке больше 5000; INVALID_QUANTITY_OR_QUANT — некорректное количество товара или грузомест; ORDER_ALREADY_CREATED — заказ уже создан; ORDER_CREATION_IN_PROGRESS — создание заказа в процессе; DRAFT_DOES_NOT_EXIST — черновик не существует; CONTRACTOR_CAN_NOT_CREATE_ORDER — контрагент не может создать заказ; INACTIVE_CONTRACT — нельзя редактировать состав поставки с неактивным договором; DRAFT_INCORRECT_STATE — некорректный статус черновика; INVALID_VOLUME — некорректный объём поставки; INVALID_ROUTE — некорректный маршрут; INVALID_STORAGE_WAREHOUSE — некорректный склад хранения; INVALID_STORAGE_REGION — некорректный регион хранения; INVALID_SPLITTING — некорректное разделение; INVALID_SUPPLY_CONTENT — некорректное содержимое поставки; TIMESLOT_NOT_AVAILABLE — нет доступных таймслотов; SKU_DISTRIBUTION_REQUIRED_BUT_NOT_POSSIBLE — требуется распределение SKU, но оно невозможно; XDOCK_IN_DELIVERY_POINT_DISABLED_FOR_SELLER — поставка кросс-докингом через пункт выдачи заказов недоступна для продавца; DRAFT_IS_LOCKED — черновик заблокирован; INVALID_PACKAGE_UNITS_COUNTS — некорректное количество грузомест; SELLER_CONVERSATION_DOES_NOT_EXIST — точка отгрузки с таким id не существует; USER_CAN_NOT_CREATE_SELLER_CONVERSATION — пользователь не может написать продавцу; SKU_WITH_ETTN_REQUIRED_TAG_NOT_ALLOWED_FOR_DROP_OFF_POINT — товар с меткой is_ettn_required не разрешён для точки отгрузки; INVALID_SELLER_WAREHOUSE — склад продавца недоступен; PICKUP_ORDER_LIMIT_EXCEEDED — превышен лимит заказов на самовывоз; MINIMUM_VOLUME_IN_LITRES_INVALID — некорректный минимальный объём в литрах; INVALID_CLUSTERS_COUNT — переданы не все кластеры из расчёта; UNDEFINED — неизвестная ошибка.
- `order_id` — integer <int64> Идентификатор заявки на поставку.
- `status` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "SUCCESS" "IN_PROGRESS" "FAILED" Статус создания заявки на поставку: UNSPECIFIED — не определён, SUCCESS — создана, IN_PROGRESS — создаётся, FAILED — не удалось создать.

Пример ответа:
```json
{
  "error_reasons": [
    "UNSPECIFIED"
  ],
  "order_id": 0,
  "status": "UNSPECIFIED"
}
```

---

## Информация о создании заявки на поставку

`POST /v1/draft/supply/create/status`

Operation ID: `SupplyDraftAPI_DraftSupplyCreateStatus`

Метод устаревает и будет отключён 16 марта 2026 года. Используйте /v2/draft/supply/create/status .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/draft/supply/create/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `operation_id required` — string Идентификатор заявки на поставку.

### Ответы

- 200 Информация о создании заявки на поставку

#### Схема ответа (200)

- `error_messages` — Array of strings Ошибки создания заявок.
- `result` — object Идентификаторы заявок на поставку.
  - `order_ids` — Array of strings <int64> Идентификаторы заявок на поставку.
- `status` — string Default: "DraftSupplyCreateStatusUnknown" Enum: "DraftSupplyCreateStatusUnknown" "DraftSupplyCreateStatusSuccess" "DraftSupplyCreateStatusFailed" "DraftSupplyCreateStatusInProgress" Статус создания заявки на поставку: DraftSupplyCreateStatusUnknown — неизвестный, DraftSupplyCreateStatusSuccess — создана, DraftSupplyCreateStatusFailed — не создана, DraftSupplyCreateStatusInProgress — создаётся.

Пример ответа:
```json
{
  "error_messages": [
    "string"
  ],
  "result": {
    "order_ids": [
      "string"
    ]
  },
  "status": "DraftSupplyCreateStatusUnknown"
}
```

---

## Установка грузомест

`POST /v1/cargoes/create`

Operation ID: `CargoesAPI_CargoesCreate`

Используйте метод, чтобы передать грузоместа и товарный состав в заявку на поставку.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cargoes": [
    {
      "key": "box-001",
      "value": {
        "items": [
          {
            "barcode": "4601234567890",
            "expires_at": "2026-06-30T23:59:59Z",
            "offer_id": "12345678",
            "quant": 1,
            "quantity": 5
          },
          {
            "barcode": "4609876543210",
            "expires_at": "2026-12-31T23:59:59Z",
            "offer_id": "87654321",
            "quant": 2,
            "quantity": 3
          }
        ],
        "type": "BOX"
      }
    }
  ],
  "delete_current_version": true,
  "supply_id": 123456
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cargoes required` — Array of objects Информация о грузоместах. Вы можете передать не больше 40 палет или 30 коробок.
- `delete_current_version` — boolean true , если нужно удалить предыдущие грузоместа.
- `supply_id required` — integer <int64> Идентификатор поставки. Можно получить с помощью метода /v3/supply-order/get . Нужное значение — в параметре ответа orders.supplies.supply_id .

### Ответы

- 200 Грузоместа установлены

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции.
- `errors` — object Ошибки.

Пример ответа:
```json
{
  "operation_id": "op-1689012345",
  "errors": {
    "error_reasons": [
      "INVALID_STATE"
    ],
    "items_validation": [
      {
        "barcode": "4601234567890",
        "cargo_key": "box-001",
        "quant": 2,
        "type": "SUPPLY_ITEM_NOT_FOUND"
      }
    ]
  }
}
```

---

## Получить информацию по установке грузомест

`POST /v2/cargoes/create/info`

Operation ID: `CargoesCreateInfoV2`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/cargoes/create/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Результат запроса

#### Схема ответа (200)

- `errors` — object Ошибки.
- `result` — object Результат запроса.
  - `cargoes` — Array of objects Информация о грузоместах.
- `status` — string Default: "STATUS_UNSPECIFIED" Enum: "STATUS_UNSPECIFIED" "SUCCESS" "IN_PROGRESS" "FAILED" Статус формирования грузоместа: SUCCESS — успешно; IN_PROGRESS — формируются; FAILED — при формировании грузомест произошла ошибка.

Пример ответа:
```json
{
  "errors": {
    "error_reasons": [
      "ERROR_REASON_UNSPECIFIED"
    ],
    "items_validation": [
      {
        "cargo_key": "string",
        "item": "string",
        "quant": 0,
        "type": "SUPPLY_ITEM_NOT_FOUND"
      }
    ]
  },
  "result": {
    "cargoes": [
      {
        "key": "string",
        "value": {
          "cargo_id": 0
        }
      }
    ]
  },
  "status": "STATUS_UNSPECIFIED"
}
```

---

## Получить информацию о грузоместах

`POST /v1/cargoes/get`

Operation ID: `CargoesGet`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `supply_ids required` — Array of strings <int64> <= 100 items Список идентификаторов поставок в заявке.

### Ответы

- 200 Информация о грузоместах

#### Схема ответа (200)

- `supply` — Array of objects Информация о грузоместах.
  - `bundle_id` — string Идентификатор товарного состава.
  - `cargoes` — Array of objects Грузоместа.
  - `supply_id` — integer <int64> Идентификатор поставки.

Пример ответа:
```json
{
  "supply": [
    {
      "bundle_id": "string",
      "cargoes": [
        {
          "bundle_id": "string",
          "cargo_id": 0,
          "content_type": "UNSPECIFIED",
          "placement_zone_type": "UNSPECIFIED",
          "tracking_info": {
            "date": "string",
            "status": "UNSPECIFIED",
            "type": "UNSPECIFIED"
          },
          "type": "UNSPECIFIED"
        }
      ],
      "supply_id": 0
    }
  ]
}
```

---

## Удалить грузоместо в заявке на поставку

`POST /v1/cargoes/delete`

Operation ID: `CargoesAPI_CargoesDelete`

Метод для удаления грузомест в заявке на поставку. Чтобы проверить статус удаления, используйте метод /v1/cargoes/delete/status .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cargo_ids": [
    "box-001"
  ],
  "supply_id": 123456
}'
```

### Тело запроса

- `cargo_ids required` — Array of strings <int64> Список идентификаторов грузомест, которые нужно удалить. Максимум 70 значений.
- `supply_id required` — integer <int64> Идентификатор поставки.

### Ответы

- 200 Грузоместо удалено

#### Схема ответа (200)

- `errors` — object Список ошибок, которые возникли при удалении грузомест.
- `operation_id` — string Идентификатор операции.

Пример ответа:
```json
{
  "errors": {
    "cargo_error_reasons": [
      {
        "cargo_id": 0,
        "error_reasons": [
          "CARGO_NOT_FOUND"
        ]
      }
    ],
    "supply_error_reasons": [
      "SUPPLY_NOT_FOUND"
    ]
  },
  "operation_id": "string"
}
```

---

## Информация о статусе удаления грузоместа

`POST /v1/cargoes/delete/status`

Operation ID: `CargoesAPI_CargoesDeleteStatus`

Метод для получения статуса удаления грузомест в заявке на поставку.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes/delete/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "operation_id": "test-operation-delete-12345-abcde"
}'
```

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Статус удаления грузоместа

#### Схема ответа (200)

- `errors` — object Список ошибок, которые возникли при удалении грузомест.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "ERROR" Статус удаления грузоместа. Возможные статусы: SUCCESS — грузоместо удалено, IN_PROGRESS — грузоместо в процессе удаления, ERROR — возникла ошибка при удалении грузоместа.

Пример ответа:
```json
{
  "status": "SUCCESS",
  "errors": {
    "supply_error_reasons": [],
    "cargo_error_reasons": []
  }
}
```

---

## Чек-лист по установке грузомест FBO

`POST /v1/cargoes/rules/get`

Operation ID: `CargoesAPI_CargoesRulesGet`

Метод для получения чек-листа с правилами по установке грузомест.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes/rules/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "supply_ids": [
    2000007863,
    2000007864,
    2000007865
  ]
}'
```

### Тело запроса

- `supply_ids required` — Array of strings <int64> Список идентификаторов поставок в заявке. Максимум 100 идентификаторов.

### Ответы

- 200 Чек-лист по установке грузомест

#### Схема ответа (200)

- `supply_check_lists` — Array of objects Список чек-листов с правилами заполнения грузомест по поставкам.
  - `cargoes_presents_rule` — object Правило указания грузомест.
  - `edit_deadline_expire_rule` — object Правило крайнего срока редактирования грузомест.
  - `expire_dates_presented_rule` — object Правило указания сроков годности для товаров.
  - `is_valid_distribution_rule` — object Правило совпадения составов грузомест с составом поставки.
  - `package_units_with_distribution_rule` — object Правило заполнения состава грузомест.
  - `placement_zones_rule` — object Правило распределения товаров в грузоместах по зонам размещения.
  - `supply_id` — integer <int64> Идентификатор поставки.

Пример ответа:
```json
{
  "supply_check_lists": [
    {
      "supply_id": 2000007863,
      "cargoes_presents_rule": {
        "count": 4,
        "satisfied": true,
        "cargo_count_per_type": [
          {
            "type": "BOX",
            "count": 3
          },
          {
            "type": "PALLET",
            "count": 1
          }
        ]
      },
      "edit_deadline_expire_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true
      },
      "expire_dates_presented_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true,
        "count_sku_with_expiration": 12,
        "count_sku_with_expiration_filled": 12
      },
      "is_valid_distribution_rule": {
        "is_applicable": true,
        "satisfied": true,
        "count_distributed_sku": 24,
        "count_sku_total": 24,
        "percents_int": 100
      },
      "package_units_with_distribution_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true,
        "count_all": 4,
        "count_with_distribution": 4
      },
      "placement_zones_rule": {
        "is_applicable": true,
        "satisfied": true,
        "count_cargoes_all": 4,
        "count_cargoes_with_mono_placement_zone": 4
      }
    },
    {
      "supply_id": 2000007864,
      "cargoes_presents_rule": {
        "count": 2,
        "satisfied": true,
        "cargo_count_per_type": [
          {
            "type": "BOX",
            "count": 2
          }
        ]
      },
      "edit_deadline_expire_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true
      },
      "expire_dates_presented_rule": {
        "is_applicable": false,
        "is_required": false,
        "satisfied": true,
        "count_sku_with_expiration": 0,
        "count_sku_with_expiration_filled": 0
      },
      "is_valid_distribution_rule": {
        "is_applicable": true,
        "satisfied": true,
        "count_distributed_sku": 8,
        "count_sku_total": 8,
        "percents_int": 100
      },
      "package_units_with_distribution_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true,
        "count_all": 2,
        "count_with_distribution": 2
      },
      "placement_zones_rule": {
        "is_applicable": true,
        "satisfied": true,
        "count_cargoes_all": 2,
        "count_cargoes_with_mono_placement_zone": 2
      }
    },
    {
      "supply_id": 2000007865,
      "cargoes_presents_rule": {
        "count": 0,
        "satisfied": false,
        "cargo_count_per_type": []
      },
      "edit_deadline_expire_rule": {
        "is_applicable": true,
        "is_required": true,
        "satisfied": true
      },
      "expire_dates_presented_rule": {
        "is_applicable": false,
        "is_required": false,
        "satisfied": true,
        "count_sku_with_expiration": 0,
        "count_sku_with_expiration_filled": 0
      },
      "is_valid_distribution_rule": {
        "is_applicable": false,
        "satisfied": false,
        "count_distributed_sku": 0,
        "count_sku_total": 5,
        "percents_int": 0
      },
      "package_units_with_distribution_rule": {
        "is_applicable": false,
        "is_required": false,
        "satisfied": false,
        "count_all": 0,
        "count_with_distribution": 0
      },
      "placement_zones_rule": {
        "is_applicable": false,
        "satisfied": false,
        "count_cargoes_all": 0,
        "count_cargoes_with_mono_placement_zone": 0
      }
    }
  ]
}
```

---

## Сгенерировать этикетки для грузомест

`POST /v1/cargoes-label/create`

Operation ID: `CargoesAPI_CargoesLabelCreate`

Используйте метод, чтобы сгенерировать этикетки для грузомест из заявки на поставку.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes-label/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "supply_id": 2000007863,
  "cargoes": [
    {
      "cargo_id": 50001234
    },
    {
      "cargo_id": 50001235
    },
    {
      "cargo_id": 50001236
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cargoes` — Array of objects Информация о грузоместах.
- `supply_id required` — integer <int64> Идентификатор поставки.

### Ответы

- 200 Результат запроса

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции.
- `errors` — object Ошибки.

Пример ответа:
```json
{
  "operation_id": "test-cargo-label-create-56789-abcde",
  "errors": {
    "error_reasons": []
  }
}
```

---

## Получить идентификатор этикетки для грузомест

`POST /v1/cargoes-label/get`

Operation ID: `CargoesAPI_CargoesLabelGet`

Возвращает статус формирования этикеток и ссылку на PDF-файл с ними.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/cargoes-label/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Этикетка для грузомест

#### Схема ответа (200)

- `result` — object Информация об этикетках.
  - `file_guid` — string Deprecated Идентификатор для получения файла с этикетками.
  - `file_url` — string Ссылка на PDF-файл с этикетками.
- `status` — string Default: "SUCCESS" Enum: "SUCCESS" "IN_PROGRESS" "FAILED" Статус формирования этикеток: SUCCESS — готовы. IN_PROGRESS — формируются. FAILED — ошибка при формировании.
- `errors` — object Ошибки.

Пример ответа:
```json
{
  "result": {
    "file_guid": "string",
    "file_url": "string"
  },
  "status": "SUCCESS",
  "errors": {
    "error_reasons": [
      "INVALID_STATE"
    ]
  }
}
```

---

## Получить PDF с этикетками грузовых мест

`GET /v1/cargoes-label/file/{file_guid}`

Operation ID: `CargoesAPI_CargoesLabelFile`

10 апреля 2026 года отключим метод. Переключитесь на /v1/cargoes-label/get .

```bash
curl -X GET "https://api-seller.ozon.ru/v1/cargoes-label/file/{file_guid}" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Этикетки грузовых мест

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

## Отменить заявку на поставку

`POST /v1/supply-order/cancel`

Operation ID: `SupplyOrderAPI_SupplyOrderCancel`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `order_id required` — integer <int64> Идентификатор заявки на поставку.

### Ответы

- 200 Отмена заявки на поставку в процессе

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции на отмену заявки.

---

## Получить статус отмены заявки на поставку

`POST /v1/supply-order/cancel/status`

Operation ID: `SupplyOrderAPI_SupplyOrderCancelStatus`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/cancel/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `operation_id required` — string Идентификатор операции на отмену заявки на поставку.

### Ответы

- 200 Статус отмены заявки на поставку

#### Схема ответа (200)

- `error_reasons` — Array of strings Items Enum: "INVALID_ORDER_STATE" "ORDER_IS_VIRTUAL" "ORDER_DOES_NOT_BELONG_TO_CONTRACTOR" "ORDER_DOES_NOT_BELONG_TO_COMPANY" "OTHER_ASYNCHRONOUS_OPERATION_IN_PROGRESS" Причина, по которой не удалось отменить заявку на поставку: INVALID_ORDER_STATE — неверный статус заявки на поставку. ORDER_IS_VIRTUAL — заявка виртуальная. ORDER_DOES_NOT_BELONG_TO_CONTRACTOR — заявка на поставку не принадлежит вашему юридическому лицу. ORDER_DOES_NOT_BELONG_TO_COMPANY — заявка на поставку не принадлежит продавцу. OTHER_ASYNCHRONOUS_OPERATION_IN_PROGRESS — заявка на поставку в процессе отмены.
- `result` — object Информация об отмене заявки на поставку.
  - `is_order_cancelled` — boolean true , если заявка на поставку отменена.
  - `supplies` — Array of objects Список отменённых поставок.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "ERROR" Статус отмены заявки на поставку. Возможные значения: SUCCESS — заявка отменена. IN_PROGRESS — заявки в процессе отмены. ERROR — ошибка.

Пример ответа:
```json
{
  "error_reasons": [
    "INVALID_ORDER_STATE"
  ],
  "result": {
    "is_order_cancelled": true,
    "supplies": [
      {
        "error_reasons": [
          "INVALID_SUPPLY_STATE"
        ],
        "is_supply_cancelled": true,
        "supply_id": 0
      }
    ]
  },
  "status": "SUCCESS"
}
```

---

## Редактирование товарного состава

`POST /v1/supply-order/content/update`

Operation ID: `SupplyOrderAPI_SupplyOrderContentUpdate`

Метод для редактирования товарного состава в заявке на поставку. Чтобы проверить статус редактирования, используйте метод /v1/supply-order/content/update/status .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/content/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "items": [
    {
      "quant": 0,
      "quantity": 0,
      "sku": 0
    }
  ],
  "order_id": 0,
  "supply_id": 0
}'
```

### Тело запроса

- `items required` — Array of objects Новый товарный состав заявки на поставку. Максимум 5000 товаров.
- `order_id required` — integer <int64> Идентификатор заявки на поставку.
- `supply_id required` — integer <int64> Идентификатор поставки.

### Ответы

- 200 Товарный состав обновлён

#### Схема ответа (200)

- `errors` — Array of strings Items Enum: "INVALID_DRAFT_BUNDLE_ID" "SOME_SERVICE_ERROR" "HAS_UTD" "ORDER_SKU_LIMIT" "SAME_SKU" "SUPPLY_LOCKED" "INBOUND_NO_CAPACITY" "INBOUND_LOCK" "SUPPLY_CONTENT_NOT_VALID" "SUPPLY_BELONG_TO_ANOTHER_CONTRACTOR" "SUPPLY_BELONG_TO_ANOTHER_COMPANY" "INCORRECT_SUPPLY_STATE" "INCORRECT_SUPPLY_SOURCE" "INCORRECT_STORAGE_WAREHOUSE" "DEADLINE" "INACTIVE_CONTRACT" "QUANTITY_OUT_OF_RANGE_BOTTOM" "QUANTITY_OUT_OF_RANGE_UPPER" "EMPTY_CONTENT" "NO_SUPPLY_PRODUCT_BUNDLE_ID" "INVALID_VOLUME" "SUPPLY_IS_VIRTUAL" "ORDER_LOCKED" "CONTRACT_IS_NOT_FOUND" "COMPANY_DOES_NOT_BELONGS_TO_CONTRACTOR" "ORDER_IS_NOT_FOUND" "ORDER_DOES_NOT_BELONGS_TO_COMPANY" "SUPPLY_IS_NOT_FOUND" "SUPPLY_DOES_NOT_BELONGS_TO_ORDER" "UTD_IS_UPLOADED" "STORAGE_WAREHOUSE_IS_NOT_WMS" "CONTRACT_IS_NOT_VALID_FOR_HANDLING_ORDERS" "ORDER_DOES_NOT_BELONG_TO_CONTRACTOR" "MINIMUM_VOLUME_IN_LITRES_INVALID" Ошибки при редактировании товарного состава: INVALID_DRAFT_BUNDLE_ID , SOME_SERVICE_ERROR , ORDER_IS_NOT_FOUND , SUPPLY_IS_NOT_FOUND , SUPPLY_DOES_NOT_BELONGS_TO_ORDER — ошибка при редактировании поставки. HAS_UTD , UTD_IS_UPLOADED — документы в системе ЭДО не удалены. Аннулируйте документы в системе ЭДО. Когда отредактируете состав, сформируйте и подпишите новые документы. ORDER_SKU_LIMIT — количество товаров в поставке должно быть меньше или равно 5000. SAME_SKU — товарный состав поставки остался прежним. SUPPLY_LOCKED — обновление товарного состава в процессе, попробуйте позже. INBOUND_NO_CAPACITY — на складе недостаточно места для поставки. INBOUND_LOCK , ORDER_LOCKED , STORAGE_WAREHOUSE_IS_NOT_WMS — нельзя редактировать товарный состав. SUPPLY_CONTENT_NOT_VALID — в составе поставки есть товары, которые склад не может принять. SUPPLY_BELONG_TO_ANOTHER_CONTRACTOR , COMPANY_DOES_NOT_BELONGS_TO_CONTRACTOR , ORDER_DOES_NOT_BELONG_TO_CONTRACTOR — заявка на поставку не принадлежит вашему юридическому лицу. SUPPLY_BELONG_TO_ANOTHER_COMPANY , ORDER_DOES_NOT_BELONGS_TO_COMPANY — заявка на поставку не принадлежит вашему кабинету. INCORRECT_SUPPLY_STATE — нельзя изменить поставку в этом статусе. INCORRECT_SUPPLY_SOURCE — нельзя изменить поставку с этим источником данных. INCORRECT_STORAGE_WAREHOUSE — нельзя изменить поставку с этим складом хранения. NO_SUPPLY_PRODUCT_BUNDLE_ID — отсутствует идентификатор товарного состава поставки. INVALID_VOLUME — некорректный объём поставки. SUPPLY_IS_VIRTUAL — нельзя редактировать виртуальную поставку. DEADLINE — нельзя изменить поставку за час до таймслота. INACTIVE_CONTRACT — нельзя редактировать состав поставки с истекшим договором. QUANTITY_OUT_OF_RANGE_BOTTOM — количество экземпляров каждого товара должно быть больше 0. QUANTITY_OUT_OF_RANGE_UPPER — количество экземпляров каждого товара должно быть меньше или равно 1 000 000. EMPTY_CONTENT — не сможем принять пустую поставку, добавьте товары. CONTRACT_IS_NOT_FOUND , CONTRACT_IS_NOT_VALID_FOR_HANDLING_ORDERS — в этом личном кабинете нельзя изменить поставку. MINIMUM_VOLUME_IN_LITRES_INVALID — объём товаров в поставке ниже минимального.
- `operation_id` — string Идентификатор операции.

Пример ответа:
```json
{
  "errors": [
    "INVALID_DRAFT_BUNDLE_ID"
  ],
  "operation_id": "string"
}
```

---

## Информация о статусе редактирования товарного состава

`POST /v1/supply-order/content/update/status`

Operation ID: `SupplyOrderAPI_SupplyOrderContentUpdateStatus`

Метод для получения статуса редактирования товарного состава.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/content/update/status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `operation_id required` — string Идентификатор операции.

### Ответы

- 200 Статус редактирования

#### Схема ответа (200)

- `errors` — Array of strings Items Enum: "INVALID_DRAFT_BUNDLE_ID" "SOME_SERVICE_ERROR" "HAS_UTD" "ORDER_SKU_LIMIT" "SAME_SKU" "SUPPLY_LOCKED" "INBOUND_NO_CAPACITY" "INBOUND_LOCK" "SUPPLY_CONTENT_NOT_VALID" "SUPPLY_BELONG_TO_ANOTHER_CONTRACTOR" "SUPPLY_BELONG_TO_ANOTHER_COMPANY" "INCORRECT_SUPPLY_STATE" "INCORRECT_SUPPLY_SOURCE" "INCORRECT_STORAGE_WAREHOUSE" "DEADLINE" "INACTIVE_CONTRACT" "QUANTITY_OUT_OF_RANGE_BOTTOM" "QUANTITY_OUT_OF_RANGE_UPPER" "EMPTY_CONTENT" "NO_SUPPLY_PRODUCT_BUNDLE_ID" "INVALID_VOLUME" "SUPPLY_IS_VIRTUAL" "ORDER_LOCKED" "MINIMUM_VOLUME_IN_LITRES_INVALID" Список ошибок при редактировании товарного состава: INVALID_DRAFT_BUNDLE_ID , SOME_SERVICE_ERROR — ошибка при редактировании поставки. HAS_UTD — документы в системе ЭДО не удалены. Аннулируйте документы в системе ЭДО. Когда отредактируете состав, сформируйте и подпишите новые документы. ORDER_SKU_LIMIT — количество товаров в поставке должно быть меньше или равно 5000. SAME_SKU — товарный состав поставки остался прежним. SUPPLY_LOCKED — обновление товарного состава в процессе, попробуйте позже. INBOUND_NO_CAPACITY — на складе недостаточно места для поставки. INBOUND_LOCK , ORDER_LOCKED — нельзя редактировать товарный состав. SUPPLY_CONTENT_NOT_VALID — в составе поставки есть товары, которые склад не может принять. Провалидируйте товарный состав методом /v1/supply-order/content/update/validation . SUPPLY_BELONG_TO_ANOTHER_CONTRACTOR — заявка на поставку не принадлежит вашему юридическому лицу. SUPPLY_BELONG_TO_ANOTHER_COMPANY — заявка на поставку не принадлежит вашему кабинету. INCORRECT_SUPPLY_STATE — нельзя изменить поставку в этом статусе. INCORRECT_SUPPLY_SOURCE — нельзя изменить поставку с этим источником данных. INCORRECT_STORAGE_WAREHOUSE — нельзя изменить поставку с этим складом хранения. NO_SUPPLY_PRODUCT_BUNDLE_ID — отсутствует идентификатор товарного состава поставки. INVALID_VOLUME — некорректный объём поставки. SUPPLY_IS_VIRTUAL — нельзя редактировать виртуальную поставку. DEADLINE — нельзя изменить поставку за час до таймслота. INACTIVE_CONTRACT — нельзя редактировать состав поставки с истекшим договором. QUANTITY_OUT_OF_RANGE_BOTTOM — количество экземпляров каждого товара должно быть больше 0. QUANTITY_OUT_OF_RANGE_UPPER — количество экземпляров каждого товара должно быть меньше или равно 1 000 000. EMPTY_CONTENT — не сможем принять пустую поставку, добавьте товары. MINIMUM_VOLUME_IN_LITRES_INVALID — объём товаров в поставке ниже минимального.
- `new_bundle_id` — string Идентификатор нового товарного состава поставки.
- `status` — string Enum: "SUCCESS" "IN_PROGRESS" "ERROR" Статус редактирования товарного состава поставки. Возможные статусы: SUCCESS — товарный состав изменён, IN_PROGRESS — товарный состав в процессе изменения, ERROR — возникла ошибка при изменении товарного состава.

Пример ответа:
```json
{
  "errors": [
    "INVALID_DRAFT_BUNDLE_ID"
  ],
  "new_bundle_id": "string",
  "status": "SUCCESS"
}
```

---

## Проверить новый товарный состав

`POST /v1/supply-order/content/update/validation`

Operation ID: `SupplyOrderContentUpdateValidation`

Используйте этот метод, если в /v1/supply-order/content/update/status вы получили ошибку SUPPLY_CONTENT_NOT_VALID .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/supply-order/content/update/validation" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "new_bundle_id": "string",
  "supply_id": 0
}'
```

### Тело запроса

- `new_bundle_id required` — string Идентификатор нового товарного состава поставки.
- `supply_id required` — integer <int64> Идентификатор поставки.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `editing_errors` — Array of strings Default: "UNSPECIFIED" Items Enum: "UNSPECIFIED" "UNKNOWN" "INCORRECT_SUPPLY_STATE" "DEADLINE" "UTD_IS_UPLOADED" "STORAGE_WAREHOUSE_IS_NOT_WMS" "CONTRACT_IS_NOT_VALID_FOR_HANDLING_ORDERS" "SUPPLY_IS_VIRTUAL" "SUPPLY_DOES_NOT_BELONG_TO_COMPANY" "ASSORTMENT_REJECTION_REASON_CORRUPTED_ASSORTMENT" "ASSORTMENT_REJECTION_REASON_STORAGE_BELARUS_SKU_HAS_NO_ANY_FEACN" "ASSORTMENT_REJECTION_REASON_STORAGE_BELARUS_SKU_HAS_NO_SELLER_FEACN" "ASSORTMENT_REJECTION_REASON_TRACEABLE_SKU_HAS_NO_GTIN_BARCODE" "ASSORTMENT_REJECTION_REASON_TRACEABLE_SKU_HAS_NO_MEASUREMENT_UNIT_QUANTITY" Ошибки: UNSPECIFIED — не определено. UNKNOWN — неизвестный тип. INCORRECT_SUPPLY_STATE — нельзя изменить поставку в этом статусе. DEADLINE — нельзя изменить поставку за час до таймслота. UTD_IS_UPLOADED — документы в системе ЭДО не удалены. Аннулируйте документы в системе ЭДО. Когда отредактируете состав, сформируйте и подпишите новые документы. STORAGE_WAREHOUSE_IS_NOT_WMS — нельзя редактировать товарный состав. CONTRACT_IS_NOT_VALID_FOR_HANDLING_ORDERS — в этом личном кабинете нельзя изменить поставку. SUPPLY_IS_VIRTUAL — нельзя редактировать виртуальную поставку. SUPPLY_DOES_NOT_BELONG_TO_COMPANY — заявка на поставку не принадлежит вашему кабинету. ASSORTMENT_REJECTION_REASON_CORRUPTED_ASSORTMENT — не получилось добавить товар в заявку. Попробуйте ещё раз. ASSORTMENT_REJECTION_REASON_STORAGE_BELARUS_SKU_HAS_NO_ANY_FEACN , ASSORTMENT_REJECTION_REASON_STORAGE_BELARUS_SKU_HAS_NO_SELLER_FEACN — у товара нет кода ТН ВЭД ЕАЭС. ASSORTMENT_REJECTION_REASON_TRACEABLE_SKU_HAS_NO_GTIN_BARCODE — у товара нет штрихкода GTIN. ASSORTMENT_REJECTION_REASON_TRACEABLE_SKU_HAS_NO_MEASUREMENT_UNIT_QUANTITY — не указано количество товара в унифицированных единицах измерения.
- `validated_assortment` — object Информация о товарном составе.

Пример ответа:
```json
{
  "editing_errors": [
    "UNSPECIFIED"
  ],
  "validated_assortment": {
    "approved_items": [
      {
        "barcode": "string",
        "item_link": "string",
        "name": "string",
        "offer_id": "string",
        "origin_quantity": 0,
        "origin_total_volume_in_litres": 0,
        "quant": 0,
        "quantity": 0,
        "sku": 0,
        "sku_quantity_limit": 0,
        "total_volume_in_litres": 0
      }
    ],
    "rejected_items": [
      {
        "barcode": "string",
        "name": "string",
        "offer_id": "string",
        "origin_quantity": 0,
        "origin_total_volume_in_litres": 0,
        "quantity": 0,
        "rejection_reason": [
          "UNSPECIFIED"
        ],
        "restrictions": {
          "reasons_restrictions": [
            "UNKNOWN"
          ],
          "sku_has_no_sales_in_days": 0,
          "sku_quantity_limit": 0
        },
        "sku": 0,
        "total_volume_in_litres": 0
      }
    ],
    "total_approved_item_count": 0,
    "total_approved_quantity": 0,
    "total_approved_volume_in_litres": 0,
    "total_rejected_item_count": 0,
    "total_restricted_item_count": 0
  }
}
```

---

## Получить список складов продавца

`POST /v1/warehouse/fbo/seller/list`

Operation ID: `WarehouseFboSellerList`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/fbo/seller/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список складов

#### Схема ответа (200)

- `warehouses` — Array of objects Список складов продавца.
  - `address` — object Информация об адресе склада продавца.
  - `contacts` — object Контакты.
  - `courier_comment` — string Комментарий для курьера.
  - `is_active` — boolean true , если склад активный.
  - `is_pickup` — boolean true , если доступна отгрузка курьером.
  - `seller_warehouse_id` — integer <int64> Идентификатор склада продавца.
  - `seller_warehouse_name` — string Название склада продавца.
  - `working_days` — Array of objects Рабочие дни склада продавца.

Пример ответа:
```json
{
  "warehouses": [
    {
      "address": {
        "address": "string",
        "city": "string",
        "coordinates": {
          "latitude": 0,
          "longitude": 0
        },
        "country_code": "string",
        "macrolocal_cluster_id": 0,
        "region": "string",
        "timezone": "string"
      },
      "contacts": {
        "phone_numbers": [
          "string"
        ]
      },
      "courier_comment": "string",
      "is_active": true,
      "is_pickup": true,
      "seller_warehouse_id": 0,
      "seller_warehouse_name": "string",
      "working_days": [
        {
          "day": "UNSPECIFIED",
          "time_from_local": "string",
          "time_to_local": "string"
        }
      ]
    }
  ]
}
```

---
