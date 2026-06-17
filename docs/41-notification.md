# Работа с пуш-уведомлениями

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `Notification` · методов: 7_

## Подключить URL-адрес для уведомлений

`POST /v1/notification/set`

Operation ID: `SetNotification`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/set" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "types": [
    "TYPE_NEW_MESSAGE"
  ],
  "url": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `types required` — Array of strings Items Enum: "TYPE_NEW_MESSAGE" "TYPE_UPDATE_MESSAGE" "TYPE_MESSAGE_READ" "TYPE_CHAT_CLOSED" "TYPE_NEW_POSTING" "TYPE_POSTING_CANCELLED" "TYPE_STATE_CHANGED" "TYPE_DELIVERY_DATE_CHANGED" "TYPE_CUTOFF_DATE_CHANGED" "TYPE_CREATE_ITEM" "TYPE_UPDATE_ITEM" "TYPE_CREATE_OR_UPDATE_ITEM" "TYPE_STOCKS_CHANGED" Типы уведомлений: TYPE_NEW_MESSAGE — новое сообщение в чате; TYPE_UPDATE_MESSAGE — изменение сообщения в чате; TYPE_MESSAGE_READ — ваше сообщение прочитано покупателем или поддержкой; TYPE_CHAT_CLOSED — чат закрыт; TYPE_NEW_POSTING — новое отправление; TYPE_POSTING_CANCELLED — отмена отправления; TYPE_STATE_CHANGED — изменение статуса отправления; TYPE_DELIVERY_DATE_CHANGED — изменение даты доставки отправления; TYPE_CUTOFF_DATE_CHANGED — изменение даты отгрузки отправления; TYPE_CREATE_ITEM — создание товара или ошибка при его создании; TYPE_UPDATE_ITEM — обновление товара или ошибка при обновлении; TYPE_CREATE_OR_UPDATE_ITEM — создание и обновление товара или ошибка в процессе; TYPE_STOCKS_CHANGED — изменение остатков на складах продавца.
- `url required` — string URL-адрес.

### Ответы

- 200 URL-адрес подключён
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

## Изменить URL-адрес для уведомлений

`POST /v1/notification/update`

Operation ID: `UpdateNotification`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "id": 0,
  "types": [
    "TYPE_NEW_MESSAGE"
  ],
  "url": "string"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `id required` — integer <int64> Идентификатор URL-адреса.
- `types` — Array of strings Items Enum: "TYPE_NEW_MESSAGE" "TYPE_UPDATE_MESSAGE" "TYPE_MESSAGE_READ" "TYPE_CHAT_CLOSED" "TYPE_NEW_POSTING" "TYPE_POSTING_CANCELLED" "TYPE_STATE_CHANGED" "TYPE_DELIVERY_DATE_CHANGED" "TYPE_CUTOFF_DATE_CHANGED" "TYPE_CREATE_ITEM" "TYPE_UPDATE_ITEM" "TYPE_CREATE_OR_UPDATE_ITEM" "TYPE_STOCKS_CHANGED" Типы уведомлений: TYPE_NEW_MESSAGE — новое сообщение в чате; TYPE_UPDATE_MESSAGE — изменение сообщения в чате; TYPE_MESSAGE_READ — ваше сообщение прочитано покупателем или поддержкой; TYPE_CHAT_CLOSED — чат закрыт; TYPE_NEW_POSTING — новое отправление; TYPE_POSTING_CANCELLED — отмена отправления; TYPE_STATE_CHANGED — изменение статуса отправления; TYPE_DELIVERY_DATE_CHANGED — изменение даты доставки отправления; TYPE_CUTOFF_DATE_CHANGED — изменение даты отгрузки отправления; TYPE_CREATE_ITEM — создание товара или ошибка при его создании; TYPE_UPDATE_ITEM — обновление товара или ошибка при обновлении; TYPE_CREATE_OR_UPDATE_ITEM — создание и обновление товара или ошибка в процессе; TYPE_STOCKS_CHANGED — изменение остатков на складах продавца.
- `url` — string Новый URL-адрес.

### Ответы

- 200 URL-адрес изменён
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

## Удалить URL-адрес для уведомлений

`POST /v1/notification/delete`

Operation ID: `DeleteNotification`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `id required` — integer <int64> Идентификатор URL-адреса.

### Ответы

- 200 URL-адрес удалён
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

## Проверить URL-адрес для уведомлений

`POST /v1/notification/check`

Operation ID: `CheckNotification`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/check" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `url required` — string URL-адрес.

### Ответы

- 200 Результат проверки
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `errors` — Array of objects Ошибки, возникшие при проверке.
- `is_active required` — boolean true , если URL-адрес активен.

Пример ответа:
```json
{
  "errors": [
    {
      "description": "string",
      "type": "REQUEST_ERROR"
    }
  ],
  "is_active": true
}
```

---

## Включить или выключить уведомления на URL-адрес

`POST /v1/notification/enable`

Operation ID: `EnableNotification`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/enable" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `enabled required` — boolean Передайте: true — чтобы включить уведомления; false — чтобы выключить уведомления.
- `id required` — integer <int64> Идентификатор URL-адреса.

### Ответы

- 200 Уведомления включены или выключены
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

## Получить информацию по подключённым URL-адресам

`POST /v1/notification/list`

Operation ID: `NotificationList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Подключённые URL-адреса
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `urls required` — Array of objects Подключённые URL-адреса.
  - `created_at required` — string <date-time> Дата подключения URL-адреса.
  - `enable required` — boolean true , если URL-адрес включён.
  - `id required` — integer <int64> Идентификатор URL-адреса.
  - `types required` — Array of objects Типы уведомлений.
  - `url required` — string URL-адрес.

Пример ответа:
```json
{
  "urls": [
    {
      "created_at": "2019-08-24T14:15:22Z",
      "enable": true,
      "id": 0,
      "types": [
        {
          "description": "string",
          "type": "TYPE_NEW_MESSAGE"
        }
      ],
      "url": "string"
    }
  ]
}
```

---

## Получить типы пуш-уведомлений

`POST /v1/notification/push-type/list`

Operation ID: `GetNotificationPushTypeList`

Вы можете оставить обратную связь о работе метода в комментариях в сообществе разработчиков Ozon for dev.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/notification/push-type/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Типы пуш-уведомлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `types required` — Array of objects Типы пуш-уведомлений.
  - `description required` — string Описание.
  - `seller_endpoint` — object URL-адрес, на который приходит тип уведомлений.
  - `type required` — string Enum: "TYPE_NEW_MESSAGE" "TYPE_UPDATE_MESSAGE" "TYPE_MESSAGE_READ" "TYPE_CHAT_CLOSED" "TYPE_NEW_POSTING" "TYPE_POSTING_CANCELLED" "TYPE_STATE_CHANGED" "TYPE_DELIVERY_DATE_CHANGED" "TYPE_CUTOFF_DATE_CHANGED" "TYPE_CREATE_ITEM" "TYPE_UPDATE_ITEM" "TYPE_CREATE_OR_UPDATE_ITEM" "TYPE_STOCKS_CHANGED" Тип уведомления: TYPE_NEW_MESSAGE — новое сообщение в чате; TYPE_UPDATE_MESSAGE — изменение сообщения в чате; TYPE_MESSAGE_READ — ваше сообщение прочитано покупателем или поддержкой; TYPE_CHAT_CLOSED — чат закрыт; TYPE_NEW_POSTING — новое отправление; TYPE_POSTING_CANCELLED — отмена отправления; TYPE_STATE_CHANGED — изменение статуса отправления; TYPE_DELIVERY_DATE_CHANGED — изменение даты доставки отправления; TYPE_CUTOFF_DATE_CHANGED — изменение даты отгрузки отправления; TYPE_CREATE_ITEM — создание товара или ошибка при его создании; TYPE_UPDATE_ITEM — обновление товара или ошибка при обновлении; TYPE_CREATE_OR_UPDATE_ITEM — создание и обновление товара или ошибка в процессе; TYPE_STOCKS_CHANGED — изменение остатков на складах продавца.

Пример ответа:
```json
{
  "types": [
    {
      "description": "string",
      "seller_endpoint": {
        "id": 0,
        "url": "string"
      },
      "type": "TYPE_NEW_MESSAGE"
    }
  ]
}
```

---
