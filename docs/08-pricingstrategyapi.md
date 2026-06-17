# Стратегии ценообразования

_Тег: `PricingStrategyAPI` · операций: 12_

## Список конкурентов

`POST /v1/pricing-strategy/competitors/list`

Operation ID: `pricing_competitors`

Метод для получения списка конкурентов — продавцов с похожими товарами в других интернет-магазинах и маркетплейсах.

### Тело запроса (application/json)

- `page required` — integer <int64> Страница списка, с которой нужно выгрузить конкурентов. Минимальное значение — 1 .
- `limit required` — integer <int64> Максимальное количество конкурентов на странице. Допустимы значения от 1 до 50 .

### Ответы

- 200 Список конкурентов

#### Схема ответа (200)

- `competitor` — Array of objects Список конкурентов.
- `total` — integer <int32> Общее количество конкурентов.

Пример ответа:

```json
{
  "competitor": [
    {
      "id": 7820251,
      "name": "grenmarketshop.ru"
    }
  ],
  "total": 33
}
```

---

## Список стратегий

`POST /v1/pricing-strategy/list`

Operation ID: `pricing_list`

### Тело запроса (application/json)

- `page required` — integer <int64> Страница списка, с которой нужно выгрузить стратегии. Минимальное значение — 1 .
- `limit required` — integer <int64> Максимальное количество стратегий на странице. Допустимые значения — от 1 до 50 .

### Ответы

- 200 Список стратегий

#### Схема ответа (200)

- `strategies` — Array of objects Список стратегий.
- `total` — integer <int32> Общее количество стратегий.

Пример ответа:

```json
{
  "strategies": [
    {
      "id": "2fb3e6a3-3db5-4bb4-8430-b2de39fc3265",
      "name": "стратегия_от_CID7",
      "type": "COMP_PRICE",
      "update_type": "strategyEnabled",
      "updated_at": "2024-05-02 14:47:02.594774+00",
      "products_count": 1,
      "competitors_count": 1,
      "enabled": true
    }
  ],
  "total": 5
}
```

---

## Создать стратегию

`POST /v1/pricing-strategy/create`

Operation ID: `pricing_create`

### Тело запроса (application/json)

- `competitors required` — Array of objects Список конкурентов.
- `strategy_name required` — string Название стратегии.

Пример запроса:

```json
{
  "strategy_name": "Новая стратегия",
  "competitors": [
    {
      "competitor_id": 1008426,
      "coefficient": 1
    },
    {
      "competitor_id": 204,
      "coefficient": 1
    },
    {
      "competitor_id": 91,
      "coefficient": 1
    },
    {
      "competitor_id": 48,
      "coefficient": 1
    }
  ],
  "company_id": 7
}
```

### Ответы

- 200 Стратегия создана

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `strategy_id` — string Идентификатор стратегии.

Пример ответа:

```json
{
  "result": {
    "id": "4f3a1d4c-5833-4f04-b69b-495cbc1f6f1c"
  }
}
```

---

## Информация о стратегии

`POST /v1/pricing-strategy/info`

Operation ID: `pricing_info`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `strategy_id required` — string Идентификатор стратегии.

Пример запроса:

```json
{
  "strategy_id": "2fb3e6a3-3db5-4bb4-8430-b2de39fc3265"
}
```

### Ответы

- 200 Информация о стратегии

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `competitors` — Array of objects Список конкурентов.
  - `enabled` — boolean Статус стратегии: true — включена, false — отключена.
  - `name` — string Название стратегии.
  - `type` — string Тип стратегии: MIN_EXT_PRICE — системная стратегия, COMP_PRICE — пользовательская стратегия.
  - `update_type` — string Тип последнего изменения стратегии: strategyEnabled — возобновлена, strategyDisabled — остановлена, strategyChanged — обновлена, strategyCreated — создана, strategyItemsListChanged — изменён набор товаров в стратегии.

Пример ответа:

```json
{
  "result": {
    "strategy_name": "стратегия_от_CID7",
    "enabled": true,
    "update_type": "strategyItemsListChanged",
    "type": "COMP_PRICE",
    "competitors": [
      {
        "competitor_id": 204,
        "coefficient": 1
      },
      {
        "competitor_id": 1008426,
        "coefficient": 1
      }
    ]
  }
}
```

---

## Обновить стратегию

`POST /v1/pricing-strategy/update`

Operation ID: `pricing_update`

Можно обновить все стратегии кроме системной.

### Тело запроса (application/json)

- `competitors required` — Array of objects Список конкурентов.
- `strategy_id required` — string Идентификатор стратегии.
- `strategy_name required` — string Название стратегии.

Пример запроса:

```json
{
  "strategy_id": "a3de1826-9c54-40f1-bb6d-1a9e2638b058",
  "strategy_name": "Новая стратегия",
  "competitors": [
    {
      "competitor_id": 1008426,
      "coefficient": 1
    },
    {
      "competitor_id": 204,
      "coefficient": 1
    },
    {
      "competitor_id": 91,
      "coefficient": 1
    },
    {
      "competitor_id": 48,
      "coefficient": 1
    },
    {
      "competitor_id": 45,
      "coefficient": 1
    }
  ]
}
```

### Ответы

- 200 Стратегия обновлена

---

## Добавить товары в стратегию

`POST /v1/pricing-strategy/products/add`

Operation ID: `pricing_items-add`

### Тело запроса (application/json)

- `product_id required` — Array of strings <int64> Список идентификаторов товаров в системе Ozon — product_id . Максимальное количество — 50.
- `strategy_id required` — string Идентификатор стратегии.

Пример запроса:

```json
{
  "product_id": [
    "29209"
  ],
  "strategy_id": "e29114f0-177d-4160-8d06-2bc528470dda"
}
```

### Ответы

- 200 Ошибки при добавлении товаров

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `errors` — Array of objects Товары с ошибками.
  - `failed_product_count` — integer <int32> Количество товаров с ошибками.

Пример ответа:

```json
{
  "result": {
    "failed_product_count": 0
  }
}
```

---

## Список идентификаторов стратегий

`POST /v1/pricing-strategy/strategy-ids-by-product-ids`

Operation ID: `pricing_ids`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `product_id required` — Array of strings <int64> Список идентификаторов товаров в системе Ozon — product_id . Максимальное количество — 50.

### Ответы

- 200 Список идентификаторов

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `products_info` — Array of objects Информация о товаре.

Пример ответа:

```json
{
  "result": {
    "products_info": [
      {
        "product_id": 29209,
        "strategy_id": "b7cd30e6-5667-424d-b105-fbec30a52477"
      }
    ]
  }
}
```

---

## Список товаров в стратегии

`POST /v1/pricing-strategy/products/list`

Operation ID: `pricing_items-list`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `strategy_id required` — string Идентификатор стратегии.

Пример запроса:

```json
{
  "strategy_id": "b7cd30e6-5667-424d-b105-fbec30a52477"
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `result` — object Список товаров.
  - `product_id` — Array of strings <int64> Идентификатор товара в системе Ozon — product_id .

Пример ответа:

```json
{
  "result": {
    "product_id": [
      "29209"
    ]
  }
}
```

---

## Цена товара у конкурента

`POST /v1/pricing-strategy/product/info`

Operation ID: `pricing_items-info`

Если вы добавили товар в стратегию ценообразования, метод вернёт цену и ссылку на товар у конкурента.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `product_id required` — integer <int64> Идентификатор товара в системе Ozon — product_id .

### Ответы

- 200 Цена товара у конкурента

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `strategy_id` — string Идентификатор стратегии.
  - `is_enabled` — boolean true , если товар участвует в стратегии ценообразования.
  - `strategy_product_price` — integer <int32> Цена по стратегии.
  - `price_downloaded_at` — string Дата установки цены по стратегии.
  - `strategy_competitor_id` — integer <int64> Deprecated Идентификатор конкурента.
  - `strategy_competitor_product_url` — string Ссылка на товар конкурента.

Пример ответа:

```json
{
  "result": {
    "strategy_id": "b7cd30e6-5667-424d-b105-fbec30a52477",
    "is_enabled": true,
    "strategy_product_price": 500,
    "price_downloaded_at": "2022-11-17T15:33:53.936Z",
    "strategy_competitor_id": "b7cd30e6-5667-424d-b105-fbec30a524778",
    "strategy_competitor_product_url": "http://primerurl/pricing-strategy/product/info.ru"
  }
}
```

---

## Удалить товары из стратегии

`POST /v1/pricing-strategy/products/delete`

Operation ID: `pricing_items-delete`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `product_id required` — Array of strings <int64> Список идентификаторов товаров в системе Ozon — product_id . Максимальное количество — 50.

### Ответы

- 200 Ошибки при удалении товаров

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `failed_product_count` — integer <int32> Количество товаров с ошибками.

Пример ответа:

```json
{
  "result": {
    "failed_product_count": 2
  }
}
```

---

## Изменить статус стратегии

`POST /v1/pricing-strategy/status`

Operation ID: `pricing_status`

Можно изменить статус любой стратегии кроме системной.

### Тело запроса (application/json)

- `enabled` — boolean Статус стратегии: true — включена, false — отключена.
- `strategy_id required` — string Идентификатор стратегии.

Пример запроса:

```json
{
  "strategy_id": "c7516438-7124-4e2c-85d3-ccd92b6b9b65",
  "enabled": true
}
```

### Ответы

- 200 Статус стратегии изменён

---

## Удалить стратегию

`POST /v1/pricing-strategy/delete`

Operation ID: `pricing_delete`

Можно удалить любую стратегию кроме системной.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `strategy_id required` — string Идентификатор стратегии.

Пример запроса:

```json
{
  "strategy_id": "b7cd30e6-5667-424d-b105-fbec30a52477"
}
```

### Ответы

- 200 Стратегия удалена

---
