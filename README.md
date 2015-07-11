# AllyChat API
версия 0.10.2

дата обновления 08/07/2015

# Список ошибок

- `1001` Missing token
- `1002` Invalid or expired token
- `1011` Missing auth_token
- `1012` Invalid or expired auth_token
- `1020` Unknown app_id

# REST API
---

## Авторизация
---

### Авторизация администратора или оператора поддержки

```
curl -X POST -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'login=admin&password=admin' http://sense-dev.achat.octoberry.net/api/auth
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzUxNDE3MywiaWF0IjoxNDMzNTEwNTczfQ.eyJ1c2VyX2lkIjoiNTU0MDljOGQyMzk3MjhkYmU0ZDRlYThiIn0.2EOlumjwRu5QkM96L906ZQ2EqGQ-mCMFvTdZxgqW3Ds"
}
```

### Авторизация клиента

Перед получением токена нужно зарегистрировать нового клиента (см. [Регистрация нового клиента](#user_register))

Для получения сессионного токена нужно передать `alias` клиента

```
curl -X POST -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'alias=test&app_id=sensetest' http://sense-dev.achat.octoberry.net/api/token
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzUxNTMxMSwiaWF0IjoxNDMzNTExNzExfQ.eyJ1c2VyX2lkIjoiNTU0Yzc5ZjQyMzk3MjgxM2E1ZTkxMjM0In0.U-2JT4jWnAe-HcCY6qRBMvPYHn7MBaw4ihcXw-iCUsA"
}
```

### Обновление токена

```
curl -X POST -H "Content-Type: application/json" -H "Cache-Control: no-cache" 'http://127.0.0.1:8080/api/token/update?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTQ0NCwiaWF0IjoxNDM2Mjc3ODQ0fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.7APan4sBB08xAZ9s9aaHLizuIp827Wdz3Ap5-4uvm9w'
```

```
{
"token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTU2NiwiaWF0IjoxNDM2Mjc3OTY2fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.OlPrIck_YrzKBS5kG8b6thnTHtG61koEuxHBm0BBGJM"
}
```

## Пользователи
---

### <a name="user_register"></a>Регистрация нового клиента

```
curl -X POST /api/user/register
```

### Список пользователей

```
curl -X GET /api/users
```

Возможные фильтры:

- `alias` поиск по алиасу
- `alias[]` поиск по нескольким алиасам
- `id[]` поиск по нескольким `id`

### Поиск пользователя по alias

```
curl -X GET -H "Cache-Control: no-cache" http://sense-dev.achat.octoberry.net/api/users?alias=AFAMJ7&token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzc3MjE5OSwiaWF0IjoxNDMzNzY4NTk5fQ.eyJ1c2VyX2lkIjoiNTU0MDljOGQyMzk3MjhkYmU0ZDRlYThiIn0.ZEKG06uTm-ObHF7u8F_emvgtQ_bVHZ5yv93mS1Htyok
```

```
{
    "users": [
        {
        "alias": "AFAMJ7",
        "id": "554c79f423972813a5e91234",
        }
    ]
}
```

### Поиск пользователя по alias пачкой

```
curl -X GET -H "Cache-Control: no-cache" http://sense-dev.achat.octoberry.net/api/users?alias[]=AFAMJ7&alias[]=AGC1O4&token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzg3MzA5OSwiaWF0IjoxNDMzODY5NDk5fQ.eyJ1c2VyX2lkIjoiNTU0Yzc5ZjQyMzk3MjgxM2E1ZTkxMjM0In0.60BxbS6cryRJpGy7LQ6W896X22UrBY5zm1N1rZM4WwI
```

```
{
    "users": [
        {
            "alias": "AFAMJ7",
            "avatar_url": "https://scontent.xx.fbcdn.net/hphotos-xpa1/v/t1.0-9/10177358_10152090522570899_1531763069910757194_n.jpg?oh=295d97d7a7e61c72ec7f66615f532333&oe=55F29D9B",
            "id": "554c79f423972813a5e91234"
        },
        {
            "alias": "AGC1O4",
            "avatar_url": "https://fbcdn-profile-a.akamaihd.net/hprofile-ak-ash2/v/t1.0-1/574679_570350889704200_903096688_n.jpg?oh=3d550ea461ccf3da9115cb506bc4882a",
            "id": "554a2168239728a0fc6c2f21"
        }
    ]
}
```

### Данные пользователя

```
curl -X GET /api/user/([^/]+)
```

### Обновление данных пользователя

```
curl -X POST -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'avatar_url=http%3A%2F%2Foctoberry.ru%2Fimg%2Flogo.png' http://127.0.0.1:8080/api/user/5565a9be659d5530dab7f588?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzc3NTQ3NywiaWF0IjoxNDMzNzcxODc3fQ.eyJ1c2VyX2lkIjoiNTU2NWE5YmU2NTlkNTUzMGRhYjdmNTg4In0.Xx5eODUsFO0JATVM6foZSn-TA3rq4TsH2vN5UDM6CVM
```

### Комнаты, доступные для пользователя

```
curl -X GET -H "Cache-Control: no-cache" http://127.0.0.1:8080/api/user/5565a9be659d5530dab7f588/rooms?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzg2OTQ4MiwiaWF0IjoxNDMzODY1ODgyfQ.eyJ1c2VyX2lkIjoiNTU2NWE5YmU2NTlkNTUzMGRhYjdmNTg4In0.tJMsriOOHVEwR4-ENOyYuiJRi8BQYSUhRgFOcFjGwyU
```

```
{
    "rooms": [
        {
            "status": 0,
            "last_issue": null,
            "first_message": null,
            "users": [
                {
                    "id": "5565a9be659d5530dab7f588"
                }
            ],
            "tags": [],
            "last_message": null,
            "id": "5565a9be659d5530dab7f589",
            "is_support": true
        },
        {
            "status": 0,
            "last_issue": null,
            "first_message": null,
            "users": [
                {
                    "id": "5565a9be659d5530dab7f588"
                },
                {
                    "id": "55507814b77ffcb9cc9e0d5c"
                }
            ],
            "tags": [],
            "last_message": {
                "room": "5565bfdf659d5544aed2714a",
                "created_at": 1432821881.148508,
                "file": null,
                "message": "Привет, приложение обновилось! Список изменений тут - https://goo.gl/ablcYh. Пользуйтесь и давайте рекомендации. Поддержка в чате работает в тестовом режиме,поэтому заранее приносим прощения за долгий ответ. С любовью команда Sense.",
                "issue": null,
                "id": "55672079659d55ca3fe3ebd4",
                "sender": "552fe60a3cf39d1fb26afe0d"
            },
            "id": "5565bfdf659d5544aed2714a",
            "is_support": false,
            "last_read_message_id": "55672079659d55ca3fe3ebd4"
        }
    ]
}
```

### Идентификаторы последних прочитанных сообщений пользователя

```
curl -X GET -H "Cache-Control: no-cache" http://127.0.0.1:8080/api/user/5565a9be659d5530dab7f588/user_rooms?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzg2MTg2NCwiaWF0IjoxNDMzODU4MjY0fQ.eyJ1c2VyX2lkIjoiNTU2NWE5YmU2NTlkNTUzMGRhYjdmNTg4In0.LVmdjPMntJ_pKQlohHpeP8CrNyusrKjAQXoGrToBDJc
```

```
{
    "user_rooms": [
        {
          "user_id": "5565a9be659d5530dab7f588",
          "room_id": "5565bfdf659d5544aed2714a",
          "id": "5576f168659d5569f811d928",
          "last_read_message_id": "55672079659d55ca3fe3ebd4"
        }
    ]
}
```

### Список активных комнат пользователя

Список комнат, обозначенных как активные для администатора или оператора. Комната может быть активной только у одного оператора или админситратора.

```
curl -X GET /api/user/([^/]+)/active_rooms
```

### Текущий пользователь

```
curl -X GET /api/me
```

## Комнаты
---

### Данные комнаты

```
curl -X GET /api/room/([^/]+
```

### Сделать комнату активной

Администратор или оператор может сделать комнату активной. Комната может быть активной только у одного оператора или админситратора. При попытке сделать активной комнату, которая уже активная для другого пользователя, сервис вернет ошибку.

```
curl -X PUT /api/room/([^/]+)/active
```

```
curl -X DELETE /api/room/([^/]+)/active
```

### Список сообщений для комнаты

```
curl -X GET /api/room/([^/]+/messages
```

### Теги комнаты

Администратор может задать тег для комнаты

```
curl -X POST -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'tag=test' /api/room/([^/]+/tags
```

```
curl -X DELETE -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'tag=test' /api/room/([^/]+)/tag
```

## Загрузка изображений
---

```
curl -X POST /api/upload
```

## Сообщения
---

### Список сообщений

Сообщения по id

```
curl -X GET /api/messages?id[]=1&id[]=2&id[]=3
```

### Данные сообщения

```
curl -X GET /api/message/([^/]+)
```

### Сообщение прочитано

```
curl -X PUT /api/message/([^/]+)/read
```

## Обращения
---

### Данные обращения

```
curl -X GET -H "Cache-Control: no-cache" http://127.0.0.1:8080/api/issue/557045ca659d55578a86ecdc?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzMzk0NjI1OCwiaWF0IjoxNDMzOTQyNjU4fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.Vl-Ldmxf5KpI_yExCfsNMjNTrGAkWJQSl2LSr-JkAJE
```

```
{
    "last_message": {
        "room": "55507814b77ffcb9cc9e0d5d",
        "created_at": 1433422432.770775,
        "file": null,
        "message": "мобильный",
        "issue": "557045ca659d55578a86ecdc",
        "id": "55704a60659d55578a934eec",
        "sender": "55507814b77ffcb9cc9e0d5c"
    },
    "first_message": {
        "room": "55507814b77ffcb9cc9e0d5d",
        "created_at": 1433421258.135883,
        "file": null,
        "message": "ЕПД",
        "issue": "557045ca659d55578a86ecdc",
        "id": "557045ca659d55578a86ecdd",
        "sender": "55507814b77ffcb9cc9e0d5c"
    },
    "is_open": true,
    "room": "55507814b77ffcb9cc9e0d5d",
    "tags": [],
    "answered_at": null,
    "closed_at": null,
    "created_at": 1433421258.138776,
    "id": "557045ca659d55578a86ecdc",
    "closed_by": null
}
```

### Ответы на обращения

Система пытается найти в истории сообщений подходящий ответ

```
curl -X GET /api/issue/([^/]+)/answers
```

### Статистика обращений по дням

```
curl -X GET /api/issues/stats
```

Возможные фильтры:

- `tag` тег, например `concierge`
- `closed_by` id оператора или админа, который закрыл обращение
- `is_open` флаг, открыто `1` или закрыто `0` обращение

## Системные 
---

### Текущее время на сервере

```
curl -X GET /api/time
```

### Список операторов и админов

```
curl -X GET -H "Cache-Control: no-cache" http://127.0.0.1:8080/api/operators?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNDAxODk3MSwiaWF0IjoxNDM0MDE1MzcxfQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.IRRYD0M3_nUEFiI8QXsLz2bKddo6KmIw75VbA9B4IOo
```

```
{
    "operators": [
        {
            "geolocation": null,
            "is_admin": true,
            "device_type": null,
            "id": "5551c9dc8f93458b46601417",
            "tzoffset": null,
            "is_operator": false,
            "os_version": null,
            "alias": null,
            "avatar_url": null,
            "login": "admin2",
            "app_version": null
        },
        {
            "geolocation": null,
            "is_admin": true,
            "device_type": null,
            "id": "552fe60a3cf39d1fb26afe0d",
            "tzoffset": null,
            "is_operator": false,
            "os_version": null,
            "alias": null,
            "avatar_url": null,
            "login": "admin",
            "app_version": null
        },
        {
            "geolocation": null,
            "is_admin": false,
            "device_type": null,
            "id": "5559fbcc724b0d1d0228976a",
            "tzoffset": null,
            "is_operator": true,
            "os_version": null,
            "alias": null,
            "avatar_url": null,
            "login": "laya",
            "app_version": null
        }
    ]
}
```

### Просмотр данных оператора

```
curl -X GET 'http://127.0.0.1:8080/api/operator/559be7d3659d55854a438214?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTQ0NCwiaWF0IjoxNDM2Mjc3ODQ0fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.7APan4sBB08xAZ9s9aaHLizuIp827Wdz3Ap5-4uvm9w'
```

### Создание нового оператора

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Cache-Control: no-cache" -d 'login=test&password=test' 'http://127.0.0.1:8080/api/operator?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTQ0NCwiaWF0IjoxNDM2Mjc3ODQ0fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.7APan4sBB08xAZ9s9aaHLizuIp827Wdz3Ap5-4uvm9w'
```

### Редактирование данных оператора

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'login=test&password=test' 'http://127.0.0.1:8080/api/operator/559be7d3659d55854a438214?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTQ0NCwiaWF0IjoxNDM2Mjc3ODQ0fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.7APan4sBB08xAZ9s9aaHLizuIp827Wdz3Ap5-4uvm9w'
```

### Удаление оператора

```
curl -X DELETE 'http://127.0.0.1:8080/api/operator/559be7d3659d55854a438214?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNjI4MTQ0NCwiaWF0IjoxNDM2Mjc3ODQ0fQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.7APan4sBB08xAZ9s9aaHLizuIp827Wdz3Ap5-4uvm9w'
```

### Список тегов

```
curl -X GET -H "Cache-Control: no-cache" http://127.0.0.1:8080/api/tags?token=eyJhbGciOiJIUzI1NiIsImV4cCI6MTQzNDAxODk3MSwiaWF0IjoxNDM0MDE1MzcxfQ.eyJ1c2VyX2lkIjoiNTUyZmU2MGEzY2YzOWQxZmIyNmFmZTBkIn0.IRRYD0M3_nUEFiI8QXsLz2bKddo6KmIw75VbA9B4IOo
```

```
{
    "tags": [
        {
            "tag": "concierge"
        }
    ]
}
```

# WebSocket API
---

## Client -> Server
---

### Send message

```
{
"type": “message”,
"content": {
         "room": "55409c8d239728dbe4d4ea8b", 
         "message": "Hello Friend!",
         "file": "http://host/1.jpg"
        }
}
```

### Send read status

```
{
"type": “read”,
"content": {
         "room": "55409c8d239728dbe4d4ea8b",
         "message": "55409c8d239728dbe4d4ea22"
        }
}
```

### User data

```
{
"type": “user_data”, 
"content": {
         "alias": “1234567890”,
         "parse_id": “AGC1O4”
        }
}
```

## Server -> Client
---

```
{
"type": “message”, 
"content": {
         "room": “55409c8d239728dbe4d4ea8b”,
         "sender": “55409c8d239728dbe4d4ea8b”,
         "message": "Hello Friend!",
         "file": "http://host/1.jpg"
        }
}
```

### Банковская часть API

## Получение alias пользователя по номеру телефона

```
curl -X GET -u admin:PKElT2jRDIWcqHIV http://testjmb.alfabank.ru/AlfaSense/support/phone/9154983795/alias
```

```
{"alias": "AFAMJ7"}
```

## Получение имени и телефона по alias

```
curl -X GET -u admin:PKElT2jRDIWcqHIV http://testjmb.alfabank.ru/AlfaSense/support/alias/AFAMJ7/customer
```

```
{"phone": "79154983795", "fio": "\u041f\u0430\u0447\u0430\u0439 \u0410\u043d\u0434\u0440\u0435\u0439 \u0412\u0430\u0434\u0438\u043c\u043e\u0432\u0438\u0447             "}
```
