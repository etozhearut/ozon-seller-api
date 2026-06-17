# Рейтинг продавца

Рейтинг продавца Работая с Ozon, продавцы должны соблюдать требования по качеству обслуживания, срокам доставки и общению с клиентами. Система рейтингов отражает качество сервиса продавца, а некоторые показатели видны покупателям — это рейтинг товаров и индекс цен. Подробнее о системе рейтингов в Базе знаний продавца

_Тег: `SellerRating` · операций: 4_

## Получить информацию о текущих рейтингах продавца

`POST /v1/rating/summary`

Operation ID: `RatingAPI_RatingSummaryV1`

Рейтинг продавца по следующим показателям: индекс цен, доставки вовремя, процент отмен, жалобы и другие. Соответствует разделу Рейтинги → Рейтинги продавца в личном кабинете.

### Ответы

- 200 Информация о рейтингах

#### Схема ответа (200)

- `groups` — Array of objects Список с группами рейтингов.
- `localization_index` — Array of objects Данные по индексу локализации. Если за последние 14 дней у вас не было продаж, поля параметра будут пустыми.
- `penalty_score_exceeded` — boolean Признак, что баланс штрафных баллов превышен.
- `premium` — boolean Признак наличия подписки Premium .
- `premium_plus` — boolean Признак наличия подписки Premium Plus .

Пример ответа:

```json
{
  "groups": [
    {
      "group_name": "string",
      "items": [
        {
          "change": {
            "direction": "string",
            "meaning": "string"
          },
          "current_value": 0,
          "name": "string",
          "past_value": 0,
          "rating": "string",
          "rating_direction": "string",
          "status": "string",
          "value_type": "string"
        }
      ]
    }
  ],
  "localization_index": [
    {
      "calculation_date": "2019-08-24T14:15:22Z",
      "localization_percentage": 0
    }
  ],
  "penalty_score_exceeded": true,
  "premium": true,
  "premium_plus": true
}
```

---

## Получить информацию о рейтингах продавца за период

`POST /v1/rating/history`

Operation ID: `RatingAPI_RatingHistoryV1`

Информация о рейтингах за заданный период и с фильтром по нужному рейтингу. Соответствует разделу Рейтинги → Рейтинги продавца в личном кабинете.

### Тело запроса (application/json)

- `date_from required` — string <date-time> Начало периода.
- `date_to required` — string <date-time> Конец периода.
- `ratings required` — Array of strings Фильтр по рейтингу. Рейтинги, по которым нужно получить значение за период: rating_on_time — процент заказов, выполненных вовремя за последние 30 дней. rating_review_avg_score_total — средняя оценка всех товаров. rating_ssl — оценка работы по FBO. Учитывает rating_on_time_supply_delivery , rating_on_time_supply_cancellation и rating_order_accuracy . rating_on_time_supply_delivery — процент поставок, которые вы привезли на склад в выбранный временной интервал за последние 60 дней. rating_order_accuracy — процент поставок без излишков, недостач, пересорта и брака за последние 60 дней. rating_on_time_supply_cancellation — процент заявок на поставку, которые завершились или были отменены без опоздания за последние 60 дней. rating_reaction_time — время в секундах, в течение которого покупатели в среднем ждали ответа на своё первое сообщение в чате за последние 30 дней. rating_average_response_time — время в секундах, в течение которого покупатели в среднем ждали вашего ответа за последние 30 дней. rating_replied_dialogs_ratio — доля диалогов хотя бы с одним вашим ответом в течение 24 часов за последние 30 дней. rating_general_indicator_fbs_rfbs — индекс ошибок FBS и rFBS. rating_price_green — выгодный индекс цен. rating_price_yellow — умеренный индекс цен. rating_price_red — невыгодный индекс цен. rating_price_super — супер-выгодный индекс цен. Если вы хотите получить информацию по начисленным штрафным баллам для рейтингов rating_on_time и rating_review_avg_score_total , передайте значения нужных рейтингов в этом параметре и with_premium_scores=true .
- `with_premium_scores` — boolean Признак, что в ответе нужно вернуть информацию о штрафных баллах в Premium-программе.

Пример запроса:

```json
{
  "date_from": "2019-08-24T14:15:22Z",
  "date_to": "2019-08-24T14:15:22Z",
  "ratings": [
    "string"
  ],
  "with_premium_scores": true
}
```

### Ответы

- 200 Информация о рейтингах

#### Схема ответа (200)

- `premium_scores` — Array of objects Информация о штрафных баллах в Premium-программе.
- `ratings` — Array of objects Информация о рейтингах продавца.

Пример ответа:

```json
{
  "premium_scores": [
    {
      "rating": "string",
      "scores": [
        {
          "date": "2019-08-24T14:15:22Z",
          "rating_value": 0,
          "value": 0
        }
      ]
    }
  ],
  "ratings": [
    {
      "danger_threshold": 0,
      "premium_threshold": 0,
      "rating": "string",
      "values": [
        {
          "date_from": "2019-08-24T14:15:22Z",
          "date_to": "2019-08-24T14:15:22Z",
          "status": {
            "danger": true,
            "premium": true,
            "warning": true
          },
          "value": 0
        }
      ],
      "warning_threshold": 0
    }
  ]
}
```

---

## Получить индекс ошибок FBS и rFBS

`POST /v1/rating/index/fbs/info`

Operation ID: `RatingAPI_GetFBSRatingIndexInfoV1`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Индекс ошибок

#### Схема ответа (200)

- `currency_code` — string Код валюты стоимости обработки ошибок.
- `defects` — Array of objects Индекс ошибок по дням.
- `index` — number <double> Значение индекса ошибок за период.
- `period_from` — string Дата начала расчётного периода в формате YYYY-MM-DD .
- `period_to` — string Дата окончания расчётного периода в формате YYYY-MM-DD .
- `processing_costs_sum` — number <double> Расходы на обработку ошибок за период.

Пример ответа:

```json
{
  "currency_code": "string",
  "defects": [
    {
      "date": "string",
      "index_by_date": 0,
      "processing_costs_sum_by_date": 0
    }
  ],
  "index": 0,
  "period_from": "string",
  "period_to": "string",
  "processing_costs_sum": 0
}
```

---

## Список отправлений, которые повлияли на индекс ошибок FBS и rFBS

`POST /v1/rating/index/fbs/posting/list`

Operation ID: `RatingAPI_ListFBSRatingIndexPostingsV1`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр.
- `limit required` — integer <int64> <= 1000 Количество значений в ответе.

Пример запроса:

```json
{
  "cursor": "string",
  "filter": {
    "date_from": "2019-08-24T14:15:22Z",
    "date_to": "2019-08-24T14:15:22Z",
    "posting_numbers": [
      "string"
    ]
  },
  "limit": 0
}
```

### Ответы

- 200 Список отправлений

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `errors` — Array of objects Отправления, которые повлияли на индекс.
- `has_next` — boolean true , если в ответе вернулись не все отправления.

Пример ответа:

```json
{
  "cursor": "string",
  "errors": [
    {
      "charge_percent": 0,
      "charge_price": 0,
      "charge_price_currency_code": "string",
      "delivery_schema": "string",
      "error_at": "2019-08-24T14:15:22Z",
      "has_grace_status": true,
      "index": 0,
      "posting_error_type": "UNSPECIFIED",
      "posting_number": "string",
      "product_price": 0,
      "product_price_currency_code": "string"
    }
  ],
  "has_next": true
}
```

---
