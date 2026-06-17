# Работа с вопросами и ответами

_Тег: `QuestionsandAnswers` · операций: 8_

## Создать ответ на вопрос

`POST /v1/question/answer/create`

Operation ID: `QuestionAnswer_Create`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `question_id required` — string Идентификатор вопроса.
- `sku required` — integer <int64> Идентификатор товара в системе Ozon — SKU.
- `text required` — string Текст ответа объёмом от 2 до 3000 символов.

Пример запроса:

```json
{
  "question_id": "string",
  "sku": 0,
  "text": "string"
}
```

### Ответы

- 200 Идентификатор ответа на вопрос

#### Схема ответа (200)

- `answer_id` — string Идентификатор ответа на вопрос.

Пример ответа:

```json
{
  "answer_id": "0192e7ce-e12c-7a74-afc7-26e877799204"
}
```

---

## Удалить ответ на вопрос

`POST /v1/question/answer/delete`

Operation ID: `QuestionAnswer_Delete`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `answer_id required` — string Идентификатор ответа.
- `sku required` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример запроса:

```json
{
  "answer_id": "0192e7ce-e12c-7a74-afc7-26e877799204",
  "sku": 646399170
}
```

### Ответы

- 200 Ответ удалён

Пример ответа:

```json
{
  "code": 0,
  "details": "string",
  "message": "string"
}
```

---

## Список ответов на вопрос

`POST /v1/question/answer/list`

Operation ID: `QuestionAnswer_List`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `last_id` — Идентификатор последнего значения на странице. Если запрос первый, оставьте поле пустым. Для следующих значений указывайте last_id из ответа предыдущего запроса.
- `question_id required` — string Идентификатор вопроса.
- `sku required` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример запроса:

```json
{
  "last_id": "",
  "question_id": "019228a7-91d8-76af-a73a-e989dfac7ac8",
  "sku": 646399170
}
```

### Ответы

- 200 Список ответов на вопрос

#### Схема ответа (200)

- `answers` — Array of objects Ответы.
- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .

Пример ответа:

```json
{
  "answers": [
    {
      "author_name": "string",
      "id": "string",
      "published_at": "2024-08-14T11:44:35.352Z",
      "question_id": "string",
      "sku": 646399170,
      "status_publication": "",
      "text": "string"
    }
  ],
  "last_id": "string"
}
```

---

## Изменить статус вопросов

`POST /v1/question/change-status`

Operation ID: `Question_ChangeStatus`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `question_ids required` — Array of strings Идентификаторы вопросов.
- `status required` — string Статусы вопросов: NEW — новые, VIEWED — просмотренные, PROCESSED — обработанные.

Пример запроса:

```json
{
  "question_ids": [
    "string"
  ],
  "status": "VIEWED"
}
```

### Ответы

- 200 Статус изменён

Пример ответа:

```json
{
  "code": 0,
  "details": "string",
  "message": "string"
}
```

---

## Количество вопросов по статусам

`POST /v1/question/count`

Operation ID: `Question_Count`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Количество вопросов по статусам

#### Схема ответа (200)

- `all` — integer <int64> Всего вопросов.
- `new` — integer <int64> Новые вопросы.
- `processed` — integer <int64> Обработанные вопросы.
- `unprocessed` — integer <int64> Необработанные вопросы.
- `viewed` — integer <int64> Просмотренные вопросы.

Пример ответа:

```json
{
  "all": 10,
  "new": 3,
  "processed": 4,
  "unprocessed": 1,
  "viewed": 1
}
```

---

## Информация о вопросе

`POST /v1/question/info`

Operation ID: `Question_Info`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `question_id required` — string Идентификатор вопроса.

### Ответы

- 200 Информация о вопросе

#### Схема ответа (200)

- `answers_count` — integer <int64> Количество ответов на вопрос.
- `author_name` — string Автор вопроса.
- `id` — string Идентификатор вопроса.
- `product_url` — string Ссылка на товар.
- `published_at` — timestamp Дата публикации вопроса.
- `question_link` — string Ссылка на вопрос.
- `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
- `status` — enum Статус вопроса: NEW — новый, ALL — все вопросы, VIEWED — просмотренный, PROCESSED — обработанный, UNPROCESSED — необработанный.
- `text` — string Текст вопроса.

Пример ответа:

```json
{
  "answers_count": "0",
  "author_name": "Пользователь OZON",
  "product_url": "https://www.ozon.ru/product/149829950/",
  "sku": 646399170,
  "id": "0192a009-769f-7ee9-b412-893045171a66",
  "text": "Можно выбрать цвет?",
  "question_link": "https://www.ozon.ru/product/149829950/questions/?qid=290125772&utm_campaign=reviews_sc_link&utm_medium=share_button&utm_source=smm",
  "published_at": "2024-10-08T10:09:29.099284Z",
  "status": "VIEWED"
}
```

---

## Список вопросов

`POST /v1/question/list`

Operation ID: `Question_List`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter` — object Фильтр.
- `last_id` — string Идентификатор последнего значения на странице. Оставьте это поле пустым при выполнении первого запроса. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit` — integer <int64> <= 100 Количество значений в ответе.
- `sort_dir` — string Default: "DESC" Enum: "DESC" "ASC" Направление сортировки: DESC — по убыванию; ASC — по возрастанию.

Пример запроса:

```json
{
  "filter": {
    "date_from": "2019-08-24T14:15:22Z",
    "date_to": "2019-08-24T14:15:22Z",
    "status": "ALL"
  },
  "limit": 100,
  "last_id": "",
  "sort_dir": "ASC"
}
```

### Ответы

- 200 Список вопросов

#### Схема ответа (200)

- `questions` — Array of objects [ 0 .. 10 ] items Вопросы.
- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .
- `has_next` — boolean true , если в ответе вернулись не все вопросы.

Пример ответа:

```json
{
  "has_next": true,
  "questions": [
    {
      "answers_count": 1,
      "author_name": "Пользователь OZON",
      "id": "019294ff-6888-7009-89d8-26569e4e450d",
      "sku": 646399170,
      "product_url": "https://www.ozon.ru/product/1649246352/",
      "published_at": "2024-08-14T12:02:01.889Z",
      "question_link": "https://www.ozon.ru/product/1649246352/questions/?qid=290180206&utm_campaign=reviews_sc_link&utm_medium=share_button&utm_source=smm",
      "text": "Новый вопрос о товаре Света",
      "status": "PROCESSED"
    }
  ],
  "last_id": "019228a7-91d8-76af-a73a-e989dfac7ac8"
}
```

---

## Товары с наибольшим количеством вопросов

`POST /v1/question/top-sku`

Operation ID: `Question_TopSku`

Доступно для продавцов с подпиской Premium Plus . Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `limit required` — integer <int64> [ 1 .. 100 ] Количество значений в ответе.

### Ответы

- 200 Идентификаторы товаров

#### Схема ответа (200)

- `sku` — Array of strings <int64> Список идентификаторов товаров в системе Ozon — SKU.

---
