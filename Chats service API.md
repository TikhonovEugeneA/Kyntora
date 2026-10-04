### Chats service API
- Общение с ИИ-моделью. ИИ отвечает на основе документов, загруженных в компанию (через KAG).
- Все запросы требуют заголовок Authorization: Bearer JWT.
#### GET /companies/{company_id}/chats
Список диалогов пользователя с ИИ в рамках компании.
##### 200 - OK
```json
[
  {
    "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "user_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "title": "Обсуждение отчёта",
    "created_at": "2025-06-01T12:30:45Z"
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
#### POST /companies/{company_id}/chats
Начать новый диалог с ИИ в компании.
##### Body:
```json
{"title": "Обсуждение отчёта"}
```
##### 201 - Created
```json
{
  "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "user_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "title": "Обсуждение отчёта",
  "created_at": "2025-06-01T12:30:45Z"
}
```
##### 400 - Bad Request
```json
{"message": "Некорректное название чата."}
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Компания не найдена."}
```
#### GET /chats/{chat_id}
Информация о диалоге.
##### 200 - OK
```json
{
  "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "company_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "user_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "title": "Обсуждение отчёта",
  "created_at": "2025-06-01T12:30:45Z"
}
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Чат не найден."}
```
#### GET /chats/{chat_id}/messages
История сообщений диалога.
##### 200 - OK
```json
[
  {
    "message_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "role": "USER",
    "content": "Что в отчёте про Kyntora?",
    "created_at": "2025-06-01T12:30:45Z"
  },
  {
    "message_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
    "role": "AI",
    "content": "В отчёте указано, что...",
    "created_at": "2025-06-01T12:30:50Z"
  }
]
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Чат не найден."}
```
#### POST /chats/{chat_id}/messages
Задать вопрос ИИ. Ответ приходит асинхронно —
через WebSocket или polling GET /chats/{chat_id}/messages.
##### Body:
```json
{"content": "Что в отчёте про Kyntora?"}
```
##### 202 - Accepted
```json
{
  "message_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "chat_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
  "role": "USER",
  "content": "Что в отчёте про Kyntora?",
  "created_at": "2025-06-01T12:30:45Z"
}
```
##### 400 - Bad Request
```json
{"message": "Сообщение не может быть пустым."}
```
##### 403 - Forbidden
```json
{"message": "Пользователь не принадлежит компании."}
```
##### 404 - Not Found
```json
{"message": "Чат не найден."}
```
#### Общие коды ответа
##### 401 - Unauthorized
{"message": "Неавторизованный пользователь."}