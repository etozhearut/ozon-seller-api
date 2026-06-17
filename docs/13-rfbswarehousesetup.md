# Создание складов rFBS Express и управление ими

_Тег: `rFBSWarehouseSetup` · операций: 7_

## Создать склад с методом доставки «Партнёры Ozon»

`POST /v1/warehouse/erfbs/aggregator/create`

Operation ID: `WarehouseERFBSAggregatorCreate`

Подробнее о схеме realFBS Express

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `address_coordinates required` — object Координаты адреса склада.
- `is_auto_assembly` — boolean Default: false true , если на складе доступна автосборка.
- `delivery_method required` — object Информация о методе доставки «Партнёры Ozon».
- `min_order_value` — integer <int64> Минимальная стоимость заказа.
- `name required` — string <= 100 characters Название склада.
- `phone required` — string +7(XXX)XXX-XX-XX Номер телефона склада.
- `timetable_warehouse required` — object Расписание работы склада.

Пример запроса:

```json
{
  "address_coordinates": {
    "latitude": 0,
    "longitude": 0
  },
  "is_auto_assembly": false,
  "delivery_method": {
    "courier_comment": "string",
    "courier_phones": [
      "string"
    ],
    "cut_in": 15,
    "deliver_to_pvz": true,
    "delivery_costs": {
      "min_weight": 0,
      "max_weight": 0,
      "min_order_price": 0,
      "max_order_price": 0,
      "seller_payment": 0
    },
    "name": "string",
    "return_settings": {
      "contact_days": 0,
      "post_office_zipcode": "string",
      "return_method": "UNSPECIFIED",
      "transport_company_name": "string"
    }
  },
  "min_order_value": 0,
  "name": "string",
  "phone": "string",
  "timetable_warehouse": {
    "holidays": [
      {
        "day": "string",
        "from": "string",
        "to": "string"
      }
    ],
    "working_days": [
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      }
    ]
  }
}
```

### Ответы

- 200 Склад создан

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Обновить склад

`POST /v1/warehouse/erfbs/update`

Operation ID: `WarehouseERFBSUpdate`

Подробнее о схеме realFBS Express

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `is_auto_assembly` — boolean Default: false true , если на складе доступна автосборка.
- `min_order_value` — integer <int64> Минимальная стоимость заказа.
- `name` — string <= 100 characters Название склада.
- `phone` — string +7(XXX)XXX-XX-XX Номер телефона склада.
- `timetable_warehouse` — object Расписание работы склада.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "is_auto_assembly": false,
  "min_order_value": 0,
  "name": "string",
  "phone": "string",
  "timetable_warehouse": {
    "holidays": [
      {
        "day": "string",
        "from": "string",
        "to": "string"
      }
    ],
    "working_days": [
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      },
      {
        "day": "UNSPECIFIED",
        "from": "string",
        "to": "string"
      }
    ]
  },
  "warehouse_id": 0
}
```

### Ответы

- 200 Склад обновлён

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Обновить метод доставки «Партнёры Ozon»

`POST /v1/warehouse/erfbs/aggregator/delivery-method/update`

Operation ID: `WarehouseERFBSAggregatorDeliveryMethodUpdate`

Подробнее о схеме rFBS Express

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `courier_comment` — string <= 3000 characters Комментарий для курьера.
- `courier_phones` — Array of strings <= 3 items +7(XXX)XXX-XX-XX Номера телефонов для связи с курьером.
- `cut_in` — integer <int64> Enum: 15 30 60 120 180 240 300 360 420 480 Время сборки.
- `deliver_to_pvz` — boolean true , если доставка Ozon Express в пункт выдачи Ozon.
- `delivery_costs` — object Расходы на доставку, которые вы готовы оплатить.
- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `name` — string <= 100 characters Название метода доставки.
- `return_settings` — object Настройки возвратов от покупателей.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "courier_comment": "string",
  "courier_phones": [
    "string"
  ],
  "cut_in": 15,
  "deliver_to_pvz": true,
  "delivery_costs": {
    "min_weight": 0,
    "max_weight": 0,
    "min_order_price": 0,
    "max_order_price": 0,
    "seller_payment": 0
  },
  "delivery_method_id": 0,
  "name": "string",
  "return_settings": {
    "contact_days": 0,
    "post_office_zipcode": "string",
    "return_method": "COURIER",
    "transport_company_name": "string"
  },
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Создать склад с методом доставки «Вы или сторонняя служба»

`POST /v1/warehouse/erfbs/non-integrated/create`

Operation ID: `WarehouseERFBSNonIntegratedCreate`

Подробнее о схеме realFBS Express

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `address_coordinates required` — object Координаты адреса склада.
- `is_auto_assembly` — boolean Default: false true , если на складе доступна автосборка.
- `delivery_method required` — object Информация о методе доставки «Вы или сторонняя служба».
- `min_order_value` — integer <int64> Минимальная стоимость заказа в рублях.
- `name required` — string <= 100 characters Название склада.
- `phone required` — string +7(XXX)XXX-XX-XX Номер телефона склада.
- `timetable_warehouse required` — object Расписание работы склада.

Пример запроса:

```json
{
  "address_coordinates": {
    "latitude": 0,
    "longitude": 0
  },
  "is_auto_assembly": false,
  "delivery_method": {
    "courier_cutoff": 5,
    "cut_in": 15,
    "delivery_polygons": [
      {
        "id": 0,
        "time": 15
      }
    ],
    "name": "string",
    "return_settings": {
      "contact_days": 0,
      "post_office_zipcode": "string",
      "return_method": "COURIER",
      "transport_company_name": "string"
    }
  },
  "min_order_value": 0,
  "name": "string",
  "phone": "string",
  "timetable_warehouse": {
    "holidays": [
      {
        "day": "string",
        "from": "string",
        "to": "string"
      }
    ],
    "working_days": [
      {
        "day": "MONDAY",
        "from": "string",
        "to": "string"
      },
      {
        "day": "MONDAY",
        "from": "string",
        "to": "string"
      },
      {
        "day": "MONDAY",
        "from": "string",
        "to": "string"
      },
      {
        "day": "MONDAY",
        "from": "string",
        "to": "string"
      },
      {
        "day": "MONDAY",
        "from": "string",
        "to": "string"
      }
    ]
  }
}
```

### Ответы

- 200 Склад создан

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Обновить метод доставки «Вы или сторонняя служба»

`POST /v1/warehouse/erfbs/non-integrated/delivery-method/update`

Operation ID: `WarehouseERFBSNonIntegratedDeliveryMethodUpdate`

Подробнее о схеме rFBS Express

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `courier_cutoff required` — integer <int64> Enum: 5 10 15 20 25 30 35 40 45 Скорость отгрузки.
- `cut_in required` — integer <int64> Enum: 15 30 60 120 180 240 300 360 420 480 Время сборки.
- `delivery_method_id required` — integer <int64> Идентификатор метода доставки.
- `name required` — string <= 100 characters Название метода доставки.
- `return_settings required` — object Настройки возвратов от покупателей.
- `warehouse_id required` — integer <int64> Идентификатор склада.

Пример запроса:

```json
{
  "courier_cutoff": 5,
  "cut_in": 15,
  "delivery_method_id": 0,
  "name": "string",
  "return_settings": {
    "contact_days": 0,
    "post_office_zipcode": "string",
    "return_method": "COURIER",
    "transport_company_name": "string"
  },
  "warehouse_id": 0
}
```

### Ответы

- 200 Успешно

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Поставить rFBS-склад на паузу

`POST /v1/warehouse/rfbs/pause`

Operation ID: `WarehouseRfbsPause`

Скрывает товары склада с витрины. Изменять сток не нужно.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `warehouse_id` — integer <int64> Идентификатор склада.

### Ответы

- 200 Склад на паузе
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---

## Снять rFBS-склад с паузы

`POST /v1/warehouse/rfbs/unpause`

Operation ID: `WarehouseRfbsUnpause`

Возвращает товары склада на витрину.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `warehouse_id` — integer <int64> Идентификатор склада.

### Ответы

- 200 Склад снят с паузы
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `operation_id` — string Идентификатор операции. Получите статус операции методом /v1/warehouse/operation/status .

---
