# Прочие методы

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `BetaMethod` · методов: 13_

## Управление остатками

`POST /v1/analytics/manage/stocks`

Operation ID: `AnalyticsAPI_ManageStocks`

22 января 2026 года метод будет отключён. Переключитесь на /v1/analytics/stocks . Используйте метод, чтобы узнать, сколько товаров осталось на складах FBO. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/analytics/manage/stocks" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "skus": [
      "string"
    ],
    "stock_types": [
      "STOCK_TYPE_VALID"
    ],
    "warehouse_ids": [
      "string"
    ]
  },
  "limit": 1,
  "offset": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтр.
- `limit` — integer <int32> [ 1 .. 1000 ] Количество значений в ответе.
- `offset` — integer <int32> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , ответ начнётся с 11-го найденного элемента.

### Ответы

- 200 Информация об остатках
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `items` — Array of objects Товары.
  - `defect_stock_count` — integer <int64> Остаток дефектного товара, шт.
  - `expiring_stock_count` — integer <int64> Остаток товара с истекающим сроком годности, шт.
  - `name` — string Название товара.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `valid_stock_count` — integer <int64> Остаток товара, доступного для продажи.
  - `waitingdocs_stock_count` — integer <int64> Остаток товара, ожидающего документы.
  - `warehouse_name` — string Название склада.

Пример ответа:
```json
{
  "items": [
    {
      "defect_stock_count": 0,
      "expiring_stock_count": 0,
      "name": "string",
      "offer_id": "string",
      "sku": 0,
      "valid_stock_count": 0,
      "waitingdocs_stock_count": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Отчёт по вывозу и утилизации с поставки FBO

`POST /v1/removal/from-supply/list`

Operation ID: `GetSupplyReturnsSummaryReport`

Метод соответствует разделу FBO → Вывоз и утилизация в личном кабинете. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/removal/from-supply/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2025-03-01",
  "date_to": "2025-03-30",
  "last_id": "",
  "limit": 500
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to required` — string Дата окончания отчётного периода в формате YYYY-MM-DD .
- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit required` — integer <int32> [ 1 .. 500 ] Количество элементов в ответе.

### Ответы

- 200 Отчёт по вывозу и утилизации с поставки FBO

#### Схема ответа (200)

- `last_id` — string Идентификатор последнего значения на странице.
- `returns_summary_report_rows` — Array of objects Информация о товарах.

Пример ответа:
```json
{
  "returns_summary_report_rows": [
    {
      "name": "Шурупы",
      "offer_id": "Шуруп-0001",
      "sku": "12314324",
      "barcode": "",
      "quantity_for_return": 1,
      "quant_count": 0,
      "box_id": "202501272",
      "return_id": "934457",
      "stock_type": "Излишек",
      "return_created_at": "2025-03-13T06:49:37.591Z",
      "is_auto_return": false,
      "clearing_warehouse_name": "ХОРУГВИНО_РФЦ",
      "return_state": "Отклонено складом",
      "delivery_type": "Вывоз с ПВЗ/СЦ",
      "preliminary_delivery_price": 15,
      "box_length": 0.1,
      "box_width": 0.1,
      "box_height": 0.1,
      "box_volume": 34,
      "box_weight": 0.01,
      "box_state": "Доступно к вывозу",
      "destination_warehouse_name": "РАЗДОРЫ_4",
      "destination_warehouse_address": "Россия, 143082, обл. Московская, р-н Одинцовский, д. Раздоры, Россия, Московская область, Красногорск, улица Липовой Рощи, 2к1",
      "delivery_date": "2025-03-13T06:49:37.591Z",
      "given_out_date": "2025-03-13T06:49:37.591Z",
      "utilization_date": "2025-03-13T06:49:37.591Z"
    }
  ],
  "last_id": "whcReturnId:934457_boxId:202501272_itemIndex:0"
}
```

---

## Отчёт по вывозу и утилизации со стока FBO

`POST /v1/removal/from-stock/list`

Operation ID: `GetSupplierReturnsSummaryReport`

Метод соответствует разделу FBO → Вывоз и утилизация в личном кабинете. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/removal/from-stock/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2025-03-01",
  "date_to": "2025-03-30",
  "last_id": "",
  "limit": 500
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to required` — string Дата окончания отчётного периода в формате YYYY-MM-DD .
- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit required` — integer <int32> [ 1 .. 500 ] Количество элементов в ответе.

### Ответы

- 200 Отчёт по вывозу и утилизации со стока FBO

#### Схема ответа (200)

- `last_id` — string Идентификатор последнего значения на странице.
- `returns_summary_report_rows` — Array of objects Информация о товарах.

Пример ответа:
```json
{
  "returns_summary_report_rows": [
    {
      "name": "Шурупы",
      "offer_id": "Шуруп-0001",
      "sku": "12314324",
      "barcode": "",
      "quantity_for_return": 1,
      "quant_count": 0,
      "box_id": "202501272",
      "return_id": "934457",
      "stock_type": "Излишек",
      "return_created_at": "2025-03-13T06:49:37.591Z",
      "is_auto_return": false,
      "clearing_warehouse_name": "ХОРУГВИНО_РФЦ",
      "return_state": "Отклонено складом",
      "delivery_type": "Вывоз с ПВЗ/СЦ",
      "preliminary_delivery_price": 15,
      "box_length": 0.1,
      "box_width": 0.1,
      "box_height": 0.1,
      "box_volume": 34,
      "box_weight": 0.01,
      "box_state": "Доступно к вывозу",
      "destination_warehouse_name": "РАЗДОРЫ_4",
      "destination_warehouse_address": "Россия, 143082, обл. Московская, р-н Одинцовский, д. Раздоры, Россия, Московская область, Красногорск, улица Липовой Рощи, 2к1",
      "delivery_date": "2025-03-13T06:49:37.591Z",
      "given_out_date": "2025-03-13T06:49:37.591Z",
      "utilization_date": "2025-03-13T06:49:37.591Z"
    }
  ],
  "last_id": "whcReturnId:934457_boxId:202501272_itemIndex:0"
}
```

---

## Управлять скидкой от количества

`POST /v1/product/stairway-discount/by-quantity/set`

Operation ID: `ProductAPI_SetProductStairwayDiscountByQuantity`

Устанавливает или удаляет скидку на товар в зависимости от его количества в заказе. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/stairway-discount/by-quantity/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "stairways": [
    {
      "enabled": true,
      "sku": 0,
      "stairway": {
        "steps": [
          {
            "discount": 0,
            "quantity": 0,
            "step": 0
          }
        ]
      }
    }
  ],
  "suppress_warnings": true
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `stairways required` — Array of objects Информация о скидке от количества по товарам.
- `suppress_warnings` — boolean Передайте true , чтобы игнорировать предупреждения и установить скидку.

### Ответы

- 200 Настройки скидки изменены
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `accepted` — boolean true , если запрос принят. Используйте метод /v1/product/stairway-discount/by-quantity/get , чтобы узнать результат изменения скидки.
- `errors` — Array of objects Описание ошибок.
- `warnings` — Array of objects Описание предупреждения.

Пример ответа:
```json
{
  "accepted": true,
  "errors": [
    {
      "data": [
        {
          "code": "string",
          "field": "string",
          "message": "string",
          "step": 0,
          "value": "string"
        }
      ],
      "sku": 0
    }
  ],
  "warnings": [
    {
      "data": [
        {
          "code": "string",
          "field": "string",
          "message": "string",
          "step": 0,
          "value": "string"
        }
      ],
      "sku": 0
    }
  ]
}
```

---

## Получить информацию о скидке от количества

`POST /v1/product/stairway-discount/by-quantity/get`

Operation ID: `ProductAPI_GetProductStairwayDiscountByQuantity`

Возвращает информацию о скидке на товар в зависимости от его количества в заказе. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/stairway-discount/by-quantity/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `skus required` — Array of strings <int64> <= 5000 items Список идентификаторов товара в системе Ozon — SKU.

### Ответы

- 200 Информация получена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `stairways` — Array of objects Информация о скидке от количества по конкретному товару.
  - `enabled` — boolean true , если скидка от количества включена.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `stairway` — object Информация об уровне скидки от количества.
  - `status` — string Enum: "IN_PROCESS" "ERROR" "SUCCESS" Статус изменения скидки от количества. Возможные значения: ERROR — ошибка при изменении скидки. Вызовите метод /v1/product/stairway-discount/by-quantity/set ещё раз. IN_PROCESS — изменение в процессе. SUCCESS — изменение скидки применено к товару.

Пример ответа:
```json
{
  "stairways": [
    {
      "enabled": true,
      "sku": 0,
      "stairway": {
        "steps": [
          {
            "discount": 0,
            "quantity": 0,
            "step": 0
          }
        ]
      },
      "status": "IN_PROCESS"
    }
  ]
}
```

---

## Получить отчёт о балансе

`POST /v1/finance/balance`

Operation ID: `GetFinanceBalanceV1`

Соответствует разделу Финансы → Баланс в личном кабинете. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/finance/balance" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2019-08-24",
  "date_to": "2019-09-24"
}'
```

### Тело запроса

- `date_from required` — string <date-time> Дата начала отчётного периода в формате YYYY-MM-DD .
- `date_to required` — string <date-time> Дата окончания отчётного периода в формате YYYY-MM-DD . Максимальный период между date_from и date_to — 30 дней.

### Ответы

- 200 Отчёт о балансе

#### Схема ответа (200)

- `cashflows` — object Информация о доходах и расходах.
- `total` — object Общие данные по балансу за период.

Пример ответа:
```json
{
  "cashflows": {
    "returns": {
      "amount": {
        "currency_code": "string",
        "value": 0
      },
      "amount_details": {
        "partner_programs": {
          "currency_code": "string",
          "value": 0
        },
        "points_for_discounts": "string",
        "revenue": {
          "currency_code": "string",
          "value": 0
        }
      },
      "fee": {
        "currency_code": "string",
        "value": 0
      }
    },
    "sales": {
      "amount": {
        "currency_code": "string",
        "value": 0
      },
      "amount_details": {
        "partner_programs": {
          "currency_code": "string",
          "value": 0
        },
        "points_for_discounts": "string",
        "revenue": {
          "currency_code": "string",
          "value": 0
        }
      },
      "fee": {
        "currency_code": "string",
        "value": 0
      }
    },
    "services": [
      {
        "amount": {
          "currency_code": "string",
          "value": 0
        },
        "name": "string"
      }
    ]
  },
  "total": {
    "accrued": {
      "currency_code": "string",
      "value": 0
    },
    "closing_balance": {
      "currency_code": "string",
      "value": 0
    },
    "opening_balance": {
      "currency_code": "string",
      "value": 0
    },
    "payments": [
      {
        "currency_code": "string",
        "value": 0
      }
    ]
  }
}
```

---

## Получить список заявок на скидку

`POST /v2/actions/discounts-task/list`

Operation ID: `GetDiscountTaskListV2`

Возвращает список товаров, которые покупатели хотят купить со скидкой. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/actions/discounts-task/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "last_id": 0,
  "limit": 50,
  "status": "ALL"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `last_id` — integer <int64> Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым.
- `limit` — integer <int64> <= 50 Default: 50 Enum: 5 10 15 20 30 50 Максимальное количество заявок на странице.
- `status` — string Default: "ALL" Enum: "ALL" "NEW" "APPROVED" "DECLINED" Статус заявки на скидку: ALL — все статусы, NEW — новая, APPROVED — одобренная, DECLINED — отклонённая.

### Ответы

- 200 Список заявок

#### Схема ответа (200)

- `tasks` — Array of objects Список заявок.
  - `approved_discount` — number <double> Скидка в рублях, которую одобрил продавец. Передайте значение 0 , если продавец не одобрил заявку.
  - `approved_price` — number <double> Одобренная цена.
  - `approved_quantity_max` — integer <uint64> Максимальное одобренное количество товаров.
  - `auto_moderated_info` — object Информация об автоматической модерации заявки.
  - `created_at` — string <date-time> Дата создания заявки.
  - `edited_till` — string <date-time> YYYY-MM-DD Время для изменения решения.
  - `edited_till_duration` — integer <uint64> Время для изменения решения в секундах.
  - `email` — string Электронный адрес сотрудника продавца, который обработал заявку.
  - `end_at` — string <date-time> Время окончания действия заявки.
  - `end_at_duration` — integer <uint64> Время окончания действия заявки в секундах.
  - `first_name` — string Имя сотрудника продавца, который обработал заявку.
  - `id` — integer <uint64> Идентификатор заявки.
  - `is_auto_moderated` — boolean true , если модерация была автоматической.
  - `last_name` — string Фамилия сотрудника продавца, который обработал заявку.
  - `min_auto_price` — number <double> Минимальное значение цены после автоприменения скидок и акций.
  - `moderated_at` — string <date-time> Дата модерации: просмотра, одобрения или отклонения заявки.
  - `name` — string Название товара.
  - `original_price` — number <double> Цена товара до всех скидок.
  - `patronymic` — string Отчество сотрудника продавца, который обработал заявку.
  - `reduction_factor` — number <double> Разница между ценой пользователя и продавца в момент создания заявки.
  - `requested_discount` — number <double> Скидка в процентах.
  - `requested_price` — number <double> Цена по заявке.
  - `requested_quantity_max` — integer <uint64> Запрошенное максимальное количество товаров.
  - `sku` — integer <uint64> Идентификатор товара в системе Ozon — SKU.
  - `status` — string Default: "ALL" Enum: "ALL" "NEW" "APPROVED" "DECLINED" Статус заявки на скидку: ALL — все статусы, NEW — новая, APPROVED — одобренная, DECLINED — отклонённая.

Пример ответа:
```json
{
  "tasks": [
    {
      "approved_discount": 0,
      "approved_price": 0,
      "approved_quantity_max": 0,
      "auto_moderated_info": {
        "max_percent": 0,
        "max_price": 0,
        "min_percent": 0,
        "min_price": 0
      },
      "created_at": "2019-08-24T14:15:22Z",
      "edited_till": "2019-08-24T14:15:22Z",
      "edited_till_duration": 0,
      "email": "string",
      "end_at": "2019-08-24T14:15:22Z",
      "end_at_duration": 0,
      "first_name": "string",
      "id": 0,
      "is_auto_moderated": true,
      "last_name": "string",
      "min_auto_price": 0,
      "moderated_at": "2019-08-24T14:15:22Z",
      "name": "string",
      "original_price": 0,
      "patronymic": "string",
      "reduction_factor": 0,
      "requested_discount": 0,
      "requested_price": 0,
      "requested_quantity_max": 0,
      "sku": 0,
      "status": "ALL"
    }
  ]
}
```

---

## Настроить видимость товара на витрине Ozon и Ozon Селект

`POST /v1/product/visibility/set`

Operation ID: `ProductVisibilitySet`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/visibility/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "item_placement": [
    {
      "placement": "OZON",
      "sku": 0
    }
  ]
}'
```

### Тело запроса

- `item_placement required` — Array of objects Информация о видимости товара.

### Ответы

- 200 Видимость товара настроена

#### Схема ответа (200)

- `items` — Array of objects Информация о видимости товаров.
- `items_errors` — Array of objects Товары с ошибками.

Пример ответа:
```json
{
  "items": [
    {
      "select_permission": "UNSPECIFIED",
      "seller_item_placement": "UNSPECIFIED",
      "seller_item_placement_list": [
        "UNSPECIFIED"
      ],
      "showcases_visibility": "UNSPECIFIED",
      "showcases_visibility_list": [
        "UNSPECIFIED"
      ],
      "sku": 0,
      "warnings": [
        "string"
      ]
    }
  ],
  "items_errors": [
    {
      "code": "string",
      "sku": 0
    }
  ]
}
```

---

## Получить список отправлений

`POST /v2/posting/digital/list`

Operation ID: `PostingDigitalList`

Возвращает список отправлений, по которым нужно загрузить коды цифровых товаров. Метод доступен только продавцам, которые работают с цифровыми товарами. Чтобы получить список отправлений в любом статусе, используйте метод /v3/posting/fbo/list . …

```bash
curl -X POST "https://api-seller.ozon.ru/v2/posting/digital/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "sort_dir": "asc",
  "filter": {
    "posting_numbers": [
      "42776709-0200-5"
    ],
    "order_numbers": [
      "12345-1"
    ],
    "since": "2025-05-10T06:49:21.122Z",
    "to": "2025-12-10T06:49:21.122Z"
  },
  "limit": 100,
  "cursor": "",
  "with": {
    "analytics_data": false,
    "financial_data": true,
    "legal_info": true
  }
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр для поиска отправлений.
- `limit` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.
- `with` — object Дополнительные поля, которые нужно добавить в ответ.

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернулись не все отправления.
- `postings` — Array of objects Список отправлений.

Пример ответа:
```json
{
  "postings": [
    {
      "order_id": 29137896277,
      "order_number": "42776709-0200",
      "posting_number": "42776709-0200-5",
      "status": "delivered",
      "cancel_reason_id": 0,
      "created_at": "2025-05-10T06:49:31.474Z",
      "in_process_at": "2025-05-10T06:49:29.108Z",
      "legal_info": {
        "company_name": "ООО Покупатель",
        "inn": "770123456789",
        "kpp": "770101001"
      },
      "products": [
        {
          "sku": 1624109204,
          "name": "Слипоны демисезонные для мальчиков",
          "offer_id": "S5257022-30",
          "price": {
            "amount": "1461.00",
            "currency": "RUB"
          },
          "quantity": 1
        }
      ],
      "analytics_data": {
        "city": "",
        "delivery_type": "",
        "is_premium": false,
        "payment_type_group_name": "",
        "warehouse_id": null,
        "warehouse_name": "",
        "is_legal": false
      },
      "financial_data": {
        "products": [
          {
            "product_id": 1624109204,
            "commission": {
              "amount": 0,
              "percent": 0,
              "currency": "RUB"
            },
            "payout": 0,
            "old_price": 1461,
            "price": 1461,
            "total_discount_value": 0,
            "actions": [],
            "quantity": 1
          }
        ],
        "cluster_from": "Екатеринбург",
        "cluster_to": "Екатеринбург"
      },
      "cancellation": {
        "cancellation_type": "Client",
        "cancellation_initiator": "Клиент"
      },
      "external_order": {
        "is_external": false,
        "platform_name": ""
      },
      "additional_data": []
    }
  ],
  "has_next": false,
  "cursor": ""
}
```

---

## Получить начисления по отправлениям

`POST /v1/finance/accrual/postings`

Operation ID: `GetFinanceAccrualPostings`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/finance/accrual/postings" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_numbers": [
    "string"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_numbers required` — Array of strings [ 1 .. 200 ] items Номера отправлений.

### Ответы

- 200 Начисления по отправлениям

#### Схема ответа (200)

- `posting_accruals` — Array of objects Список начислений по отправлениям.
  - `accruals` — Array of objects Список начислений.
  - `posting_number` — string Номер отправления.

Пример ответа:
```json
{
  "posting_accruals": [
    {
      "accruals": [
        {
          "accrual_date": "string",
          "accrued": {
            "amount": "string",
            "currency": "string"
          },
          "quantity": 0,
          "seller_price": {
            "amount": "string",
            "currency": "string"
          },
          "sku": 0,
          "type_id": 0
        }
      ],
      "posting_number": "string"
    }
  ]
}
```

---

## Получить справочник начислений

`POST /v1/finance/accrual/types`

Operation ID: `GetFinanceAccrualTypes`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/finance/accrual/types" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Справочник начислений

#### Схема ответа (200)

- `accrual_types` — Array of objects Информация о начислениях.
  - `description` — string Описание начисления.
  - `id` — integer <int32> Идентификатор начисления.
  - `name` — string Название начисления.

Пример ответа:
```json
{
  "accrual_types": [
    {
      "description": "string",
      "id": 0,
      "name": "string"
    }
  ]
}
```

---

## Получить начисления за день

`POST /v1/finance/accrual/by-day`

Operation ID: `GetFinanceAccrualByDay`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/finance/accrual/by-day" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date": "string",
  "last_id": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date required` — string YYYY-MM-DD Дата начислений. Самая ранняя — 1 января 2022 года.
- `last_id required` — string Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.

### Ответы

- 200 Начисления за день

#### Схема ответа (200)

- `accruals` — Array of objects Список начислений.
- `last_id` — string Идентификатор последнего значения на странице.

Пример ответа:
```json
{
  "accruals": [
    {
      "accrued_category": "UNSPECIFIED",
      "date": "string",
      "item_fees": {
        "fees": [
          {
            "fees": [
              {
                "accrued": {
                  "amount": "string",
                  "currency": "string"
                },
                "type_id": 0
              }
            ],
            "sku": 0
          }
        ]
      },
      "non_item_fee": {
        "accrued": {
          "amount": "string",
          "currency": "string"
        },
        "type_id": 0
      },
      "posting": {
        "delivery_schema": "string",
        "delivery_speed": 0,
        "products": [
          {
            "commission": {
              "bonus": {
                "amount": "string",
                "currency": "string"
              },
              "coinvestment": {
                "amount": "string",
                "currency": "string"
              },
              "commission": {
                "amount": "string",
                "currency": "string"
              },
              "commission_ratio": "string",
              "sale_amount": {
                "amount": "string",
                "currency": "string"
              },
              "sale_commission": {
                "amount": "string",
                "currency": "string"
              },
              "sale_price": {
                "amount": "string",
                "currency": "string"
              },
              "seller_price": {
                "amount": "string",
                "currency": "string"
              }
            },
            "delivery": {
              "services": [
                {
                  "accrued": {
                    "amount": null,
                    "currency": null
                  },
                  "type_id": 0
                }
              ],
              "total_accrued": {
                "amount": "string",
                "currency": "string"
              }
            },
            "sku": 0
          }
        ]
      },
      "total_amount": {
        "amount": "string",
        "currency": "string"
      },
      "accrual_id": 0,
      "unit_number": "string"
    }
  ],
  "last_id": "string"
}
```

---

## Получить информацию о видимости товара

`POST /v1/product/visibility/info`

Operation ID: `ProductVisibilityInfo`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/visibility/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `skus` — Array of strings <int64> [ 1 .. 350 ] items Идентификаторы товаров в системе Ozon — SKU.

### Ответы

- 200 Информация о видимости товара

#### Схема ответа (200)

- `items` — Array of objects Список товаров.
  - `showcases_visibility` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "OZON" "SELECT" "OZON_SELECT" "NONE" На каких витринах показывается товар: UNSPECIFIED — не определено; OZON — только на Ozon; SELECT — только на Селект; OZON_SELECT — на Селект и Ozon; NONE — товар скрыт везде.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример ответа:
```json
{
  "items": [
    {
      "showcases_visibility": "UNSPECIFIED",
      "sku": 0
    }
  ]
}
```

---
