# Работа с грузоместами FBS

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `CarriageAPI` · методов: 13_

Работа с грузоместами FBS Методы в этом разделе работают только для доверительной приёмки грузовых мест. Подробнее о доверительной приёмке грузового места в Базе знаний продавца

## Создать грузоместо

`POST /v1/carriage/container/create`

Operation ID: `CarriageContainerCreate`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/create" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cargo_type": "string",
  "containers_count": 1,
  "sort_type": "string",
  "warehouse_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cargo_type required` — string Тип грузоместа: box — коробка; pallet — палета.
- `containers_count required` — integer <int32> [ 1 .. 100 ] Количество грузомест.
- `sort_type required` — string Тип сортировки: sort — сортируемый; non-sort — несортируемый.
- `warehouse_id required` — integer <int64> Идентификатор склада.

### Ответы

- 200 Грузоместо создано

#### Схема ответа (200)

- `container_ids` — Array of strings <int64> Идентификаторы грузомест.

---

## Наполнить грузоместо отправлениями

`POST /v1/carriage/container/fill`

Operation ID: `CarriageContainerFill`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/fill" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "container_id": 0,
  "posting_numbers": [
    "string"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `container_id required` — integer <int64> Идентификатор грузоместа.
- `posting_numbers required` — Array of strings <= 300 Номера отправлений.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `error_postings` — Array of objects Ошибки по отправлениям.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_postings": [
    {
      "error_message": "string",
      "posting_number": "string"
    }
  ],
  "task_id": 0
}
```

---

## Подтвердить состав грузоместа

`POST /v1/carriage/container/approve`

Operation ID: `CarriageContainerApprove`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/approve" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `container_ids required` — Array of strings <int64> Идентификаторы грузомест.

### Ответы

- 200 Состав подтверждён

#### Схема ответа (200)

- `error_containers` — Array of objects Ошибки по грузоместам.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_containers": [
    {
      "container_id": 0,
      "error_message": "string"
    }
  ],
  "task_id": 0
}
```

---

## Разместить коробки на палете

`POST /v1/carriage/container/place-into`

Operation ID: `CarriageContainerPlaceInto`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/place-into" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "child_container_ids": [
    "string"
  ],
  "parent_container_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `child_container_ids required` — Array of strings <int64> Идентификаторы грузомест.
- `parent_container_id required` — integer <int64> Идентификатор родительского грузоместа — палеты.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `error_containers` — Array of objects Ошибки по грузоместам.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_containers": [
    {
      "container_id": 0,
      "error_message": "string"
    }
  ],
  "task_id": 0
}
```

---

## Убрать отправления из грузоместа

`POST /v1/carriage/container/remove-postings`

Operation ID: `CarriageContainerRemovePostings`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/remove-postings" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "container_id": 0,
  "posting_numbers": [
    "string"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `container_id required` — integer <int64> Идентификатор грузоместа.
- `posting_numbers required` — Array of strings Номера отправлений.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `error_postings` — Array of objects Ошибки по отправлениям.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_postings": [
    {
      "error_message": "string",
      "posting_number": "string"
    }
  ],
  "task_id": 0
}
```

---

## Убрать коробки с палеты

`POST /v1/carriage/container/remove-from`

Operation ID: `CarriageContainerRemoveFrom`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/remove-from" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "child_container_ids": [
    "string"
  ],
  "parent_container_id": 0
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `child_container_ids required` — Array of strings <int64> Идентификаторы грузомест.
- `parent_container_id required` — integer <int64> Идентификатор родительского грузоместа — палеты.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `error_containers` — Array of objects Ошибки по грузоместам.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_containers": [
    {
      "container_id": 0,
      "error_message": "string"
    }
  ],
  "task_id": 0
}
```

---

## Отменить грузоместо

`POST /v1/carriage/container/cancel`

Operation ID: `CarriageContainerCancel`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/cancel" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `container_ids required` — Array of strings <int64> <= 100 Идентификаторы грузомест.

### Ответы

- 200 Задание создано

#### Схема ответа (200)

- `error_containers` — Array of objects Ошибки по грузоместам.
- `task_id` — integer <int64> Идентификатор задания.

Пример ответа:
```json
{
  "error_containers": [
    {
      "container_id": 0,
      "error_message": "string"
    }
  ],
  "task_id": 0
}
```

---

## Получить список грузомест

`POST /v1/carriage/container/list`

Operation ID: `CarriageContainerList`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "cursor": "string",
  "filter": {
    "cargo_type": "string",
    "created_from": "2019-08-24T14:15:22Z",
    "created_to": "2019-08-24T14:15:22Z",
    "sort_type": "string",
    "statuses": [
      "string"
    ],
    "warehouse_id": 0
  },
  "limit": 1,
  "sort_dir": "ASC"
}'
```

### Тело запроса

- `cursor` — string <= 1000 Указатель для выборки следующих данных.
- `filter` — object Фильтр.
- `limit` — integer <int64> [ 1 .. 300 ] Количество значений в ответе. Значение по умолчанию — 100.
- `sort_dir` — string Default: "ASC" Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию; DESC — по убыванию.

### Ответы

- 200 Список грузомест

#### Схема ответа (200)

- `containers` — Array of objects Информация о грузоместах.
- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.

Пример ответа:
```json
{
  "containers": [
    {
      "available_actions": [
        "string"
      ],
      "cargo_type": "string",
      "container_id": 0,
      "container_number": 0,
      "count_of_postings": 0,
      "created_at": "2019-08-24T14:15:22Z",
      "related_containers": [],
      "sort_type": "string",
      "status": "string",
      "warehouse_date": "string",
      "warehouse_id": 0,
      "warehouse_name": "string",
      "weight": 0
    }
  ],
  "cursor": "string"
}
```

---

## Получить информацию о грузоместах

`POST /v1/carriage/container/get`

Operation ID: `CarriageContainerGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `container_id required` — integer <int64> Идентификатор грузоместа.

### Ответы

- 200 Информация о грузоместах

#### Схема ответа (200)

- `available_actions` — Array of strings Действия с грузоместом: approve — подтвердить состав; get_label_container — получить этикетку; delete — удалить; place_box_into_pallet — добавить коробку на палету; remove_box_from_pallet — убрать коробку с палеты; place_posting_into_container — перенести отправление в другое грузоместо; remove_posting_from_container — убрать отправление из грузоместа; get_documents — получить транспортную накладную.
- `cargo_type` — string Тип грузоместа.
- `container_id` — integer <int64> Идентификатор грузоместа.
- `container_number` — integer <int32> Порядковый номер грузоместа.
- `count_of_postings` — integer <int32> Количество отправлений в грузоместе.
- `created_at` — string <date-time> Дата создания грузоместа в UTC.
- `parent_container_id` — integer <int64> Идентификатор родительского грузоместа.
- `postings` — Array of objects Список отправлений.
- `related_container_ids` — Array of strings <int64> Идентификаторы дочерних грузомест.
- `sort_type` — string Тип сортировки грузоместа: sort — сортируемый; non-sort — несортируемый.
- `status` — string Статус грузоместа.
- `warehouse_date` — string Дата создания грузоместа в часовом поясе склада.
- `warehouse_id` — integer <int64> Идентификатор склада продавца.
- `warehouse_name` — string Название склада.
- `weight` — number <float> Суммарный вес отправлений в грузоместе, кг.

Пример ответа:
```json
{
  "available_actions": [
    "string"
  ],
  "cargo_type": "string",
  "container_id": 0,
  "container_number": 0,
  "count_of_postings": 0,
  "created_at": "2019-08-24T14:15:22Z",
  "parent_container_id": 0,
  "postings": [
    {
      "available_actions": [
        "string"
      ],
      "in_process_at": "2019-08-24T14:15:22Z",
      "posting_number": "string",
      "products": [
        {
          "sku": 0,
          "name": "string",
          "offer_id": "string",
          "quantity": 0,
          "picture_url": "string",
          "product_color": "string",
          "product_size_manufacturer": "string",
          "product_size_russian": "string"
        }
      ],
      "sort_type": "string",
      "weight": 0
    }
  ],
  "related_container_ids": [
    "string"
  ],
  "sort_type": "string",
  "status": "string",
  "warehouse_date": "string",
  "warehouse_id": 0,
  "warehouse_name": "string",
  "weight": 0
}
```

---

## Получить статус грузомест FBS

`POST /v1/carriage/container/status/get`

Operation ID: `CarriageContainerStatusGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/status/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `container_ids required` — Array of strings <int64> [ 1 .. 1000 ] Идентификаторы грузомест.

### Ответы

- 200 Статус грузомест

#### Схема ответа (200)

- `containers` — Array of objects Список грузомест.
  - `container_id` — integer <int64> Идентификатор грузоместа.
  - `status` — string Статус грузоместа.

Пример ответа:
```json
{
  "containers": [
    {
      "container_id": 0,
      "status": "string"
    }
  ]
}
```

---

## Получить статус задачи грузового места

`POST /v1/carriage/container/task/info`

Operation ID: `CarriageContainerTaskInfo`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/task/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `task_id` — integer <int64> Идентификатор задачи. Получите его в ответе метода /v1/carriage/container/fill или /v1/carriage/container/approve .

### Ответы

- 200 Статус задачи

#### Схема ответа (200)

- `error_message` — string Текст ошибки.
- `status` — string Статус выполнения задачи: pending — в ожидании; in_progress — в процессе; completed — выполнено; failed — ошибка при выполнении;

Пример ответа:
```json
{
  "error_message": "string",
  "status": "string"
}
```

---

## Получить документы по грузоместам — ТрН и лист отгрузки

`POST /v1/carriage/container/document/get`

Operation ID: `CarriageContainerDocumentGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/document/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `container_ids required` — Array of strings <int64> Идентификаторы грузомест.

### Ответы

- 200 Список документов

#### Схема ответа (200)

- `content_type` — string Тип файла: application или pdf.
- `file_content` — string <byte> Содержание файла в бинарном виде.
- `file_name` — string Название файла.

Пример ответа:
```json
{
  "content_type": "string",
  "file_content": "string",
  "file_name": "string"
}
```

---

## Получить этикетку по грузоместам

`POST /v1/carriage/container/label/get`

Operation ID: `CarriageContainerLabelGet`

Вы можете оставить обратную связь по этому методу в комментариях к обсуждению в сообществе разработчиков Ozon for dev .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/carriage/container/label/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Тело запроса

- `container_ids` — Array of strings <int64> <= 300 Идентификаторы грузомест.

### Ответы

- 200 Список этикеток

#### Схема ответа (200)

- `content` — object Информация о сформированной этикетке.
- `error_containers` — Array of objects Ошибки грузомест, по которым не удалось сформировать этикетку.

Пример ответа:
```json
{
  "content": {
    "content_type": "string",
    "file_content": "string",
    "file_name": "string"
  },
  "error_containers": [
    {
      "container_id": 0,
      "error_message": "string"
    }
  ]
}
```

---
