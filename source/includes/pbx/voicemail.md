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