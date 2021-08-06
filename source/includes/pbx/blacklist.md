# Blacklist

## Get Blacklists

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/blacklist' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "phone_number": "0899990988",
      "description": "spam"
    },
    {
      "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "phone_number": "0899990989",
      "description": "dnc"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 2
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/blaclist`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |

## Get Blacklist By Id or Phone

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/blacklist/0899990988' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
  "phone_number": "0899990988",
  "description": "spam"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/blacklist/{{id}}`

### Query Parameters

| Parameter | Description                         |
| --------- | ----------------------------------- |
| id        | Id của blacklist hoặc số điện thoại |

## Insert Blacklists

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/blacklist' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "blacklist":[
        {
          "phone_number": "0899990988",
          "description": "spam"
        },
        {
          "phone_number": "0899990989",
          "description": "dnc"
        }
    ]
}'
```

> Response trả về:

```json
{
  "fail": [
    {
      "0899990988": "phone_number is duplicate"
    }
  ],
  "message": "successfully",
  "success": ["0899990989"]
}
```

### HTTP Request

`POST https://{{API_HOST}}/v1/blacklist`

### Body

| Parameter              | Description             | Required |
| ---------------------- | ----------------------- | -------- |
| blacklist              | Danh sách số điện thoại | x        |
| blacklist.phone_number | Số điện thoại           | x        |
| blacklist.description  | Chú thích               |          |

## Remove Blacklists

```shell
curl -L -X DELETE 'https://{{API_HOST}}/v1/blacklist' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "blacklist":[
      "0899990988",
      "0899990989"
    ]
}'
```

> Response trả về:

```json
{
  "fail": [],
  "message": "successfully",
  "success": ["0899990988", "0899990989"]
}
```

### HTTP Request

`DELETE https://{{API_HOST}}/v1/blacklist`

### Body

| Parameter | Description             | Required |
| --------- | ----------------------- | -------- |
| blacklist | Danh sách số điện thoại | x        |
