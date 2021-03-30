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

Xin chào! Đây là bộ API tích hợp tổng đài VoIP - PBX vào các hệ thống CRM, WebApp.

Nếu bạn cần thông tin để tích hợp hoặc cần hỗ trợ vui lòng liên hệ mail: support@tel4vn.com.

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
      "expired": 1613636318,
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

# Event

## Get Events

```shell
curl -L -X GET 'http://{API_HOST}/v1/event' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE='
```
> Response trả về:

```json
{
  "data": [
    {
      "event_domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
      "domain_uuid": "dddddddd-1111-2222-3333-eeeeeeee",
      "domain_name": "test.tel4vn.com",
      "event_name": "hangup",
      "callback_url": "https://webhook.demo/",
      "callback_apikey": "foo"
    },
    ...
  ]
}
```

Trả về các call events của tenant.

### HTTP Request

`GET http://{API_HOST}/v1/event`

## Create Events

```shell
curl -L -X POST 'http://{API_HOST}/v1/event' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=' \
-H 'Content-Type: application/json' \
--data-raw '{
    "callback_url" : "https://webhook.demo/",
    "callback_apikey" : "foo",
    "event" : "create"
}'
```

> Response trả về:

```json
{
  "created": true
}
```

Tạo Event Hook, mỗi lần bắt được {event} tổng đài sẽ hook dữ liệu về {callback_url}.

### HTTP Request

`POST http://{API_HOST}/v1/event`

### Body

| Parameter       | Description                                    |
| --------------- | ---------------------------------------------- |
| callback_url    | Domain Url mà tổng đài sẽ hook dữ liệu tới     |
| callback_apikey | ApiKey hoặc access token của domain (optional) |
| event           | SIP Call Event                                 |

### SIP Call Event

| Parameter | Description                        |
| --------- | ---------------------------------- |
| hangup    | Sự kiện khi cuộc gọi bị ngắt, huỷ  |
| ringing   | Sự kiện khi cuộc gọi được khởi tạo |
| answered  | Sự kiện khi cuộc gọi được nhấc máy |
| cdr       | Sự kiện sau khi cdr được tạo xong  |

<aside class="danger">Nếu bạn tạo event nằm ngoài các event ở trên, hệ thống sẽ không nhận diện được nên sẽ không hook data về.</aside>
<aside class="warning">Nếu bạn cần hook event ngoài các event ở trên, vui lòng gửi mail hỗ trợ.</aside>

## Delete Event

```shell
curl -L -X DELETE 'http://{API_HOST}/v1/event/eeeeeeee-1111-2222-3333-eeeeeeee' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=' 
```

> Response trả về:

```json
{
  "deleted": true,
}
```

API dùng để xoá một event_domain.

### HTTP Request

`DELETE http://{API_HOST}/v1/event/<ID>`

### URL Parameters

| Parameter | Description  |
| --------- | ------------ |
| ID        | ID của event |

## Event Hook Data

> Response trả về:

```json
{
  "call_id": "avavavav-1111-2222-3333-eeeeeeee",
  "sip_call_id": "dddddddd-1111-2222-3333-eeeeeeee",
  "domain": "test.tel4vn.com",
  "direction": "outbound",
  "from_number": "0899888999",
  "to_number": "101",
  "hotline": "19001919",
  "state": "hangup",
  "duration": 10,
  "billsec": 5,
  "recording_url": "http://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623"
}
```

### Description

| Parameter     | Description                                                    |
| ------------- | -------------------------------------------------------------- |
| call_id       | Id định danh cuộc gọi                                          |
| sip_call_id   | SIP Call Id                                                    |
| domain        | Domain nhận hoặc thực hiện cuộc gọi                            |
| direction     | Hướng cuộc gọi (inbound / outbound)                            |
| from_number   | Số gọi. Sẽ là số ext nếu cuộc gọi là outbound                  |
| to_number     | Số nhận. Sẽ là số ext nếu cuộc gọi là inbound                  |
| hotline       | Đầu số nhận hoặc thực hiện cuộc gọi                            |
| state         | ringing / answered / hangup / cdr                              |
| duration      | Tổng thời lượng cuộc gọi. (Riêng sự kiện hangup)               |
| billsec       | Thời lượng tính từ khi hai bên kết nối. (Riêng sự kiện hangup) |
| recording_url | URL public để play file ghi âm. (Riêng sự kiện cdr)            |

# CDRs - Call Detail Records

Lịch sử cuộc gọi

### Attributes

| Attribute     | Description                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------- |
| id            | Id của CDR                                                                                  |
| sip_call_id   | call_id trong bản tin SIP                                                                   |
| cause         | Trạng thái cuộc gọi dựa theo mã phản hồi giao thức SIP. Vd: NORMAL_CLEARING, NO_ANSWER, ... |
| duration      | Thời hạn thực hiện cuộc gọi                                                                 |
| direction     | Chiều cuộc gọi (inbound, outbound, local)                                                   |
| recording_url | Đường dẫn file ghi âm cuộc gọi                                                              |
| extension     | Extension nhận hoặc thực hiện cuộc gọi                                                      |
| from_number   | Cuộc gọi từ số nào                                                                          |
| to_number     | Cuộc gọi đến số nào                                                                         |
| receive_dest  | Ringroup hoặc queue của extension nhận cuộc gọi                                             |
| time_started  | Thời gian bắt đầu cuộc gọi                                                                  |
| time_answered | Thời gian khi cuộc gọi kết nối                                                              |
| time_ended    | Thời gian kết thúc cuộc gọi                                                                 |
| status        | Trạng thái cuộc gọi                                                                         |

## Get CDRs

```shell
curl -L -X GET 'http://{API_HOST}/v1/cdr?' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE='
```

> Pagination response trả về:

```json
{
  "data": [
    {
      "id": "ad4c9b90-c071-405a-9723-980d2e5e1623",
      "sip_call_id": "112233aabbccddee..",
      "cause": "NORMAL_CLEARING",
      "duration": 11,
      "direction": 3,
      "recording_url": "http://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-17 17:30:35",
      "time_answered": "2021-02-17 17:30:43",
      "time_ended": "2021-02-17 17:30:46",
      "status": "ANSWERED"
    },
    {
      "id": "01b7d166-b564-42ec-80a1-4ad343225934 ",
      "sip_call_id": "aabbccddee112233..",
      "cause": "NORMAL_CLEARING",
      "duration": 7,
      "direction": 3,
      "recording_url": "",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-18 17:20:58",
      "time_answered": "",
      "time_ended": "2021-02-18 17:21:05",
      "status": "BUSY"
    },
    ...
  ],
  "limit": 10,
  "page": 1,
  "total": 22
}
```
> Scroll response trả về:

```json
{
  "data": [
    {
      "id": "ad4c9b90-c071-405a-9723-980d2e5e1623",
      "sip_call_id": "112233aabbccddee..",
      "cause": "NORMAL_CLEARING",
      "duration": 11,
      "direction": 3,
      "recording_url": "http://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623.wav",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-17 17:30:35",
      "time_answered": "2021-02-17 17:30:43",
      "time_ended": "2021-02-17 17:30:46",
      "status": "ANSWERED"
    },
    {
      "id": "01b7d166-b564-42ec-80a1-4ad343225934",
      "sip_call_id": "aabbccddee112233..",
      "cause": "NORMAL_CLEARING",
      "duration": 7,
      "direction": 3,
      "recording_url": "",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-18 17:20:58",
      "time_answered": "",
      "time_ended": "2021-02-18 17:21:05",
      "status": "BUSY"
    },
    ...
  ],
  "scroll_id": "111222333444aaabbbcccddd=="
}
```

Trả về danh sách lịch sử cuộc gọi.
API CDRs sử dụng 2 cơ chế để trả về dữ liệu.
Pagination: Phân trang.
Scroll: Cuộn trang. (Mặc định)

Nếu user cung cấp trong param: page - Số trang, limit - số lượng trả về thì API sẽ trả về dữ liêu theo cơ chế Pagination.

### HTTP Request

`GET http://{API_HOST}/v1/cdr`

### Query Parameters

| Parameter     | Description                               | Example                             |
| ------------- | ----------------------------------------- | ----------------------------------- |
| start_date    | Tìm kiếm cdrs theo khoảng thời gian       | 2021-02-18 hoặc 2021-02-18 17:20:58 |
| end_date      | Tìm kiếm cdrs theo khoảng thời gian       | 2021-02-19 hoặc 2021-02-19 00:00:00 |
| duration      | Thời hạn của cuộc gọi                     | 10                                  |
| extension     | Cuộc gọi từ extension nào                 | 101                                 |
| recordingfile | File recording của cuộc gọi               | abcd.mp3                            |
| status        | Trạng thái cuộc gọi                       | ANSWERED                            |
| phone         | Từ hoặc tới số điện thoại nào             | 0899888999                          |
| direction     | Chiều cuộc gọi (inbound, outbound, local) | outbound                            |

## Get a Specific CDR

```shell
curl -L -X GET 'http://{API_HOST}/v1/cdr/01b7d166-b564-42ec-80a1-4ad343225934' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE='
```

> Response trả về:

```json
{
  "id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "sip_call_id": "aabbccddee112233..",
  "cause": "NORMAL_CLEARING",
  "duration": 7,
  "direction": 3,
  "recording_url": "",
  "extension": "101",
  "from_number": "19001919",
  "to_number": "0899888999",
  "receive_dest": "",
  "time_started": "2021-02-18 17:20:58",
  "time_answered": "",
  "time_ended": "2021-02-18 17:21:05",
  "status": "BUSY"
}
```

API dùng để lấy một cdr cụ thể.
Id có thể id của CDR hoặc sip_call_id trong bản tin

### HTTP Request

`GET http://{API_HOST}/v1/cdr/<ID>`

### URL Parameters

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| ID        | Id của CDR hoặc sip_call_id trong bản tin |

# Click-to-call

```shell
curl -L -X GET 'http://{API_HOST}/v1/click2call?ext=101&phone=0899098899' \
-H 'Authorization: Bearer eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE='
```

> Response trả về:

```json
{
  "data": {
    "message": "success"
  }
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

### HTTP Request

`GET http://{API_HOST}/v1/click2call?ext=<EXTENSION>&phone=<PHONE>`

### URL Parameters

| Parameter       | Description                                               | Required |
| --------------- | --------------------------------------------------------- | -------- |
| ext             | Extension thực hiện cuộc gọi                              | true     |
| phone           | Số điện thoại sẽ được gọi tới                             | true     |
| src_cid_name    | Tên hiển thị trên máy extension. Default: Click2Call      | false    |
| src_cid_number  | Số hiển thị trên máy extension. Default: Số của extension | false    |
| dest_cid_name   | Tên của số điện thoại sẽ chèn vào bản tin SIP             | false    |
| dest_cid_number | Số điện thoại sẽ chèn vào bản tin SIP                     | false    |
| auto_answer     | Tự động nhấc máy phía extension. Default: false           | false    |
