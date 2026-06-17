# Доставка

_Тег: `DeliveryAPI` · операций: 5_

## Проверить доступность доставки для покупателя

`POST /v1/delivery/check`

Operation ID: `DeliveryCheck`

Проверяет доступность доставки Ozon для покупателя. Не учитывает ограничения по сумме покупки, категории товаров и географии.

### Тело запроса (application/json)

- `client_phone required` — string 7XXXXXXXXXX Номер телефона покупателя.

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `is_possible` — boolean true , если доставка доступна.

---

## Получить доступные варианты доставки

`POST /v2/delivery/checkout`

Operation ID: `DeliveryCheckout`

Проверяет доступность доставки товаров на указанный адрес или в точку выдачи и отображает сроки доставки. Проверяйте наличие товаров и маршруты во время оформления заказа, чтобы точно рассчитать сроки доставки.

### Тело запроса (application/json)

- `buyer_phone` — string Номер телефона покупателя.
- `delivery_schema` — string Default: "MIX" Enum: "MIX" "FBO" "FBS" Схема доставки: MIX — на выбор Ozon; FBO — FBO; FBS — FBS.
- `delivery_type` — object Способ доставки.
- `items` — Array of objects <= 1000 items Информация о товарах.

Пример запроса:

```json
{
  "buyer_phone": "string",
  "delivery_schema": "MIX",
  "delivery_type": {
    "courier": {
      "coordinates": {
        "latitude": 0,
        "longitude": 0
      }
    },
    "pick_up": {
      "map_point_id": 0
    }
  },
  "items": [
    {
      "offer_id": "string",
      "quantity": 0,
      "sku": 0
    }
  ]
}
```

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `splits` — Array of objects Результат запроса.
  - `delivery_method` — object Метод доставки.
  - `delivery_schema` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "FBO" "FBS" Схема доставки: UNSPECIFIED — не определена; FBO — FBO; FBS — FBS.
  - `items` — Array of objects Информация о товарах.
  - `unavailable_reason` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "UNKNOWN" "OUT_OF_STOCK" "BANNED_FOR_AREA" "BANNED_FOR_LEGAL" "BANNED" "BANNED_FOR_NOT_PREMIUM" "DELIVERY_UNAVAILABLE" "BANNED_FOR_INDIVIDUAL" "INVALID_WEIGHT" "INVALID_MULTIPLICITY" "NOT_FOUND_POINTS_DARK_STORES" "INVALID_MULTI_WAREHOUSES" "MIN_PRICE" "OZONE_DELIVERY_UNAVAILABLE" "RFBS_DELIVERY_UNAVAILABLE" "HACK_COURIER_TAGS" "NO_SLA" "DELIVERY_VARIANT_IS_CLOSING" "TPL_NOT_INTEGRATED" "NOT_ALL_WAREHOUSES_ARE_SERVED" "DELIVERY_SLOTS_NOT_FOUND" "NO_ROUTE" "CAPACITY_LIMIT" "PACKAGE_MAX_VOLUME_WEIGHT_RESTRICTION" "PACKAGE_MAX_WEIGHT_RESTRICTION" "MAX_COST_RESTRICTION" "MIN_PACKAGE_WEIGHT_RESTRICTION" "MIN_COST_RESTRICTION" "MAX_DIMENSIONS_RESTRICTION" "PRODUCT_TYPES_RESTRICTION" "PRODUCT_TAGS_RESTRICTION" "SELECTED_DELIVERY_METHOD_UNAVAILABLE" "SELECTED_DELIVERY_TIMESLOT_UNAVAILABLE" "MARKETPLACE_UNAVAILABLE" "INVALID_PVZ_FOR_KGT" "LEGAL_USER_PREMIUM_SPLIT" "USER_ALREADY_HAS_PREMIUM" "WAIT_FOR_PAY_SUBSCRIPTION" "ADDRESS_NOT_SET" "PICKUP_POINT_DISABLED" "LEGAL_PREORDER" "DELIVERY_TYPE_FOR_PREORDER" "CROSS_BORDER_PICKUP" "ORDER_CUSTOMS_TYPES" "PACKAGE_MAX_COST" "SUPER_ECONOM" "ECONOM_NOT_FULL_QUANT" "EMPTY_DELIVERY_METHODS" Причина недоступности: UNSPECIFIED — доставка доступна; UNKNOWN — неизвестная причина; OUT_OF_STOCK — товар закончился; BANNED_FOR_AREA — товар заблокирован для этой области; BANNED_FOR_LEGAL — товар заблокирован для юридических лиц; BANNED — товар заблокирован; BANNED_FOR_NOT_PREMIUM — товар заблокирован для покупателей без подписки Premium; DELIVERY_UNAVAILABLE — доставка недоступна, например, курьеры перегружены; BANNED_FOR_INDIVIDUAL — товар заблокирован для физических лиц; INVALID_WEIGHT — вес товара не указан; INVALID_MULTIPLICITY — недопустимая кратность в штуках; NOT_FOUND_POINTS_DARK_STORES — пункты в массиве дарксторов не найдены; INVALID_MULTI_WAREHOUSES — товары в заказе с разными схемами доставки неверно распределены; MIN_PRICE — сплит не прошёл минимальную цену; OZONE_DELIVERY_UNAVAILABLE — доставка Ozon недоступна; RFBS_DELIVERY_UNAVAILABLE — доставка по системе rFBS недоступна; HACK_COURIER_TAGS — способ доставки исключён по правилу приоритетного показа; NO_SLA — норматив комплектации отсутствует; DELIVERY_VARIANT_IS_CLOSING — способ доставки в неподходящем статусе; TPL_NOT_INTEGRATED — точки, по которым возврат невозможен; NOT_ALL_WAREHOUSES_ARE_SERVED — доставка со склада отправления отсутствует; DELIVERY_SLOTS_NOT_FOUND — таймслоты отсутствуют; NO_ROUTE — маршрут не найден; CAPACITY_LIMIT — таймслоты отсеяны из-за заполненности капаситета; PACKAGE_MAX_VOLUME_WEIGHT_RESTRICTION — ограничение на максимальный объёмный вес посылки; PACKAGE_MAX_WEIGHT_RESTRICTION — ограничение на максимальный физический вес посылки, в килограммах до тысячных грамма; MAX_COST_RESTRICTION — ограничение на максимальную стоимость заказа, учитывается стоимость товаров, их доставки, но не учитывается страховой сбор в рублях; MIN_PACKAGE_WEIGHT_RESTRICTION — ограничение на минимальный физический вес посылки в килограммах до тысячных грамма; MIN_COST_RESTRICTION — ограничение на минимальную стоимость товаров заказа в рублях; MAX_DIMENSIONS_RESTRICTION — ограничение на максимальные габариты посылки в сантиметрах; PRODUCT_TYPES_RESTRICTION — ограничение на допустимые в заказе товарные категории; PRODUCT_TAGS_RESTRICTION — ограничение на допустимые в заказе теги товаров; SELECTED_DELIVERY_METHOD_UNAVAILABLE — выбранный способ доставки стал недоступным; SELECTED_DELIVERY_TIMESLOT_UNAVAILABLE — выбранный таймслот стал недоступным; MARKETPLACE_UNAVAILABLE — в заказе товары нескольких маркетплейсов, оформить заказ можно только с 1; INVALID_PVZ_FOR_KGT — выбранный ПВЗ не подходит для КГТ; LEGAL_USER_PREMIUM_SPLIT — юридическим лицам запрещена покупка подписки Premium; USER_ALREADY_HAS_PREMIUM — у пользователя уже есть подписка Premium и она не подарочная; WAIT_FOR_PAY_SUBSCRIPTION — пользователь купил премиум-подписку, но не оплатил заказ или подписка ещё не перешла в активный статус; ADDRESS_NOT_SET — адрес не установлен; PICKUP_POINT_DISABLED — ПВЗ недоступен; LEGAL_PREORDER — предзаказ недоступен юридическим лицам; DELIVERY_TYPE_FOR_PREORDER — тип доставки недоступен для предзаказа; CROSS_BORDER_PICKUP — CrossBorder-товары не доставляются в пункты выдачи; ORDER_CUSTOMS_TYPES — ограничения на таможенные типы; PACKAGE_MAX_COST — ограничение на максимальную стоимость посылки,учитывается стоимость товаров и их доставки; SUPER_ECONOM — недоступный «суперэконом»; ECONOM_NOT_FULL_QUANT — неполный квант; EMPTY_DELIVERY_METHODS — нет доступных способов доставки;
  - `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:

```json
{
  "splits": [
    {
      "delivery_method": {
        "delivery_time_zone_offset": 0,
        "delivery_type": "UNSPECIFIED",
        "id": 0,
        "name": "string",
        "timeslots": [
          {
            "client_date_range": {
              "from": "2019-08-24T14:15:22Z",
              "to": "2019-08-24T14:15:22Z"
            },
            "logistic_date_range": {
              "from": "2019-08-24T14:15:22Z",
              "to": "2019-08-24T14:15:22Z"
            },
            "timeslot_id": 0
          }
        ],
        "unavailable_reason": "UNSPECIFIED",
        "warehouse_time_zone_offset": 0
      },
      "delivery_schema": "UNSPECIFIED",
      "items": [
        {
          "offer_id": "string",
          "quantity": 0,
          "sku": 0
        }
      ],
      "unavailable_reason": "UNSPECIFIED",
      "warehouse_id": 0
    }
  ]
}
```

---

## Отрисовать точки на карте

`POST /v1/delivery/map`

Operation ID: `DeliveryMap`

Возвращает объединённые кластеры точек самовывоза на области из параметра viewport . Используйте значения из параметра clusters.viewport , чтобы получить список точек или мелких кластеров внутри большого кластера. Используйте метод /v1/delivery/point/info , чтобы получить информацию о конкретной точке самовывоза.

### Тело запроса (application/json)

- `viewport` — object Область карты для получения кластеров и точек самовывоза.
- `zoom` — integer <int32> [ 0 .. 19 ] Масштаб карты.

Пример запроса:

```json
{
  "viewport": {
    "left_bottom": {
      "lat": 0,
      "long": 0
    },
    "right_top": {
      "lat": 0,
      "long": 0
    }
  },
  "zoom": 0
}
```

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `clusters` — Array of objects Кластеры.
  - `coordinate` — object Координаты.
  - `is_same_building` — boolean true , если все точки находятся в одном здании.
  - `map_point_ids` — Array of strings <int64> Идентификаторы точек на карте.
  - `points_count` — integer <int32> Количество точек в кластере.
  - `viewport` — object Область карты для получения точек в кластере.

Пример ответа:

```json
{
  "clusters": [
    {
      "coordinate": {
        "lat": 0,
        "long": 0
      },
      "is_same_building": true,
      "map_point_ids": [
        "string"
      ],
      "points_count": 0,
      "viewport": {
        "left_bottom": {
          "lat": 0,
          "long": 0
        },
        "right_top": {
          "lat": 0,
          "long": 0
        }
      }
    }
  ]
}
```

---

## Получить информацию о точке самовывоза

`POST /v1/delivery/point/info`

Operation ID: `DeliveryPointInfo`

Возвращает подробную информацию о точке самовывоза для пользователя.

### Тело запроса (application/json)

- `map_point_ids` — Array of strings <int64> <= 100 items Идентификаторы точек на карте.

### Ответы

- 200 Информация о точке самовывоза

#### Схема ответа (200)

- `points` — Array of objects Информация о пунктах самовывоза.
  - `delivery_method` — object Метод доставки.
  - `enabled` — boolean true , если пункт самовывоза доступен.

Пример ответа:

```json
{
  "points": [
    {
      "delivery_method": {
        "address": "string",
        "address_details": {
          "city": "string",
          "house": "string",
          "region": "string",
          "street": "string"
        },
        "coordinates": {
          "lat": 0,
          "long": 0
        },
        "delivery_type": {
          "id": 0,
          "name": "string"
        },
        "description": "string",
        "fitting_rooms_count": 0,
        "holidays": [
          {
            "begin": "2019-08-24T14:15:22Z",
            "end": "2019-08-24T14:15:22Z"
          }
        ],
        "holidays_filled": true,
        "images": [
          "string"
        ],
        "location_id": "string",
        "map_point_id": 0,
        "name": "string",
        "properties": [
          {
            "enabled": true,
            "name": "string"
          }
        ],
        "pvz_rating": 0,
        "storage_period": 0,
        "working_hours": [
          {
            "date": "2019-08-24T14:15:22Z",
            "periods": [
              {
                "max": {
                  "hours": 0,
                  "minutes": 0
                },
                "min": {
                  "hours": 0,
                  "minutes": 0
                }
              }
            ]
          }
        ]
      },
      "enabled": true
    }
  ]
}
```

---

## Получить список точек самовывоза

`POST /v1/delivery/point/list`

Operation ID: `DeliveryAPI_DeliveryPointList`

Возвращает координаты всех точек самовывоза без объединения в кластеры.

### Ответы

- 200 Список точек самовывоза

#### Схема ответа (200)

- `points` — Array of objects Точки самовывоза.
  - `coordinate` — object Координаты.
  - `map_point_id` — integer <int64> Идентификатор точки на карте.

Пример ответа:

```json
{
  "points": [
    {
      "coordinate": {
        "lat": 0,
        "long": 0
      },
      "map_point_id": 0
    }
  ]
}
```

---
