---
title: PBX API Reference

language_tabs:
  - shell: cURL

toc_footers:
  - <p>If any problem please contact us</p>
  - <li>Email &#58; tech@tel4vn.com</li>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Xin chào! Đây là bộ API tích hợp của hệ thống billing TEL4VN.

Nếu bạn cần thông tin để tích hợp hoặc cần hỗ trợ vui lòng liên hệ mail: tech@tel4vn.com.

Các thông tin như {API_HOST}, {API_KEY}, tài khoản admin, tài khoản SIP test sẽ được bên phía Tổng đài cung cấp.

# Authentication

## Login Account

```shell
curl -L -X POST 'http://{API_HOST}/v1/auth' \
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

`POST http://{API_HOST}/v1/auth`

### Body

| Parameter | Description        |
| --------- | ------------------ |
| username  | Account's username |
| password  | Account's password |

## Get Access Token

```shell
curl -L -X POST 'http://{API_HOST}/v1/auth/token' \
-H 'Content-Type: application/json' \
--data-raw '{
    "api_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee"
}'
```

> Response trả về:

```json
{
  "data": {
      "expire_at": 1613636318,
      "token": "eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=",
      "user_id": "aaaaaaaa-1111-2222-3333-eeeeeeee"
  }
}
```

PBX API sử dụng API Token để xác thực truy cập tới API. API Token bạn lấy từ service thông qua {API_KEY} được Tổng đài cung cấp.

Tất cả các API của PBX đều yêu cầu user cung cấp Token trong header giống phía dưới.

`Authorization: Bearer {TOKEN}`

<aside class="notice">
Bạn vui lòng thay đổi <code>{TOKEN}</code> bằng token đã lấy được.
</aside>

### HTTP Request

`POST http://{API_HOST}/v1/auth/token`

### Body

| Parameter | Description                            |
| --------- | -------------------------------------- |
| api_key   | api_key có trong thông tin của account |

# Call

## Call Logs

```shell
curl -L -X GET 'http://{API_HOST}/v1/call/log?end_date=2021-06-01%2000:00:00&start_date=2021-06-01%2023:59:59' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE='
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
    "data": [
        {
            "amount": 285,
            "destination": "0123456789",
            "duration": 38,
            "end_time": "2021-06-04T12:13:37Z",
            "source": "0899123456",
            "start_time": "2021-06-04T12:12:59Z"
        },
        {
            "amount": 1600,
            "destination": "0123456789",
            "duration": 120,
            "end_time": "2021-06-01T13:04:13Z",
            "source": "0899123456",
            "start_time": "2021-06-01T13:02:13Z"
        },
        ...
    ],
    "limit": 10,
    "offset": 0,
    "total": 15
}
```

> Error Response trả về:

```json
{
  "error": "date range must be in 31 days"
}
```

API dùng để lấy danh sách các cuộc gọi và số tiền theo khoảng thời gian.

### HTTP Request

`GET http://{API_HOST}/v1/call/log`

### Query Parameters

| Parameter  | Description                             | Example             |
| ---------- | --------------------------------------- | ------------------- |
| start_date | Tìm kiếm call log theo khoảng thời gian | 2021-02-18 17:20:58 |
| end_date   | Tìm kiếm call log theo khoảng thời gian | 2021-02-19 00:00:00 |
| source     | Đầu số thực hiện cuộc gọi               | 0899123456          |
| limit      | Giới hạn số lượng record trả về         | 10                  |
| offset     | Vị trí record bắt đầu query             | 0                   |
