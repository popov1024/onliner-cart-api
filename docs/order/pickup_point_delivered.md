## Подтвердить доставку заказа в пункт выдачи

### POST /orders/{orderKey}/pickup-point-delivered

Подтверждение доставки заказа в пункт выдачи возможно только при выполнении всех условий:
 - Способ доставки указан `pickup_point`
 - Заказ находится в статусе `shipping` (Передан в доставку)
 - Подтверждение доставки производится только 1 раз

### Параметры запроса

|Параметр|Тип|Описание|
|---|---|---|
|comment|string|(optional) Комментарий для покупателя (максимум 255 символов)|

### Пример запроса

```http
POST /orders/{orderKey}/pickup-point-delivered
Authorization: Bearer <token>
Accept: application/json; charset=utf-8
Content-Type: application/json; charset=utf-8
```
```json
{
    "comment": "Ваш заказ приехал в пункт выдачи. Для получения необходим паспорт."
}
```

### Пример ответа
```http
HTTP/2 204 No Content
```

### Ответ при попытке изменить несуществующий заказ

```http
HTTP/2 404 Not Found
```
```json
{
    "message": "Заказ не найден"
}
```

### Ответ при обращении к заказу в невалидном состоянии для подтверждения доставки в пункт выдачи

```http
HTTP/2 409 Conflict
Content-Type: application/json; charset=utf-8
```
```json
{
    "message": "Недопустимый статус заказа"
}
```

#### Возможные ошибки
|Причина|Ошибка|
|---|---|
|Статус заказа не "Передан в доставку"|Недопустимый статус заказа|
|Способ доставки заказа не "Доставка в ПВЗ"|Недопустимый способ доставки|
|Уже есть отметка о том, что заказ доставлен в ПВЗ|Отметка о поступлении заказа в пункт выдачи уже есть|

### Пример ответа при возникновении ошибок валидации

```http
HTTP/2 422 Unprocessable Entity
Content-Type: application/json; charset=utf-8
```
```json
{
    "message": "Validation failed",
    "errors": {
        "comment": [
            "Максимум 255 символов"
        ]
    }
}
```

### Возможные ошибки для полей:

|Параметр|Ошибки|
|---|---|
|comment|Значение поля должно быть строкой<br>Максимум 255 символов|