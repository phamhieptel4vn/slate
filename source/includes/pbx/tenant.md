# Tenant

## Create Tenant

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/tenant' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
--data-raw '{
  "tenant_id" : test01"
}'
```

> Response trả về:

```json
{
  "created": true,
  "data": {
    "domain": "test01.tel4vn.com",
    "tenant_url": "https://{{API_HOST}}/v1/tenant/41d0f364-79f8-4abb-9834-ee803320ea8d",
    "outbound_proxy": "pbx01.tel4vn.com:50061",
    "port": "50061",
    "transports": "both",
    "wss": "wss://pbx01.tel4vn.com",
    "user": {
      "api_key": "5294243b-1ace-45a9-b2ef-d66222cb1729",
      "enabled": "true",
      "group": "admin",
      "password": "abcabc123321",
      "user_id": "938b4d25-4b25-4f32-9169-7411734ef990",
      "username": "admin"
    }
  }
}
```

### HTTP Request

`POST https://{{API_HOST}}/v1/tenant`

### Body

| Parameter | Description |
| --------- | ----------- |
| tenant_id | Tenant'id   |

## Get Tenant By ID

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
```

> Response trả về:

```json
{
  "domain": "test01.tel4vn.com",
  "tenant_url": "https://{{API_HOST}}/v1/tenant/41d0f364-79f8-4abb-9834-ee803320ea8d",
  "outbound_proxy": "pbx01.tel4vn.com:50061",
  "port": "50061",
  "transports": "both",
  "wss": "wss://pbx01.tel4vn.com"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/tenant/{{tenant_id}}`

### Query Parameters

| Parameter | Description | Example |
| --------- | ----------- | ------- |
| tenant_id | Tenant'id   | test01  |

## Update Tenant

```shell
curl -L -X PUT 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'\
--data-raw '{
    "enable" : false
}'
```

> Response trả về:

```json
{
  "message": "update successfully"
}
```

### HTTP Request

`PUT https://{{API_HOST}}/v1/tenant/{{tenant_id}}`

### Body

| Parameter | Description               | Example       |
| --------- | ------------------------- | ------------- |
| enable    | Active or Deactive tenant | true of false |

## Create Extension

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
--data-raw '{
    "ext" : "101",
    "pwd" : "123Abc!@#"
}'
```

> Response trả về:

```json
{
  "created": true,
  "data": {
    "enabled": "true",
    "ext": "104",
    "pwd": "123Abc!@#"
  }
}
```

### HTTP Request

`POST https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension`

### Body

| Parameter | Description |
| --------- | ----------- |
| ext       | Extension   |
| pwd       | Password    |

## Get Extensions

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension?limit=2&offset=0' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "ext": "101",
      "pwd": "123Abc!@#",
      "enabled": "true"
    },
    {
      "ext": "102",
      "pwd": "123Abc!@#",
      "enabled": "true"
    }
  ],
  "limit": 2,
  "offset": 0,
  "total": 10
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension`

### Query Parameters

| Parameter | Description            | Example |
| --------- | ---------------------- | ------- |
| tenant_id | Tenant'id              | test01  |
| limit     | Số lượng record trả về | 50      |
| offset    | Vị trí bắt đầu khi lấy | 0       |

## Get Extension By Ext

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension/{{ext}}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
```

> Response trả về:

```json
{
  "ext": "104",
  "pwd": "123Abc!@#",
  "enabled": "true"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension/{{ext}}`

### Query Parameters

| Parameter | Description | Example |
| --------- | ----------- | ------- |
| tenant_id | Tenant'id   | test01  |
| ext       | Extension   | 101     |

## Update Extension Password

```shell
curl -L -X PUT 'https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension/{{ext}}/password' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{SECRET_TOKEN}}'
--data-raw '{
    "pwd" : "123Abc!@#"
}'
```

> Response trả về:

```json
{
  "message": "update successfully"
}
```

### HTTP Request

`PUT https://{{API_HOST}}/v1/tenant/{{tenant_id}}/extension/{{ext}}/password`

### Query Parameters

| Parameter | Description | Example |
| --------- | ----------- | ------- |
| tenant_id | Tenant'id   | test01  |
| ext       | Extension   | 101     |

### Body

| Parameter | Description |
| --------- | ----------- |
| pwd       | Password    |
