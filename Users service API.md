## Users service API
Все запросы требуют заголовок Authorization: Bearer JWT
### Company
#### POST /companies
##### Body:
```json
{"name": "Kyntora"}
```
##### 201 - Created
```json
{"company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"company_name": "Kyntora",
"created_at": "2025-06-01T12:30:45Z",
"owner_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx"}
```
##### 400 - Bad Request
```json
{"message": "Некорректное название компании."}
```
#### POST /companies/{company_id}/join-requests
- company_id берется из path-параметра;
- user_id берется из токена.
##### 201 - Created
```json
{"join_request_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"user_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"status": "Pending",
"created_at": "2025-06-01T12:30:45Z"}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
##### 409 - Conflict
```json
{"message": "Заявка уже отправлена."}
```
#### GET /companies/{company_id}/join-requests
Только владелец компании.
##### 200 - OK
```json
[
  {
    "join_request_id": "...",
    "user_id": "...",
    "user_first_name": "Eugene",
    "user_last_name": "Tikhonov",
    "status": "Pending",
    "created_at": "..."
  }
]
```
##### 403 - Forbidden
```json
{"message": "Только владелец компании может просматривать заявки."}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
#### PATCH /companies/{company_id}/join-requests/{request_id}
Только владелец компании.
##### Body:
```json
{"status": "ACCEPT"} 
```
##### 200 - OK
```json
{
  "join_request_id": "...",
  "user_id": "...",
  "company_id": "...",
  "status": "ACCEPT",
  "updated_at": "2025-06-01T12:31:00Z"
}
```
##### 400 - Bad Request
```json
{"message": "Некорректный статус."}
```
##### 403 - Forbidden
```json
{"message": "Только владелец компании может одобрять заявки."}
```
##### 404 - Not Found
```json
{"message": "Заявка не найдена."}
```
##### 409 - Conflict
```json
{"message": "Заявка уже обработана."}
```
### Content
#### POST /companies/{company_id}/contents
##### Поддерживаемые форматы файлов: 
###### Аудио:
- mp3, ogg;
- ограничение до 100MB.
###### Видео:
- mp4, mov;
- ограничение до 300MB.
###### Документы:
- pdf, md, doc, docx, txt;
- ограничение до 30MB.
##### form-data
```
file
```
##### 201 - Created
```json
{"content_id":"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"company_id":"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"user_id":"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"original_name":"REST_API_Scheme.md",
"mime_type":"text/markdown",
"size":31457280,
"status":"UPLOADED",
"created_at":"2025-06-01T12:30:45Z"}
```
##### 400 - Bad Request
```json
{"message":"Файл не передан."}
```
##### 403 - Forbidden
```json
{"message":"Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
##### 413 - Payload Too Large
```json
{"message": "Файл превышает максимальный размер для своего типа."}
```
##### 415 - Unsupported Media Type
```json
{"message": "Тип файла не поддерживается."}
```
#### POST /companies/{company_id}/contents/text
##### Body:
```json
{
  "title": "Заметка по проекту",
  "text": "Содержимое документа..."
}
```
##### 201 - Created
```json
{
  "content_id": "...",
  "company_id": "...",
  "user_id": "...",
  "title": "Заметка по проекту",
  "status": "UPLOADED",
  "created_at": "..."
}
```
##### 400 - Bad Request
```json
{"message": "Текст не передан."}
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
##### 413 - Payload Too Large
```json
{"message": "Текст превышает максимальный размер 30 МБ."}
```
#### GET /companies/{company_id}/contents/{content_id}
##### 200 - OK
```json
{
  "content_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "user_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "original_name": "REST_API_Scheme.md",
  "mime_type": "text/markdown",
  "size": 31457280,
  "status": "ACTIVE",
  "created_at": "2025-06-01T12:30:45Z",
  "updated_at": "2025-06-01T12:31:00Z"
}
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found (компания)
```json
{"message": "Компания не найдена."}
```
##### 404 - Not Found (файл)
```json
{"message": "Файл не найден."}
```
#### GET /companies/{company_id}/contents
##### 200 - OK
```json
[
  {
    "content_id": "...",
    "original_name": "REST_API_Scheme.md",
    "mime_type": "text/markdown",
    "size": 31457280,
    "status": "ACTIVE",
    "created_at": "2025-06-01T12:30:45Z"
  },
  {
    "content_id": "...",
    "original_name": "meeting.mp4",
    "mime_type": "video/mp4",
    "size": 104857600,
    "status": "PREPARING",
    "created_at": "2025-06-01T12:31:00Z"
  }
]
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
#### DELETE /companies/{company_id}/contents/{content_id}
В данном методе применяется техника "Soft Delete".
##### 200 - OK
```json
{"content_id":"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"status":"DELETED",
"updated_at":"2025-06-01T12:30:45Z"}
```
##### 403 - Forbidden
```json
{"message":"Пользователь не принадлежит компании."}
```
##### 404 - Not Found (компания)
```json
{"message": "Компания не найдена."}
```
##### 404 - Not Found (файл)
```json
{"message":"Файл не найден."}
```