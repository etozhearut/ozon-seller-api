# Чаты с покупателями

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `ChatAPI` · методов: 3_

Чаты с покупателями Больше методов в разделе Premium-методы .

## Отправить файл

`POST /v1/chat/send/file`

Operation ID: `ChatAPI_ChatSendFile`

Отправляет файл в существующий чат по его идентификатору. Отправить файл в чат с покупателем могут только продавцы с подпиской Premium Plus или Premium Pro . Получите список чатов с покупателем chats.chat.chat_type="Buyer_Seller" в ответе метода /v3/chat/list . …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/chat/send/file" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "base64_content": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNk+A8AAQUBAScY42YAAAAASUVORK5CYII=",
  "chat_id": "99feb3fc-a474-469f-95d5-268b470cc607",
  "name": "tempor"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `base64_content required` — string Файл в виде строки base64.
- `chat_id required` — string Идентификатор чата.
- `name required` — string Название файла с расширением.

### Ответы

- 200 Файл отправлен
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — string Результат обработки запроса.

---

## Список чатов

`POST /v3/chat/list`

Operation ID: `ChatAPI_ChatListV3`

Возвращает информацию о чатах по указанным фильтрам.

```bash
curl -X POST "https://api-seller.ozon.ru/v3/chat/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "chat_status": "OPENED",
    "unread_only": true
  },
  "limit": 1,
  "cursor": "304000402034"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтр по чатам.
- `limit required` — integer <int64> Количество значений в ответе. Значение по умолчанию — 30. Максимальное значение — 100.
- `cursor` — string Указатель для выборки следующих данных.

### Ответы

- 200 Список чатов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `chats` — Array of objects Данные чатов.
- `total_unread_count` — integer <int64> Общее количество непрочитанных сообщений.
- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean Признак, что в ответе вернулись не все чаты: true — сделайте повторный запрос с новым параметром cursor для получения остальных чатов; false — ответ содержит все чаты для фильтра, который был задан в запросе.

Пример ответа:
```json
{
  "chats": [
    {
      "chat": {
        "created_at": "2022-07-22T08:07:19.581Z",
        "chat_id": "5e767w03-b400-4y1b-a841-75319ca8a5c8",
        "chat_status": "OPENED",
        "chat_type": "SELLER_SUPPORT"
      },
      "first_unread_message_id": "3000000000118021931",
      "last_message_id": "30000000001280042740",
      "unread_count": 1
    }
  ],
  "total_unread_count": 5,
  "cursor": "30000002342123123",
  "has_next": "true"
}
```

---

## История чата

`POST /v3/chat/history`

Operation ID: `ChatAPI_ChatHistoryV3`

Возвращает историю сообщений чата. По умолчанию от самого нового сообщения к старым. Получите список чатов с покупателем chats.chat.chat_type="Buyer_Seller" в ответе метода /v3/chat/list .

```bash
curl -X POST "https://api-seller.ozon.ru/v3/chat/history" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "chat_id": "18b8e1f9-4ae7-461c-84ea-8e1f54d1a45e",
  "direction": "Forward",
  "filter": {
    "message_ids": [
      "3000000300211559667"
    ]
  },
  "from_message_id": 3000000000118032000,
  "limit": 1
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `chat_id required` — string Идентификатор чата.
- `direction` — string Направление сортировки сообщений: Forward — от старых к новым. Backward — от новых к старым. Значение по умолчанию — Backward . Количество сообщений можно установить в параметре limit .
- `filter` — object Фильтр по сообщениям.
- `from_message_id` — integer <uint64> Идентификатор сообщения, с которого нужно начать вывод истории чата. По умолчанию — последнее видимое сообщение. Параметр from_message_id обязательный, если direction = Forward .
- `limit` — integer <int64> Количество сообщений в ответе. По умолчанию — 50. Максимальное значение — 1000.

### Ответы

- 200 История чата
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернули не все сообщения.
- `messages` — Array of objects Массив сообщений, отсортированный в соответствии с параметром direction из тела запроса.

Пример ответа:
```json
{
  "has_next": true,
  "messages": [
    {
      "context": {
        "order_number": "123456789",
        "sku": "987654321"
      },
      "created_at": "2019-08-24T14:15:22Z",
      "data": [
        "Здравствуйте, у меня вопрос по вашему товару \"Стекло защитное для смартфонов\", артикул 11223. Подойдет ли он на данную [ модель ](https://www.ozon.ru/product/smartfon-samsung-galaxy-a03s-4-64-gb-chernyy) телефона?"
      ],
      "is_image": true,
      "is_read": true,
      "message_id": "3000000000817031942",
      "moderate_image_status": "SUCCESS",
      "user": {
        "id": "115568",
        "type": "Сustomer"
      }
    }
  ]
}
```

---
