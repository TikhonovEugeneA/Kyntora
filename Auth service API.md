### Auth service API
- Эндпоинты регистрации и входа не требуют заголовка Authorization - они сами выдают JWT, который затем используется в других сервисах.
#### POST /auth/sign-up
##### Body 
```json
{"first_name":"Eugene",
"last_name":"Tikhonov",
"login":"EugeneTikhonov123",
"password":"Ab~12!"}
```
##### 201 - Created
```json
{"user_id":"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx",
"first_name":"Eugene",
"last_name":"Tikhonov",
"login":"EugeneTikhonov123",
"created_at":"2025-06-01T12:30:45Z"}
```
##### 400 - Bad Request
```json
{"message":"Данные не прошли валидацию."}
```
##### 409 - Conflict
```json
{"message": "Логин уже занят."}
```
#### POST /auth/sign-in
Вход в систему. Возвращает JWT.
##### Body:
```json
{
  "login": "EugeneTikhonov123",
  "password": "Ab~12!"
}
```
##### 200 - OK
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_at": "2025-06-01T13:30:45Z"
}
```
##### 401 - Unauthorized
```json
{"message": "Неверный логин или пароль."}
```

