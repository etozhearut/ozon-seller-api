# Атрибуты и характеристики Ozon

_Тег: `CategoryAPI` · операций: 5_

## Дерево категорий и типов товаров

`POST /v1/description-category/tree`

Operation ID: `DescriptionCategoryAPI_GetTree`

Возвращает категории и типы для товаров в виде дерева. Создание товаров доступно только в категориях последнего уровня, сравните именно их с категориями на своей площадке. Категории не создаются по запросу пользователя. Внимательно выбирайте категорию для товара: для разных категорий применяется разный размер комиссии.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `language` — string Default: "DEFAULT" Enum: "DEFAULT" "RU" "EN" "TR" "ZH_HANS" Язык в ответе: EN — английский, RU — русский, TR — турецкий, ZH_HANS — китайский. По умолчанию используется русский язык.

### Ответы

- 200 Дерево категорий

#### Схема ответа (200)

- `result` — Array of objects Список категорий.
  - `description_category_id` — integer <int64> Идентификатор категории.
  - `category_name` — string Название категории.
  - `children` — Array of objects Дерево подкатегорий.
  - `disabled` — boolean true , если в категории нельзя создавать товары. false , если можно.
  - `type_id` — integer <int64> Идентификатор типа товара.
  - `type_name` — string Название типа товара.

Пример ответа:

```json
{
  "result": [
    {
      "description_category_id": 17027492,
      "category_name": "Канцелярские товары",
      "disabled": false,
      "children": [
        {
          "description_category_id": 17029016,
          "category_name": "Печати и штампы",
          "disabled": false,
          "children": [
            {
              "type_name": "Пистолет-маркиратор",
              "type_id": 970778135,
              "disabled": false,
              "children": []
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Список характеристик категории

`POST /v1/description-category/attribute`

Operation ID: `DescriptionCategoryAPI_GetAttributes`

Получение характеристик для указанных категории и типа товара. Если у dictionary_id значение 0 , у атрибута нет вложенных справочников. Если значение другое, то справочники есть. Запросите их методом /v1/description-category/attribute/values .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `description_category_id required` — integer <int64> Идентификатор категории. Можно получить с помощью метода /v1/description-category/tree .
- `language` — string Default: "DEFAULT" Enum: "DEFAULT" "RU" "EN" "TR" "ZH_HANS" Язык в ответе: EN — английский, RU — русский, TR — турецкий, ZH_HANS — китайский. По умолчанию используется русский язык.
- `type_id required` — integer <int64> Идентификатор типа товара. Можно получить с помощью метода /v1/description-category/tree .

Пример запроса:

```json
{
  "description_category_id": 200000933,
  "language": "DEFAULT",
  "type_id": 93080
}
```

### Ответы

- 200 Характеристики категории

#### Схема ответа (200)

- `result` — Array of objects Результат запроса.
  - `category_dependent` — boolean Признак, что значения словарного атрибута зависят от категории: true — у атрибута разные значения для каждой категории. false — у атрибута одинаковые значения для всех категорий.
  - `description` — string Описание характеристики.
  - `dictionary_id` — integer <int64> Идентификатор справочника.
  - `group_id` — integer <int64> Идентификатор группы характеристик.
  - `group_name` — string Название группы характеристик.
  - `id` — integer <int64> Идентификатор характеристики.
  - `is_aspect` — boolean Признак аспектного атрибута. Аспектный атрибут — характеристика, по которой отличаются товары одной модели. Например, у одежды и обуви одной модели могут быть разные расцветки и размеры. То есть цвет и размер — это аспектные атрибуты. Значения поля: true — атрибут аспектный и его нельзя изменить после поставки товара на склад или продажи со своего склада. false — атрибут не аспектный, можно изменить в любое время.
  - `is_collection` — boolean true , если характеристика — набор значений. false , если характеристика — одно значение.
  - `is_required` — boolean Признак обязательной характеристики: true — обязательная характеристика, false — характеристику можно не указывать.
  - `name` — string Название.
  - `type` — string Тип характеристики.
  - `attribute_complex_id` — integer <int64> Идентификатор комплексного атрибута.
  - `max_value_count` — integer <int64> Максимальное количество значений для атрибута.
  - `complex_is_collection` — boolean Признак, что комплексная характеристика — набор значений: true , если комплексная характеристика — набор значений, false , если комплексная характеристика — одно значение.

Пример ответа:

```json
{
  "result": [
    {
      "id": 31,
      "attribute_complex_id": 32,
      "name": "Бренд в одежде и обуви",
      "description": "Укажите наименование бренда, под которым произведён товар. Если товар не имеет бренда, используйте значение \"Нет бренда\"",
      "type": "string",
      "is_collection": false,
      "is_required": true,
      "is_aspect": false,
      "max_value_count": 30,
      "group_name": "",
      "group_id": 33,
      "dictionary_id": 28732849,
      "category_dependent": true,
      "complex_is_collection": true
    }
  ]
}
```

---

## Справочник значений характеристики

`POST /v1/description-category/attribute/values`

Operation ID: `DescriptionCategoryAPI_GetAttributeValues`

Возвращает справочник значений характеристики. Узнать, есть ли вложенный справочник, можно через метод /v1/description-category/attribute .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `attribute_id required` — integer <int64> Идентификатор характеристики. Можно получить с помощью метода /v1/description-category/attribute .
- `description_category_id required` — integer <int64> Идентификатор категории. Можно получить с помощью метода /v1/description-category/tree .
- `language` — string Default: "DEFAULT" Enum: "DEFAULT" "RU" "EN" "TR" "ZH_HANS" Язык в ответе: EN — английский, RU — русский, TR — турецкий, ZH_HANS — китайский. По умолчанию используется русский язык.
- `last_value_id` — integer <int64> Идентификатор справочника, с которого нужно начать ответ. Если last_value_id — 10, то в ответе будут справочники, начиная с одиннадцатого.
- `limit required` — integer <int64> Количество значений в ответе: максимум — 2000, минимум — 1.
- `type_id required` — integer <int64> Идентификатор типа товара. Можно получить с помощью метода /v1/description-category/tree .

Пример запроса:

```json
{
  "attribute_id": 85,
  "description_category_id": 17054869,
  "language": "DEFAULT",
  "last_value_id": 100,
  "limit": 100,
  "type_id": 97311
}
```

### Ответы

- 200 Справочник характеристик

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернулась только часть значений характеристики: true — сделайте повторный запрос с новым параметром last_value_id для получения остальных значений; false — ответ содержит все значения характеристики.
- `result` — Array of objects Значения характеристики.
  - `id` — integer <int64> Идентификатор значения характеристики.
  - `info` — string Дополнительное описание.
  - `picture` — string Ссылка на изображение.
  - `value` — string Значение характеристики товара.

Пример ответа:

```json
{
  "result": [
    {
      "id": 5055881,
      "value": "Sunshine",
      "info": "Здоровье и красота",
      "picture": "https://cdn1.ozone.ru/s3/multimedia-i/6010930878.jpg"
    },
    {
      "id": 5056737,
      "value": "Essence",
      "info": "Красота и здоровье",
      "picture": "https://cdn1.ozone.ru/s3/multimedia-v/6088253599.jpg"
    }
  ],
  "has_next": true
}
```

---

## Поиск по справочным значениям характеристики

`POST /v1/description-category/attribute/values/search`

Operation ID: `DescriptionCategoryAPI_SearchAttributeValues`

Возвращает справочные значения характеристики по заданному значению value в запросе. Узнать, есть ли вложенный справочник, можно через метод /v1/description-category/attribute .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `attribute_id required` — integer <int64> Идентификатор характеристики. Можно получить с помощью метода /v1/description-category/attribute .
- `description_category_id required` — integer <int64> Идентификатор категории. Можно получить с помощью метода /v1/description-category/tree .
- `limit required` — integer <int64> Количество значений в ответе: максимум — 100, минимум — 1.
- `type_id required` — integer <int64> Идентификатор типа товара. Можно получить с помощью метода /v1/description-category/tree .
- `value required` — string Значение, по которому система будет искать справочные значения. Минимум — 2 символа.

Пример запроса:

```json
{
  "attribute_id": 85,
  "description_category_id": 17054869,
  "limit": 100,
  "type_id": 97311,
  "value": "Name"
}
```

### Ответы

- 200 Справочные значения характеристики.

#### Схема ответа (200)

- `result` — Array of objects Значения характеристики.
  - `id` — integer <int64> Идентификатор значения характеристики.
  - `info` — string Дополнительная информация.
  - `picture` — string Ссылка на изображение.
  - `value` — string Значение характеристики товара.

Пример ответа:

```json
{
  "result": [
    {
      "id": 1,
      "info": "Черный-товар",
      "picture": "https://example.com/images/color_black.jpg",
      "value": "Чёрный"
    }
  ]
}
```

---

## Получить зоны размещения товаров по SKU перед поставкой

`POST /v1/product/placement-zone/info`

Operation ID: `ProductAPI_GetProductPlacementZoneInfo`

Вы можете отправить не больше 10 запросов в секунду. Подробнее о зонах размещения в Базе знаний продавца

### Тело запроса (application/json)

- `skus required` — Array of strings <int64> [ 1 .. 150 ] items Список идентификаторов товаров в системе Ozon — SKU.

### Ответы

- 200 Информация о зонах размещения

#### Схема ответа (200)

- `products_placement` — Array of objects Список товаров с их зонами размещения.
  - `placement_zone` — string Default: "UNSPECIFIED" Enum: "UNSPECIFIED" "CLOSED_ZONE" "DANGEROUS_GOODS" "PRODUCTS" "SORT" "NON_SORT" "OVERSIZE" "JEWELRY" "UNRESOLVED" Зона размещения товара: UNSPECIFIED — не указана; CLOSED_ZONE — закрытая зона; DANGEROUS_GOODS — товар 2–4 класса опасности; PRODUCTS — продукты; SORT — сортируемый товар; NON_SORT — несортируемый товар; OVERSIZE — крупногабаритный товар; JEWELRY — ювелирные изделия; UNRESOLVED — неизвестная зона.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример ответа:

```json
{
  "products_placement": [
    {
      "placement_zone": "UNSPECIFIED",
      "sku": 0
    }
  ]
}
```

---
