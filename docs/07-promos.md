# Акции Ozon

Акции Ozon Для продвижения товаров участвуйте в акциях, которые Ozon проводит для покупателей. Подробнее об акциях в Базе знаний продавца .

_Тег: `Promos` · операций: 8_

## Список акций

`GET /v1/actions`

Operation ID: `Promos`

Метод для получения списка акций Ozon, в которых можно участвовать. Подробнее об акциях Ozon

### Ответы

- 200 Список акций

#### Схема ответа (200)

- `result` — Array of objects Результаты запроса.
  - `id` — number <double> Идентификатор акции.
  - `title` — string Название акции.
  - `action_type` — string Тип акции.
  - `description` — string Описание акции.
  - `date_start` — string Дата начала акции.
  - `date_end` — string Дата окончания акции.
  - `auto_add_dates` — Array of strings <date-time> Дата и время автодобавления товаров в акцию.
  - `freeze_date` — string Дата приостановки акции. Если поле заполнено, продавец не может повышать цены, изменять список товаров и уменьшать количество единиц товаров в акции. Продавец может понижать цены и увеличивать количество единиц товара в акции.
  - `potential_products_count` — number <double> Количество товаров, доступных для акции.
  - `participating_products_count` — number <double> Количество товаров, которые участвуют в акции.
  - `is_participating` — boolean Участвуете вы в этой акции или нет.
  - `is_voucher_action` — boolean Признак, что для участия в акции покупателям нужен промокод.
  - `banned_products_count` — number <double> Количество заблокированных товаров.
  - `with_targeting` — boolean Признак, что акция с целевой аудиторией.
  - `order_amount` — number <double> Сумма заказа.
  - `discount_type` — string Тип скидки.
  - `discount_value` — number <double> Размер скидки.

Пример ответа:

```json
{
  "result": [
    {
      "id": 71342,
      "title": "test voucher #2",
      "date_start": "2021-11-22T09:46:38Z",
      "date_end": "2021-11-30T20:59:59Z",
      "auto_add_dates": [
        "2025-12-30T20:59:59Z"
      ],
      "potential_products_count": 0,
      "is_participating": true,
      "participating_products_count": 5,
      "description": "",
      "action_type": "DISCOUNT",
      "banned_products_count": 0,
      "with_targeting": false,
      "discount_type": "UNKNOWN",
      "discount_value": 0,
      "order_amount": 0,
      "freeze_date": "",
      "is_voucher_action": true
    }
  ]
}
```

---

## Список доступных для акции товаров

`POST /v1/actions/candidates`

Operation ID: `PromosCandidates`

Метод для получения списка товаров, которые могут участвовать в акции, по её идентификатору.

### Тело запроса (application/json)

- `action_id required` — number <double> Идентификатор акции. Можно получить с помощью метода /v1/actions .
- `limit` — number <double> Количество ответов на странице. По умолчанию — 100.
- `last_id` — number <double> Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым.

Пример запроса:

```json
{
  "action_id": 63337,
  "limit": 10,
  "last_id": "bnVсbA==123123das"
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `products` — Array of objects Список товаров.
  - `total` — number <double> Общее количество товаров, которое доступно для акции.
  - `last_id` — number <double> Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .

Пример ответа:

```json
{
  "result": {
    "products": [
      {
        "id": 226,
        "price": 250,
        "action_price": 10,
        "alert_max_action_price_failed": true,
        "alert_max_action_price": 31,
        "max_action_price": 175,
        "add_mode": "NOT_SET",
        "stock": 2,
        "min_stock": 1,
        "current_boost": 3,
        "price_min_elastic": 150,
        "price_max_elastic": 300,
        "min_boost": 12,
        "max_boost": 15
      }
    ],
    "total": 2,
    "last_id": "bnVсbA=="
  }
}
```

---

## Список участвующих в акции товаров

`POST /v1/actions/products`

Operation ID: `PromosProducts`

Метод для получения списка товаров, участвующих в акции, по её идентификатору.

### Тело запроса (application/json)

- `action_id required` — number <double> Идентификатор акции. Можно получить с помощью метода /v1/actions .
- `limit` — number <double> Количество ответов на странице. По умолчанию — 100.
- `last_id` — number <double> Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым.

Пример запроса:

```json
{
  "action_id": 66011,
  "limit": 10,
  "last_id": "bnVсbA=="
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `products` — Array of objects Список товаров.
  - `total` — number <double> Общее количество товаров, которое доступно для акции.
  - `last_id` — number <double> Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .

Пример ответа:

```json
{
  "result": {
    "products": [
      {
        "id": 28745,
        "price": 99,
        "action_price": 50,
        "alert_max_action_price_failed": true,
        "alert_max_action_price": 31,
        "max_action_price": 32,
        "add_mode": "MANUAL",
        "stock": 20,
        "min_stock": 3,
        "current_boost": 1,
        "price_min_elastic": 2,
        "price_max_elastic": 5,
        "min_boost": 10,
        "max_boost": 15
      }
    ],
    "total": 263,
    "last_id": "bnVсbA=="
  }
}
```

---

## Добавить товар в акцию

`POST /v1/actions/products/activate`

Operation ID: `PromosProductsActivate`

Метод для добавления товаров в доступную акцию.

### Тело запроса (application/json)

- `action_id required` — number <double> Идентификатор акции. Можно получить с помощью метода /v1/actions .
- `products required` — Array of objects <= 1000 items Список товаров.

Пример запроса:

```json
{
  "action_id": 60564,
  "products": [
    {
      "action_price": 356,
      "product_id": 1389,
      "stock": 10
    }
  ]
}
```

### Ответы

- 200 Товар добавлен в акцию

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `product_ids` — Array of numbers <double> Список идентификаторов товаров, которые добавлены в акцию.
  - `rejected` — Array of objects Список товаров, которые не удалось добавить в акцию.

Пример ответа:

```json
{
  "result": {
    "product_ids": [
      1389
    ],
    "rejected": []
  }
}
```

---

## Удалить товары из акции

`POST /v1/actions/products/deactivate`

Operation ID: `PromosProductsDeactivate`

Метод для удаления товаров из акции.

### Тело запроса (application/json)

- `action_id required` — number <double> Идентификатор акции. Можно получить с помощью метода /v1/actions .
- `product_ids required` — Array of numbers <double> Список идентификаторов товаров в системе Ozon — product_id .

Пример запроса:

```json
{
  "action_id": 66011,
  "product_ids": [
    14975
  ]
}
```

### Ответы

- 200 Товары удалены из акции

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `product_ids` — Array of numbers <double> Список идентификаторов товаров, которые удалены из акции.
  - `rejected` — Array of objects Список товаров, которые не удалось удалить из акции.

Пример ответа:

```json
{
  "result": {
    "product_ids": [
      14975
    ],
    "rejected": []
  }
}
```

---

## Список заявок на скидку

`POST /v1/actions/discounts-task/list`

Operation ID: `promos_task_list`

Метод устаревает и будет отключён в будущем. Переключитесь на /v2/actions/discounts-task/list . Метод для получения списка товаров, которые покупатели хотят купить со скидкой.

### Тело запроса (application/json)

- `status required` — string Default: "UNKNOWN" Enum: "NEW" "SEEN" "APPROVED" "PARTLY_APPROVED" "DECLINED" "AUTO_DECLINED" "DECLINED_BY_USER" "COUPON" "PURCHASED" Статус заявки на скидку: NEW — новая, SEEN — просмотренная, APPROVED — одобренная, PARTLY_APPROVED — одобренная частично, DECLINED — отклонённая, AUTO_DECLINED — отклонена автоматически, DECLINED_BY_USER — отклонена покупателем, COUPON — скидка по купону, PURCHASED — купленная.
- `page required` — integer <uint64> Страница, с которой нужно выгрузить список заявок на скидку.
- `limit required` — integer <uint64> Максимальное количество заявок на странице.

Пример запроса:

```json
{
  "status": "UNKNOWN",
  "page": 1,
  "limit": 50
}
```

### Ответы

- 200 Список заявок

#### Схема ответа (200)

- `result` — Array of objects Список заявок.
  - `id` — integer <uint64> Идентификатор заявки.
  - `created_at` — string <date-time> Дата создания заявки.
  - `end_at` — string <date-time> Время окончания действия заявки.
  - `edited_till` — string <date-time> Время для изменения решения.
  - `status` — string Статус заявки.
  - `customer_name` — string Имя покупателя.
  - `sku` — integer <uint64> Идентификатор товара в системе Ozon — SKU.
  - `user_comment` — string Комментарий покупателя к заявке.
  - `seller_comment` — string Комментарий продавца к заявке.
  - `requested_price` — number <double> Цена по заявке.
  - `approved_price` — number <double> Одобренная цена.
  - `original_price` — number <double> Цена товара до всех скидок.
  - `discount` — number <double> Скидка в рублях.
  - `discount_percent` — number <double> Скидка в процентах.
  - `base_price` — number <double> Базовая цена, по которой товар продаётся на Ozon, если не участвует в акции.
  - `min_auto_price` — number <double> Минимальное значение цены после автоприменения скидок и акций.
  - `prev_task_id` — integer <uint64> Идентификатор предыдущей заявки от покупателя по этому товару.
  - `is_damaged` — boolean Является ли товар уценённым. true , если уценённый.
  - `moderated_at` — string <date-time> Дата модерации: просмотра, одобрения или отклонения заявки.
  - `approved_discount` — number <double> Скидка в рублях, которую одобрил продавец. Передайте значение 0 , если продавец не одобрял заявку.
  - `approved_discount_percent` — number <double> Скидка в процентах, которую одобрил продавец. Передайте значение 0 , если продавец не одобрял заявку.
  - `is_purchased` — boolean Покупал ли пользователь товар. true , если покупал.
  - `is_auto_moderated` — boolean Была ли заявка промодерирована автоматически. true , если модерация была автоматической.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `email` — string Электронный адрес сотрудника продавца, который обработал заявку.
  - `last_name` — string Фамилия сотрудника продавца, который обработал заявку.
  - `first_name` — string Имя сотрудника продавца, который обработал заявку.
  - `patronymic` — string Отчество сотрудника продавца, который обработал заявку.
  - `approved_quantity_min` — integer <uint64> Минимальное одобренное количество товаров.
  - `approved_quantity_max` — integer <uint64> Максимальное одобренное количество товаров.
  - `requested_quantity_min` — integer <uint64> Запрошенное минимальное количество товаров.
  - `requested_quantity_max` — integer <uint64> Запрошенное максимальное количество товаров.
  - `requested_price_with_fee` — number <double> Цена по заявке c региональной наценкой.
  - `approved_price_with_fee` — number <double> Одобренная цена с региональной наценкой.
  - `approved_price_fee_percent` — number <double> Региональная наценка в процентах.

Пример ответа:

```json
{
  "result": [
    {
      "id": 12345,
      "created_at": "2023-10-15T09:30:00Z",
      "end_at": "2023-10-20T18:45:00Z",
      "edited_till": "2023-10-18T12:00:00Z",
      "status": "approved",
      "customer_name": "Иван Иванов",
      "sku": 100500,
      "user_comment": "Товар необходим срочно, прошу ускорить обработку",
      "seller_comment": "Согласовано с менеджером, скидка предоставлена",
      "requested_price": 15000,
      "approved_price": 14500,
      "original_price": 16000,
      "discount": 1000,
      "discount_percent": 6.25,
      "base_price": 15500,
      "min_auto_price": 14000,
      "prev_task_id": 12344,
      "is_damaged": false,
      "moderated_at": "2023-10-16T10:15:00Z",
      "approved_discount": 500,
      "approved_discount_percent": 3.125,
      "is_purchased": true,
      "is_auto_moderated": false,
      "offer_id": "OFFER-2023-1001",
      "email": "ivan.ivanov@example.com",
      "last_name": "Иванов",
      "first_name": "Иван",
      "patronymic": "Петрович",
      "approved_quantity_min": 1,
      "approved_quantity_max": 5,
      "requested_quantity_min": 2,
      "requested_quantity_max": 10,
      "requested_price_with_fee": 15500,
      "approved_price_with_fee": 15000,
      "approved_price_fee_percent": 3.2
    }
  ]
}
```

---

## Согласовать заявку на скидку

`POST /v1/actions/discounts-task/approve`

Operation ID: `promos_task_approve`

Вы можете согласовывать заявки в статусах: NEW — новые, SEEN — просмотренные.

### Тело запроса (application/json)

- `tasks required` — Array of objects Список заявок.

Пример запроса:

```json
{
  "tasks": [
    {
      "id": 78901,
      "approved_price": 12500,
      "seller_comment": "ok",
      "approved_quantity_min": 3,
      "approved_quantity_max": 7
    }
  ]
}
```

### Ответы

- 200 Заявки согласованы

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `fail_details` — Array of objects Ошибки при создании заявки.
  - `success_count` — integer <int32> Количество заявок с успешной сменой статуса.
  - `fail_count` — integer <int32> Количество заявок, у которых не удалось сменить статус.

Пример ответа:

```json
{
  "result": {
    "fail_details": [
      {
        "task_id": 78901,
        "error_for_user": "Скидка отклонена"
      }
    ],
    "success_count": 5,
    "fail_count": 1
  }
}
```

---

## Отклонить заявку на скидку

`POST /v1/actions/discounts-task/decline`

Operation ID: `promos_task_decline`

Вы можете отклонить заявки в статусах: NEW — новые, SEEN — просмотренные.

### Тело запроса (application/json)

- `tasks required` — Array of objects Список заявок.

Пример запроса:

```json
{
  "tasks": [
    {
      "id": 78901,
      "seller_comment": "Ok"
    }
  ]
}
```

### Ответы

- 200 Заявки отклонены

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `fail_details` — Array of objects Ошибки при создании заявки.
  - `success_count` — integer <int32> Количество заявок с успешной сменой статуса.
  - `fail_count` — integer <int32> Количество заявок, у которых не удалось сменить статус.

Пример ответа:

```json
{
  "result": {
    "fail_details": [
      {
        "task_id": 78901,
        "error_for_user": "Скидка отклонена"
      }
    ],
    "success_count": 5,
    "fail_count": 1
  }
}
```

---
