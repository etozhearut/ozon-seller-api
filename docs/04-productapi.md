# Загрузка и обновление товаров

Базовый URL: `https://api-seller.ozon.ru`. Заголовки авторизации: `Client-Id`, `Api-Key`.

_Тег: `ProductAPI` · методов: 19_

## Создать или обновить товар

`POST /v3/product/import`

Operation ID: `ProductAPI_ImportProductsV3`

Метод для создания товаров и обновления информации о них. В сутки можно создать или обновить определённое количество товаров. Чтобы узнать лимит, используйте /v4/product/info/limit . Если количество загрузок и обновлений товаров превысит лимит, появится ошибка item_limit_exceeded . …

```bash
curl -X POST "https://api-seller.ozon.ru/v3/product/import" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "items": [
    {
      "attributes": [
        {
          "complex_id": 0,
          "id": 5076,
          "values": [
            {
              "dictionary_value_id": 971082156,
              "value": "Стойка для акустической системы"
            }
          ]
        },
        {
          "complex_id": 0,
          "id": 9048,
          "values": [
            {
              "value": "Комплект защитных плёнок для X3 NFC. Темный хлопок"
            }
          ]
        },
        {
          "complex_id": 0,
          "id": 8229,
          "values": [
            {
              "dictionary_value_id": 95911,
              "value": "Комплект защитных плёнок для X3 NFC. Темный хлопок"
            }
          ]
        },
        {
          "complex_id": 0,
          "id": 85,
          "values": [
            {
              "dictionary_value_id": 5060050,
              "value": "Samsung"
            }
          ]
        },
        {
          "complex_id": 0,
          "id": 10096,
          "values": [
            {
              "dictionary_value_id": 61576,
              "value": "серый"
            }
          ]
        }
      ],
      "barcode": "112772873170",
      "description_category_id": 17028922,
      "new_description_category_id": 0,
      "color_image": "",
      "complex_attributes": [],
      "currency_code": "RUB",
      "depth": 10,
      "dimension_unit": "mm",
      "height": 250,
      "images": [],
      "images360": [],
      "name": "Комплект защитных плёнок для X3 NFC. Темный хлопок",
      "offer_id": "143210608",
      "old_price": "1100",
      "pdf_list": [],
      "price": "1000",
      "primary_image": "",
      "promotions": [
        {
          "operation": "UNKNOWN",
          "type": "REVIEWS_PROMO"
        }
      ],
      "type_id": 91565,
      "vat": "0.1",
      "weight": 100,
      "weight_unit": "g",
      "width": 150
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `items` — Array of objects Массив данных.

### Ответы

- 200 Создан новый товар / Информация о товаре обновлена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 429 Слишком много запросов
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результаты запроса.
  - `task_id` — integer <int64> Номер задания на загрузку товаров. Проверьте статус создания или обновления товара методом /v1/product/import/info .

Пример ответа:
```json
{
  "result": {
    "task_id": 172549793
  }
}
```

---

## Узнать статус добавления или обновления товара

`POST /v1/product/import/info`

Operation ID: `ProductAPI_GetImportProductsInfo`

Позволяет получить статус создания или обновления карточки товара. Если вы обновили изображение или видео по ссылке, но ссылка не изменилась, для изображения или видео в параметре result.items.status вернётся skipped .

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/import/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `task_id required` — integer <int64> Код задачи на импорт товаров. Можно получить с помощью метода /v3/product/import .

### Ответы

- 200 Статус добавления или обновления товара
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object
  - `items` — Array of objects Информация о товарах.
  - `total` — integer <int32> Идентификатор товара в системе продавца — артикул.

Пример ответа:
```json
{
  "result": {
    "items": [
      {
        "offer_id": "143210608",
        "product_id": 137285792,
        "status": "imported",
        "errors": []
      }
    ],
    "total": 1
  }
}
```

---

## Создать товар по SKU

`POST /v1/product/import-by-sku`

Operation ID: `ProductAPI_ImportProductsBySKU`

Метод создаёт копию карточки товара с указанным SKU. Метод копирует только карточку товара другого продавца, скопировать свой товар не получится. Создать копию не получится, если продавец запретил копирование своих карточек. Обновить товар по SKU нельзя. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/import-by-sku" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "items": [
    {
      "sku": 298789742,
      "name": "Кружка",
      "offer_id": "91132",
      "currency_code": "RUB",
      "old_price": "2590",
      "price": "2300",
      "vat": "0.1"
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `items` — Array of objects <= 1000 items Информация о товарах.

### Ответы

- 200 Товар создан
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 429 Слишком много запросов
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object
  - `task_id` — integer <int64> Код задачи на импорт товаров.
  - `unmatched_sku_list` — Array of integers <int64> Список идентификаторов товаров в системе Ozon — product_id .

Пример ответа:
```json
{
  "result": {
    "task_id": 176594213,
    "unmatched_sku_list": []
  }
}
```

---

## Обновить характеристики товара

`POST /v1/product/attributes/update`

Operation ID: `ProductAPI_ProductUpdateAttributes`

Метод позволяет добавлять характеристики и изменять их значения. Изменятся только характеристики, которые вы указали в запросе. Удалить уже заполненные характеристики не получится. Для полного обновления характеристик используйте /v3/product/import . …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/attributes/update" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "items": [
    {
      "offer_id": "PROD-2023-001",
      "attributes": [
        {
          "complex_id": 1,
          "id": 1,
          "values": [
            {
              "dictionary_value_id": 1001,
              "value": "Синий"
            }
          ]
        },
        {
          "complex_id": 2,
          "id": 2,
          "values": [
            {
              "dictionary_value_id": 2001,
              "value": "42"
            }
          ]
        },
        {
          "complex_id": 3,
          "id": 3,
          "values": [
            {
              "dictionary_value_id": 3001,
              "value": "Хлопок"
            },
            {
              "dictionary_value_id": 3002,
              "value": "Полиэстер"
            }
          ]
        }
      ]
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `items` — Array of objects <= 100 items Товары и характеристики, которые нужно обновить.

### Ответы

- 200 Создано задание на обновление товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 429 Слишком много запросов
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `task_id` — integer <int64> Номер задания на обновление товаров. Чтобы проверить статус обновления, передайте полученное значение в метод /v1/product/import/info .

---

## Загрузить или обновить изображения товара

`POST /v1/product/pictures/import`

Operation ID: `ProductAPI_ProductImportPictures`

Метод для загрузки или обновления изображений товара. При каждом вызове метода передавайте все изображения, которые должны быть на карточке товара. Например, если вы вызвали метод и загрузили 10 изображений, а затем вызвали метод второй раз и загрузили ещё одно, то все 10 предыдущих сотрутся. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/pictures/import" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "color_image": "https://example.com/cloud-storage/color/marketing-color-red.jpg",
  "images": [
    "https://example.com/cloud-storage/images/main-image-front.jpg",
    "https://example.com/cloud-storage/images/secondary-image-side.jpg",
    "https://example.com/cloud-storage/images/secondary-image-back.jpg",
    "https://example.com/cloud-storage/images/secondary-image-detail1.jpg",
    "https://example.com/cloud-storage/images/secondary-image-detail2.jpg"
  ],
  "images360": [
    "https://example.com/cloud-storage/images360/360-view-001.jpg",
    "https://example.com/cloud-storage/images360/360-view-002.jpg",
    "https://example.com/cloud-storage/images360/360-view-003.jpg",
    "https://example.com/cloud-storage/images360/360-view-004.jpg",
    "https://example.com/cloud-storage/images360/360-view-005.jpg"
  ],
  "product_id": 123456789
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `color_image` — string Маркетинговый цвет.
- `images` — Array of strings Массив ссылок на изображения. До 30 штук. Изображения в массиве расположены в порядке их расположения на сайте. Первое изображение в массиве будет главным.
- `images360` — Array of strings Массив изображений 360. До 70 штук. Формат: адрес ссылки на изображение в общедоступном облачном хранилище. Формат изображения по ссылке — JPG.
- `product_id required` — integer <int64> Идентификатор товара в системе Ozon — product_id .

### Ответы

- 200 Предварительный результат работы метода
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 429 Слишком много запросов
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `pictures` — Array of objects

Пример ответа:
```json
{
  "result": {
    "pictures": [
      {
        "is_360": true,
        "is_color": true,
        "is_primary": true,
        "product_id": 123456789,
        "state": "uploaded",
        "url": "https://example.com/cloud-storage/images360/360-view-001.jpg"
      }
    ]
  }
}
```

---

## Список товаров

`POST /v3/product/list`

Operation ID: `ProductAPI_GetProductList`

Метод для получения списка всех товаров. Если вы используете фильтр по идентификатору offer_id или product_id , остальные параметры заполнять не обязательно. Вернутся все товары с указанными идентификаторами, независимо от видимости. …

```bash
curl -X POST "https://api-seller.ozon.ru/v3/product/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "offer_id": [
      "136748"
    ],
    "product_id": [
      "223681945"
    ],
    "visibility": "ALL"
  },
  "last_id": "",
  "limit": 100
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтр по товарам.
  - `offer_id` — Array of strings Фильтр по параметру offer_id . Вы можете передавать список значений.
  - `product_id` — Array of strings <int64> Фильтр по параметру product_id . Вы можете передавать список значений.
  - `visibility` — string Default: "ALL" Enum: "ALL" "VISIBLE" "INVISIBLE" "EMPTY_STOCK" "NOT_MODERATED" "MODERATED" "DISABLED" "STATE_FAILED" "READY_TO_SUPPLY" "VALIDATION_STATE_PENDING" "VALIDATION_STATE_FAIL" "VALIDATION_STATE_SUCCESS" "TO_SUPPLY" "IN_SALE" "REMOVED_FROM_SALE" "OVERPRICED" "CRITICALLY_OVERPRICED" "EMPTY_BARCODE" "BARCODE_EXISTS" "QUARANTINE" "ARCHIVED" "OVERPRICED_WITH_STOCK" "PARTIAL_APPROVED" "AUTO_ARCHIVED" "MANUAL_ARCHIVED" "SEASONAL_AUTO_ARCHIVED" "VISIBLE_WITH_FBO_STOCK" Фильтр по видимости товара: ALL — все товары, кроме архивных; VISIBLE — товары, которые видны покупателям; INVISIBLE — товары, которые не видны покупателям; EMPTY_STOCK — товары, у которых не указано наличие; NOT_MODERATED — товары, которые не прошли модерацию; MODERATED — товары, которые прошли модерацию; DISABLED — товары, которые видны покупателям, но недоступны к покупке; STATE_FAILED — товары, создание которых завершилось ошибкой; READY_TO_SUPPLY — товары, готовые к поставке; VALIDATION_STATE_PENDING — товары, которые проходят проверку валидатором на премодерации; VALIDATION_STATE_FAIL — товары, которые не прошли проверку валидатором на премодерации; VALIDATION_STATE_SUCCESS — товары, которые прошли проверку валидатором на премодерации; TO_SUPPLY — товары, готовые к продаже; IN_SALE — товары в продаже; REMOVED_FROM_SALE — товары, скрытые от покупателей; OVERPRICED — товары с завышенной ценой; CRITICALLY_OVERPRICED — товары со слишком завышенной ценой; EMPTY_BARCODE — товары без штрихкода; BARCODE_EXISTS — товары со штрихкодом; QUARANTINE — товары на карантине после изменения цены более чем на 50%; ARCHIVED — товары в архиве; OVERPRICED_WITH_STOCK — товары в продаже со стоимостью выше, чем у конкурентов; PARTIAL_APPROVED — товары в продаже с пустым или неполным описанием; AUTO_ARCHIVED — товары, которые система перенесла в архив автоматически; MANUAL_ARCHIVED — товары, которые продавец перенёс в архив вручную; SEASONAL_AUTO_ARCHIVED — сезонные товары, которые система перенесла в архив автоматически; VISIBLE_WITH_FBO_STOCK — товары с остатками на FBO, которые видят покупатели.
- `last_id` — string Идентификатор последнего значения на странице. При первом запросе оставьте это поле пустым. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit` — integer <int64> Количество значений на странице. Минимум — 1, максимум — 1000.

### Ответы

- 200 Список товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат.
  - `items` — Array of objects Список товаров.
  - `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, передайте полученное значение в следующем запросе в параметре last_id .
  - `total` — integer <int32> Всего товаров.

Пример ответа:
```json
{
  "result": {
    "items": [
      {
        "product_id": 3397917680,
        "offer_id": "2026-01-13 16:56:03 PDF",
        "has_fbo_stocks": false,
        "has_fbs_stocks": false,
        "archived": false,
        "is_discounted": false,
        "quants": []
      }
    ],
    "total": 1,
    "last_id": "WzMzOTc5MTc2ODAsMzM5NzkxNzY4MF0="
  }
}
```

---

## Получить контент-рейтинг товаров по SKU

`POST /v1/product/rating-by-sku`

Operation ID: `ProductAPI_GetProductRatingBySku`

Метод для получения контент-рейтинга товаров, а также рекомендаций по его увеличению. Подробнее о контент-рейтинге

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/rating-by-sku" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `skus required` — Array of strings <int64> Идентификаторы товаров в системе Ozon — SKU, для которых нужно вернуть контент-рейтинг.

### Ответы

- 200 Контент-рейтинг товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `products` — Array of objects Контент-рейтинг товаров.
  - `sku` — integer <int64> Идентификатор товара на Ozon.
  - `rating` — number <float> Контент-рейтинг товара: от 0 до 100.
  - `groups` — Array of objects Группы характеристик, из которых складывается контент-рейтинг.

Пример ответа:
```json
{
  "products": [
    {
      "sku": 179737222,
      "rating": 42.5,
      "groups": [
        {
          "key": "media",
          "name": "Медиа",
          "rating": 70,
          "weight": 25,
          "conditions": [
            {
              "key": "media_images_2",
              "description": "Добавлено 2 изображения",
              "fulfilled": true,
              "cost": 50
            },
            {
              "key": "media_images_3",
              "description": "Добавлено 3 изображения и более",
              "fulfilled": true,
              "cost": 20
            },
            {
              "key": "media_image_3d",
              "description": "Добавлено 3D-изображение",
              "fulfilled": false,
              "cost": 15
            },
            {
              "key": "media_video",
              "description": "Добавлено видео",
              "fulfilled": false,
              "cost": 15
            }
          ],
          "improve_attributes": [
            {
              "id": 4074,
              "name": "Код ролика на YouTube"
            },
            {
              "id": 4080,
              "name": "3D-изображение"
            }
          ],
          "improve_at_least": 2
        },
        {
          "key": "important_attributes",
          "name": "Важные атрибуты",
          "rating": 50,
          "weight": 30,
          "conditions": [
            {
              "key": "important_2",
              "description": "Заполнено 2 атрибута и более",
              "fulfilled": true,
              "cost": 50
            },
            {
              "key": "important_50_percent",
              "description": "Заполнено более 50% атрибутов",
              "fulfilled": false,
              "cost": 25
            },
            {
              "key": "important_70_percent",
              "description": "Заполнено более 70% атрибутов",
              "fulfilled": false,
              "cost": 25
            }
          ],
          "improve_attributes": [
            {
              "id": 4385,
              "name": "Гарантийный срок"
            },
            {
              "id": 4389,
              "name": "Страна-изготовитель"
            },
            {
              "id": 8513,
              "name": "Количество в упаковке, шт"
            },
            {
              "id": 8590,
              "name": "Макс. диагональ, дюймы"
            },
            {
              "id": 8591,
              "name": "Мин. диагональ, дюймы"
            },
            {
              "id": 9336,
              "name": "Модель браслета/умных часов"
            },
            {
              "id": 11046,
              "name": "Покрытие"
            },
            {
              "id": 11047,
              "name": "Прозрачность покрытия"
            },
            {
              "id": 11048,
              "name": "Дополнительные свойства покрытия"
            },
            {
              "id": 11049,
              "name": "Вид стекла"
            },
            {
              "id": 11603,
              "name": "Размер циферблата"
            }
          ],
          "improve_at_least": 6
        },
        {
          "key": "other_attributes",
          "name": "Остальные атрибуты",
          "rating": 0,
          "weight": 25,
          "conditions": [
            {
              "key": "other_2",
              "description": "Заполнено 2 атрибута и более",
              "fulfilled": false,
              "cost": 50
            },
            {
              "key": "other_50_percent",
              "description": "Заполнено более 50% атрибутов",
              "fulfilled": false,
              "cost": 50
            }
          ],
          "improve_attributes": [
            {
              "id": 4382,
              "name": "Размеры, мм"
            }
          ],
          "improve_at_least": 1
        },
        {
          "key": "text",
          "name": "Текстовое описание",
          "rating": 50,
          "weight": 20,
          "conditions": [
            {
              "key": "text_annotation_100_chars",
              "description": "Аннотация более 100 знаков",
              "fulfilled": true,
              "cost": 25
            },
            {
              "key": "text_annotation_500_chars",
              "description": "Аннотация более 500 знаков",
              "fulfilled": true,
              "cost": 25
            },
            {
              "key": "text_rich",
              "description": "Заполнен Rich-контент",
              "fulfilled": false,
              "cost": 100
            }
          ],
          "improve_attributes": [
            {
              "id": 11254,
              "name": "Rich-контент JSON"
            }
          ],
          "improve_at_least": 1
        }
      ]
    }
  ]
}
```

---

## Получить информацию о товарах по идентификаторам

`POST /v3/product/info/list`

Operation ID: `ProductAPI_GetProductInfoList`

Метод для получения информации о товарах по их идентификаторам. В теле запроса должен быть массив однотипных идентификаторов, в ответе будет массив items . В одном запросе вы можете передать не больше 1000 товаров по параметрам offer_id , product_id и sku в сумме.

```bash
curl -X POST "https://api-seller.ozon.ru/v3/product/info/list" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "offer_id": [
    "2026113PDF"
  ],
  "product_id": [
    "3339791176800"
  ],
  "sku": [
    "33373353831"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `offer_id` — Array of strings Идентификатор товара в системе продавца — артикул.
- `product_id` — Array of strings <int64> Идентификатор товара в системе Ozon — product_id .
- `sku` — Array of strings <int64> Идентификатор товара в системе Ozon — SKU.

### Ответы

- 200 Список товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `items` — Array of objects Массив данных.
  - `availabilities` — Array of objects Информация о доступности товара.
  - `barcodes` — Array of strings Все штрихкоды товара.
  - `color_image` — Array of strings Изображение цвета товара.
  - `commissions` — Array of objects Информация о комиссиях.
  - `created_at` — string <date-time> Дата и время создания товара.
  - `currency_code` — string Валюта.
  - `description_category_id` — integer <int64> Идентификатор категории. Используйте его с методами /v1/description-category/attribute и /v1/description-category/attribute/values .
  - `discounted_fbo_stocks` — integer <int32> Остатки уценённого товара на складе Ozon.
  - `errors` — Array of objects Информация об ошибках при создании или валидации товара.
  - `has_discounted_fbo_item` — boolean Признак, что у товара есть уценённые аналоги на складе Ozon.
  - `id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `images` — Array of strings Массив ссылок на изображения. Изображения в массиве расположены в порядке их расположения на сайте. Если параметр primary_image не указан, первое изображение в массиве главное для товара.
  - `images360` — Array of strings Массив изображений 360.
  - `is_archived` — boolean true , если товар архивирован вручную.
  - `is_autoarchived` — boolean true , если товар архивирован автоматически.
  - `is_discounted` — boolean Признак, является ли товар уценённым: Если товар создавался продавцом как уценённый — true . Если товар не уценённый или был уценён Ozon — false .
  - `is_kgt` — boolean true , если товар крупногабаритный. Только для схемы FBS.
  - `is_prepayment_allowed` — boolean Deprecated true , если возможна предоплата.
  - `is_super` — boolean Признак супер-товара. Подробнее о супер-товарах в Базе знаний продавца
  - `min_price` — string Минимальная цена товара после применения акций.
  - `model_info` — object Информация о модели товара.
  - `name` — string Название.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `old_price` — string Цена до учёта скидок. На карточке товара отображается зачёркнутой.
  - `price` — string Цена товара с учётом скидок — это значение показывается на карточке товара.
  - `price_indexes` — object Ценовые индексы товара.
  - `primary_image` — Array of strings Главное изображение товара.
  - `promotions` — Array of objects Акции.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.
  - `sources` — Array of objects Информация об источниках создания товара. С 1 июля 2023 года продавцы создают товары по схеме SDS.
  - `statuses` — object Информация о статусах товара.
  - `stocks` — object Информация об остатках товара.
  - `type_id` — integer <int64> Идентификатор типа товара.
  - `updated_at` — string <date-time> Дата последнего обновления товара.
  - `vat` — string Ставка НДС для товара.
  - `visibility_details` — object Настройки видимости товара.
  - `volume_weight` — number <double> Объёмный вес товара.

Пример ответа:
```json
{
  "items": [
    {
      "availabilities": [
        {
          "availability": "in_stock",
          "reasons": [
            {
              "human_text": {
                "text": "Товар доступен на складе"
              },
              "id": 12345
            }
          ],
          "sku": 987654321,
          "source": "main_warehouse"
        }
      ],
      "barcodes": [
        "123456789012"
      ],
      "color_image": [
        "https://example.com/color_image_123.jpg"
      ],
      "commissions": [
        {
          "delivery_amount": 150,
          "percent": 10,
          "return_amount": 50,
          "sale_schema": "standard",
          "value": 200
        }
      ],
      "created_at": "2023-01-15T10:30:00Z",
      "currency_code": "RUB",
      "description_category_id": 42,
      "discounted_fbo_stocks": 5,
      "errors": [
        {
          "attribute_id": 101,
          "code": "INVALID_PRICE",
          "field": "price",
          "level": "ERROR",
          "state": "pending",
          "texts": {
            "attribute_name": "Цена",
            "description": "Цена не может быть отрицательной",
            "hint_code": "PRICE_NEGATIVE",
            "message": "Исправьте цену",
            "params": [
              {
                "name": "min_price",
                "value": "100"
              }
            ],
            "short_description": "Ошибка цены"
          }
        }
      ],
      "has_discounted_fbo_item": false,
      "id": 100001,
      "images": [
        "https://example.com/image1.jpg",
        "https://example.com/image2.jpg"
      ],
      "images360": [
        "https://example.com/360_image1.jpg"
      ],
      "is_archived": false,
      "is_autoarchived": false,
      "is_discounted": true,
      "is_kgt": false,
      "is_prepayment_allowed": true,
      "is_super": false,
      "min_price": "999.99",
      "model_info": {
        "count": 3,
        "model_id": 777
      },
      "name": "Тестовый товар 123",
      "offer_id": "OFFER_123456789",
      "old_price": "1499.99",
      "price": "1299.99",
      "price_indexes": {
        "color_index": "RED",
        "external_index_data": {
          "minimal_price": "1199.99",
          "minimal_price_currency": "RUB",
          "price_index_value": 85
        },
        "ozon_index_data": {
          "minimal_price": "1250.00",
          "minimal_price_currency": "RUB",
          "price_index_value": 90
        },
        "self_marketplaces_index_data": {
          "minimal_price": "1200.00",
          "minimal_price_currency": "RUB",
          "price_index_value": 88
        }
      },
      "primary_image": [
        "https://example.com/primary_image.jpg"
      ],
      "promotions": [
        {
          "is_enabled": true,
          "type": "DISCOUNT"
        }
      ],
      "sku": 123456789,
      "sources": [
        {
          "created_at": "2023-01-10T09:15:00Z",
          "quant_code": "QUANT_123",
          "shipment_type": "EXPRESS",
          "sku": 123456789,
          "source": "supplier_1"
        }
      ],
      "statuses": {
        "is_created": true,
        "moderate_status": "approved",
        "status": "active",
        "status_description": "Товар доступен для продажи",
        "status_failed": "none",
        "status_name": "Активен",
        "status_tooltip": "Товар прошёл модерацию",
        "status_updated_at": "2023-01-14T11:20:00Z",
        "validation_status": "valid"
      },
      "stocks": {
        "has_stock": true,
        "stocks": [
          {
            "present": 50,
            "reserved": 5,
            "sku": 123456789,
            "source": "main_warehouse"
          }
        ]
      },
      "type_id": 1,
      "updated_at": "2023-01-16T12:45:00Z",
      "vat": "20%",
      "visibility_details": {
        "has_price": true,
        "has_stock": true
      },
      "volume_weight": 1.5
    }
  ]
}
```

---

## Получить описание характеристик товара

`POST /v4/product/info/attributes`

Operation ID: `ProductAPI_GetProductAttributesV4`

Возвращает описание характеристик товаров по идентификатору и видимости. Товар можно искать по offer_id , product_id или sku .

```bash
curl -X POST "https://api-seller.ozon.ru/v4/product/info/attributes" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "filter": {
    "product_id": [
      "0"
    ],
    "offer_id": [
      "string"
    ],
    "sku": [
      "0"
    ],
    "visibility": "ALL"
  },
  "limit": 100,
  "sort_dir": "ASC"
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `filter` — object Фильтр по товарам.
- `last_id` — string Идентификатор последнего значения на странице. Оставьте это поле пустым при выполнении первого запроса. Чтобы получить следующие значения, укажите last_id из ответа предыдущего запроса.
- `limit` — integer <int32> [ 1 .. 1000 ] Количество значений на странице.
- `sort_by` — string Параметр, по которому товары будут отсортированы: sku — сортировка по идентификатору товара в системе Ozon; offer_id — сортировка по артикулу товара; id — сортировка по идентификатору товара; title — сортировка по названию товара.
- `sort_dir` — string Направление сортировки: asc — по возрастанию, desc — по убыванию.

### Ответы

- 200 Описание характеристик товара
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результаты запроса.
  - `attributes` — Array of objects Список характеристик товара.
  - `attributes_with_defaults` — Array of integers <int64> Список идентификаторов характеристик со значением по умолчанию.
  - `barcode` — string Штрихкод.
  - `barcodes` — array of strings Все штрихкоды товара.
  - `description_category_id` — integer <int64> Идентификатор категории. Используйте его с методами /v1/description-category/attribute и /v1/description-category/attribute/values .
  - `color_image` — string Маркетинговый цвет.
  - `complex_attributes` — Array of objects Массив вложенных характеристик.
  - `depth` — integer <int64> Глубина.
  - `dimension_unit` — string Единица измерения габаритов: mm — миллиметры, cm — сантиметры, in — дюймы.
  - `height` — integer <int64> Высота упаковки.
  - `id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `images` — array of strings Массив ссылок на изображения товара. Порядок изображений аналогичен порядку в карточке товаров.
  - `model_info` — object Информация о модели.
  - `name` — string <= 500 characters Название товара.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.
  - `pdf_list` — Array of objects Массив PDF-файлов.
  - `primary_image` — string Ссылка на главное изображение товара.
  - `sku` — string Идентификатор товара в системе Ozon — SKU.
  - `type_id` — integer <int64> Идентификатор типа товара.
  - `weight` — integer <int64> Вес товара в упаковке.
  - `weight_unit` — string Единица измерения веса.
  - `width` — integer <int64> Ширина упаковки.
- `last_id` — string Идентификатор последнего значения на странице. Чтобы получить следующие значения, укажите полученное значение в следующем запросе в параметре last_id .
- `total` — string <int64> Количество товаров в списке.

Пример ответа:
```json
{
  "result": [
    {
      "id": 213761435,
      "barcode": "",
      "barcodes": [
        "123124123",
        "123342455"
      ],
      "name": "Пленка защитная для Xiaomi Redmi Note 10 Pro 5G",
      "offer_id": "21470",
      "type_id": 124572394,
      "height": 10,
      "depth": 210,
      "width": 140,
      "dimension_unit": "mm",
      "weight": 50,
      "weight_unit": "g",
      "primary_image": "https://cdn1.ozone.ru/s3/multimedia-4/6804736960.jpg",
      "sku": 423434534,
      "model_info": {
        "model_id": 43445453,
        "count": 4
      },
      "images": [
        "https://cdn1.ozone.ru/s3/multimedia-4/6804736960.jpg",
        "https://cdn1.ozone.ru/s3/multimedia-j/6835412647.jpg"
      ],
      "pdf_list": [],
      "attributes": [
        {
          "id": 5219,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 970718176,
              "value": "универсальный"
            }
          ]
        },
        {
          "id": 11051,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 970736931,
              "value": "Прозрачный"
            }
          ]
        },
        {
          "id": 10100,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "false"
            }
          ]
        },
        {
          "id": 11794,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 970860783,
              "value": "safe"
            }
          ]
        },
        {
          "id": 9048,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "Пленка защитная для Xiaomi Redmi Note 10 Pro 5G"
            }
          ]
        },
        {
          "id": 5076,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 39638,
              "value": "Xiaomi"
            }
          ]
        },
        {
          "id": 9024,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "21470"
            }
          ]
        },
        {
          "id": 10015,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "false"
            }
          ]
        },
        {
          "id": 85,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 971034861,
              "value": "Brand"
            }
          ]
        },
        {
          "id": 9461,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 349824787,
              "value": "Защитная пленка для смартфона"
            }
          ]
        },
        {
          "id": 4180,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "Пленка защитная для Xiaomi Redmi Note 10 Pro 5G"
            }
          ]
        },
        {
          "id": 4191,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 0,
              "value": "Пленка предназначена для модели Xiaomi Redmi Note 10 Pro 5G. Защитная гидрогелевая пленка обеспечит защиту вашего смартфона от царапин, пыли, сколов и потертостей."
            }
          ]
        },
        {
          "id": 8229,
          "complex_id": 0,
          "values": [
            {
              "dictionary_value_id": 91521,
              "value": "Защитная пленка"
            }
          ]
        }
      ],
      "attributes_with_defaults": [
        5435,
        3452
      ],
      "complex_attributes": [],
      "color_image": "",
      "description_category_id": 71107562
    }
  ],
  "total": 1,
  "last_id": "onVsfA=="
}
```

---

## Получить описание товара

`POST /v1/product/info/description`

Operation ID: `ProductAPI_GetProductInfoDescription`

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/info/description" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "offer_id": "5",
  "product_id": 73453843
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `offer_id required` — string Идентификатор товара в системе продавца — артикул.
- `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .

### Ответы

- 200 Описание товара
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object
  - `description` — string Описание.
  - `id` — integer <int64> Идентификатор.
  - `name` — string Название.
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.

Пример ответа:
```json
{
  "result": {
    "id": 73453843,
    "offer_id": "5",
    "name": "Онлайн курс по дрессировке собак \"Воспитанная собака за 3 недели\"",
    "description": "Экспресс-курс - это сокращённый вариант курса \"Собака: инструкция по применению\", дающий базовый минимум знаний, навыков, умений. Это оптимальный вариант для совершения первых шагов по воспитанию!<br/><br/>Что дает Экспресс-курс:<ul><li>Контакт с собакой </li></ul>К концу экспресс-курса дрессировки вы получаете воспитанного друга и соратника, который ориентируется на вас в любой ситуации.<ul><li>Уверенность в безопасности</li></ul>Благополучие собаки: больше не будет срывов с поводка, преследования кошек, попыток съесть что-либо на улице и т. д.<ul><li>Комфортная жизнь</li></ul>Принципиально другой уровень общения, без раздражения, криков и недовольства поведением животного."
  }
}
```

---

## Лимиты на ассортимент, создание и обновление товаров

`POST /v4/product/info/limit`

Operation ID: `ProductAPI_GetUploadQuota`

Метод для получения информации о лимитах: На ассортимент — сколько всего товаров можно создать в вашем личном кабинете. На создание товаров — сколько товаров можно создать в сутки. На обновление товаров — сколько товаров можно отредактировать в сутки. На работу с товарами — сколько товаров можно создать в минуту. …

```bash
curl -X POST "https://api-seller.ozon.ru/v4/product/info/limit" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Ответы

- 200 Лимиты на ассортмент, создание и обновление товаров
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `daily_create` — object Суточный лимит на создание товаров.
- `daily_update` — object Суточный лимит на обновление товаров.
- `operation_limits` — object Доступный лимит на работу с товарами.
- `total` — object Лимит на ассортимент.

Пример ответа:
```json
{
  "daily_create": {
    "limit": 0,
    "reset_at": "2019-08-24T14:15:22Z",
    "usage": 0
  },
  "daily_update": {
    "limit": 0,
    "reset_at": "2019-08-24T14:15:22Z",
    "usage": 0
  },
  "operation_limits": {
    "limit": 0,
    "limit_type": "UNSPECIFIED"
  },
  "total": {
    "limit": 0,
    "usage": 0
  }
}
```

---

## Изменить артикулы товаров из системы продавца

`POST /v1/product/update/offer-id`

Operation ID: `ProductAPI_ProductUpdateOfferID`

Метод для изменения offer_id , привязанных к товарам. Вы можете изменить несколько offer_id . Если offer_id уже используется для другого товара, вернётся ошибка OFFER_ID_ALREADY_EXISTS . У метода есть лимит на количество операций c товарами в минуту. …

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/update/offer-id" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "update_offer_id": [
    {
      "new_offer_id": "haval",
      "offer_id": "chery"
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `update_offer_id required` — Array of objects [ 1 .. 25 ] items Список пар с новыми и старыми значениями артикулов.

### Ответы

- 200 Информация об изменении артикулов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 429 Слишком много запросов
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `errors` — Array of objects Список ошибок.
  - `message` — string Сообщение об ошибке.
  - `offer_id` — string Артикул товара, который не получилось изменить.

Пример ответа:
```json
{
  "errors": [
    {
      "offer_id": "haval",
      "message": "Товар с артикулом haval уже существует."
    }
  ]
}
```

---

## Перенести товар в архив

`POST /v1/product/archive`

Operation ID: `ProductAPI_ProductArchive`

Возвращает статус запроса на архивацию. Чтобы проверить фактическое состояние товара после архивации, используйте метод /v3/product/info/list . Подробнее об управлении товарами в архиве в Базе знаний продавца

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/archive" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "product_id": [
    "125529926",
    "987654321"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `product_id required` — Array of integers <int64> Список идентификаторов товаров в системе Ozon — product_id . Вы можете передать до 100 идентификаторов за раз.

### Ответы

- 200 Успешно
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнен без ошибок.

---

## Вернуть товар из архива

`POST /v1/product/unarchive`

Operation ID: `ProductAPI_ProductUnarchive`

Восстанавливает товары, которые были архивированы автоматически. Если вы превысите лимит на восстановление, вернётся ошибка autoarchive_restore_failed . Подробнее об управлении товарами в архиве в Базе знаний продавца

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/unarchive" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "product_id": [
    "125529926",
    "987654321"
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `product_id required` — Array of integers <int64> Список идентификаторов товаров в системе Ozon — product_id . Вы можете передать до 100 идентификаторов за раз. В сутки можно восстановить из архива не больше 100 товаров, которые были архивированы автоматически. Лимит обновляется в 03:00 по московскому времени. На разархивацию товаров, перенесённых в архив вручную, ограничений нет.

### Ответы

- 200 Товар из архива возвращён
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — boolean Результат обработки запроса. true , если запрос выполнен без ошибок.

---

## Удалить товар без SKU из архива

`POST /v2/products/delete`

Operation ID: `ProductAPI_DeleteProducts`

В одном запросе можно передать до 500 идентификаторов.

```bash
curl -X POST "https://api-seller.ozon.ru/v2/products/delete" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "products": [
    {
      "offer_id": "033"
    }
  ]
}'
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `products required` — Array of objects Идентификатор товара в системе Ozon — product_id .

### Ответы

- 200 Товар удалён
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `status` — Array of objects Статус обработки запроса.
  - `error` — string Причина ошибки, которая возникла при обработке запроса.
  - `is_deleted` — boolean Если запрос выполнен без ошибок и товары удалены — true .
  - `offer_id` — string Идентификатор товара в системе продавца — артикул.

Пример ответа:
```json
{
  "status": [
    {
      "offer_id": "033",
      "is_deleted": true,
      "error": ""
    }
  ]
}
```

---

## Количество подписавшихся на товар пользователей

`POST /v1/product/info/subscription`

Operation ID: `ProductAPI_GetProductInfoSubscription`

Метод для получения количества пользователей, которые нажали Узнать о поступлении на странице товара. Вы можете передать несколько товаров в запросе.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/info/subscription" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
  "skus": [
    "889283932",
    "123456789",
    "987654321"
  ]
}'
```

### Тело запроса

- `skus required` — Array of strings <int64> Список SKU, идентификаторов товара в системе Ozon.

### Ответы

- 200 Количество подписавшихся пользователей

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `count` — integer <int64> Количество подписавшихся пользователей.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon, SKU.

Пример ответа:
```json
{
  "result": [
    {
      "count": 3,
      "sku": 889283932
    }
  ]
}
```

---

## Получить связанные SKU

`POST /v1/product/related-sku/get`

Operation ID: `ProductAPI_ProductGetRelatedSKU`

Метод для получения единого SKU по старым идентификаторам SKU FBS и SKU FBO. В ответе будут все SKU, связанные с переданными. Метод может обработать любые SKU, даже скрытые или удалённые. Передавайте до 200 SKU в одном запросе.

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/related-sku/get" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `sku required` — Array of strings <int64> Список SKU.

### Ответы

- 200 Информация об SKU

#### Схема ответа (200)

- `items` — Array of objects Информация о связанных SKU.
- `errors` — Array of objects Ошибки.

Пример ответа:
```json
{
  "items": [
    {
      "availability": "AVAILABLE",
      "deleted_at": "2019-08-24T14:15:22Z",
      "delivery_schema": "SDS",
      "product_id": "door",
      "sku": "88997766"
    }
  ],
  "errors": [
    {
      "code": "das",
      "sku": "88997766",
      "message": "sku does not exist"
    }
  ]
}
```

---

## Получить изображения товаров

`POST /v2/product/pictures/info`

Operation ID: `ProductAPI_ProductInfoPicturesV2`

```bash
curl -X POST "https://api-seller.ozon.ru/v2/product/pictures/info" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `product_id required` — Array of strings <int64> <= 1000 items Список идентификаторов товаров в системе Ozon — product_id .

### Ответы

- 200 Изображения товаров

#### Схема ответа (200)

- `items` — Array of objects Изображения товаров.
  - `product_id` — integer <int64> Идентификатор товара в системе Ozon — product_id .
  - `primary_photo` — Array of strings Ссылка на главное изображение.
  - `photo` — Array of strings Ссылки на фотографии товара.
  - `color_photo` — Array of strings Ссылки на загруженные образцы цвета.
  - `photo_360` — Array of strings Ссылки на изображения 360.
  - `errors` — Array of objects Список ошибок по изображениям товара.

Пример ответа:
```json
{
  "items": [
    {
      "product_id": 100500,
      "primary_photo": [
        "https://test-test.ru/images/products/100500/primary.jpg"
      ],
      "photo": [
        "https://test-test.ru/images/products/100500/photo1.jpg",
        "https://test-test.ru/images/products/100500/photo2.jpg"
      ],
      "color_photo": [
        "https://test-test.ru/images/products/100500/color_red.jpg",
        "https://test-test.ru/images/products/100500/color_blue.jpg"
      ],
      "photo_360": [
        "https://test-test.ru/images/products/100500/360_view1.jpg",
        "https://test-test.ru/images/products/100500/360_view2.jpg"
      ],
      "errors": [
        {
          "message": "Не удалось загрузить дополнительное изображение: превышен размер файла",
          "url": "https://test-test.ru/api/v1/products/100500/errors/12345"
        },
        {
          "message": "Отсутствует описание для цвета 'зелёный'",
          "url": "https://test-test.ru/api/v1/products/100500/errors/12346"
        }
      ]
    }
  ]
}
```

---

## Список товаров с некорректными ОВХ

`POST /v1/product/info/wrong-volume`

Operation ID: `ProductAPI_ProductInfoWrongVolume`

Возвращает список товаров с некорректными объёмно-весовыми характеристиками (ОВХ). Если вы указали размеры правильно, обратитесь в поддержку Ozon. Подробнее об объёмно-весовых характеристиках в Базе знаний продавца

```bash
curl -X POST "https://api-seller.ozon.ru/v1/product/info/wrong-volume" \
  -H "Client-Id: <CLIENT_ID>" \
  -H "Api-Key: <API_KEY>"
```

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса

- `cursor` — string Указатель для выборки следующих данных.
- `limit required` — integer <int64> [ 1 .. 1000 ] Максимальное количество элементов в ответе.

### Ответы

- 200 Информация о товарах с некорректными ОВХ
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `products` — Array of objects Список товаров.

Пример ответа:
```json
{
  "cursor": "string",
  "products": [
    {
      "height": 0,
      "length": 0,
      "name": "string",
      "offer_id": "string",
      "product_id": 0,
      "sku": 0,
      "weight": 0,
      "width": 0
    }
  ]
}
```

---
