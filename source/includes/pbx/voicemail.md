# Voicemail

## Get Voicemails

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/voicemail' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "extension": "101",
      "password": "abc123def",
      "mail_to": "",
      "local_after_email": true,
      "enabled": true,
      "description": "For Tech"
    },
    {
      "id": "aaaaaaaa-1111-2222-3333-ffffffff",
      "extension": "102",
      "password": "abc123def",
      "mail_to": "tech@tel4vn.com",
      "local_after_email": true,
      "enabled": true,
      "description": "For Tech"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 10
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/voicemail`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |

## Get Voicemail By Id or Extension

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/voicemail/aaaaaaaa-1111-2222-3333-eeeeeeee' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
  "extension": "101",
  "password": "abc123def",
  "mail_to": "",
  "local_after_email": true,
  "enabled": true,
  "description": "For Tech"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/voicemail/{{id}}`

### Query Parameters

| Parameter | Description       |
| --------- | ----------------- |
| id        | Id hoặc Extension |

## Insert Voicemail

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/voicemail' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "extension": "102",
    "password": "abc123def",
    "mail_to": "tech@tel4vn.com",
    "local_after_email": true,
    "enabled": true,
    "description": "For Tech"
}'
```

> Response trả về:

```json
{
  "data": {
    "id": "aaaaaaaa-1111-2222-3333-ffffffff",
    "extension": "102",
    "password": "abc123def",
    "mail_to": "tech@tel4vn.com",
    "local_after_email": true,
    "enabled": true,
    "description": "For Tech"
  },
  "message": "successfully"
}
```

### HTTP Request

`POST https://{{API_HOST}}/v1/voicemail`

### Body

| Parameter         | Description                                          | Required |
| ----------------- | ---------------------------------------------------- | -------- |
| extension         | Extension                                            | x        |
| password          | Mật khẩu voicemail                                   | x        |
| enabled           | true hoặc false                                      | x        |
| local_after_email | Lưu file âm thanh sau khi gửi mail (true hoặc false) |          |
| mail_to           | Email nhận khi có voicemail                          |          |
| description       | Mô tả nếu có                                         |          |

## Update Voicemail

```shell
curl -L -X PUT 'https://{{API_HOST}}/v1/voicemail/{{id}}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}' \
--data-raw '{
    "password": "abc123def",
    "mail_to": "tech@tel4vn.com",
    "local_after_email": true,
    "enabled": true,
    "description": "For Tech"
}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

### HTTP Request

`PUT https://{{API_HOST}}/v1/voicemail/{{id}}`

### Query Parameters

| Parameter | Description       |
| --------- | ----------------- |
| id        | Id hoặc Extension |

### Body

| Parameter         | Description                                          | Required |
| ----------------- | ---------------------------------------------------- | -------- |
| password          | Mật khẩu voicemail                                   | x        |
| enabled           | true hoặc false                                      | x        |
| local_after_email | Lưu file âm thanh sau khi gửi mail (true hoặc false) |          |
| mail_to           | Email nhận khi có voicemail                          |          |
| description       | Mô tả nếu có                                         |          |

## Remove Voicemail

```shell
curl -L -X DELETE 'https://{{API_HOST}}/v1/voicemail/{{id}}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

### HTTP Request

`DELETE https://{{API_HOST}}/v1/voicemail/{{id}}`

### Query Parameters

| Parameter | Description       |
| --------- | ----------------- |
| id        | Id hoặc Extension |

## Get Voicemail Messages

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/voicemail/message' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "created_at": "2021-08-27T11:13:44+07:00",
      "caller_id_name": "0891234567",
      "caller_id_number": "0891234567",
      "duration": 7,
      "extension": "101",
      "download_url": "https://pbx01.tel4vn.com/v1/voicemail/message/aaaaaaaa-1111-2222-3333-eeeeeeee/download"
    },
    {
      "id": "aaaaaaaa-2222-2222-3333-ffffffff",
      "created_at": "2021-08-27T15:13:44+07:00",
      "caller_id_name": "0891234567",
      "caller_id_number": "0891234567",
      "duration": 120,
      "extension": "102",
      "download_url": "https://pbx01.tel4vn.com/v1/voicemail/message/aaaaaaaa-2222-2222-3333-ffffffff/download"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 10
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/voicemail/message`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |
| extension | Tìm theo extension       | 101     |

## Get Voicemail Message By Id

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/voicemail/message/aaaaaaaa-1111-2222-3333-eeeeeeee' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
  "created_at": "2021-08-27T11:13:44+07:00",
  "caller_id_name": "0891234567",
  "caller_id_number": "0891234567",
  "duration": 120,
  "extension": "102",
  "download_url": "https://pbx01.tel4vn.com/v1/voicemail/message/aaaaaaaa-1111-2222-3333-ffffffff/download"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/voicemail/message/{{id}}`

### Query Parameters

| Parameter | Description       |
| --------- | ----------------- |
| id        | Id hoặc Extension |
