# Сертификаты брендов

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `BrandAPI` · методов: 1_

## Список сертифицируемых брендов

`POST /v1/brand/company-certification/list`

Operation ID: `BrandAPI_BrandCompanyCertificationList`

Метод для получения списка брендов, для которых требуется предоставить сертификат. Ответ содержит список брендов, товары которых есть в вашем личном кабинете. Список брендов может изменяться, если Ozon получит требование от бренда предоставлять сертификат. Подробнее о работе с брендами в Базе знаний продавца

```bash
curl -X POST "https://api-seller.ozon.ru/v1/brand/company-certification/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `page required` — integer <int32> Номер страницы, возвращаемой в запросе.
- `page_size required` — integer <int32> Количество элементов на странице.

### Ответы

- 200 Список брендов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат запроса.
  - `brand_certification` — Array of objects Информация о сертифицируемых брендах.
  - `total` — integer <int64> Общее количество брендов.

Пример ответа:
```json
{
  "result": {
    "brand_certification": [
      {
        "brand_id": 135476863,
        "brand_name": "Sea of Spa",
        "has_certificate": false
      }
    ],
    "total": 1
  }
}
```

---
