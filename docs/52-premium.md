# Premium-методы

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `Premium` · методов: 10_

## Отправить сообщение

`POST /v1/chat/send/message`

Operation ID: `ChatAPI_ChatSendMessage`

Доступно для продавцов с подпиской Premium Plus или Premium Pro . Отправляет сообщение в существующий чат по его идентификатору. Получите список чатов с покупателем chats.chat.chat_type="Buyer_Seller" в ответе метода /v3/chat/list . …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/chat/send/message" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "chat_id": "99feb3fc-a474-469f-95d5-268b470cc607",
  "text": "test"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `chat_id required` — string Идентификатор чата.
- `text required` — string Текст сообщения в формате plain text от 1 до 1000 символов.

### Ответы

- 200 Сообщение отправлено
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — string Результат обработки запроса.

---

## Создать новый чат

`POST /v1/chat/start`

Operation ID: `ChatAPI_ChatStart`

Доступно для продавцов с подпиской Premium Plus или Premium Pro . Создает новый чат с покупателем по отправлению. Например, чтобы уточнить адрес или модель товара. Для отправлений: FBO — начать чат может только покупатель. FBS и rFBS — вы можете открыть чат в течение 72 часов после оплаты или доставки отправления.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/chat/start" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "posting_number": "47873153-0052-1"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `posting_number required` — string Идентификатор отправления.

### Ответы

- 200 Создан новый чат
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `chat_id` — string Идентификатор чата.

Пример ответа:
```json
{
  "result": {
    "chat_id": "5969c331-2e64-44b7-8a0e-ff9526762c62"
  }
}
```

---

## Отметить сообщения как прочитанные

`POST /v2/chat/read`

Operation ID: `ChatAPI_ChatReadV2`

Доступно для продавцов с подпиской Premium Plus или Premium Pro . Метод для отметки выбранного сообщения и сообщений до него прочитанными. Получите список чатов с покупателем chats.chat.chat_type="Buyer_Seller" в ответе метода /v3/chat/list .

```bash
curl -X POST "https://api-seller.ozon.ru/v2/chat/read" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "chat_id": "99feb3fc-a474-469f-95d5-268b470cc607",
  "from_message_id": 3000000000118032000
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `chat_id required` — string Идентификатор чата.
- `from_message_id` — integer <uint64> Идентификатор сообщения.

### Ответы

- 200 Успешно
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `unread_count` — integer <int64> Количество непрочитанных сообщений в чате.

---

## Данные аналитики

`POST /v1/analytics/data`

Operation ID: `AnalyticsAPI_AnalyticsGetData`

Уĸажите период и метриĸи, ĸоторые нужно посчитать. В ответе будет аналитиĸа, сгруппированная по параметру dimensions . Для продавцов без подписки Premium Plus : доступны данные за последние 3 месяца, есть ограничения по способам группировки данных и метрикам. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/analytics/data" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2020-09-01",
  "date_to": "2021-10-15",
  "metrics": [
    "hits_view_search"
  ],
  "dimension": [
    "sku",
    "day"
  ],
  "filters": [],
  "sort": [
    {
      "key": "hits_view_search",
      "order": "DESC"
    }
  ],
  "limit": 1000,
  "offset": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string Дата, с которой будут данные в отчёте. Если у вас нет Premium-подписки, укажите дату в пределах последних трёх месяцев.
- `date_to required` — string Дата, по которую будут данные в отчёте.
- `dimension required` — Array of strings Items Enum: "unknownDimension" "sku" "spu" "day" "week" "month" "year" "category1" "category2" "brand" "modelID" "descriptionType" Группировка данных в отчёте. Способы группировки, доступные всем продавцам: unknownDimension — неизвестное измерение; sku — идентификатор товара; spu — идентификатор товара — объединённая карточка; day — день; week — неделя; month — месяц. Способы группировки, доступные только продавцам с подпиской Premium Plus : year — год; category1 — категория первого уровня; category2 — категория второго уровня; brand — бренд; modelID — модель; descriptionType — тип товара.
- `filters` — Array of objects Фильтры.
- `limit required` — integer <int64> Количество значений в ответе: максимум — 1000, минимум — 1.
- `metrics required` — Array of strings Укажите до 14 метрик. Если их будет больше, вы получите ошибку с кодом InvalidArgument . Список метриĸ, по ĸоторым будет сформирован отчёт. Метрики, доступные всем продавцам: revenue — заказано на сумму, ordered_units — заказано товаров. Метрики, доступные только продавцам с подпиской Premium Plus : unknown_metric — неизвестная метрика. hits_view_search — показы в поиске и в категории. hits_view_pdp — показы на карточке товара. hits_view — всего показов. hits_tocart_search — в корзину из поиска или категории. hits_tocart_pdp — в корзину из карточки товара. hits_tocart — всего добавлено в корзину. session_view_search — сессии с показом в поиске или в каталоге. Считаются уникальные посетители с просмотром в поиске или каталоге. session_view_pdp — сессии с показом на карточке товара. Считаются уникальные посетители, которые просмотрели карточку товара. session_view — всего сессий. Считаются уникальные посетители. conv_tocart_search — конверсия в корзину из поиска или категории. conv_tocart_pdp — конверсия в корзину из карточки товара. conv_tocart — общая конверсия в корзину. returns — возвращено товаров. cancellations — отменено товаров. delivered_units — доставлено товаров. position_category — позиция в поиске и категории.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `sort` — Array of objects Настройки сортировки отчёта.

### Ответы

- 200 Данные аналитики
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `data` — Array of objects Массив данных.
  - `totals` — Array of numbers <double> Итоговые и средние значения метрик.
- `timestamp` — string Время создания отчёта.

Пример ответа:
```json
{
  "result": {
    "data": [],
    "totals": [
      0
    ]
  },
  "timestamp": "2021-11-25 15:19:21"
}
```

---

## Получить информацию о запросах моих товаров

`POST /v1/analytics/product-queries`

Operation ID: `AnalyticsAPI_AnalyticsProductQueries`

Используйте метод, чтобы получить данные о запросах ваших товаров. Полная аналитика доступна с подпиской Premium , Premium Plus или Premium Pro . Без подписки вы можете посмотреть часть показателей. Метод аналогичен вкладке Товары в поиске → Запросы моего товара в личном кабинете. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/analytics/product-queries" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2019-08-24T14:15:22Z",
  "date_to": "2019-08-24T14:15:22Z",
  "page": 0,
  "page_size": 1000,
  "skus": [
    "string"
  ],
  "sort_by": "BY_SEARCHES",
  "sort_dir": "DESCENDING"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string <date-time> Дата начала формирования аналитики.
- `date_to` — string <date-time> Дата окончания формирования аналитики.
- `page` — integer <int32> >= 0 Индекс страницы, которую возвращает запрос.
- `page_size required` — integer <int32> <= 1000 Количество элементов на странице.
- `skus required` — Array of strings <int64> Список SKU, идентификаторов товара в системе Ozon. По ним вернётся аналитика по запросам. Максимум — 1000 SKU.
- `sort_by` — string Default: "BY_SEARCHES" Enum: "BY_SEARCHES" "BY_VIEWS" "BY_POSITION" "BY_CONVERSION" "BY_GMV" Параметр, по которому товары будут отсортированы. Возможные значения: BY_SEARCHES — по количеству запросов; BY_VIEWS — по количеству просмотров; BY_POSITION — по средней позиции товара; BY_CONVERSION — по значению конверсии; BY_GMV — по объёму продаж по запросам.
- `sort_dir` — string Default: "DESCENDING" Enum: "DESCENDING" "ASCENDING" Направление сортировки: DESCENDING — по убыванию; ASCENDING — по возрастанию.

### Ответы

- 200 Информация о запросах моих товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `analytics_period` — object Период, за который формируется аналитика.
- `items` — Array of objects Список товаров.
- `page_count` — integer <int64> Количество страниц.
- `total` — integer <int64> Общее количество запросов.

Пример ответа:
```json
{
  "analytics_period": {
    "date_from": "string",
    "date_to": "string"
  },
  "items": [
    {
      "category": "string",
      "currency": "string",
      "gmv": 0,
      "name": "string",
      "offer_id": "string",
      "position": 0,
      "sku": 0,
      "unique_search_users": 0,
      "unique_view_users": 0,
      "view_conversion": 0
    }
  ],
  "page_count": 0,
  "total": 0
}
```

---

## Получить детализацию запросов по товару

`POST /v1/analytics/product-queries/details`

Operation ID: `AnalyticsAPI_AnalyticsProductQueriesDetails`

Используйте метод, чтобы получить данные по запросам на конкретный товар. Полная аналитика доступна с подпиской Premium , Premium Plus или Premium Pro . Без подписки вы можете посмотреть часть показателей. Метод аналогичен просмотру данных по товару на вкладке Товары в поиске → Запросы моего товара в личном кабинете. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/analytics/product-queries/details" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "date_from": "2019-08-24T14:15:22Z",
  "date_to": "2019-08-24T14:15:22Z",
  "limit_by_sku": 0,
  "page": 0,
  "page_size": 1000,
  "skus": [
    "string"
  ],
  "sort_by": "BY_SEARCHES",
  "sort_dir": "DESCENDING"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `date_from required` — string <date-time> Дата начала формирования аналитики.
- `date_to` — string <date-time> Дата окончания формирования аналитики.
- `limit_by_sku required` — integer <int32> Лимит числа запросов по одному SKU. Максимум — 15 запросов.
- `page` — integer <int32> Номер страницы, возвращаемой в запросе. Минимум — 0.
- `page_size required` — integer <int32> Количество элементов на странице. Максимум — 100.
- `skus required` — Array of strings <int64> Список SKU, идентификаторов товара в системе Ozon. По ним вернётся аналитика по запросам. Максимум — 1000 SKU.
- `sort_by` — string Default: "BY_SEARCHES" Enum: "BY_SEARCHES" "BY_VIEWS" "BY_POSITION" "BY_CONVERSION" "BY_GMV" Параметр, по которому товары будут отсортированы. Возможные значения: BY_SEARCHES — по количеству запросов; BY_VIEWS — по количеству просмотров; BY_POSITION — по средней позиции товара; BY_CONVERSION — по значению конверсии; BY_GMV — по объёму продаж по запросам. Сортировка по параметрам BY_VIEWS , BY_POSITION и BY_CONVERSION доступна только с подпиской Premium или Premium Plus .
- `sort_dir` — string Default: "DESCENDING" Enum: "DESCENDING" "ASCENDING" Направление сортировки: DESCENDING — по убыванию; ASCENDING — по возрастанию.

### Ответы

- 200 Информация о запросах по конкретному товару
- 400 Неверный параметр

#### Схема ответа (200)

- `analytics_period` — object Период, за который формируется аналитика.
- `page_count` — integer <int64> Количество страниц.
- `queries` — Array of objects Список запросов.
- `total` — integer <int64> Общее количество запросов.

Пример ответа:
```json
{
  "analytics_period": {
    "date_from": "string",
    "date_to": "string"
  },
  "page_count": 0,
  "queries": [
    {
      "currency": "string",
      "gmv": 0,
      "order_count": 0,
      "position": 0,
      "query": "string",
      "query_index": 0,
      "sku": 0,
      "unique_search_users": 0,
      "unique_view_users": 0,
      "view_conversion": 0
    }
  ],
  "total": 0
}
```

---

## Отчёт о реализации товаров за день

`POST /v1/finance/realization/by-day`

Operation ID: `FinanceAPI_GetRealizationByDayReportV1`

Доступно для продавцов с подпиской Premium Plus или Premium Pro . Возвращает данные о суммах реализации из отчёта о реализации товаров за день. Отмены и невыкупы не включаются. Данные доступны не более чем за 32 календарных дня от текущей даты.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/finance/realization/by-day" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "day": 0,
  "month": 0,
  "year": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `day required` — integer <int32> День.
- `month required` — integer <int32> Месяц.
- `year required` — integer <int32> Год.

### Ответы

- 200 Отчёт о реализации за день
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `rows` — Array of objects Таблица отчёта.
  - `commission_ratio` — number <double> Доля комиссии за продажу по категории.
  - `delivery_commission` — object Комиссия за доставку.
  - `item` — object Информация о товаре.
  - `return_commission` — object Комиссия за возврат товара.
  - `rowNumber` — integer <int32> Номер строки в отчёте.
  - `seller_price_per_instance` — number <double> Цена продавца с учётом скидки.

Пример ответа:
```json
{
  "rows": [
    {
      "commission_ratio": 0,
      "delivery_commission": {
        "amount": 0,
        "bonus": 0,
        "commission": 0,
        "compensation": 0,
        "price_per_instance": 0,
        "quantity": 0,
        "standard_fee": 0,
        "bank_coinvestment": 0,
        "stars": 0,
        "pick_up_point_coinvestment": 0,
        "total": 0
      },
      "item": {
        "barcode": "string",
        "name": "string",
        "offer_id": "string",
        "sku": 0
      },
      "return_commission": {
        "amount": 0,
        "bonus": 0,
        "commission": 0,
        "compensation": 0,
        "price_per_instance": 0,
        "quantity": 0,
        "standard_fee": 0,
        "bank_coinvestment": 0,
        "stars": 0,
        "pick_up_point_coinvestment": 0,
        "total": 0
      },
      "rowNumber": 0,
      "seller_price_per_instance": 0
    }
  ]
}
```

---

## Получить список поисковых запросов по тексту

`POST /v1/search-queries/text`

Operation ID: `SearchQueriesAPI_SearchQueriesText`

Доступно для продавцов с подпиской Premium Pro .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/search-queries/text" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "limit": "50",
  "offset": "0",
  "sort_by": "CLIENT_COUNT",
  "sort_dir": "ASC",
  "text": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `limit required` — string <int64> <= 50 Количество значений на странице.
- `offset required` — string <int64> <= 50 Количество элементов, которое будет пропущено в ответе.
- `sort_by` — string Enum: "CLIENT_COUNT" "ADD_TO_CART" "CONVERSION_TO_CART" "AVG_PRICE" Параметр, по которому сортируются поисковые запросы: CLIENT_COUNT — популярность запроса; ADD_TO_CART — добавления в корзину; CONVERSION_TO_CART — конверсия в корзине; AVG_PRICE — средняя цена.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.
- `text required` — string Поиск по тексту.

### Ответы

- 200 Список поисковых запросов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `offset` — string <int64> Количество поисковых запросов на странице.
- `search_queries` — Array of objects Информация о поисковых запросах.
- `total` — string <int64> Общее количество поисковых запросов.

Пример ответа:
```json
{
  "search_queries": [
    {
      "avg_price": 3786.6,
      "conversion_to_cart": 0.163,
      "client_count": 165418,
      "items_views": 140.828,
      "query": "куртка женская демисезон",
      "add_to_cart": 26977,
      "sellers_count": 63.833
    },
    {
      "avg_price": 3786.6,
      "conversion_to_cart": 0.163,
      "client_count": 165418,
      "items_views": 140.828,
      "query": "куртка женская демисезон",
      "add_to_cart": 26977,
      "sellers_count": 63.833
    }
  ],
  "offset": "string",
  "total": "string"
}
```

---

## Получить список популярных поисковых запросов

`POST /v1/search-queries/top`

Operation ID: `SearchQueriesAPI_SearchQueriesTop`

Доступно для продавцов с подпиской Premium Pro .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/search-queries/top" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `limit required` — string <int64> <= 50 Количество значений на странице.
- `offset required` — string <int64> <= 1000 Количество элементов, которое будет пропущено в ответе.

### Ответы

- 200 Список популярных поисковых запросов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `offset` — string <int64> Количество поисковых запросов на странице.
- `search_queries` — Array of objects Информация о поисковых запросах.
- `total` — string <int64> Общее количество поисковых запросов.

Пример ответа:
```json
{
  "search_queries": [
    {
      "avg_price": 3786.6,
      "conversion_to_cart": 0.163,
      "client_count": 165418,
      "items_views": 140.828,
      "query": "куртка женская демисезон",
      "add_to_cart": 26977,
      "sellers_count": 63.833
    }
  ],
  "offset": "1",
  "total": "1"
}
```

---

## Получить подробную информацию о ценах товаров

`POST /v1/product/prices/details`

Operation ID: `ProductPricesDetails`

Доступно для продавцов с подпиской Premium Pro.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/prices/details" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `skus required` — Array of strings <int64> [ 1 .. 1000 ] items Список SKU.

### Ответы

- 200 Информация о ценах товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `prices` — Array of objects Цены товаров.
  - `customer_price` — object Цена товара на сайте.
  - `discount_percent` — number <float> Deprecated Процент скидки за счёт Ozon.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `price` — object Цена товара с учётом акции или продвижения.
  - `price_indexes` — Array of objects Индекс цен.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример ответа:
```json
{
  "prices": [
    {
      "customer_price": {
        "amount": "string",
        "currency": "string"
      },
      "discount_percent": 0,
      "offer_id": "string",
      "price": {
        "amount": "string",
        "currency": "string"
      },
      "price_indexes": [
        {
          "external_index_data": {
            "min_price": {
              "amount": "string",
              "currency": "string"
            },
            "price_index": 0,
            "url": "string"
          },
          "self_index_data": {
            "min_price": {
              "amount": "string",
              "currency": "string"
            },
            "price_index": 0,
            "url": "string"
          }
        }
      ],
      "sku": 0
    }
  ]
}
```

---
