# AllyChat API
версия 0.43.0

дата обновления 09/12/2016

# Список ошибок

- `1001` Missing token
- `1002` Invalid or expired token
- `1011` Missing auth_token
- `1012` Invalid or expired auth_token
- `1020` Unknown app_id
- `1030` Missing alias
- `1031` Can not register user
- `1032` Wrong alias, you should register user first
- `1040` Missing device_id
- `1050` Missing device_platform
- `1060` Missing device_token
- `1070` Unsupported tag
- `1080` Unsupported room status
- `1100` Unsupported Content-Type
- `1110` Invalid id
- `1120` Missing current_password
- `1130` Sender not found
- `1140` ADMIN_ID incorrect or not set
- `1150` Email is not allowed for this instance

# Общие правила

## Alias

Для идентификации клиентов SDK передает на сервер `alias`. Приложение может передать в это поле свой внутренний идентификатор клиента, чтобы операторы смогли идентифицировать клиента.
 
## Правила генерации alias если он не задан 

Приложоение может не задавать `alias`. В этом случае SDK генерирует по следующим правилам:

`allychat_anonym_client_<uid>`, где `<uid>` - уникальный идентификатор, состоящий из 32 двух символов, латинские буквы и цифры.

Регистр не важен.

Например: `allychat_anonym_client_430886438DC14931A3E75A4F36AB1D42`

# REST API
---

GET методы возвращают JSON, который содержит запрашиваемый объект, либо коллекцию объектов с указанием общего количества объектов в поле `count` без учета пагинации.

## Объекты
---

\* - поля, обязательные к заполнению. Остальные могут принимать null.

### <a name="user"></a>User

1. **id***
2. **alias*** - идентификатор клиента
3. name - имя
4. app_version - версия приложения
5. os_version - версия ОСи
6. **device_type*** - тип устройства (os/android)
7. geolocation - геолокация (долгота, широта)
8. tzoffset - смещение времени относительно UTC в секундах
9. avatar_url - URL аватара
10. is_anonym - клиент, для которого `alias` не был задан явно, а был сгенерирован в SDK

### Room

0. **id***
1. first_message - идентификатор исторически первого сообщения в комнате
2. **users*** - список идентификаторов объекта `user`, участников этой комнаты, без учета операторов
3. first_message_id - идентификатор `id` исторически первого сообщения `message` в комнате
3. last_message - исторически последнее сообщение `message` в комнате
4. active_operators - идентификаторы объекта `user` операторов, подключенных к комнате
5. **is_support*** - `true` для комнаты поддержки, иначе `false`
6. last_read_message_id - идентификатор сообщения `message` прочитанное текущим клиентом, отображается исторически последнее такое сообщение

### Message

0. **id***
1. **read*** - `true` если сообщение прочитано текущим клиентов, иначе `false`
2. client_id - клиентский идентификатор, если задан
3. message - текст сообщения
4. event - событие (например: `review_requested`)
5. room - `id` объекта `room`
6. sender - отправитель `operator`
1. file - URL файла
8. **created_at*** - время создания сообщения в секундах (например: `1445943734.019197`)
9. **created_at_millis*** - время создания сообщения в миллисекундах (например: `1445943734019`)

### Operator

0. **id*** - `id` объекта `user`
1. alias - `alias` объекта `user`
2. avatar_url - `avatar_url` объекта `user`
3. name - `name` объекта `user`

### Device

0. **id***
1. **app_id*** - `app_id`
2. **user_id*** - `id` объекта `user`
3. **device_id*** - заданый SDK идентификатор устройства
4. **device_platform*** - `ios` для iOS, `android` для Android
5. **device_token*** - PUSH токен

### File

1. **file*** - URL файла

## Авторизация
---

Большинство методов REST API требуют авторизацию. Для доступа к ним нужно передать сессионный токен в заголовке.

Пример:

```
curl -X POST -H "Content-Type: application/json" -H "Authorization: Token eyJhbGciOiJIUzI1NiIsImV4cCI6MTQ0NjYyMzkwNywiaWF0IjoxNDQ2NjIwMzA3fQ.eyJ1c2VyX2lkIjoiNTYzMjFkODFkNDhmNTRmNmFkMzZmZmZhIn0.n9zA2I8tU-khX7g7AAT_mjBi-j0IwPYK0ForFa0smF8" https://my.allychat.ru/api/token/update
```

### Получение сессионного токена для администратора или оператора поддержки

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'login=admin&password=admin' 'https://my.allychat.ru/api/auth'
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzUxNDE3MywiaWF0IjoxNDMzNTEwNTczfQ.eyJ1c2VyX2lkIjoiNTU0MDljOGQyMzk3MjhkYmU0ZDRlYThiIn0.2EOlumjwRu5QkM96L906ZQ2EqGQ-mCMFvTdZxgqW3Ds"
}
```

### Получение сессионного токена для клиента

Перед получением токена нужно зарегистрировать нового клиента (см. [Регистрация нового клиента](#user_register))

Для получения сессионного токена нужно передать `alias` клиента и `app_id`

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'alias=test&app_id=app' https://my.allychat.ru/api/2/token
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzUxNTMxMSwiaWF0IjoxNDMzNTExNzExfQ.eyJ1c2VyX2lkIjoiNTU0Yzc5ZjQyMzk3MjgxM2E1ZTkxMjM0In0.U-2JT4jWnAe-HcCY6qRBMvPYHn7MBaw4ihcXw-iCUsA"
}
```

### Обновление токена

```
curl -X POST -H "Content-Type: application/json" -H "Authorization: Token eyJhbGciOiJIUzI1NiIsImV4cCI6MTQ0NjYyMzkwNywiaWF0IjoxNDQ2NjIwMzA3fQ.eyJ1c2VyX2lkIjoiNTYzMjFkODFkNDhmNTRmNmFkMzZmZmZhIn0.n9zA2I8tU-khX7g7AAT_mjBi-j0IwPYK0ForFa0smF8" https://my.allychat.ru/api/token/update
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTU2NiwiaWF0IjoxNDM2Mjc3OTY2fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.OlPrIck_YrzKBS5kG8b6thnTHtG61koEuxHBm0BBGJM"
}
```

## OAuth2 
---

Поддержка авторизации через OAuth2 токен.

Пример:

```
GET https://my.allychat.ru/api/me
Authorization: Bearer ad0ace58-0d3e-3cb9-8fa7-5f0126895a1f 
```

### Получение OAuth2 токена для клиента

Для получения OAuth2 токена нужно передать `code` выданный сервером для кросс-авторизации и `app_id`

```
POST https://my.allychat.ru/api/oauth2/token
Content-Type: application/x-www-form-urlencoded

code=ecf397d7-6aa7-3bf3-9e71-6eb5c7e62bce&app_id=app
```

```
{
  "token": "490742ee-a8ee-3bea-bbb0-6d36e7aebc27",
  "refresh_token": "aad66e40-ee56-32b4-83fc-1215828a383c"
}
```

### Обновление OAuth2 токена

```
POST https://my.allychat.ru/api/oauth2/token/refresh
Content-Type: application/x-www-form-urlencoded

refresh_token=aad66e40-ee56-32b4-83fc-1215828a383c
```

```
{
  "access_token": "379667a8-07fe-3630-8e68-90cb81292b57",
  "refresh_token": "495b8471-74d0-35ff-86e9-8b193a5dec81"
}
```

## Пользователи
---

### <a name="user_register"></a>Регистрация нового клиента

```
POST /api/2/user/register
```

### Текущий пользователь

```
GET /api/2/me
```

### Список пользователей

```
GET /api/2/users
```

Возможные фильтры:

- `alias` поиск по алиасу
- `alias[]` поиск по нескольким алиасам
- `id[]` поиск по нескольким `id`

### Данные пользователя

```
GET /api/2/user/<id>
```

В качестве id можно передать:

- идентификатор `id` объекта `user`
- `alias`

### Обновление данных пользователя

```
POST /api/2/user/<id>
```

В качестве id можно передать:

- идентификатор `id` объекта `user`
- `alias`

## Комнаты
---

### Комнаты, доступные для пользователя

```
GET /api/2/rooms
```

### Комната поддержки

```
GET /api/2/room/support
```

### Создание новой комнаты

```
POST /api/2/rooms
```

user = \<alias\>

alias клиента, с которым нужно сделать новую комнату

## Устройства (для PUSH уведомлений)
---

### Регистрация нового устройства для отправки PUSH уведомлений

```
POST /api/2/devices
```

### Удаление устройства, отказ от PUSH уведомлений

```
DELETE /api/2/devices
```

## Загрузка изображений
---

```
POST /api/2/upload
```

Бинарный файл в поле `file`. Будет возвращен объект `file`.

## Сообщения
---

### Список сообщений

```
GET /api/2/room/<id>/messages
```

В качестве id можно передать:

- `id` объекта `room`
- 'support', в этом случае вернется комната поддержки

Возможные фильтры:

- `unread`, показать только непрочитанные сообщения

Пагинация:

- `limit`, лимит количества объектов в результате
- `offset`, смещение

### Отправка сообщения

```
POST /api/2/room/<id>/messages
```

В качестве id можно передать:

- `id` объекта `room`
- 'support', в этом случае вернется комната поддержки

### Данные сообщения

```
GET /api/2/message/<id>
```

### Сообщение прочитано

```
PUT /api/2/message/<id>/read
```

## Системные 
---

### Текущее время на сервере

```
GET /api/2/time
```

### Список операторов и админов

```
GET /api/2/operators
```

### Просмотр данных оператора

```
curl /api/2/operator/<id>
```

### Список тегов

```
GET /api/2/tags
```

# WebSocket API
---

```
/api/2/relay
```

## Client -> Server
---

### Send message

```
{
"type": “message”,
"content": {
         "room": "55409c8d239728dbe4d4ea8b", 
         "message": "Hello Friend!",
         "file": "http://host/1.jpg",
         "client_id": "1234567890"
        }
}
```

## Server -> Client
---

```
{
"type": “message”, 
"content": {
        "file": null,
        "created_at_long": 1445943734019197,
        "read": true,
        "client_id": null,
        "message": "yo",
        "event": null,
        "room": "5614fee3659d55f84720a105",
        "sender": {
            "alias": null,
            "avatar_url": "",
            "id": "552fe60a3cf39d1fb26afe0d",
            "name": "Админ"
        },
        "created_at": 1445943734.019197,
        "id": "562f59b6659d553d5dedf1ce",
        "is_hidden": false,
        "issue": "562f59b4659d553d5dedf1cc"
       }
}
```

# Webhooks

## Авторизация

При каждом запросе методов webhook нужно передать заголовок `X-Magneta-Signature` содержащий подпись запроса, вычесленную по технологии `sha256`.

Пример генерации подписи на python:

```
import hashlib
from itsdangerous import HMACAlgorithm, base64_encode
def sign(self, value):
    """Returns the signature for the given value"""
    key = self.settings['WEBHOOK_SECRET']
    sig = HMACAlgorithm(hashlib.sha256).get_signature(key, value)
    return base64_encode(sig)
```

Подпись вычисляется из тела запроса.

## Создание или обновление данных клиента

```
POST /hook/user_register
```

```
{
"alias": "<alias>",
"email": "<email>"
}
```

- alias* - идентификатор клиента в системе поддержки,
- email - email клиента

\* - обязательное к заполнению поле


## Отправка нового сообщения клиенту

```
POST /hook/new_message_to_client
```

```
{
"alias": "<alias>",
"message": "<message>",
"sender": "<sender>"
}
```

- alias* - идентификатор клиента в системе поддержки,
- message - текст сообщения
- sender - `login` отправителя

\* - обязательное к заполнению поле

## Отправка информации новом письме от клиента

```
POST /hook/new_email_from_client
```

```
{
"alias": "<alias>",
"subject": "<subject>",
"message": "<message>",
"attachments": ["<attachment1>", "<attachment2>"]
}
```

- alias* - идентификатор клиента в системе поддержки,
- subject - заголовок письма
- message - текст письма
- attachment1, attachment2 - URL на вложения, файлы вложений должны быть предварительно загружены на сервер и доступны для скачивания по URL

\* - обязательное к заполнению поле

## Отправка информации о новом звонке

```
POST /hook/new_call_from_client
```

```
{
"alias": "<alias>",
"message": "<message>",
"record": "<record>"
}
```

- alias* - идентификатор клиента в системе поддержки,
- message - сопроводительное сообщение, например: Продолжительность 1 минута 6 секунд,
- record - URL на загруженную на сервер запись

\* - обязательное к заполнению поле
