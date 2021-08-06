# Call

## List Calls

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/call' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": [
      {
        "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
        "context": "test.tel4vn.com",
        "created_time": "2021-06-08 17:37:37",
        "destination": "0899123456",
        "direction": "outbound",
        "ip": "1.2.3.4",
        "source": "101",
        "state": "CS_EXECUTE"
      },
      ...
  ],
  "total": 1
}
```

API dùng để lấy danh sách các cuộc gọi theo thời gian thực.

### HTTP Request

`GET https://{{API_HOST}}/v1/call`

## Transfer a call

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/call/01b7d166-b564-42ec-80a1-4ad343225934/transfer' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101",
    "legs" : "bleg"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện chuyển cuộc gọi sang extension khác.

### HTTP Request

`POST https://{{API_HOST}}/v1/call/<CALL_ID>/transfer`

### Body

| Parameter | Description                                                                           | Required |
| --------- | ------------------------------------------------------------------------------------- | -------- |
| ext       | Ext nhận cuộc gọi mới                                                                 | x        |
| legs      | aleg hoặc bleg. aleg có thể hiểu là bên thực hiện cuộc gọi, bleg là bên nhận cuộc gọi | x        |

## Listen a call

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/call/01b7d166-b564-42ec-80a1-4ad343225934/listen' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện nghe lén cuộc gọi của một extension khác.

### HTTP Request

`POST https://{{API_HOST}}/v1/call/<CALL_ID>/listen`

### Body

| Parameter | Description       |
| --------- | ----------------- |
| ext       | Ext nhận cuộc gọi |

## Whisper a call

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/call/01b7d166-b564-42ec-80a1-4ad343225934/whisper' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện cuộc gọi với extension, mobile sẽ không nghe được.

### HTTP Request

`POST https://{{API_HOST}}/v1/call/<CALL_ID>/whisper`

### Body

| Parameter | Description           |
| --------- | --------------------- |
| ext       | Ext nhận cuộc gọi mới |

## Barge a call

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/call/01b7d166-b564-42ec-80a1-4ad343225934/barge' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện cuộc gọi 3 bên với extension và mobile.

### HTTP Request

`POST https://{{API_HOST}}/v1/call/<CALL_ID>/barge`

### Body

| Parameter | Description           |
| --------- | --------------------- |
| ext       | Ext nhận cuộc gọi mới |

## Park a call

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/call/01b7d166-b564-42ec-80a1-4ad343225934/park' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "legs" : "aleg"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934"
}
```

> Error Response trả về:

```json
{
  "status": "fail"
}
```

API dùng để ngắt một bên cuộc gọi và cho bên còn lại chờ tới khi có cuộc gọi transfer tới hoặc sau 10s sẽ tự động ngắt.

### HTTP Request

`POST https://{{API_HOST}}/v1/call/<CALL_ID>/park`

### Body

| Parameter | Description                                                                           | Required |
| --------- | ------------------------------------------------------------------------------------- | -------- |
| legs      | aleg hoặc bleg. aleg có thể hiểu là bên thực hiện cuộc gọi, bleg là bên nhận cuộc gọi | x        |