# Акции продавца

Акции продавца Как работать с акциями продавца Подробнее об акциях продавца в Базе знаний продавца

_Тег: `SellerActions` · операций: 18_

## Создать акцию с механикой «Скидка»

`POST /v1/seller-actions/create/discount`

Operation ID: `SellerActionsCreateDiscount`

Недоступен для продавцов из СНГ. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_end required` — string <date-time> Дата и время окончания акции.
- `date_start required` — string <date-time> Дата и время начала акции.
- `min_action_percent required` — number <double> Минимальный процент скидки.
- `title` — string [ 1 .. 256 ] characters Название акции.

Пример запроса:

```json
{
  "date_end": "2019-08-24T14:15:22Z",
  "date_start": "2019-08-24T14:15:22Z",
  "min_action_percent": 0,
  "title": "string"
}
```

### Ответы

- 200 Акция создана

#### Схема ответа (200)

- `action_id` — integer <uint64> Идентификатор акции.

---

## Создать акцию с механикой «Скидка от суммы заказа»

`POST /v1/seller-actions/create/discount-with-condition`

Operation ID: `SellerActionsCreateDiscountWithCondition`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_end required` — string <date-time> Дата и время окончания акции.
- `date_start required` — string <date-time> Дата и время начала акции.
- `discount_type required` — string Enum: "PERCENT" "CURRENCY" Тип скидки: PERCENT — скидка в процентах; CURRENCY — скидка в валюте.
- `discount_value required` — number <float> Размер скидки.
- `min_order_amount required` — number <double> Сумма заказа, с которой действует скидка.
- `title` — string [ 1 .. 256 ] characters Название акции.

Пример запроса:

```json
{
  "date_end": "2019-08-24T14:15:22Z",
  "date_start": "2019-08-24T14:15:22Z",
  "discount_type": "PERCENT",
  "discount_value": 0,
  "min_order_amount": 0,
  "title": "string"
}
```

### Ответы

- 200 Акция создана

#### Схема ответа (200)

- `action_id` — integer <uint64> Идентификатор акции.

---

## Создать акцию с механикой «Беспроцентная рассрочка»

`POST /v1/seller-actions/create/installment`

Operation ID: `SellerActionsCreateInstallment`

Период рассрочки — 6 месяцев. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_start required` — string <date-time> Дата и время начала акции.
- `title required` — string [ 1 .. 256 ] characters Название акции.

Пример запроса:

```json
{
  "date_start": "2019-08-24T14:15:22Z",
  "title": "string"
}
```

### Ответы

- 200 Акция создана

#### Схема ответа (200)

- `action_id` — integer <uint64> Идентификатор акции.

---

## Создать акцию с механикой «Многоуровневая скидка от суммы»

`POST /v1/seller-actions/create/multi-level-discount`

Operation ID: `SellerActionsCreateMultiLevelDiscount`

Товары в акцию добавляются автоматически, использовать метод /v1/seller-actions/products/add не нужно. Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `date_end required` — string <date-time> Дата и время окончания акции.
- `date_start required` — string <date-time> Дата и время начала акции.
- `discount_levels required` — Array of objects [ 2 .. 4 ] items Уровни скидки.
- `discount_type required` — string Enum: "PERCENT" "CURRENCY" Тип скидки: PERCENT — скидка в процентах; CURRENCY — скидка в валюте.
- `is_legal_entities_segment` — boolean true , если акция только для юридических лиц.
- `title` — string [ 1 .. 256 ] characters Название акции.

Пример запроса:

```json
{
  "date_end": "2019-08-24T14:15:22Z",
  "date_start": "2019-08-24T14:15:22Z",
  "discount_levels": [
    {
      "discount_value": 0,
      "order_amount": 0
    },
    {
      "discount_value": 0,
      "order_amount": 0
    }
  ],
  "discount_type": "PERCENT",
  "is_legal_entities_segment": true,
  "title": "string"
}
```

### Ответы

- 200 Акция создана

#### Схема ответа (200)

- `action_id` — integer <uint64> Идентификатор акции.

---

## Создать акцию с механикой «Скидка по промокоду»

`POST /v1/seller-actions/create/voucher`

Operation ID: `SellerActionsCreateVoucher`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `budget required` — integer <int64> Бюджет акции. Если бюджет закончится, акция остановится.
- `date_end required` — string <date-time> Дата и время окончания акции.
- `date_start required` — string <date-time> Дата и время начала акции.
- `discount_type required` — string Enum: "PERCENT" "CURRENCY" Тип скидки: PERCENT — скидка в процентах; CURRENCY — скидка в валюте.
- `discount_value required` — number <double> Размер скидки.
- `title required` — string [ 1 .. 256 ] characters Название акции.
- `user_ids` — Array of strings <uint64> <= 50 Идентификаторы пользователей, которым доступен промокод.
- `voucher_parameters required` — object Параметры промокодов.

Пример запроса:

```json
{
  "budget": 0,
  "date_end": "2019-08-24T14:15:22Z",
  "date_start": "2019-08-24T14:15:22Z",
  "discount_type": "PERCENT",
  "discount_value": 0,
  "title": "string",
  "user_ids": [
    "string"
  ],
  "voucher_parameters": {
    "count_codes": 0,
    "is_private": true,
    "type": "ONE"
  }
}
```

### Ответы

- 200 Акция создана

#### Схема ответа (200)

- `action_id` — integer <uint64> Идентификатор акции.

---

## Обновить акцию с механикой «Скидка»

`POST /v1/seller-actions/update/discount`

Operation ID: `SellerActionsUpdateDiscount`

Недоступен для продавцов из СНГ. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `action_parameters` — object Параметры акции.

Пример запроса:

```json
{
  "action_id": 0,
  "action_parameters": {
    "date_end": "2019-08-24T14:15:22Z",
    "date_start": "2019-08-24T14:15:22Z",
    "title": "string"
  }
}
```

### Ответы

- 200 Акция обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

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

## Обновить акцию с механикой «Скидка от суммы заказа»

`POST /v1/seller-actions/update/discount-with-condition`

Operation ID: `SellerActionsUpdateDiscountWithCondition`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `action_parameters` — object Параметры акции.

Пример запроса:

```json
{
  "action_id": 0,
  "action_parameters": {
    "date_end": "2019-08-24T14:15:22Z",
    "date_start": "2019-08-24T14:15:22Z",
    "discount_value": 0,
    "min_order_amount": 0,
    "title": "string"
  }
}
```

### Ответы

- 200 Акция обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

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

## Обновить акцию с механикой «Беспроцентная рассрочка»

`POST /v1/seller-actions/update/installment`

Operation ID: `SellerActionsUpdateInstallment`

Период рассрочки — 6 месяцев. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `action_parameters` — object Параметры акции.

Пример запроса:

```json
{
  "action_id": 0,
  "action_parameters": {
    "date_start": "2019-08-24T14:15:22Z",
    "title": "string"
  }
}
```

### Ответы

- 200 Акция обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

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

## Обновить акцию с механикой «Многоуровневая скидка от суммы»

`POST /v1/seller-actions/update/multi-level-discount`

Operation ID: `SellerActionsUpdateMultiLevelDiscount`

Товары в акцию добавляются автоматически, вызывать метод /v1/seller-actions/products/add не нужно. Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `action_parameters` — object Параметры акции.

Пример запроса:

```json
{
  "action_id": 0,
  "action_parameters": {
    "date_end": "2019-08-24T14:15:22Z",
    "date_start": "2019-08-24T14:15:22Z",
    "discount_levels": [
      {
        "discount_value": 0,
        "order_amount": 0
      },
      {
        "discount_value": 0,
        "order_amount": 0
      }
    ],
    "is_legal_entities_segment": true,
    "title": "string"
  }
}
```

### Ответы

- 200 Акция обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

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

## Обновить акцию с механикой «Скидка по промокоду»

`POST /v1/seller-actions/update/voucher`

Operation ID: `SellerActionsUpdateVoucher`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `action_parameters` — object Параметры акции.

Пример запроса:

```json
{
  "action_id": 0,
  "action_parameters": {
    "budget": 0,
    "date_end": "2019-08-24T14:15:22Z",
    "date_start": "2019-08-24T14:15:22Z",
    "discount_value": 0,
    "title": "string",
    "user_ids": [
      "string"
    ]
  }
}
```

### Ответы

- 200 Акция обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

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

## Добавить товары в акцию

`POST /v1/seller-actions/products/add`

Operation ID: `SellerActionsProductsAdd`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `products required` — Array of objects <= 100 items Информация о товарах.

Пример запроса:

```json
{
  "action_id": 0,
  "products": [
    {
      "currency": "RUB",
      "discount_percent": 0,
      "sku": 0
    }
  ]
}
```

### Ответы

- 200 Товары добавлены

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

## Получить список доступных для акции товаров

`POST /v1/seller-actions/products/candidates`

Operation ID: `SellerActionsProductsCandidates`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `cursor` — integer <uint64> Указатель для выборки следующих данных.
- `limit required` — integer <int64> [ 1 .. 100 ] Default: 100 Максимальное количество элементов в ответе.

Пример запроса:

```json
{
  "action_id": 0,
  "cursor": 0,
  "limit": 100
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `cursor` — integer <uint64> Указатель для выборки следующих данных.
- `has_next` — boolean Признак, что в ответе вернулась только часть значений: true — сделайте повторный запрос с новым параметром cursor для получения остальных значений; false — ответ содержит все значения.
- `products` — Array of objects Информация о товарах.

Пример ответа:

```json
{
  "cursor": 0,
  "has_next": true,
  "products": [
    {
      "action_price": 0,
      "base_price": 0,
      "currency": "string",
      "discount_percent": 0,
      "is_active": true,
      "min_seller_price": 0,
      "name": "string",
      "offer_id": "string",
      "price": 0,
      "product_id": 0,
      "quant_size": 0,
      "quant_type": "UNSPECIFIED",
      "sku": [
        "string"
      ]
    }
  ]
}
```

---

## Удалить товары из акции

`POST /v1/seller-actions/products/delete`

Operation ID: `SellerActionsProductsDelete`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `skus required` — Array of strings <uint64> <= 100 items Идентификаторы товаров в системе Ozon — SKU.

Пример запроса:

```json
{
  "action_id": 0,
  "skus": [
    "string"
  ]
}
```

### Ответы

- 200 Товары удалены

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

## Получить список участвующих в акции товаров

`POST /v1/seller-actions/products/list`

Operation ID: `SellerActionsProductsList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `cursor` — integer <uint64> Указатель для выборки следующих данных.
- `limit required` — integer <int64> [ 1 .. 100 ] Default: 100 Максимальное количество элементов в ответе.

Пример запроса:

```json
{
  "action_id": 0,
  "cursor": 0,
  "limit": 100
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `cursor` — integer <uint64> Указатель для выборки следующих данных.
- `has_next` — boolean Признак, что в ответе вернулась только часть значений: true — сделайте повторный запрос с новым параметром cursor для получения остальных значений; false — ответ содержит все значения.
- `products` — Array of objects Информация о товарах.

Пример ответа:

```json
{
  "cursor": 0,
  "has_next": true,
  "products": [
    {
      "action_price": 0,
      "base_price": 0,
      "currency": "string",
      "discount_percent": 0,
      "is_active": true,
      "min_seller_price": 0,
      "name": "string",
      "offer_id": "string",
      "price": 0,
      "product_id": 0,
      "quant_size": 0,
      "quant_type": "UNSPECIFIED",
      "sku": [
        "string"
      ]
    }
  ]
}
```

---

## Перенести акцию в архив

`POST /v1/seller-actions/archive`

Operation ID: `SellerActionsArchive`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .

### Ответы

- 200 Акция в архиве

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

## Включить или выключить акцию

`POST /v1/seller-actions/change-activity`

Operation ID: `SellerActionsChangeActivity`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .
- `is_turn_on required` — boolean true , чтобы включить акцию.

Пример запроса:

```json
{
  "action_id": 0,
  "is_turn_on": true
}
```

### Ответы

- 200 Успешно

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

## Получить список акций

`POST /v1/seller-actions/list`

Operation ID: `SellerActionsList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_ids` — Array of strings <uint64> <= 100 items Идентификаторы акций.
- `action_type` — Array of strings Items Enum: "DISCOUNT" "VOUCHER_DISCOUNT" "DISCOUNT_WITH_CONDITION" "INSTALLMENT" "INDIVIDUAL_DISCOUNT_BY_PRODUCTS" "OZON_ACCOUNT_DISCOUNT" "MULTI_LEVEL_DISCOUNT_ON_AMOUNT" Механика акции: DISCOUNT — скидка; VOUCHER_DISCOUNT — скидка по промокоду; DISCOUNT_WITH_CONDITION — скидка от суммы заказа; INSTALLMENT — беспроцентная рассрочка; INDIVIDUAL_DISCOUNT_BY_PRODUCTS — бонусы продавца; OZON_ACCOUNT_DISCOUNT — повышенная скидка с картой Ozon Банка; MULTI_LEVEL_DISCOUNT_ON_AMOUNT — многоуровневая скидка от суммы.
- `limit required` — integer <uint64> [ 1 .. 100 ] Количество значений на странице.
- `offset` — integer <uint64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `search` — string >= 3 characters Поиск по названию акции.
- `status` — Array of strings Items Enum: "ACTIVE" "ENDED" "PLANNED" "PAUSED" Статус акции: ACTIVE — активна; ENDED — завершена; PLANNED — запланирована; PAUSED — приостановлена.

Пример запроса:

```json
{
  "action_ids": [
    "string"
  ],
  "action_type": [
    "DISCOUNT"
  ],
  "limit": 1,
  "offset": 0,
  "search": "string",
  "status": [
    "ACTIVE"
  ]
}
```

### Ответы

- 200 Список акций

#### Схема ответа (200)

- `actions` — Array of objects Список акций.
- `total` — integer <uint64> Общее количество акций.

Пример ответа:

```json
{
  "actions": [
    {
      "action_id": 0,
      "action_parameters": {
        "addresses": [
          "string"
        ],
        "auto_stop_action_reason": "UNSPECIFIED",
        "budget": 0,
        "budget_spent": 0,
        "date_end": "2019-08-24T14:15:22Z",
        "date_start": "2019-08-24T14:15:22Z",
        "discount_levels": [
          {
            "discount_value": 0,
            "order_amount": 0
          }
        ],
        "discount_type": "UNSPECIFIED",
        "discount_value": 0,
        "is_legal_entities_segment": true,
        "min_action_percent": 0,
        "min_order_amount": 0,
        "picked_segments": [
          {
            "segments": [
              {
                "description": "string",
                "id": 0,
                "name": "string",
                "type": "UNSPECIFIED"
              }
            ]
          }
        ],
        "status": "ACTIVE",
        "title": "string",
        "type": "DISCOUNT",
        "voucher_parameters": {
          "count_codes": 0,
          "is_private": true,
          "type": "UNSPECIFIED"
        },
        "warehouses": [
          "string"
        ]
      },
      "allow_delete": true,
      "highlight_url": "string",
      "is_editable": true,
      "is_participated": true,
      "is_turn_on": true,
      "sku_count": 0
    }
  ],
  "total": 0
}
```

---

## Получить файл с промокодами в формате CSV

`POST /v1/seller-actions/voucher/get`

Operation ID: `SellerActionsVoucherGet`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `action_id required` — integer <uint64> Идентификатор акции. Получите значение параметра методом /v1/seller-actions/list .

### Ответы

- 200 Файл с промокодами

#### Схема ответа (200)

- `file` — string Ссылка на CSV-файл с промокодами.

---
