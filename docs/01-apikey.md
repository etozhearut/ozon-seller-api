# Информация по API-ключу

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `APIkey` · методов: 1_

## Получить список ролей и методов по API-ключу

`POST /v1/roles`

Operation ID: `AccessAPI_RolesByToken`

Метод для получения информации и ролях и методах, привязанных к API-ключу.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/roles" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Список ролей и методов

#### Схема ответа (200)

- `expires_at` — string <date-time> Дата истечения срока действия ключа.
- `roles` — Array of objects Информация о доступных ролях и методах.

Пример ответа:
```json
{
  "expires_at": "2026-02-18T09:54:23.296Z",
  "roles": [
    {
      "name": "Admin",
      "methods": [
        "/v1/actions"
      ]
    },
    {
      "name": "Posting FBS",
      "methods": [
        "/v1/posting"
      ]
    }
  ]
}
```

---
