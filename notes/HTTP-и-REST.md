# HTTP, HTTPS и разбор запросов

Ответы на задание из файла `комп.docx`.

## 1. Определения

**HTTP (HyperText Transfer Protocol)** — протокол прикладного уровня для обмена данными между клиентом и сервером. Клиент отправляет запрос, сервер отвечает статусом и телом. Используется для веб-страниц, API и загрузки файлов.

**HTTPS** — HTTP поверх TLS. Данные шифруются, сервер обычно подтверждает сертификатом. В отличие от HTTP, трафик сложнее перехватить и подменить.

**Клиент-серверная модель** — одна сторона (клиент) инициирует запрос, другая (сервер) обрабатывает его и отдаёт ответ. Клиент не ждёт запросов от сервера в классической схеме.

**Идемпотентность** — повтор одного и того же запроса не меняет результат на сервере сверх первого успешного выполнения. Примеры: `GET`, `PUT`, `DELETE`. `POST` обычно не идемпотентен.

## 2.1. Разбор запроса

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Length: 58

{"name": "Иван", "email": "ivan@mail.com"}
```

| Вопрос | Ответ |
| --- | --- |
| Метод | `POST` |
| Путь (endpoint) | `/api/users` |
| Версия HTTP | `1.1` |
| Content-Type | тело в формате JSON (`application/json`) |
| Authorization | доступ по токену Bearer (JWT) |
| Тело запроса | JSON с полями `name` и `email` |

## 2.2. Разбор ответа

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/42

{"id": 42, "name": "Иван", "email": "ivan@mail.com"}
```

| Вопрос | Ответ |
| --- | --- |
| Статус 201 | ресурс успешно создан |
| Location | URI нового ресурса: `/api/users/42` |
| Тело ответа | созданный пользователь с `id`, `name`, `email` |

## 2.3. Собранные запросы

Получить список товаров:

```http
GET /api/products HTTP/1.1
Host: example.com
```

Создать товар:

```http
POST /api/products HTTP/1.1
Host: example.com
Content-Type: application/json

{"name": "Ноутбук", "price": 50000}
```

Удалить товар с id=7:

```http
DELETE /api/products/7 HTTP/1.1
Host: example.com
```

Обновить цену товара с id=3:

```http
PUT /api/products/3 HTTP/1.1
Host: example.com
Content-Type: application/json

{"price": 45000}
```

(Допустимо и `PATCH /api/products/3` только с полем `price`.)
