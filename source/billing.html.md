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
    "client_id": "82ab1e65-aaeb-4ab7-bcd1-d8e4a8aae4ed",
    "expired_in": 15483776,
    "refresh_token": "",
    "token": "eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=",
    "token_type": "Bearer",
    "user_id": "1"
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

# Customer

## Get Customers

```shell
curl -L -X GET 'http://{API_HOST}/v1/customer?limit=10&offset=0' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": 1,
      "creationdate": "2021-05-31T17:17:35Z",
      "firstusedate": "2021-05-31T20:08:56Z",
      "expirationdate": "2031-05-31T10:15:27Z",
      "enableexpire": 0,
      "expiredays": 0,
      "username": "0906237580",
      "useralias": "12345790644",
      "credit": 12343,
      "activated": "f",
      "status": 1,
      "lastuse": "2021-07-20T12:00:54Z",
      "creditlimit": null,
      "id_group": 2,
      "caller_ids": [
        {
          "cid": "0123456787",
          "activated": "t"
        }
      ]
    },
    {
      "id": 2,
      "creationdate": "2021-06-05T12:59:50Z",
      "firstusedate": "2021-06-05T13:01:58Z",
      "expirationdate": "2031-06-05T05:58:42Z",
      "enableexpire": 0,
      "expiredays": 0,
      "username": "1234567890",
      "useralias": "12345790645",
      "credit": 12345,
      "activated": "f",
      "status": 1,
      "lastuse": "2021-07-27T19:07:18Z",
      "creditlimit": null,
      "id_group": 2,
      "caller_ids": [
        {
          "cid": "0123456788",
          "activated": "t"
        },
        {
          "cid": "0123456789",
          "activated": "t"
        }
      ]
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 50
}
```

API dùng để lấy danh sách khách hàng.

### HTTP Request

`GET http://{API_HOST}/v1/customer`

### Query Parameters

| Parameter | Description                     | Example |
| --------- | ------------------------------- | ------- |
| limit     | Giới hạn số lượng record trả về | 10      |
| offset    | Vị trí record bắt đầu query     | 0       |

## Get Customer By Id

```shell
curl -L -X GET 'http://{API_HOST}/v1/customer/1' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "id": 1,
  "creationdate": "2021-05-31T17:17:35Z",
  "firstusedate": "2021-05-31T20:08:56Z",
  "expirationdate": "2031-05-31T10:15:27Z",
  "enableexpire": 0,
  "expiredays": 0,
  "username": "0906237580",
  "useralias": "12345790644",
  "credit": 12343,
  "activated": "f",
  "status": 1,
  "lastuse": "2021-07-20T12:00:54Z",
  "creditlimit": null,
  "id_group": 2,
  "caller_ids": [
    {
      "cid": "0123456787",
      "activated": "t"
    }
  ]
}
```

API dùng để lấy thông tin khách hàng theo id.

### HTTP Request

`GET http://{API_HOST}/v1/customer/{{customer_id}}`

### Query Parameters

| Parameter   | Description                                                | Example    |
| ----------- | ---------------------------------------------------------- | ---------- |
| customer_id | Id hoặc username hoặc 1 trong các caller_id của khách hàng | 0906237580 |

## Update Customer Credit

```shell
curl -L -X PUT 'http://{API_HOST}/v1/customer/{{customer_id}}/credit' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "credit" : 12
}'
```

> Response trả về:

```json
{
  "id": "1",
  "message": "successfully",
  "credit": 12
}
```

API dùng để cập nhật số dư khách hàng theo id. (Log sẽ được lưu mỗi khi update thành công)

### HTTP Request

`PUT http://{API_HOST}/v1/customer/{{customer_id}}/credit`

### Query Parameters

| Parameter   | Description                                                | Example    |
| ----------- | ---------------------------------------------------------- | ---------- |
| customer_id | Id hoặc username hoặc 1 trong các caller_id của khách hàng | 0906237580 |

### Body

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| credit    | Số dư mới của khách hàng | 12      |

## Add Customer Credit

```shell
curl -L -X PUT 'http://{API_HOST}/v1/customer/{{customer_id}}/credit/add' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "credit" : 12
}'
```

> Response trả về:

```json
{
  "id": "1",
  "message": "successfully",
  "credit": 15
}
```

API dùng để cập nhật số dư khách hàng theo id. Khác với API Update Credit phía trên, API này sẽ cộng thêm vào số dư hiện tại của khách hàng. Ví dụ: Khách hàng đang còn 3, credit truyền vào là 12 thì số dư của khách sẽ là 15. (Log sẽ được lưu mỗi khi update thành công)

### HTTP Request

`PUT http://{API_HOST}/v1/customer/{{customer_id}}/credit/add`

### Query Parameters

| Parameter   | Description                                                | Example    |
| ----------- | ---------------------------------------------------------- | ---------- |
| customer_id | Id hoặc username hoặc 1 trong các caller_id của khách hàng | 0906237580 |

### Body

| Parameter | Description                           | Example |
| --------- | ------------------------------------- | ------- |
| credit    | Số tiền cộng vào số dư của khách hàng | 12      |

## Update Customer Status

```shell
curl -L -X PUT 'http://{API_HOST}/v1/customer/{{customer_id}}/status' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "status" : "active"
}'
```

> Response trả về:

```json
{
  "id": "1",
  "message": "successfully"
}
```

API dùng để cập nhật trạng thái khách hàng theo id. (Log sẽ được lưu mỗi khi update thành công)

### HTTP Request

`PUT http://{API_HOST}/v1/customer/{{customer_id}}/status`

### Query Parameters

| Parameter   | Description                                                | Example    |
| ----------- | ---------------------------------------------------------- | ---------- |
| customer_id | Id hoặc username hoặc 1 trong các caller_id của khách hàng | 0906237580 |

### Body

| Parameter | Description                   | Example |
| --------- | ----------------------------- | ------- |
| status    | Trạng thái mới của khách hàng | active  |

### Status

| Name      | Description         |
| --------- | ------------------- |
| active    | Được phép hoạt động |
| expired   | Hết hạn             |
| cancelled | Bị huỷ              |
| suspended | Tạm ngưng dịch vụ   |

## Create Customer

```shell
curl -L -X POST 'http://{API_HOST}/v1/customer' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "type_paid": 0,
    "username": "5000000000",
    "password": "123abc123",
    "cid": "0899999999",
    "credit": 100
}'
```

> Response trả về:

```json
{
  "message": "successfully",
  "id": 10,
  "username": "5000000004",
  "password": "123abc123",
  "cid": "0899999999",
  "status": 1,
  "sip": true,
  "iax": true
}
```

API dùng để tạo khách hàng.

### HTTP Request

`POST http://{API_HOST}/v1/customer`

### Body

| Parameter    | Description                                                         | Example    |
| ------------ | ------------------------------------------------------------------- | ---------- |
| username     | username của customer. Dãy bao gồm 10 số                            | 5000000000 |
| password     | Mật khẩu                                                            | 123abc123  |
| type_paid    | Loại thanh toán. Trả trước : 0, Trả sau : 1                         | 1          |
| cid          | caller_id sẽ gắn vào cho khách hàng                                 | 0899999999 |
| credit       | Số tiền sẽ nạp vào tài khoản khách hàng                             | 100        |
| credit_limit | Số tiền sẽ giới hạn tài khoản khách hàng. Yêu cầu khi type_paid = 1 | 100        |

# Caller ID

## Create Caller Id

```shell
curl -L -X POST 'http://{API_HOST}/v1/callerid' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "user_id" : "10",
    "cid": "0899999999"
}'
```

> Response trả về:

```json
{
  "cid": "0899999999",
  "message": "successfully",
  "user_id": "10"
}
```

API dùng để tạo khách hàng.

### HTTP Request

`POST http://{API_HOST}/v1/callerid`

### Body

| Parameter | Description                         | Example    |
| --------- | ----------------------------------- | ---------- |
| user_id   | id của khách hàng                   | 10         |
| cid       | caller_id sẽ gắn vào cho khách hàng | 0899999999 |

## Update Caller Id To New Customer

```shell
curl -L -X PUT 'http://{API_HOST}/v1/callerid/{{cid}}' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
--data-raw '{
    "user_id" : "11"
}'
```

> Response trả về:

```json
{
  "cid": "0899999999",
  "message": "successfully",
  "user_id": "11"
}
```

API dùng để cập nhật caller_id cho khách hàng mới. (Log sẽ được lưu mỗi khi update thành công)

### HTTP Request

`PUT http://{API_HOST}/v1/callerid/{{cid}}`

### Query Parameters

| Parameter | Description                         | Example    |
| --------- | ----------------------------------- | ---------- |
| cid       | caller_id sẽ gắn vào cho khách hàng | 0899999999 |

### Body

| Parameter | Description           | Example |
| --------- | --------------------- | ------- |
| user_id   | id của khách hàng mới | 11      |

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
            "end_time": "2021-06-04T21:13:37Z",
            "id": 42,
            "source": "0899123456",
            "start_time": "2021-06-04T21:12:59Z",
            "uniqueid": "1622815952.3252"
        },
        {
            "amount": 1600,
            "destination": "0123456789",
            "duration": 120,
            "end_time": "2021-06-01T22:04:13Z",
            "id": 29,
            "source": "0899123456",
            "start_time": "2021-06-01T22:02:13Z",
            "uniqueid": "1622559713.3225"
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
