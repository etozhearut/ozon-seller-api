# Работа с отзывами

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `ReviewAPI` · методов: 12_

## Оставить комментарий на отзыв

`POST /v1/review/comment/create`

Operation ID: `ReviewAPI_CommentCreate`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/comment/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "mark_review_as_processed": true,
  "parent_comment_id": "string",
  "review_id": "string",
  "text": "string"
}'
```

### Тело запроса

- `mark_review_as_processed` — boolean Обновление статуса у отзыва: true — статус изменится на Processed . false — статус не изменится.
- `parent_comment_id` — string Идентификатор родительского комментария, на который вы отвечаете.
- `review_id required` — string Идентификатор отзыва.
- `text required` — string Текст комментария.

### Ответы

- 200 Комментарий создан

#### Схема ответа (200)

- `comment_id` — string Идентификатор комментария.

---

## Удалить комментарий на отзыв

`POST /v2/review/comment/delete`

Operation ID: `ReviewCommentDeleteV2`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/review/comment/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "comment_id": "string",
  "sku": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `comment_id required` — string Идентификатор комментария.
- `sku required` — integer <int64> Идентификатор товара в системе Ozon — SKU.

### Ответы

- 200 Комментарий удалён
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

## Удалить комментарий на отзыв Deprecated

`POST /v1/review/comment/delete`

Operation ID: `ReviewAPI_CommentDelete`

Метод устаревает. Переключитесь на /v2/review/comment/delete . Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/comment/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `comment_id required` — string Идентификатор комментария.

### Ответы

- 200 Комментарий удалён

---

## Получить список комментариев на отзыв

`POST /v1/review/comment/list`

Operation ID: `ReviewAPI_CommentList`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev. Метод возвращает информацию по комментариям на отзывы, которые прошли модерацию.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/comment/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "review_id": "0000-0000",
  "sort_dir": "ASC",
  "filters": {
    "sku": 0,
    "published_from": "2026-03-10T14:08:00.257Z",
    "published_to": "2026-03-10T14:08:00.257Z"
  },
  "limit": 100,
  "offset": 123
}'
```

### Тело запроса

- `filter` — object Фильтры для поиска отзывов.
- `limit required` — integer <int32> Ограничение значений в ответе. Минимум — 20. Максимум — 100.
- `offset` — integer <int32> Количество элементов, которое будет пропущено с начала списка в ответе. Например, если offset = 10 , то ответ начнётся с 11-го найденного элемента.
- `review_id required` — string Идентификатор отзыва.
- `sort_dir` — string Default: "ASC" Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию, DESC — по убыванию.

### Ответы

- 200 Информация о комментариях на отзыв

#### Схема ответа (200)

- `comments` — Array of objects Информация о комментарии.
- `offset` — integer <int32> Количество элементов в выдаче.

Пример ответа:
```json
{
  "comments": [
    {
      "id": "1234-5678",
      "text": "comment text",
      "parent_comment_id": "3333-3333",
      "is_owner": true,
      "is_official": false,
      "is_published": true,
      "is_rejected": true,
      "likes_amount": 0,
      "dislikes_amount": 0,
      "deviation_reason": "string",
      "published_at": "2021-09-03T07:04:53.220Z"
    }
  ],
  "offset": 124
}
```

---

## Изменить статус отзывов

`POST /v2/review/change-status`

Operation ID: `ReviewChangeStatusV2`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/review/change-status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "status": "NEW",
  "review_ids": [
    "01920411-f6af-7e58-a8a4-63c5cfc007d0"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `review_ids` — Array of strings [ 1 .. 100 ] items Список идентификаторов отзывов.
- `status` — string Enum: "NEW" "VIEWED" "PROCESSED" Новый статус отзыва: NEW — новый; VIEWED — просмотренный; PROCESSED — обработанный.

### Ответы

- 200 Статус изменён
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

## Изменить статус отзывов Deprecated

`POST /v1/review/change-status`

Operation ID: `ReviewAPI_ReviewChangeStatus`

Метод устаревает. Переключитесь на /v2/review/change-status . Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/change-status" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "review_ids": [
    "string"
  ],
  "status": "string"
}'
```

### Тело запроса

- `review_ids required` — Array of strings Массив с идентификаторами отзывов от 1 до 100.
- `status required` — string Статус отзыва: PROCESSED — обработанный, UNPROCESSED — необработанный.

### Ответы

- 200 Статус изменён

---

## Получить количество отзывов по статусам

`POST /v2/review/count`

Operation ID: `ReviewCountV2`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/review/count" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Количество отзывов по статусам
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `new` — integer <int32> Количество новых отзывов.
- `processed` — integer <int32> Количество обработанных отзывов.
- `total` — integer <int32> Количество всех отзывов.
- `viewed` — integer <int32> Количество просмотренных отзывов.

Пример ответа:
```json
{
  "total": 3,
  "processed": 1,
  "new": 1,
  "viewed": 1
}
```

---

## Количество отзывов по статусам Deprecated

`POST /v1/review/count`

Operation ID: `ReviewAPI_ReviewCount`

Метод устаревает. Переключитесь на /v2/review/count . Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/count" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Ответы

- 200 Количество обработанных и необработанных отзывов

#### Схема ответа (200)

- `processed` — integer <int32> Количество обработанных отзывов.
- `total` — integer <int32> Количество всех отзывов.
- `unprocessed` — integer <int32> Количество необработанных отзывов.

Пример ответа:
```json
{
  "processed": 0,
  "total": 0,
  "unprocessed": 0
}
```

---

## Получить информацию по отзыву

`POST /v2/review/info`

Operation ID: `ReviewInfoV2`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/review/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `review_id required` — string Идентификатор отзыва.

### Ответы

- 200 Информация об отзыве
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `comments_amount` — integer <int32> Количество комментариев к отзыву.
- `dislikes_amount` — integer <int32> Количество дизлайков на отзыве.
- `id` — string Идентификатор отзыва.
- `is_rating_participant` — boolean true , если отзыв участвует в подсчёте рейтинга.
- `likes_amount` — integer <int32> Количество лайков на отзыве.
- `order_status` — string Enum: "DELIVERED" "CANCELLED" Статус заказа, на который покупатель оставил отзыв: DELIVERED — доставлен; CANCELLED — отменён.
- `photos` — Array of objects Информация об изображениях.
- `photos_amount` — integer <int32> Количество изображений у отзыва.
- `published_at` — string <date-time> Дата публикации отзыва.
- `rating` — integer <int32> Оценка отзыва.
- `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
- `status` — string Enum: "NEW" "VIEWED" "PROCESSED" Статус отзыва: NEW — новый; VIEWED — просмотренный; PROCESSED — обработанный.
- `text` — string Текст отзыва.
- `videos` — Array of objects Информация о видео.
- `videos_amount` — integer <int32> Количество видео у отзыва.

Пример ответа:
```json
{
  "comments_amount": 0,
  "dislikes_amount": 0,
  "id": "string",
  "is_rating_participant": true,
  "likes_amount": 0,
  "order_status": "DELIVERED",
  "photos": [
    {
      "height": 0,
      "url": "string",
      "width": 0
    }
  ],
  "photos_amount": 0,
  "published_at": "2019-08-24T14:15:22Z",
  "rating": 0,
  "sku": 0,
  "status": "NEW",
  "text": "string",
  "videos": [
    {
      "height": 0,
      "preview_url": "string",
      "short_video_preview_url": "string",
      "url": "string",
      "width": 0
    }
  ],
  "videos_amount": 0
}
```

---

## Получить информацию об отзыве Deprecated

`POST /v1/review/info`

Operation ID: `ReviewAPI_ReviewInfo`

Метод устаревает. Переключитесь на /v2/review/info . Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `review_id required` — string Идентификатор отзыва.

### Ответы

- 200 Информация об отзыве

#### Схема ответа (200)

- `comments_amount` — integer <int32> Количество комментариев к отзыву.
- `dislikes_amount` — integer <int32> Количество дизлайков на отзыве.
- `id` — string Идентификатор отзыва.
- `is_rating_participant` — boolean true , если отзыв участвует в подсчёте рейтинга.
- `likes_amount` — integer <int32> Количество лайков на отзыве.
- `order_status` — string Статус заказа, на который покупатель оставил отзыв: DELIVERED — доставлен, CANCELLED — отменён.
- `photos` — Array of objects Информация об изображении.
- `photos_amount` — integer <int32> Количество изображений у отзыва.
- `published_at` — string <date-time> Дата публикации отзыва.
- `rating` — integer <int32> Оценка отзыва.
- `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
- `status` — string Статус отзыва: UNPROCESSED — не обработан, PROCESSED — обработан.
- `text` — string Текст отзыва.
- `videos` — Array of objects Информация о видео.
- `videos_amount` — integer <int32> Количество видео у отзыва.

Пример ответа:
```json
{
  "comments_amount": 0,
  "dislikes_amount": 0,
  "id": "string",
  "is_rating_participant": true,
  "likes_amount": 0,
  "order_status": "string",
  "photos": [
    {
      "height": 0,
      "url": "string",
      "width": 0
    }
  ],
  "photos_amount": 0,
  "published_at": "2019-08-24T14:15:22Z",
  "rating": 0,
  "sku": 0,
  "status": "string",
  "text": "string",
  "videos": [
    {
      "height": 0,
      "preview_url": "string",
      "short_video_preview_url": "string",
      "url": "string",
      "width": 0
    }
  ],
  "videos_amount": 0
}
```

---

## Получить список отзывов

`POST /v2/review/list`

Operation ID: `ReviewListV2`

Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/review/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filters": {
    "sku": [
      0
    ],
    "order_status": "NEW",
    "status": "DELIVERED",
    "published_from": "2026-03-10T14:08:00.257Z",
    "published_to": "2026-03-10T14:08:00.257Z"
  },
  "last_id": "string",
  "limit": 0,
  "sort_dir": "ASC"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filters` — object Фильтры для поиска отзывов.
- `last_id` — string Идентификатор последнего отзыва в ответе.
- `limit required` — integer <int32> [ 20 .. 100 ] Количество отзывов в ответе.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.

### Ответы

- 200 Список отзывов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `has_next` — boolean true , если в ответе вернули не все отзывы.
- `last_id` — string Идентификатор последнего отзыва на странице.
- `reviews` — Array of objects Список отзывов.

Пример ответа:
```json
{
  "reviews": [
    {
      "id": "017c0d1c-66d3-b838-3d29-cf9b95a6ac48",
      "sku": "148591503",
      "text": "Не лучший товар, встречал и получше за те же деньги.",
      "published_at": "2024-10-10T07:23:55.970Z",
      "rating": 2,
      "status": "UNPROCESSED",
      "comments_amount": 0,
      "photos_amount": 0,
      "videos_amount": 0,
      "order_status": "DELIVERED",
      "is_rating_participant": true
    }
  ],
  "has_next": true,
  "last_id": "017c0d53-a7c8-81ef-53de-7d32fcbd7421"
}
```

---

## Получить список отзывов Deprecated

`POST /v1/review/list`

Operation ID: `ReviewAPI_ReviewList`

Метод устаревает. Переключитесь на /v2/review/list . Доступно для продавцов с подпиской Управление отзывами или Premium Pro . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/review/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "last_id": "",
  "limit": 100,
  "sort_dir": "ASC",
  "status": "ALL"
}'
```

### Тело запроса

- `last_id` — string Идентификатор последнего отзыва на странице.
- `limit required` — integer <int32> Количество отзывов в ответе. Минимум — 20, максимум — 100.
- `sort_dir` — string Направление сортировки: ASC — по возрастанию, DESC — по убыванию.
- `status` — string Статусы отзывов: ALL — все, UNPROCESSED — необработанные, PROCESSED — обработанные.

### Ответы

- 200 Список отзывов

#### Схема ответа (200)

- `has_next` — boolean true , если в ответе вернули не все отзывы.
- `last_id` — string Идентификатор последнего отзыва на странице.
- `reviews` — Array of objects Информация об отзыве.

Пример ответа:
```json
{
  "has_next": true,
  "last_id": "string",
  "reviews": [
    {
      "comments_amount": 0,
      "id": "string",
      "is_rating_participant": true,
      "order_status": "string",
      "photos_amount": 0,
      "published_at": "2019-08-24T14:15:22Z",
      "rating": 0,
      "sku": 0,
      "status": "string",
      "text": "string",
      "videos_amount": 0
    }
  ]
}
```

---
