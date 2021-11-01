# Authentication

## Login Account

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/auth' \
-H 'Content-Type: application/json' \
--data-raw '{
    "username" : "foo@test.tel4vn.com",
    "password" : "foo123"
}'
```

> Response trả về:

```json
{
  "data": {
    "user_uuid": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "domain_uuid": "dddddddd-1111-2222-3333-eeeeeeee",
    "username": "foo",
    "api_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "user_enabled": "true",
    "level": "admin"
  }
}
```

Login thành công sẽ trả về thông tin account.

### HTTP Request

`POST https://{{API_HOST}}/v1/auth`

### Body

| Parameter | Description        |
| --------- | ------------------ |
| username  | Account's username |
| password  | Account's password |

## Get Access Token

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/auth/token' \
-H 'Content-Type: application/json' \
--data-raw '{
    "api_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee"
}'
```

> Response trả về:

```json
{
  "data": {
    "expired": 1613636318,
    "token": "eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=",
    "user_id": "aaaaaaaa-1111-2222-3333-eeeeeeee"
  }
}
```

PBX API sử dụng API Token để xác thực truy cập tới API. API Token bạn lấy từ service thông qua {{API_KEY}} được Tổng đài cung cấp.

Tất cả các API của PBX đều yêu cầu user cung cấp Token trong header giống phía dưới.

`Authorization: Bearer {TOKEN}`

<div class="alert alert-success alert-dismissible fade show" role="alert">
Bạn vui lòng thay đổi <code>{TOKEN}</code> bằng token đã lấy được.
</div>

### HTTP Request

`POST https://{{API_HOST}}/v1/auth/token`

### Body

| Parameter | Description                            |
| --------- | -------------------------------------- |
| api_key   | api_key có trong thông tin của account |
