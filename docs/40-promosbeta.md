# Акции Ozon

_Тег: `PromosBeta` · операций: 4_

## Получить список товаров из автодобавления в акцию

`POST /v1/actions/auto-add/products/list`

Operation ID: `ActionsAutoAddProductsList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции.
- `auto_add_date required` — string <date-time> Дата и время автодобавления товаров в акцию из параметра result.auto_add_dates в ответе метода /v1/actions .
- `limit required` — integer <uint64> [ 1 .. 100 ] Количество значений в ответе.
- `offset` — integer <uint64> Default: 0 Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , ответ начнётся с 11-го найденного элемента.

Пример запроса:

```json
{
  "action_id": "250204",
  "auto_add_date": "2035-08-28T14:00:00Z",
  "limit": "1",
  "offset": "0"
}
```

### Ответы

- 200 Список товаров с автодобавлением
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `products` — Array of objects Список товаров с автодобавлением.
- `total` — integer <uint64> Общее количество товаров.

Пример ответа:

```json
{
  "products": [
    {
      "produсt_id": "14903",
      "offer_id": "PS0007",
      "sku": "146279508",
      "name": "Ароматизатор / Масло для бани / Эфирное масло \"Пихта\", 250 мл",
      "price": 114,
      "max_discount_price": 79,
      "min_seller_price:": 50,
      "marketplace_seller_price": 59,
      "action_price_to_auto_add": 79,
      "min_action_quantity": "0",
      "quantity_to_auto_add": "10",
      "currency": "RUB",
      "add_mode": "MANUAL"
    }
  ],
  "total": "443"
}
```

---

## Получить список доступных товаров для автодобавления в акцию

`POST /v1/actions/auto-add/products/candidates`

Operation ID: `ActionsAutoAddProductsCandidates`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции.
- `auto_add_date required` — string <date-time> Дата и время автодобавления товаров в акцию из параметра result.auto_add_dates в ответе метода /v1/actions .
- `limit required` — integer <uint64> [ 1 .. 100 ] Количество значений в ответе.
- `offset` — integer <uint64> Default: 0 Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , ответ начнётся с 11-го найденного элемента.

Пример запроса:

```json
{
  "action_id": "250204",
  "auto_add_date": "2035-08-28T14:00:00Z",
  "limit": "1",
  "offset": "0"
}
```

### Ответы

- 200 Список доступных товаров для автодобавления в акцию
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `products` — Array of objects Список доступных товаров для автодобавления.
- `total` — integer <uint64> Общее количество товаров.

Пример ответа:

```json
{
  "products": [
    {
      "produсt_id": "14903",
      "offer_id": "PS0007",
      "sku": "146279508",
      "name": "Ароматизатор / Масло для бани / Эфирное масло \"Пихта\", 250 мл",
      "price": 114,
      "base_price": 346,
      "max_discount_price": 79,
      "min_seller_price:": 50,
      "marketplace_seller_price": 59,
      "action_price_to_auto_add": 79,
      "min_action_quantity": "0",
      "quantity_to_auto_add": "10",
      "currency": "RUB"
    }
  ],
  "total": "443"
}
```

---

## Удалить товары из автодобавления в акцию

`POST /v1/actions/auto-add/products/delete`

Operation ID: `ActionsAutoAddProductsDelete`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции.
- `auto_add_date` — string <date-time> Дата и время автодобавления товаров в акцию из параметра result.auto_add_dates в ответе метода /v1/actions .
- `product_ids` — Array of strings <uint64> [ 1 .. 1000 ] items Идентификаторы товаров в системе Ozon — product_id .

Пример запроса:

```json
{
  "action_id": "250204",
  "auto_add_date": "2035-08-28T14:00:00Z",
  "product_ids": [
    "14914"
  ]
}
```

### Ответы

- 200 Товары удалены из автодобавления
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `product_ids` — Array of strings <uint64> Идентификаторы товаров, которые удалены из автодобавления.

---

## Добавить или обновить товары в автодобавлении в акцию

`POST /v1/actions/auto-add/products/update`

Operation ID: `ActionsAutoAddProductsUpdate`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции.
- `auto_add_date` — string <date-time> Дата и время автодобавления товаров в акцию из параметра result.auto_add_dates в ответе метода /v1/actions .
- `to_update` — Array of objects Список товаров, которые нужно добавить или обновить в автодобавлении.

Пример запроса:

```json
{
  "action_id": "250204",
  "auto_add_dates": "2035-08-28T14:00:00Z",
  "to_update": [
    {
      "currency": "RUB",
      "product_id": "14914",
      "quantity": 10,
      "action_price": 100
    }
  ]
}
```

### Ответы

- 200 Товары добавлены или обновлены в автодобавлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `below_min_price` — Array of objects Список товаров с ценой ниже минимальной.
- `extremely_low_price` — Array of objects Список товаров со скидкой больше 70%.
- `failed_price` — Array of objects Список товаров, которые не прошли валидацию по цене.
- `rejected` — Array of objects Идентификаторы товаров, которые не получилось добавить или обновить.
- `updated_ids` — Array of strings <uint64> Идентификаторы товаров, которые получилось добавить или обновить.

Пример ответа:

```json
{
  "updated_ids": [
    "14914"
  ],
  "rejected": [],
  "below_min_price": [
    {
      "key": "14914",
      "value": 100
    }
  ],
  "extremely_low_price": [],
  "failed_price": []
}
```

---
