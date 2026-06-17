# Информация по кабинету продавца

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `SellerInfo` · методов: 2_

## Информация о кабинете продавца

`POST /v1/seller/info`

Operation ID: `SellerAPI_SellerInfo`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/seller/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Информация о кабинете продавца

#### Схема ответа (200)

- `company` — object Компания.
- `ratings` — Array of objects Список рейтингов.
- `subscription` — object Подписка.

Пример ответа:
```json
{
  "company": {
    "country": "Россия",
    "currency": "RUB",
    "inn": "7707083893",
    "legal_name": "Общество с ограниченной ответственностью 'Ромашка'",
    "name": "ООО 'Ромашка'",
    "ogrn": "1027700132195",
    "ownership_form": "Частная собственность",
    "tax_system": "ОСНО"
  },
  "ratings": [
    {
      "current_value": {
        "date_from": "2023-01-15T09:00:00Z",
        "date_to": "2023-12-31T23:59:59Z",
        "formatted": "AA+",
        "status": {
          "danger": false,
          "premium": true,
          "warning": false
        },
        "value": 85
      },
      "name": "Финансовая устойчивость",
      "past_value": {
        "date_from": "2022-01-01T00:00:00Z",
        "date_to": "2022-12-31T23:59:59Z",
        "formatted": "A",
        "status": {
          "danger": false,
          "premium": false,
          "warning": true
        },
        "value": 78
      },
      "rating": "Высокий уровень",
      "status": "Активно",
      "value_type": "Количественный"
    }
  ],
  "subscription": {
    "is_premium": true,
    "type": "Профессиональный"
  }
}
```

---

## Информация о подключении Ozon Доставки

`POST /v1/seller/ozon-logistics/info`

Operation ID: `SellerAPI_SellerOzonLogisticsInfo`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/seller/ozon-logistics/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Информация о подключении Ozon Доставки

#### Схема ответа (200)

- `available_schemas` — Array of strings Default: "UNKNOWN" Items Enum: "UNKNOWN" "FBO" "FBS" Тип доступной схемы: UNKNOWN — не определён, FBO , FBS .
- `ozon_logistics_enabled` — boolean true , если Ozon Доставка подключена.

Пример ответа:
```json
{
  "available_schemas": [
    "UNKNOWN"
  ],
  "ozon_logistics_enabled": true
}
```

---
