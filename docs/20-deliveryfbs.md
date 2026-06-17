# Доставка FBS

_Тег: `DeliveryFBS` · операций: 27_

## Создание отгрузки

`POST /v1/carriage/create`

Operation ID: `CarriageAPI_CarriageCreate`

Если вы продавец не из России, обратите внимание на доступность рекомендованного времени в личном кабинете. Если вам не доступен этот функционал, создайте отгрузку через метод /v2/posting/fbs/act/create . Подтверждать отгрузку, которую создали через этот метод, не нужно. Вы не сможете отредактировать состав отгрузки. Используйте метод для создания первой FBS отгрузки. В неё попадут все отправления со статусом «Готов к отгрузке». Созданная отгрузка получит статус new . Для отгрузки в статусе new можно перезаписать состав отправлений методом /v1/carriage/set-postings . Если из отгрузки исключить часть отправлений, они могут попасть в следующую отгрузку. Чтобы получить список отправлений в отгрузке, используйте метод /v2/posting/fbs/act/get-postings .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `all_blr_traceable` — boolean true , если нужно создать отгрузку с прослеживаемыми товарами.
- `delivery_method_id` — integer <int64> Идентификатор метода доставки.
- `departure_date` — string <date-time> Дата отгрузки. По умолчанию — текущая дата.

Пример запроса:

```json
{
  "all_blr_traceable": true,
  "delivery_method_id": 0,
  "departure_date": "2019-08-24T14:15:22Z"
}
```

### Ответы

- 200 Информация об отгрузке
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `carriage_id` — integer <int64> Идентификатор перевозки.

---

## Подтверждение отгрузки

`POST /v1/carriage/approve`

Operation ID: `CarriageAPI_CarriageApprove`

Используйте метод, чтобы подтвердить отгрузку после её создания. После подтверждения отгрузка перейдёт в статус «Сформирована». После подтверждения отгрузки вы можете получить лист отгрузки методом /v2/posting/fbs/act/get-pdf и штрихкод отгрузки методом /v2/posting/fbs/act/get-barcode .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор отгрузки.
- `containers_count` — integer <int32> Количество грузовых мест. Используйте параметр, если вы подключены к доверительной приёмке и отгружаете заказы грузовыми местами. Если вы не подключены к доверительной приёмке, пропустите его.

Пример запроса:

```json
{
  "carriage_id": 0,
  "containers_count": 0
}
```

### Ответы

- 200 Отгрузка подтверждена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

---

## Изменение состава отгрузки

`POST /v1/carriage/set-postings`

Operation ID: `CarriageAPI_SetPostings`

Метод недоступен для продавцов из СНГ. Полностью перезаписывает список заказов в отгрузке. Передавайте только те заказы, которые находятся в статусе Ожидает отгрузки , и вы готовы их отгрузить. Чтобы вернуться к списку заказов, удалите отгрузку с помощью метода /v1/carriage/cancel , и создайте новую.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор отгрузки.
- `posting_numbers required` — Array of strings Актуальный список отправлений.

Пример запроса:

```json
{
  "carriage_id": 0,
  "posting_numbers": [
    "string"
  ]
}
```

### Ответы

- 200 Информация об отправлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects
  - `error` — string Описание ошибки.
  - `posting_number` — string Номер отправления.
  - `result` — boolean Результат обработки запроса. true , если запрос был обработан успешно.

Пример ответа:

```json
{
  "result": [
    {
      "error": "string",
      "posting_number": "string",
      "result": true
    }
  ]
}
```

---

## Удаление отгрузки

`POST /v1/carriage/cancel`

Operation ID: `CarriageAPI_CarriageCancel`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор отгрузки.

### Ответы

- 200 Информация об отправлении
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `error` — string Описание ошибки.
- `carriage_status` — string Статус отгрузки.

Пример ответа:

```json
{
  "error": "string",
  "carriage_status": "string"
}
```

---

## Список методов доставки и отгрузок

`POST /v2/carriage/delivery/list`

Operation ID: `CarriageAPI_CarriageDeliveryListV2`

Метод не возвращает информацию по методам доставки, у которых нет отправлений.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter` — object Фильтр для поиска методов доставки и отгрузок.
- `limit required` — integer <int64> <= 1000 Количество значений на странице.

Пример запроса:

```json
{
  "cursor": "string",
  "filter": {
    "delivery_method_id": 0,
    "departure_date": "string"
  },
  "limit": 0
}
```

### Ответы

- 200 Список методов и отгрузок
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных.
- `has_next` — boolean true , если в ответе вернулись не все методы доставки.
- `methods` — Array of objects Список методов доставки.

Пример ответа:

```json
{
  "cursor": "string",
  "has_next": true,
  "methods": [
    {
      "carriage_postings_count": 0,
      "carriages": [
        {
          "all_blr_traceable": true,
          "available_actions": [
            "string"
          ],
          "carriage_volume": 0,
          "id": 0,
          "pickup_fee": {
            "currency_code": "string",
            "value": 0
          },
          "postings_count": 0,
          "quantum_count": 0,
          "status": "string"
        }
      ],
      "cut_in": "string",
      "cutoff_at": "string",
      "delivery_method_id": 0,
      "delivery_method_name": "string",
      "delivery_method_status": "string",
      "departure_date": "string",
      "dropoff_address": "string",
      "dropoff_change_availability": "string",
      "dropoff_point_id": 0,
      "dropoff_point_type": "string",
      "errors": [
        {
          "code": "string",
          "description": "string",
          "status": "string"
        }
      ],
      "first_mile_changing": true,
      "first_mile_type": "string",
      "has_entrusted_acceptance": true,
      "integration_type": "string",
      "is_optional_carriage": true,
      "is_presort": true,
      "is_rfbs": true,
      "mandatory_packaged_count": 0,
      "mandatory_postings_count": 0,
      "optional_packaged_count": 0,
      "recommended_time_local": "string",
      "recommended_time_utc_offset_in_minutes": 0,
      "timeslot_from": "string",
      "timeslot_to": "string",
      "tpl_provider_icon_url": "string",
      "tpl_provider_name": "string",
      "warehouse_city": "string",
      "warehouse_id": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Список методов доставки и отгрузок

`POST /v1/carriage/delivery/list`

Operation ID: `CarriageAPI_CarriageDeliveryList`

Метод не возвращает информацию по методам доставки, у которых нет отправлений. Используйте метод, чтобы получить список созданных отгрузок для метода доставки и их статусы. 20 марта 2026 года отключим метод. Переключитесь на /v2/carriage/delivery/list .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id` — integer <int64> Идентификатор метода доставки.
- `departure_date` — string <date-time> Дата отгрузки. По умолчанию — текущая дата.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "departure_date": "2019-08-24T14:15:22Z"
}
```

### Ответы

- 200 Список методов и отгрузок
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects
  - `assembly_list_availability` — boolean true , если доступен лист подбора.
  - `can_create_another_carriage` — boolean true , если можно создать ещё одну перевозку.
  - `carriage_postings_count` — integer <int32> Количество отправлений в перевозке.
  - `carriage_quantum_count` — integer <int32> Количество квантов в перевозке.
  - `carriages` — Array of objects Список перевозок.
  - `cut_in` — string <date-time> Время начала сборки и часовой пояс времени склада.
  - `delivery_method_id` — integer Идентификатор метода доставки.
  - `delivery_method_name` — string Название метода доставки.
  - `delivery_method_status` — string Статус метода доставки.
  - `departure_date` — string <date-time> Дата отгрузки.
  - `dropoff_address` — string Адрес точки отгрузки.
  - `dropoff_change_availability` — string Статус возможности смены точки отгрузки.
  - `dropoff_point_id` — integer <int64> Идентификатор точки отгрузки.
  - `dropoff_point_type` — string Способ отгрузки.
  - `errors` — Array of objects Массив ошибок, которые возникли при обработке запроса.
  - `first_mile_changing` — boolean true , если точка отгрузки изменилась.
  - `first_mile_type` — string Тип первой мили.
  - `has_entrusted_acceptance` — boolean Признак доверительной приёмки. true , если доверительная приёмка включена на складе.
  - `integration_type` — string Тип интеграции со службой доставки.
  - `is_presort` — boolean true , если отгрузка с предсортировкой.
  - `is_rfbs` — boolean true , если склад работает по схеме rFBS.
  - `recommended_time_local` — string Рекомендуемое местное время отгрузки в пункт приёма заказов.
  - `recommended_time_utc_offset_in_minutes` — number <int32> Смещение часового пояса рекомендуемого времени отгрузки от UTC-0 в минутах.
  - `cutoff_at` — string <date-time> Дата и время, до которых нужно собрать отправление.
  - `mandatory_packaged_count` — integer <int32> Количество «обязательных» собранных отправлений.
  - `mandatory_packaged_quantum_count` — integer <int32> Количество «обязательных» собранных квантов.
  - `mandatory_postings_count` — integer <int32> Количество отправлений, которые нужно собрать.
  - `mandatory_quantum_count` — integer <int32> Количество квантов, которые нужно собрать.
  - `optional_packaged_count` — integer <int32> Количество собранных «необязательных» отправлений.
  - `postings_for_another_carriage_count` — integer <int32> Количество отправлений, которые могут попасть в следующую перевозку.
  - `quantum_for_another_carriage_count` — integer <int32> Количество квантов, которые могут попасть в следующую перевозку.
  - `timeslot_from` — string <date-time> Начало таймслота в точке отгрузки.
  - `timeslot_to` — string <date-time> Окончание таймслота в точке отгрузки.
  - `tpl_provider_icon_url` — string Ссылка на иконку службы доставки.
  - `tpl_provider_name` — string Название службы доставки.
  - `warehouse_city` — string Город склада.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `warehouse_name` — string Название склада.

Пример ответа:

```json
{
  "result": [
    {
      "assembly_list_availability": true,
      "can_create_another_carriage": true,
      "carriage_postings_count": 0,
      "carriage_quantum_count": 0,
      "carriages": [
        {
          "id": "string",
          "postings_count": 0,
          "quantum_count": 0,
          "status": "string"
        }
      ],
      "cut_in": "2019-08-24T14:15:22Z",
      "delivery_method_id": 0,
      "delivery_method_name": "string",
      "delivery_method_status": "string",
      "departure_date": "2019-08-24T14:15:22Z",
      "dropoff_address": "string",
      "dropoff_change_availability": "string",
      "dropoff_point_id": 0,
      "dropoff_point_type": "string",
      "errors": [
        {
          "code": "string",
          "description": "string",
          "status": "string"
        }
      ],
      "first_mile_changing": true,
      "first_mile_type": "string",
      "has_entrusted_acceptance": true,
      "integration_type": "string",
      "is_presort": true,
      "is_rfbs": true,
      "recommended_time_local": "string",
      "recommended_time_utc_offset_in_minutes": 0,
      "cutoff_at": "2019-08-24T14:15:22Z",
      "mandatory_packaged_count": 0,
      "mandatory_packaged_quantum_count": 0,
      "mandatory_postings_count": 0,
      "mandatory_quantum_count": 0,
      "optional_packaged_count": 0,
      "postings_for_another_carriage_count": 0,
      "quantum_for_another_carriage_count": 0,
      "timeslot_from": "2019-08-24T14:15:22Z",
      "timeslot_to": "2019-08-24T14:15:22Z",
      "tpl_provider_icon_url": "string",
      "tpl_provider_name": "string",
      "warehouse_city": "string",
      "warehouse_id": 0,
      "warehouse_name": "string"
    }
  ]
}
```

---

## Подтвердить отгрузку и создать документы

`POST /v2/posting/fbs/act/create`

Operation ID: `PostingAPI_PostingFBSActCreate`

Подтверждает отгрузку и запускает формирование транспортной накладной и штрихкода для отгрузки. Для продавцов из России также запускается формирование листа отгрузки, а для продавцов из СНГ — акта приёма-передачи. Чтобы сформировать и получить документы, переведите отправление в статус awaiting_deliver .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `containers_count` — integer <int32> Количество грузовых мест. Используйте параметр, если вы подключены к доверительной приёмке и отгружаете заказы грузовыми местами. Если вы не подключены к доверительной приёмке, пропустите его. Подробнее в Базе знаний продавца
- `delivery_method_id required` — integer <int64> Идентификатор метода доставки. Для realFBS-складов получите его с помощью метода /v2/delivery-method/list . Для FBS-складов используйте значение параметра warehouse_id . Его можно получить с помощью метода /v2/warehouse/list .
- `departure_date` — string <date-time> Дата отгрузки.

Пример запроса:

```json
{
  "containers_count": 1,
  "delivery_method_id": 230039077005,
  "departure_date": "2022-06-10T11:42:06.444Z"
}
```

### Ответы

- 200 Отгрузка подтверждена
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `id` — integer <int64> Номер задания на формирование штрихкода и документов.

Пример ответа:

```json
{
  "result": {
    "id": 5819327210249
  }
}
```

---

## Список доступных перевозок

`POST /v1/posting/carriage-available/list`

Operation ID: `PostingAPI_GetCarriageAvailableList`

20 марта 2026 года отключим метод. Переключитесь на /v2/carriage/delivery/list . Метод для получения перевозок, по которым нужно распечатать штрихкод для отгрузки и документы: для продацов из России — лист отгрузки и транспортную накладную; для продавцов из СНГ — акт и транспортную накладную.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `delivery_method_id required` — integer <int64> Фильтр по методу доставки. Можно получить с помощью метода /v2/delivery-method/list .
- `departure_date` — string <date-time> Дата отгрузки. По умолчанию — текущая дата.

Пример запроса:

```json
{
  "delivery_method_id": 0,
  "departure_date": "2019-08-24T14:15:22Z"
}
```

### Ответы

- 200 Список перевозок
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат работы метода.
  - `carriage_id` — integer <int64> Идентификатор перевозки (также номер задания на формирование документов).
  - `carriage_postings_count` — integer <int32> Количество отправлений в перевозке.
  - `carriage_status` — string Статус перевозки для запрашиваемого метода доставки и даты отгрузки.
  - `cutoff_at` — string <date-time> Дата и время, до которых нужно собрать отправление.
  - `delivery_method_id` — integer <int64> Идентификатор метода доставки.
  - `delivery_method_name` — string Название метода доставки.
  - `errors` — Array of objects Список ошибок.
  - `first_mile_type` — string Тип первой мили.
  - `has_entrusted_acceptance` — boolean Признак доверительной приёмки. true , если доверительная приёмка включена на складе.
  - `mandatory_postings_count` — integer <int32> Количество отправлений, которые нужно собрать.
  - `mandatory_packaged_count` — integer <int32> Количество собранных отправлений.
  - `recommended_time_local` — string Рекомендуемое местное время отгрузки на пункт приёма заказов.
  - `recommended_time_utc_offset_in_minutes` — number <int32> Смещение часового пояса рекомендуемого времени отгрузки от UTC-0 в минутах.
  - `tpl_provider_icon_url` — string Ссылка на иконку службы доставки.
  - `tpl_provider_name` — string Название службы доставки.
  - `warehouse_city` — string Город склада.
  - `warehouse_id` — integer <int64> Идентификатор склада.
  - `warehouse_name` — string Название склада.
  - `warehouse_timezone` — string Часовой пояс, в котором находится склад.

Пример ответа:

```json
{
  "result": [
    {
      "carriage_id": 0,
      "carriage_postings_count": 0,
      "carriage_status": "string",
      "cutoff_at": "2019-08-24T14:15:22Z",
      "delivery_method_id": 0,
      "delivery_method_name": "string",
      "errors": [
        {
          "code": "string",
          "status": "string"
        }
      ],
      "first_mile_type": "string",
      "has_entrusted_acceptance": true,
      "mandatory_postings_count": 0,
      "mandatory_packaged_count": 0,
      "recommended_time_local": "string",
      "recommended_time_utc_offset_in_minutes": 0,
      "tpl_provider_icon_url": "string",
      "tpl_provider_name": "string",
      "warehouse_city": "string",
      "warehouse_id": 0,
      "warehouse_name": "string",
      "warehouse_timezone": "string"
    }
  ]
}
```

---

## Информация о перевозке

`POST /v1/carriage/get`

Operation ID: `CarriageGet`

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор перевозки.

### Ответы

- 200 Информация о перевозке

#### Схема ответа (200)

- `act_type` — string Тип акта приёма-передачи. Актуально для продавцов FBS.
- `all_blr_traceable` — boolean true , если отгрузка с прослеживаемыми товарами.
- `is_waybill_enabled` — boolean true , если доступна печать транспортной накладной.
- `is_econom` — boolean true , если отгрузка относится к товарам «Суперэконом».
- `arrival_pass_ids` — Array of strings <int64> Список идентификаторов пропусков, оформленных на перевозку.
- `available_actions` — Array of strings Доступные действия с перевозкой: get_shipping_list — получить лист отгрузки; get_act_of_acceptance — получить акт приёма-передачи; get_waybill — получить товарную накладную в формате PDF; set_arrival_passes — оформить пропуск .
- `cancel_availability` — object Возможность отмены.
- `carriage_id` — integer <int64> Идентификатор перевозки.
- `company_id` — integer <int64> Идентификатор продавца.
- `containers_count` — integer <int32> Количество грузовых мест.
- `created_at` — string <date-time> Дата создания перевозки.
- `delivery_method_id` — integer <int64> Идентификатор метода доставки.
- `departure_date` — string Дата выполнения перевозки.
- `first_mile_type` — string Тип первой мили.
- `has_postings_for_next_carriage` — boolean true , если есть отправления, которые не попали в перевозку, но нужно отгрузить.
- `integration_type` — string Тип перевозки.
- `is_container_label_printed` — boolean true , если вы уже напечатали этикетки на грузовые места.
- `is_partial` — boolean true , если перевозка частичная.
- `partial_num` — integer <int64> Порядковый номер частичной перевозки.
- `retry_count` — integer <int32> Количество повторных попыток создания перевозки.
- `status` — string Статус перевозки: received — идёт приёмка, closed — завершена после приёмки, sended — отправлена, cancelled — отменена.
- `tpl_provider_id` — integer <int64> Идентификатор провайдера доставки.
- `updated_at` — string <date-time> Дата последнего обновления информации о перевозке.
- `warehouse_id` — integer <int64> Идентификатор склада.

Пример ответа:

```json
{
  "act_type": "string",
  "all_blr_traceable": true,
  "is_waybill_enabled": true,
  "is_econom": true,
  "arrival_pass_ids": [
    "string"
  ],
  "available_actions": [
    "string"
  ],
  "cancel_availability": {
    "is_cancel_available": true,
    "reason": "string"
  },
  "carriage_id": 0,
  "company_id": 0,
  "containers_count": 0,
  "created_at": "2019-08-24T14:15:22Z",
  "delivery_method_id": 0,
  "departure_date": "string",
  "first_mile_type": "string",
  "has_postings_for_next_carriage": true,
  "integration_type": "string",
  "is_container_label_printed": true,
  "is_partial": true,
  "partial_num": 0,
  "retry_count": 0,
  "status": "string",
  "tpl_provider_id": 0,
  "updated_at": "2019-08-24T14:15:22Z",
  "warehouse_id": 0
}
```

---

## Разделить заказ на отправления без сборки

`POST /v1/posting/fbs/split`

Operation ID: `FbsSplit`

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.
- `postings required` — Array of objects Список отправлений, на которые поделится заказ. За один запрос можно разделить один заказ.

Пример запроса:

```json
{
  "posting_number": "string",
  "postings": [
    {
      "products": [
        {
          "product_id": 0,
          "quantity": 0
        }
      ]
    }
  ]
}
```

### Ответы

- 200 Заказ разделён

#### Схема ответа (200)

- `parent_posting` — object Информация об изначальном отправлении.
- `postings` — Array of objects Список отправлений, на которые разделился заказ.

Пример ответа:

```json
{
  "parent_posting": {
    "posting_number": "string",
    "products": [
      {
        "product_id": 0,
        "quantity": 0
      }
    ]
  },
  "postings": [
    {
      "posting_number": "string",
      "products": [
        {
          "product_id": 0,
          "quantity": 0
        }
      ]
    }
  ]
}
```

---

## Список отправлений в акте

`POST /v2/posting/fbs/act/get-postings`

Operation ID: `PostingAPI_ActPostingList`

Возвращает список отправлений в акте по его идентификатору.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — int <int64> Идентификатор акта. Получите значение параметра методом /v2/posting/fbs/act/list или /v1/carriage/create .

### Ответы

- 200 Список отправлений
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Информация об отправлениях.
  - `id` — integer <int64> Идентификатор акта.
  - `multi_box_qty` — integer <int32> Количество коробок, в которые упакован товар.
  - `posting_number` — string Номер отправления.
  - `status` — string Статус отправления.
  - `seller_error` — string Расшифровка кода ошибки.
  - `updated_at` — string <date-time> Дата и время обновления записи об отправлении.
  - `created_at` — string <date-time> Дата и время создания записи об отправлении.
  - `products` — Array of objects Список товаров в отправлении.

Пример ответа:

```json
{
  "result": [
    {
      "id": 0,
      "multi_box_qty": 0,
      "posting_number": "string",
      "status": "string",
      "seller_error": "string",
      "updated_at": "2019-08-24T14:15:22Z",
      "created_at": "2019-08-24T14:15:22Z",
      "products": [
        {
          "name": "string",
          "offer_id": "string",
          "price": "string",
          "quantity": 0,
          "sku": 0
        }
      ]
    }
  ]
}
```

---

## Этикетки для грузового места

`POST /v2/posting/fbs/act/get-container-labels`

Operation ID: `PostingAPI_PostingFBSActGetContainerLabels`

Метод создает этикетки для грузового места.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Номер задания на формирование документов (также идентификатор перевозки) из метода POST /v2/posting/fbs/act/create .

### Ответы

- 200 Этикетки для грузового места
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `file_content` — string <byte> Содержание файла в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:

```json
{
  "content_type": "application/pdf",
  "file_name": "carriage-containers-20903594.pdf",
  "file_content": "%PDF-1.4\n%âãÏÓ\n2 0 obj\n<</Length 2992/Filter/FlateDecode>>stream\nxµ}[ێ\u001c·\u0011}¯èç\u0000¦Èb\u0015/ @»+\u0019y0Ë\u0002ù\u0000%q\u0010X\u0001ìü?Ãn²ÉéfÍì(ò®\u001duMÝ/<Å\u0019\u001bòyýY,0Ã?=[ccyýåëå×K¡§\u000bAÂâ؉x\u001dßþqùÛ\u001fÿà-dp¢UÔø\u001aün)¿ùqÙ^üöóåݏùù¿«X¶i\t²JúçåÏøýõÙ$Gxn²\u0011&\u000f¥ÉCj¾§2aæºr&­^,~hI²)F¤ù7¥íu£:oÊ\u0013Ùȹ0ûLdB\u001a\u0018y§xk;ë<^Lv)%¼)í\u0014öcyóÎX:\u0018ÚõIXå\u0015\u0013╏\rõɌ5dýÆ\u0016Ê!6Ñpys\u001aÄXYÃ1­Ô\r:H©(%U´³bR"
}
```

---

## Штрихкод для отгрузки отправления

`POST /v2/posting/fbs/act/get-barcode`

Operation ID: `PostingAPI_PostingFBSGetBarcode`

Метод для получения штрихкода, который нужно показать в пункте выдачи или сортировочном центре при отгрузке отправления.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Идентификатор перевозки.

### Ответы

- 200 Штрихкод для отправления
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `file_content` — string Изображение со штрихкодом в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:

```json
{
  "content_type": "image/png",
  "file_name": "20913984_barcode.png",
  "file_content": "PNG\r\n\u001a\n\u0000\u0000\u0000\rIHDR\u0000\u0000\u0003\u0010\u0000\u0000\u0000\u0010\u0000\u0000\u0000\u0000íZ\u000e'\u0000\u0000\u0002pIDATxìÕÁJ\u00031\u0014@Q+þÿ/×E\u0017\u000e¼\u0010u¡-ç¬$£Éˌp?î÷·§t» }ýü¸Ãcåz¹2wOWû\\Ϛ뫧×Ùö;ì|rÇýßîç¼úî{§¬N?í7oìv¸®µ¹Ãùû¹¾ÿÏ9ÿî?a¸ºéê7O&߿É9çÉ\u000eÏáý¯\u0007\u0000à\u0012\b\u0000@\u0000\u0004\u0002$\u0010\u0000$\u0000 \t\u0004\u0000I \u0000H\u0002\u0001@\u0012\b\u0000@\u0000\u0004\u0002$\u0010\u0000$\u0000 \t\u0004\u0000I \u0000H\u0002\u0001@\u0012\b\u0000@\u0000\u0004\u0002$\u0010\u0000$\u0000 \t\u0004\u0000I \u0000H\u0002\u0001@\u0012\b\u0000@\u0000\u0004\u0002$\u0010\u0000$\u0000 \t\u0004\u0000I \u0000H\u0002\u0001@\u0012\b\u0000@\u0000\u0004\u0002$\u0010\u0000"
}
```

---

## Значение штрихкода для отгрузки отправления

`POST /v2/posting/fbs/act/get-barcode/text`

Operation ID: `PostingAPI_PostingFBSGetBarcodeText`

Используйте этот метод, чтобы получить штрихкод из ответа /v2/posting/fbs/act/get-barcode в текстовом виде.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Идентификатор перевозки.

### Ответы

- 200 Значение штрихкода

#### Схема ответа (200)

- `result` — string Штрихкод в текстовом виде.

---

## Статус формирования накладной Deprecated

`POST /v2/posting/fbs/digital/act/check-status`

Operation ID: `PostingAPI_PostingFBSDigitalActCheckStatus`

Метод устаревает и будет отключён 22 марта 2026 года. Переключитесь на /v2/posting/fbs/act/check-status .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Номер задания на формирование документов (также идентификатор перевозки) из метода POST /v2/posting/fbs/act/create .

### Ответы

- 200 Статус формирования накладной
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `id` — integer <int64> Номер задания на формирование документов.
- `status` — string Cтатус формирования документов: FORMING — ещё не готовы, FORMED — сформированы успешно, CONFIRMED — подписаны Ozon, CONFIRMED_WITH_MISMATCH — подписаны Ozon с расхождениями, NOT_FOUND — документы не найдены, UNKNOWN_ERROR — произошла ошибка.

---

## Получить PDF c документами

`POST /v2/posting/fbs/act/get-pdf`

Operation ID: `PostingAPI_PostingFBSGetAct`

С помощью метода можно получить: продацам из России — лист отгрузки и транспортную накладную; продавцам из СНГ — акт и транспортную накладную. Получите список доступных документов для отгрузки в параметре available_actions метода /v1/carriage/get .

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Номер задания на формирование документов (также идентификатор перевозки) из методов /v2/posting/fbs/act/create или /v1/carriage/create .

### Ответы

- 200 Документы
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `file_content` — string <byte> Содержание файла в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:

```json
{
  "content_type": "application/pdf",
  "file_name": "20928233.pdf",
  "file_content": "%PDF-1.4\n%âãÏÓ\n2 0 obj\n<</Length 13528/Filter/FlateDecode>>stream\nxí }[ ¯ä:vÞ{ ÿz\u000e{ { { }\\\\x1d\\\\xde/\\\\xc0{ } } } ]µw\u000fò` 9ój8ö\u0000í$¶\u0003ä燔Dº|\"µo ]ÝØ})D\\\\÷õ­NHßÿº°ðûoºì¿NñÎsïÝå\u001fþõÇÿù\u0011¯\u000be}ǍÑ\u0017©í¤4âòïÿøãïþËåßÂ7dÇ\u0014÷BðþYËÿGðKüýßÿt\u0019þñïýñ۟äå¯ÿÑ?Ùq}\u0011Éø¸ê?añ«Ã?ú¯ªøÕN_\r8®.9\u0013¿þ\u001fáÜ]ò?íÛ\u000fç\u0011½Há/V°ø\"qÄø«O\\G\u000bµþDëõ'έ>ñ|}ëïpÆÅæ#³\u0018?ܣDxM>èT?¹ìÏ8ï͇aÞ×ßüöß.øÃo{û¯¯"
}
```

---

## Получить акт о расхождениях по отгрузке FBS

`POST /v1/carriage/act-discrepancy/pdf`

Operation ID: `CarriageActDiscrepancyPDF`

Акт о расхождениях доступен только для отгрузок в статусе closed и только для продавцов из СНГ.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор отгрузки.

### Ответы

- 200 Акт о расхождениях
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `content` — string <byte> Содержание файла в бинарном виде.
- `name` — string Название файла.
- `type` — string Тип файла.

Пример ответа:

```json
{
  "content": "string",
  "name": "string",
  "type": "string"
}
```

---

## Список актов по отгрузкам

`POST /v2/posting/fbs/act/list`

Operation ID: `PostingAPI_FbsActList`

Возвращает список актов по отгрузкам с возможностью отфильтровать отгрузки по периоду, статусу и типу интеграции.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter` — object Параметры фильтра.
- `limit required` — integer <int64> <= 50 Максимальное количество актов в ответе.

Пример запроса:

```json
{
  "filter": {
    "date_from": "2021-08-04",
    "date_to": "2022-08-04",
    "integration_type": "ozon",
    "status": [
      "delivered"
    ]
  },
  "limit": 50
}
```

### Ответы

- 200 Список актов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — Array of objects Результат запроса.
  - `id` — int <int64> Идентификатор отгрузки.
  - `delivery_method_id` — int <int64> Идентификатор метода доставки.
  - `delivery_method_name` — string Название метода доставки.
  - `integration_type` — string Тип интеграции со службой доставки: ozon — доставка через Ozon логистику. 3pl — доставка внешней службой, продавец регистрирует заказ.
  - `containers_count` — int <int32> Число грузовых мест.
  - `status` — string Статус отгрузки.
  - `departure_date` — string Дата отгрузки.
  - `created_at` — string <date-time> Дата создания записи об отгрузке.
  - `updated_at` — string <date-time> Дата обновления записи об отгрузке.
  - `act_type` — string Тип акта приёма-передачи для FBS продавцов.
  - `is_partial` — boolean Признак частичной перевозки. true , если перевозка частичная. Частичная перевозка значит, что отгрузка была разделена на несколько частей и по каждой из частей формируются отдельные акты.
  - `has_postings_for_next_carriage` — boolean Признак наличия подлежащих отгрузке отправлений, которые не попали в текущую перевозку. true , если такие отправления есть.
  - `partial_num` — integer <int64> Порядковый номер частичной перевозки.
  - `related_docs` — object Информация про акты перевозки.

Пример ответа:

```json
{
  "result": [
    {
      "id": null,
      "delivery_method_id": null,
      "delivery_method_name": "string",
      "integration_type": "string",
      "containers_count": null,
      "status": "string",
      "departure_date": "string",
      "created_at": "2019-08-24T14:15:22Z",
      "updated_at": "2019-08-24T14:15:22Z",
      "act_type": "string",
      "is_partial": true,
      "has_postings_for_next_carriage": true,
      "partial_num": 0,
      "related_docs": {
        "act_of_acceptance": {
          "created_at": "2019-08-24T14:15:22Z",
          "document_status": "string"
        },
        "act_of_mismatch": {
          "created_at": "2019-08-24T14:15:22Z",
          "document_status": "string"
        },
        "act_of_excess": {
          "created_at": "2019-08-24T14:15:22Z",
          "document_status": "string"
        }
      }
    }
  ]
}
```

---

## Получить лист отгрузки по перевозке Deprecated

`POST /v2/posting/fbs/digital/act/get-pdf`

Operation ID: `PostingAPI_PostingFBSGetDigitalAct`

Метод устаревает и будет отключён 22 марта 2026 года. Переключитесь на /v2/posting/fbs/act/get-pdf . Вы можете получить документы, если в ответе метода /v2/posting/fbs/digital/act/check-status был один из статусов: FORMED — перевозка сформирована успешно, CONFIRMED — перевозка подтверждена Ozon, CONFIRMED_WITH_MISMATCH — перевозка принята Ozon с расхождениями.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Номер задания на формирование документов (также идентификатор перевозки) из метода POST /v2/posting/fbs/act/create .
- `doc_type` — any <string> Тип электронного документа: act_of_acceptance — лист отгрузки, act_of_mismatch — акт о расхождениях, act_of_excess — акт об излишках, waybill — транспортная накладная.

Пример запроса:

```json
{
  "id": 900000250859000,
  "doc_type": "act_of_acceptance"
}
```

### Ответы

- 200 Файл с документом
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `file_content` — string <byte> Содержание файла в бинарном виде.
- `file_name` — string Название файла.
- `content_type` — string Тип файла.

Пример ответа:

```json
{
  "content_type": "application/pdf",
  "file_name": "20816409_act_of_mismatch.pdf",
  "file_content": "%PDF-1.4\n%ÓôÌá\n1 0 obj\n<<\n/Creator(Chromium)\n/Producer(PDFsharp 1.50.5147 \\([www.pdfsharp.com|http://www.pdfsharp.com/]\\) \\(Original: Skia/PDF m103\\))\n/CreationDate(D:20230625092529+00'00')\n/ModDate(D:20230625092529+00'00')\n>>\nendobj\n2 0 obj\n<<\n/Type/Page\n/Resources\n<<\n/ProcSet[/PDF/Text/ImageB/ImageC/ImageI]\n/ExtGState\n<<\n/G3 3 0 R\n/G8 8 0 R\n>>\n/XObject\n<<\n/X6 6 0 R\n/X7 7 0 R\n>>\n/Font\n<<\n/F4 4 0 R\n/F5 5 0 R\n>>\n>>\n/MediaBox[0 0 594.96 841.92]\n/Contents 9 0 R\n/StructParents 0\n/Parent 13 0 R\n/Group\n<<\n/CS/DeviceRGB\n/S/Transparency\n>>\n>>\nendobj\n3 0 obj\n<<\n/ca 1\n/BM/Normal\n>>\nendobj\n4 0 obj\n<<\n/Type/Font\n/Subtype/Type0\n/BaseFont/AAAAAA+LiberationSans\n/Encoding/Identity-H\n/DescendantFonts[160 0 R]\n/ToUnicode 161 0 R\n>>\nendobj\n5 0 obj\n<<\n/Type/Font\n/Subtype/Type0\n/BaseFont/BAAAAA+LiberationSans-Bold\n/Encoding/Identity-H\n/DescendantFonts[164 0"
}
```

---

## Статус отгрузки и документов

`POST /v2/posting/fbs/act/check-status`

Operation ID: `PostingAPI_PostingFBSActCheckStatus`

Возвращает статус формирования штрихкода для отгрузки и документов: для продавцов из России — транспортной накладной и листа отгрузки; для продавцов из СНГ — транспортной накладной и акта приёма-передачи.

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `id required` — integer <int64> Номер задания на формирование документов (также идентификатор перевозки) из метода POST /v2/posting/fbs/act/create .

### Ответы

- 200 Статус отгрузки и документов
- 400 Неверный параметр
- 403 Доступ запрещён
- 404 Ответ не найден
- 409 Конфликт запроса
- 500 Внутренняя ошибка сервера

#### Схема ответа (200)

- `result` — object Результат работы метода.
  - `act_type` — string Тип документов.
  - `added_to_act` — Array of strings Массив c номерами отправлений, которые добавлены в перевозку. Эти отправления нужно передать сегодня.
  - `removed_from_act` — Array of strings Массив с номерами отправлений, которые не попали в перевозку. Такие отправления нужно передавать со следующей отгрузкой.
  - `status` — string Статус запроса: in_process — документы формируются, нужно подождать. ready — документы сформированы и готовы для скачивания. error — произошла ошибка при формировании документов, запросите документы повторно. cancelled — создание документов отменено, запросите их повторно. The next postings aren't ready — произошла ошибка, отправления не включены в отгрузку. Подождите некоторое время и проверьте результат запроса. Если ошибка повторяется, обратитесь в службу поддержки.
  - `is_partial` — boolean Признак частичной перевозки. true , если перевозка частичная. Частичная перевозка значит, что отгрузка была разделена на несколько частей.
  - `has_postings_for_next_carriage` — boolean true , если есть отправления, не попавшие в текущую перевозку, но которые нужно отгрузить. Если в ответе вернулось true , подтвердите отгрузку или создайте новый акт через метод /v2/posting/fbs/act/create и проверьте их статус. Повторяйте действия, пока в ответе не вернётся false .
  - `partial_num` — integer <int64> Порядковый номер частичной перевозки.

Пример ответа:

```json
{
  "result": {
    "status": "ready",
    "added_to_act": [
      "76673629-0020-1"
    ],
    "removed_from_act": [
      "87784739-0030-1"
    ],
    "act_type": "ozon_digital",
    "is_partial": false,
    "has_postings_for_next_carriage": false,
    "partial_num": 0
  }
}
```

---

## Разделить отправление с прослеживаемыми товарами

`POST /v1/posting/fbs/traceable/split`

Operation ID: `PostingFbsTraceableSplit`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Заказ разделён

#### Схема ответа (200)

- `postings` — Array of objects Информация об отправлениях.
  - `posting_number` — string Номер отправления.
  - `potential_blr_traceable` — boolean Признак, что товар потенциально прослеживаемый: true — отправление считается прослеживаемым на данный момент. При сборке статус может измениться. false — отправление не прослеживаемое на данный момент или его статус неизвестный.
  - `products` — Array of objects Список товаров в отправлении.

Пример ответа:

```json
{
  "postings": [
    {
      "posting_number": "string",
      "potential_blr_traceable": true,
      "products": [
        {
          "quantity": 0,
          "sku": 0
        }
      ]
    }
  ]
}
```

---

## Получить список незаполненных атрибутов для прослеживаемых товаров

`POST /v1/posting/fbs/product/traceable/attribute`

Operation ID: `PostingFbsProductTraceableAttribute`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `posting_number required` — string Номер отправления.

### Ответы

- 200 Список незаполненных атрибутов

#### Схема ответа (200)

- `products` — Array of objects Список товаров в отправлении.
  - `required_attributes` — Array of strings Обязательные атрибуты.
  - `sku` — integer <int64> Идентификатор товара в системе Ozon — SKU.

Пример ответа:

```json
{
  "products": [
    {
      "required_attributes": [
        "string"
      ],
      "sku": 0
    }
  ]
}
```

---

## Получить статус проверки электронной ТТН на прослеживаемой перевозке FBS

`POST /v1/carriage/ettn/status`

Operation ID: `CarriageEttnStatus`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `carriage_id required` — integer <int64> Идентификатор перевозки.

### Ответы

- 200 Статус проверки электронной ТТН

#### Схема ответа (200)

- `errors` — Array of strings Ошибки проверки электронной ТТН на прослеживаемой отгрузке.
- `status` — string Enum: "NOT_UPLOADED" "PROCESSING" "SUCCESS" "FAILED" Статус проверки электронной ТТН на прослеживаемой отгрузке: NOT_UPLOADED — не загружена; PROCESSING — в процессе проверки; SUCCESS — проверена; FAILED — ошибка.

Пример ответа:

```json
{
  "errors": [
    "string"
  ],
  "status": "NOT_UPLOADED"
}
```

---

## Получить список отправлений в отгрузке

`POST /v1/assembly/carriage/posting/list`

Operation ID: `AssemblyCarriagePostingList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр.
- `limit required` — integer <int64> <= 100 Количество значений на странице.

Пример запроса:

```json
{
  "cursor": "string",
  "filter": {
    "carriage_id": 0,
    "cutoff_from": "2019-08-24T14:15:22Z",
    "cutoff_to": "2019-08-24T14:15:22Z",
    "delivery_method_id": 0
  },
  "limit": 0
}
```

### Ответы

- 200 Список отправлений

#### Схема ответа (200)

- `can_print_mass_label` — boolean true , если можно распечатать этикетки массово.
- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.
- `postings` — Array of objects Список отправлений.

Пример ответа:

```json
{
  "can_print_mass_label": true,
  "cursor": "string",
  "postings": [
    {
      "assembly_code": "string",
      "can_print_label": true,
      "posting_number": "string",
      "products": [
        {
          "offer_id": "string",
          "picture_url": "string",
          "product_name": "string",
          "quantity": 0,
          "sku": 0
        }
      ]
    }
  ]
}
```

---

## Получить список товаров в отгрузке

`POST /v1/assembly/carriage/product/list`

Operation ID: `AssemblyCarriageProductList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр.
- `limit required` — integer <int64> <= 100 Количество значений на странице.

Пример запроса:

```json
{
  "cursor": "string",
  "filter": {
    "carriage_id": 0,
    "cutoff_from": "2019-08-24T14:15:22Z",
    "cutoff_to": "2019-08-24T14:15:22Z",
    "delivery_method_id": 0
  },
  "limit": 0
}
```

### Ответы

- 200 Список товаров

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.
- `products` — Array of objects Список товаров.

Пример ответа:

```json
{
  "cursor": "string",
  "products": [
    {
      "offer_id": "string",
      "picture_url": "string",
      "posting_numbers": [
        "string"
      ],
      "product_name": "string",
      "quantity": 0,
      "sku": 0
    }
  ]
}
```

---

## Получить список отправлений

`POST /v1/assembly/fbs/posting/list`

Operation ID: `AssemblyFbsPostingList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `cursor` — string Указатель для выборки следующих данных.
- `filter required` — object Фильтр.
- `limit required` — integer <int64> <= 1000 Количество значений на странице.
- `sort_dir required` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию, DESC — по убыванию.

Пример запроса:

```json
{
  "filter": {
    "cutoff_from": "2026-03-01T00:00:00Z",
    "cutoff_to": "2026-03-07T23:59:59Z",
    "delivery_method_id": 323123321
  },
  "limit": 50,
  "sort_dir": "ASC",
  "cursor": ""
}
```

### Ответы

- 200 Список отправлений

#### Схема ответа (200)

- `cursor` — string Указатель для выборки следующих данных. Если параметр пустой, данных больше нет.
- `cutoff` — string <date-time> Время, до которого продавцу нужно собрать заказ.
- `postings` — Array of objects Список отправлений.

Пример ответа:

```json
{
  "cutoff": "2026-03-07T10:00:00Z",
  "postings": [
    {
      "posting_number": "789456123-0002-3",
      "assembly_code": "test-assembly-code-12345",
      "products": [
        {
          "sku": 1000123456,
          "offer_id": "test-offer-123456",
          "product_name": "Тестовый товар 1",
          "picture_url": "https://test-example.com/images/product-123456.jpg",
          "quantity": 2
        },
        {
          "sku": 1000123457,
          "offer_id": "test-offer-123457",
          "product_name": "Тестовый товар 2",
          "picture_url": "https://test-example.com/images/product-123457.jpg",
          "quantity": 1
        }
      ]
    },
    {
      "posting_number": "123456789-0001-1",
      "assembly_code": "test-assembly-code-12346",
      "products": [
        {
          "sku": 1000123458,
          "offer_id": "test-offer-123458",
          "product_name": "Тестовый товар 3",
          "picture_url": "https://test-example.com/images/product-123458.jpg",
          "quantity": 3
        }
      ]
    },
    {
      "posting_number": "456123789-0003-5",
      "assembly_code": "test-assembly-code-12347",
      "products": [
        {
          "sku": 1000123459,
          "offer_id": "test-offer-123459",
          "product_name": "Тестовый товар 4",
          "picture_url": "https://test-example.com/images/product-123459.jpg",
          "quantity": 1
        },
        {
          "sku": 1000123460,
          "offer_id": "test-offer-123460",
          "product_name": "Тестовый товар 5",
          "picture_url": "https://test-example.com/images/product-123460.jpg",
          "quantity": 2
        },
        {
          "sku": 1000123461,
          "offer_id": "test-offer-123461",
          "product_name": "Тестовый товар 6",
          "picture_url": "https://test-example.com/images/product-123461.jpg",
          "quantity": 1
        }
      ]
    }
  ],
  "cursor": "test-cursor-next-page-67890"
}
```

---

## Получить список товаров в отправлениях

`POST /v1/assembly/fbs/product/list`

Operation ID: `AssemblyFbsProductList`

### Параметры

- `Client-Id required` — string Идентификатор клиента.
- `Api-Key required` — string API-ключ.

### Тело запроса (application/json)

- `filter required` — object Фильтр.
- `limit required` — integer <int64> <= 1000 Количество значений на странице.
- `offset` — integer <int64> Количество элементов, которое будет пропущено в ответе. Например, если offset = 10 , ответ начнётся с 11 найденного элемента.
- `sort_dir` — string Enum: "ASC" "DESC" Направление сортировки: ASC — по возрастанию, DESC — по убыванию.

Пример запроса:

```json
{
  "filter": {
    "cutoff_from": "2019-08-24T14:15:22Z",
    "cutoff_to": "2019-08-24T14:15:22Z",
    "delivery_method_id": 0
  },
  "limit": 0,
  "offset": 0,
  "sort_dir": "ASC"
}
```

### Ответы

- 200 Список товаров в отправлениях

#### Схема ответа (200)

- `has_next` — boolean Признак, что в ответе вернули не все товары: true — сделайте повторный запрос с другим значением offset , чтобы получить остальные значения; false — ответ содержит все значения.
- `products` — Array of objects Список товаров.
- `products_count` — integer <int32> Количество товаров.

Пример ответа:

```json
{
  "products": [
    {
      "sku": 1000123456,
      "offer_id": "test-offer-123456",
      "product_name": "Тестовый товар 1",
      "picture_url": "https://test-example.com/images/product-123456.jpg",
      "quantity": 15,
      "postings": [
        {
          "posting_number": "789456123-0002-3",
          "quantity": 2
        },
        {
          "posting_number": "123456789-0001-1",
          "quantity": 3
        },
        {
          "posting_number": "456123789-0003-5",
          "quantity": 1
        }
      ]
    },
    {
      "sku": 1000123457,
      "offer_id": "test-offer-123457",
      "product_name": "Тестовый товар 2",
      "picture_url": "https://test-example.com/images/product-123457.jpg",
      "quantity": 8,
      "postings": [
        {
          "posting_number": "321654987-0004-2",
          "quantity": 2
        },
        {
          "posting_number": "987321654-0005-7",
          "quantity": 1
        },
        {
          "posting_number": "789456123-0006-8",
          "quantity": 3
        },
        {
          "posting_number": "123456789-0007-9",
          "quantity": 2
        }
      ]
    },
    {
      "sku": 1000123458,
      "offer_id": "test-offer-123458",
      "product_name": "Тестовый товар 3",
      "picture_url": "https://test-example.com/images/product-123458.jpg",
      "quantity": 23,
      "postings": [
        {
          "posting_number": "456123789-0008-1",
          "quantity": 5
        },
        {
          "posting_number": "321654987-0009-3",
          "quantity": 4
        },
        {
          "posting_number": "987321654-0010-2",
          "quantity": 3
        },
        {
          "posting_number": "789456123-0011-4",
          "quantity": 6
        },
        {
          "posting_number": "123456789-0012-6",
          "quantity": 5
        }
      ]
    },
    {
      "sku": 1000123459,
      "offer_id": "test-offer-123459",
      "product_name": "Тестовый товар 4",
      "picture_url": "https://test-example.com/images/product-123459.jpg",
      "quantity": 5,
      "postings": [
        {
          "posting_number": "456123789-0013-8",
          "quantity": 2
        },
        {
          "posting_number": "321654987-0014-1",
          "quantity": 3
        }
      ]
    },
    {
      "sku": 1000123460,
      "offer_id": "test-offer-123460",
      "product_name": "Тестовый товар 5",
      "picture_url": "https://test-example.com/images/product-123460.jpg",
      "quantity": 12,
      "postings": [
        {
          "posting_number": "987321654-0015-3",
          "quantity": 4
        },
        {
          "posting_number": "789456123-0016-5",
          "quantity": 3
        },
        {
          "posting_number": "123456789-0017-7",
          "quantity": 5
        }
      ]
    }
  ],
  "products_count": 5,
  "has_next": true
}
```

---
