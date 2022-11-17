# Extension

## Get Extensions

```shell
curl -L -X GET 'https://{{API_HOST}}/v3/extension' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
      {
          "extension_uuid": "9c405db5-9f24-4075-b261-7795eda0d4ab",
          "extension": "101",
          "user_uuid": "16258396-d395-42d3-9b0d-14d3035b5e3a",
          "username": "Leader_Dev",
          "enabled": true,
          "domain_uuid": "b10a2e29-9fee-4f9f-89f4-b9677bad186d",
          "domain_name": "dev.tel4vn.com",
          "is_link_call_center": true,
          "follow_me": ""
      },
      {
          "extension_uuid": "71cb1d6b-e8c2-4df4-bde2-9afbbc4f0125",
          "extension": "102",
          "user_uuid": "",
          "username": "",
          "enabled": false,
          "domain_uuid": "b10a2e29-9fee-4f9f-89f4-b9677bad186d",
          "domain_name": "dev.tel4vn.com",
          "is_link_call_center": true,
          "follow_me": "0123456789"
      },
      ...
  ],
  "limit": 25,
  "offset": 0,
  "total": 40
}
```

### HTTP Request

`GET https://{{API_HOST}}/v2/extension`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |
| extension | Giá trị                  | 101     |

## Find Extension By Id

```shell
curl -L -X GET 'https://{{API_HOST}}/v3/extension/71cb1d6b-e8c2-4df4-bde2-9afbbc4f0125' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "extension_uuid": "71cb1d6b-e8c2-4df4-bde2-9afbbc4f0125",
  "extension": "102",
  "user_uuid": "",
  "username": "",
  "enabled": false,
  "domain_uuid": "b10a2e29-9fee-4f9f-89f4-b9677bad186d",
  "domain_name": "dev.tel4vn.com",
  "is_link_call_center": true,
  "follow_me": "0123456789"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v2/extension/{{extension_uuid or extension}}`

## Post Extension

```shell
curl -L -X POST 'https://{{API_HOST}}/v3/extension' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "extension": "101",
    "password": "123abc@#123",
    "enabled": true,
    "user_uuid": ""
}'
```

> Response trả về:

```json
{
  "created": true,
  "extension_uuid": "9c405db5-9f24-4075-b261-7795eda0d4ab",
  "enabled": true,
  "extension": "101"
}
```

### HTTP Request

`POST https://{{API_HOST}}/v3/extension`

### Body

| Parameter | Description                           | Required |
| --------- | ------------------------------------- | -------- |
| extension | Số máy nhánh                          | x        |
| password  | Mật khẩu                              | x        |
| enabled   | Kích hoạt                             | x        |
| user_uuid | Map với một user trên hệ thống        |          |
| follow_me | Số điện thoại gọi ra khi không online |          |

## PUT Extension

```shell
curl -L -X PUT 'https://{{API_HOST}}/v3/extension/9c405db5-9f24-4075-b261-7795eda0d4ab' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "password": "123abc@#123",
    "enabled": true,
    "user_uuid": ""
}'
```

> Response trả về:

```json
{
  "message": "success",
  "extension_uuid": "9c405db5-9f24-4075-b261-7795eda0d4ab",
  "enabled": true,
  "extension": "101"
}
```

### HTTP Request

`PUT https://{{API_HOST}}/v3/extension/{{extension hoặc extension_uuid}}`

### Body

| Parameter | Description                           | Required |
| --------- | ------------------------------------- | -------- |
| password  | Mật khẩu                              | x        |
| enabled   | Kích hoạt                             | x        |
| user_uuid | Map với một user trên hệ thống        |          |
| follow_me | Số điện thoại gọi ra khi không online |          |

## Note\*

Nếu bạn muốn giữ mật khẩu cũ vui lòng fill password với "deFauLt".

## Delete Extension

```shell
curl -L -X DELETE 'https://{{API_HOST}}/v3/extension/9c405db5-9f24-4075-b261-7795eda0d4ab' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "message": "success",
  "extension_uuid": "9c405db5-9f24-4075-b261-7795eda0d4ab",
  "extension": "101"
}
```

### HTTP Request

`DELETE https://{{API_HOST}}/v3/extension`
