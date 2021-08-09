# Smart Routing

## Get Smart Routings

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/smartrouting' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": 1,
      "phone_number": "0899990988",
      "description": "101",
      "created_at": "2021-08-06 10:00:00"
    },
    {
      "id": 2,
      "phone_number": "0899990989",
      "description": "102",
      "created_at": "2021-08-06 10:00:01"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 2
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/smartrouting`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |

## Get Smart Routing By Id or Phone

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/smartrouting/0899990988' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "id": 1,
  "phone_number": "0899990988",
  "description": "101",
  "created_at": "2021-08-06 10:00:00"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/smartrouting/{{id}}`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- |
| id        | Id hoặc số điện thoại |

## Insert Smart Routing

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/smartrouting' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
  "agent" : "104",
  "phone_number" : "0899990988"
}'
```

> Response trả về:

```json
{
  "data": {
    "id": "1",
    "phone_number": "0899990988",
    "agent": "104",
    "created_at": "2021-08-06 10:00:00"
  },
  "message": "successfully"
}
```

API dùng để thêm hoặc cập nhật định tuyến thông minh, cuộc gọi inbound sẽ kiểm tra nếu số điện thoại có trong API thì sẽ tự động đưa tới extension, nếu không tồn tại sẽ xử lý tiếp tục theo rule đã set.

Nếu số điện thoại đã tồn tại thì khi gọi API sẽ tự động update.

### HTTP Request

`POST https://{{API_HOST}}/v1/smartrouting`

### Body

| Parameter    | Description                      | Required |
| ------------ | -------------------------------- | -------- |
| phone_number | Số điện thoại                    | x        |
| agent        | Extension nhận cuộc gọi gần nhất | x        |

## Remove Smart Routing

```shell
curl -L -X DELETE 'https://{{API_HOST}}/v1/smartrouting/{{id}}' \
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

`DELETE https://{{API_HOST}}/v1/smartrouting/{{id}}`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- |
| id        | Id hoặc số điện thoại |
