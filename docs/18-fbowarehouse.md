# Склады FBO

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `FBOWarehouse` · методов: 1_

## Получить список складов Ozon

`POST /v1/warehouse/ozon/list`

Operation ID: `WarehouseOZONList`

Возвращает список складов Ozon, которые работают по схемам FBO и FBO Fresh, и возвратных складов.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/warehouse/ozon/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "warehouse_types": [
    "FULL_FILLMENT"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `warehouse_types` — Array of strings Items Enum: "FULL_FILLMENT" "FULL_FILLMENT_RETURNS" "FULL_FILLMENT_DEFECT" "EXPRESS_DARK_STORE" "CROSS_DOCK" "SORTING_CENTER" "PHARMACY" "DISTRIBUTION_CENTER" "ORDERS_RECEIVING_POINT" "OUTSOURCE_FF" "B2B" "EXTERNAL_FF" Тип склада Ozon: FULL_FILLMENT — фулфилмент; FULL_FILLMENT_RETURNS — склад возвратов; FULL_FILLMENT_DEFECT — склад брака; EXPRESS_DARK_STORE — фреш; CROSS_DOCK — кросс-док; SORTING_CENTER — сортировочный центр; PHARMACY — склад аптеки; DISTRIBUTION_CENTER — распределительный центр; ORDERS_RECEIVING_POINT — пункты приёма заказов; OUTSOURCE_FF — аутсорс-склады; B2B — B2B-склад; EXTERNAL_FF — склады партнёров.

### Ответы

- 200 Список складов Ozon

#### Схема ответа (200)

- `warehouses` — Array of objects Список складов.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `name` — string Название склада.
  - `short_name` — string Короткое название склада.
  - `address` — string Адрес склада.
  - `timezone` — string Часовой пояс склада.
  - `is_active` — boolean true , если склад активный.
  - `warehouse_type` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "FULL_FILLMENT" "FULL_FILLMENT_RETURNS" "FULL_FILLMENT_DEFECT" "EXPRESS_DARK_STORE" "CROSS_DOCK" "SORTING_CENTER" "PHARMACY" "DISTRIBUTION_CENTER" "ORDERS_RECEIVING_POINT" "OUTSOURCE_FF" "B2B" "EXTERNAL_FF" Тип склада: UNSPECIFIED — не указан; FULL_FILLMENT — фулфилмент; FULL_FILLMENT_RETURNS — склад возвратов; FULL_FILLMENT_DEFECT — склад брака; EXPRESS_DARK_STORE — фреш; CROSS_DOCK — кросс-док; SORTING_CENTER — сортировочный центр; PHARMACY — склад аптеки; DISTRIBUTION_CENTER — распределительный центр; ORDERS_RECEIVING_POINT — пункты приёма заказов; OUTSOURCE_FF — аутсорс-склады; B2B — B2B-склад; EXTERNAL_FF — склады партнёров.
  - `country_iso_numeric` — integer <int32> Код страны в формате ISO 3166-1 numeric.
  - `is_cross_dock` — boolean true , если тип склада — кросс-док.
  - `is_distribution_center` — boolean true , если тип склада — распределительный центр.
  - `is_edo` — boolean true , если склад работает с электронным документооборотом.
  - `is_express` — boolean true , если тип склада — фреш.
  - `is_for_supply` — boolean true , если склад доступен для создания поставки.

Пример ответа:
```json
{
  "warehouses": [
    {
      "warehouse_id": 0,
      "name": "string",
      "short_name": "string",
      "address": "string",
      "timezone": "string",
      "is_active": true,
      "warehouse_type": "UNSPECIFIED",
      "country_iso_numeric": 0,
      "is_cross_dock": true,
      "is_distribution_center": true,
      "is_edo": true,
      "is_express": true,
      "is_for_supply": true
    }
  ]
}
```

---
