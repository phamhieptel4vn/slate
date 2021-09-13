---
title: Call Center API Reference

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

Xin chào! Đây là bộ API tích hợp tổng đài VoIP - Call Center vào các hệ thống CRM, WebApp.

Nếu bạn cần thông tin để tích hợp hoặc cần hỗ trợ vui lòng liên hệ mail: tech@tel4vn.com.

Các thông tin như {{API_HOST}}, {{API_KEY}}, tài khoản admin, tài khoản SIP test sẽ được bên phía Tổng đài cung cấp.

# Authentication

## Login Account

```shell
curl -L -X POST 'http://{{API_HOST}}/v1/auth' \
-H 'Content-Type: application/json' \
--data-raw '{
    "username" : "foo@test.tel4vn.com",
    "password" : "foo123"
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": {
    "api_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "domain_uuid": "dddddddd-1111-2222-3333-eeeeeeee",
    "enabled": "true",
    "extension": "110",
    "level": "admin",
    "user_uuid": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "username": "adminfortest"
  }
}
```

Login thành công sẽ trả về thông tin account.

### HTTP Request

`POST http://{{API_HOST}}/v1/auth`

### Body

| Parameter | Mô tả              |
| --------- | ------------------ |
| username  | Account's username |
| password  | Account's password |

## Get Access Token

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/auth/token' \
-H 'Content-Type: application/json' \
--data-raw '{
    "api_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee"
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": {
    "client_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "domain_uuid": "dddddddd-1111-2222-3333-eeeeeeee",
    "expired_in": 14763877,
    "refresh_token": "",
    "token": "eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=",
    "token_type": "Bearer",
    "user_id": "avavavav-1111-2222-3333-eeeeeeee"
  }
}
```

PBX API sử dụng API Token để xác thực truy cập tới API. API Token bạn lấy từ service thông qua {{API_KEY}} được Tổng đài cung cấp.

Tất cả các API của PBX đều yêu cầu user cung cấp Token trong header giống phía dưới.

`Authorization: Bearer {TOKEN}`

<aside class="notice">
Bạn vui lòng thay đổi <code>{TOKEN}</code> bằng token đã lấy được.
</aside>

### HTTP Request

`POST http://{{API_HOST}}/v2/auth/token`

### Body

| Parameter | Mô tả                                  |
| --------- | -------------------------------------- |
| api_key   | api_key có trong thông tin của account |

# Event

## Get Events

```shell
curl -L -X GET 'http://{{API_HOST}}/v1/event' \
-H 'Authorization: Bearer {{TOKEN}}'
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

`GET http://{{API_HOST}}/v1/event`

## Create Events

```shell
curl -L -X POST 'http://{{API_HOST}}/v1/event' \
-H 'Authorization: Bearer {{TOKEN}}' \
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

`POST http://{{API_HOST}}/v1/event`

### Body

| Parameter       | Mô tả                                          |
| --------------- | ---------------------------------------------- |
| callback_url    | Domain Url mà tổng đài sẽ hook dữ liệu tới     |
| callback_apikey | ApiKey hoặc access token của domain (optional) |
| event           | SIP Call Event                                 |

### SIP Call Event

| Parameter | Mô tả                              |
| --------- | ---------------------------------- |
| hangup    | Sự kiện khi cuộc gọi bị ngắt, huỷ  |
| ringing   | Sự kiện khi cuộc gọi được khởi tạo |
| answered  | Sự kiện khi cuộc gọi được nhấc máy |
| cdr       | Sự kiện sau khi cdr được tạo xong  |

<aside class="danger">Nếu bạn tạo event nằm ngoài các event ở trên, hệ thống sẽ không nhận diện được nên sẽ không hook data về.</aside>
<aside class="warning">Nếu bạn cần hook event ngoài các event ở trên, vui lòng gửi mail hỗ trợ.</aside>

## Delete Event

```shell
curl -L -X DELETE 'http://{{API_HOST}}/v1/event/eeeeeeee-1111-2222-3333-eeeeeeee' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "deleted": true
}
```

API dùng để xoá một event_domain.

### HTTP Request

`DELETE http://{{API_HOST}}/v1/event/<ID>`

### URL Parameters

| Parameter | Mô tả        |
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

### Mô tả

| Parameter     | Mô tả                                                          |
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
| status        | Trạng thái cuộc gọi. (Riêng sự kiện cdr)                       |

## Note\*

Một số thông tin cần lưu ý khi tích hợp event:

- Event ringing, answered, hangup là các event theo luồng của một cuộc gọi.
- Event cdr là event sẽ gửi tới webhook sau khi tổng đài cập nhật CDR.
- Đối với các case outbound: call_id của các event sẽ map giống nhau do tồn tại trên một cuộc gọi.
- ĐốI với các case inbound:
  - Trường hợp 1 mobile - 1 extension: call_id của các event sẽ map giống nhau do tồn tại trên một cuộc gọi.
  - Trường hợp 1 mobile - nhiều extension (Ringing, Queue, IVR): call_id của các event ringing, answered, hangup sẽ giống nhau. call_id của event cdr sẽ khác nhau. Do call_id của các event ringing, answered, hangup là call_id xử lý của cuộc gọi trên từng extension, call_id của event cdr là call_id của cả một luồng inbound (Ringing, Queue, IVR).

# CDRs - Call Detail Records

Lịch sử cuộc gọi

## CDR Mapping

### Default Mapping

| Thông tin     | Mô tả                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------- |
| id            | Id của CDR                                                                                  |
| sip_call_id   | call_id trong bản tin SIP                                                                   |
| cause         | Trạng thái cuộc gọi dựa theo mã phản hồi giao thức SIP. Vd: NORMAL_CLEARING, NO_ANSWER, ... |
| duration      | Thời hạn thực hiện cuộc gọi.                                                                |
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
| customer_id   | Mã khách hàng                                                                               |

### Flex Mapping

| Thông tin   | Mô tả                                                |
| ----------- | ---------------------------------------------------- |
| cdrId       | Id của cuộc gọi.                                     |
| sip_call_id | call_id trong bản tin SIP                            |
| customerId  | Mã khách hàng                                        |
| extensionId | Số máy lẻ (username) của agent sẽ tiếp nhận cuộc gọi |
| recordFile  | Đường dẫn file ghi âm cuộc gọi                       |
| to          | Mã khách hàng                                        |
| dialAt      | Thời gian bắt đầu cuộc gọi                           |
| ringAt      | Thời gian bắt đầu có chuông                          |
| answeredAt  | Thời gian khi cuộc gọi kết nối                       |
| hangupAt    | Thời gian kết thúc cuộc gọi                          |
| duration    | Thời hạn thực hiện cuộc gọi.                         |
| direction   | Chiều cuộc gọi (inbound, outbound, local)            |
| status      | Trạng thái cuộc gọi                                  |

| Status        | Mô tả                                                                                |
| ------------- | ------------------------------------------------------------------------------------ |
| answered      | Có kết nối và nói chuyện với khách hàng                                              |
| busy          | Máy bận hoặc khách hàng bấm từ chối trả lời                                          |
| no-answered   | Outbound - Khách hàng không nghe máy, để hết chuông. Inbound - agent để hết chuông   |
| cancel        | Outbound - agent ngắt máy trước khi kết nối. Inbound - agent ngắt máy                |
| not-available | Cuộc gọi báo thuê bao không liên lạc được, chế độ máy bay                            |
| invalid       | Không hợp lệ, cuộc gọi bị lỗi, (Ví dụ: click-to-call agent không nghe máy, ngắt máy) |

## Get CDRs

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/cdr?' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Default response trả về:

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
      "status": "ANSWERED",
      "customer_id": "KH1"
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
      "status": "BUSY",
      "customer_id": "KH1"
    },
    ...
  ],
  "limit": 10,
  "offset": 10,
  "total": 35
}
```

> Flex response trả về:

```json
{
  "data": [
    {
      "cdrId": "0c1d6ff3-c9b0-4fd1-8f5c-7be53f5f6973",
      "sip_call_id": "80bea701-49de-123a-89ab-fa163e3b9b38",
      "recordFile": "http://recording.demo/0c1d6ff3-c9b0-4fd1-8f5c-7be53f5f6973",
      "extensionId": "101",
      "to": "0899888999",
      "hangupAt": "2021-06-17 14:14:56",
      "answeredAt": "2021-06-17 14:14:33",
      "ringAt": "2021-06-17 14:14:36",
      "dialAt": "2021-06-17 14:14:31",
      "status": "answered",
      "duration": 25,
      "direction": "outbound",
      "billsec": 23,
      "customerId": "KH1"
    },
    {
      "cdrId": "b689a2e3-0e12-47f4-8dc7-516c1dd10828",
      "sip_call_id": "c92973c8-49e1-123a-89ab-fa163e3b9b38",
      "recordFile": "",
      "extensionId": "101",
      "to": "0899888999",
      "hangupAt": "2021-06-17 14:38:10",
      "answeredAt": "",
      "ringAt": "2021-06-17 14:38:05",
      "dialAt": "2021-06-17 14:38:01",
      "status": "busy",
      "duration": 9,
      "direction": "outbound",
      "billsec": 0,
      "customerId": "KH1"
    },
    ...
  ],
  "limit": 10,
  "offset": 10,
  "total": 35
}
```

Trả về danh sách lịch sử cuộc gọi.
API CDRs sử dụng 2 cơ chế để trả về dữ liệu.
Pagination: Phân trang.
Scroll: Cuộn trang. (Mặc định)

Nếu user cung cấp trong param: page - Số trang, limit - số lượng trả về thì API sẽ trả về dữ liêu theo cơ chế Pagination.

### HTTP Request

`GET http://{{API_HOST}}/v2/cdr`

### Query Parameters

| Parameter  | Mô tả                                                                        | Example                             |
| ---------- | ---------------------------------------------------------------------------- | ----------------------------------- |
| start_date | Tìm kiếm cdrs theo khoảng thời gian (Khởi tạo cuộc gọi)                      | 2021-02-18 hoặc 2021-02-18 17:20:58 |
| end_date   | Tìm kiếm cdrs theo khoảng thời gian (Khởi tạo cuộc gọi)                      | 2021-02-19 hoặc 2021-02-19 00:00:00 |
| duration   | Thời hạn của cuộc gọi                                                        | 10                                  |
| extension  | Cuộc gọi từ extension nào                                                    | 101                                 |
| phone      | Từ hoặc tới số điện thoại nào                                                | 0899888999                          |
| direction  | Chiều cuộc gọi (inbound, outbound, local)                                    | outbound                            |
| limit      | Số lượng record trả về                                                       | 50                                  |
| offset     | Vị trí bắt đầu khi query. (offset sẽ thay thế page nếu có data) (Pagination) | 0                                   |

## Get a Specific CDR

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/cdr/01b7d166-b564-42ec-80a1-4ad343225934' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Default Response trả về:

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
  "status": "BUSY",
  "customer_id": "KH1"
}
```

> Flex response trả về:

```json
{
  "cdrId": "0c1d6ff3-c9b0-4fd1-8f5c-7be53f5f6973",
  "sip_call_id": "80bea701-49de-123a-89ab-fa163e3b9b38",
  "recordFile": "http://recording.demo/0c1d6ff3-c9b0-4fd1-8f5c-7be53f5f6973",
  "extensionId": "101",
  "to": "0899888999",
  "hangupAt": "2021-06-17 14:14:56",
  "answeredAt": "2021-06-17 14:14:33",
  "ringAt": "2021-06-17 14:14:36",
  "dialAt": "2021-06-17 14:14:31",
  "status": "answered",
  "duration": 25,
  "direction": "outbound",
  "billsec": 23,
  "customerId": "KH1"
}
```

API dùng để lấy một cdr cụ thể.
Id có thể id của CDR hoặc sip_call_id trong bản tin

### HTTP Request

`GET http://{{API_HOST}}/v2/cdr/<ID>`

### URL Parameters

| Parameter | Mô tả                                     |
| --------- | ----------------------------------------- |
| ID        | Id của CDR hoặc sip_call_id trong bản tin |

# Click-to-call

## Synchronous - GET

```shell
curl -L -X GET 'http://{{API_HOST}}/v1/click2call?ext=101&phone=0899098899' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

Nếu Extension đã login thì API Click-to-call Synchronous sẽ chờ tới khi extension nhấc máy hoặc ngắt máy.

<aside class="notice">
Đầu số hotline dùng để gọi ra ngoài bạn vui lòng liên hệ team TEL4VN để được cung cấp và cài đặt.
</aside>

### HTTP Request

`GET http://{{API_HOST}}/v1/click2call?ext=<EXTENSION>&phone=<PHONE>`

### URL Parameters

| Parameter       | Mô tả                                                     | Required |
| --------------- | --------------------------------------------------------- | -------- |
| ext             | Extension thực hiện cuộc gọi                              | true     |
| phone           | Số điện thoại sẽ được gọi tới                             | true     |
| src_cid_name    | Tên hiển thị trên máy extension. Default: Click2Call      | false    |
| src_cid_number  | Số hiển thị trên máy extension. Default: Số của extension | false    |
| dest_cid_name   | Tên của số điện thoại sẽ chèn vào bản tin SIP             | false    |
| dest_cid_number | Số điện thoại sẽ chèn vào bản tin SIP                     | false    |
| auto_answer     | Tự động nhấc máy phía extension. Default: false           | false    |
| hotline         | Đầu số hotline để thực hiện cuộc gọi ra ngoài             | false    |
| customer_id     | Mã khách hàng                                             | false    |

## Asynchronous - GET

```shell
curl -L -X GET 'http://{{API_HOST}}/v1/click2call/async?ext=101&phone=0899098899' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "message": "User not registered extension with Softphone or IP Phone",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

API Click-to-call Asynchronous sẽ không chờ tới khi extension nhấc máy hoặc ngắt máy, mà sẽ trả về call_id nếu extension đã login và trả về mã lỗi nếu extension không login.

<aside class="notice">
Đầu số hotline dùng để gọi ra ngoài bạn vui lòng liên hệ team TEL4VN để được cung cấp và cài đặt.
</aside>

### HTTP Request

`GET http://{{API_HOST}}/v1/click2call/async?ext=<EXTENSION>&phone=<PHONE>`

### URL Parameters

| Parameter       | Mô tả                                                     | Required |
| --------------- | --------------------------------------------------------- | -------- |
| ext             | Extension thực hiện cuộc gọi                              | true     |
| phone           | Số điện thoại sẽ được gọi tới                             | true     |
| src_cid_name    | Tên hiển thị trên máy extension. Default: Click2Call      | false    |
| src_cid_number  | Số hiển thị trên máy extension. Default: Số của extension | false    |
| dest_cid_name   | Tên của số điện thoại sẽ chèn vào bản tin SIP             | false    |
| dest_cid_number | Số điện thoại sẽ chèn vào bản tin SIP                     | false    |
| auto_answer     | Tự động nhấc máy phía extension. Default: false           | false    |
| hotline         | Đầu số hotline để thực hiện cuộc gọi ra ngoài             | false    |
| customer_id     | Mã khách hàng                                             | false    |

## Asynchronous - POST

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/click2call' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
  "id": "KH1",
  "mobile": "0899098899",
  "agent": "101"
}'
```

> Response trả về:

```json
{
  "code": "0",
  "call_id": "50038035-0717-40f1-b7af-eb92021b012e",
  "status": "success"
}
```

> Error Response trả về:

```json
{
  "code": "1",
  "status": "fail",
  "message": "User not registered extension with Softphone or IP Phone",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

API Click-to-call Asynchronous sẽ không chờ tới khi extension nhấc máy hoặc ngắt máy, mà sẽ trả về call_id nếu extension đã login và trả về mã lỗi nếu extension không login.

### HTTP Request

`POST http://{{API_HOST}}/v2/click2call`

### Body

| Parameter | Mô tả                                |
| --------- | ------------------------------------ |
| id        | ID của khách hàng.                   |
| mobile    | Số điện thoại cần gọi của khách hàng |
| agent     | Số máy lẻ của chuyên viên            |

## Error

| Parameter           | Mô tả                                         |
| ------------------- | --------------------------------------------- |
| USER_BUSY           | Người dùng hiện tại đang bận                  |
| USER_NOT_REGISTERED | Người dùng chưa login Softphone hoặc IP Phone |

# Call

## List Calls

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/call' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": [
      {
        "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
        "context": "test.tel4vn.com",
        "created_time": "2021-06-08 17:37:37",
        "destination": "0899123456",
        "direction": "outbound",
        "ip": "1.2.3.4",
        "source": "101",
        "state": "CS_EXECUTE"
      },
      ...
  ],
  "total": 1
}
```

API dùng để lấy danh sách các cuộc gọi theo thời gian thực.

### HTTP Request

`GET http://{{API_HOST}}/v2/call`

## Transfer a call

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/call/01b7d166-b564-42ec-80a1-4ad343225934/transfer' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện chuyển cuộc gọi sang extension khác.

### HTTP Request

`POST http://{{API_HOST}}/v2/call/<CALL_ID>/transfer`

### Body

| Parameter | Mô tả                 |
| --------- | --------------------- |
| ext       | Ext nhận cuộc gọi mới |

## Listen a call

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/call/01b7d166-b564-42ec-80a1-4ad343225934/listen' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện nghe lén cuộc gọi của một extension khác.

### HTTP Request

`POST http://{{API_HOST}}/v2/call/<CALL_ID>/listen`

### Body

| Parameter | Mô tả             |
| --------- | ----------------- |
| ext       | Ext nhận cuộc gọi |

## Whisper a call

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/call/01b7d166-b564-42ec-80a1-4ad343225934/whisper' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện cuộc gọi với extension, mobile sẽ không nghe được.

### HTTP Request

`POST http://{{API_HOST}}/v2/call/<CALL_ID>/whisper`

### Body

| Parameter | Mô tả                 |
| --------- | --------------------- |
| ext       | Ext nhận cuộc gọi mới |

## Barge a call

```shell
curl -L -X POST 'http://{{API_HOST}}/v2/call/01b7d166-b564-42ec-80a1-4ad343225934/barge' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json' \
--data-raw '{
    "ext" : "101"
}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "ext": "101"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện cuộc gọi 3 bên với extension và mobile.

### HTTP Request

`POST http://{{API_HOST}}/v2/call/<CALL_ID>/barge`

### Body

| Parameter | Mô tả                 |
| --------- | --------------------- |
| ext       | Ext nhận cuộc gọi mới |

# Autodialer

## Nhận dữ liệu queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v2/autodialer/queue' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autodialer",
  "precall_ratio": "150",
  "max_recall_count": "2",
  "queue_agents": "5001",
  "customers": [
    {
      "id": "TEL4VN_Test",
      "mobiles": ["0899123456"],
    }
  ]
}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

> Error Response trả về:

```json
{
  "message": "campaign is invalid"
}
```

API này nhằm mục đích nhận thông tin về queue để tiến hành tự động gọi ra.

### HTTP Request

`POST https://{{API_HOST}}/v2/autodialer/queue`

### Body

> Sample data:

```json
{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autodialer",
  "precall_ratio": "150",
  "max_recall_count": "2",
  "queue_agents": "5001",
  "customers": [
    {
      "id": "TEL4VN_Test",
      "mobiles": ["0899123456"]
    }
  ]
}
```

| Parameter         | Description                                      | Required |
| ----------------- | ------------------------------------------------ | -------- |
| campaign_id       | Id của campaign                                  | x        |
| queue_code        | Mã queue                                         | x        |
| precall_ratio     | Tỉ lệ thực hiện cuộc gọi đựa trên số lượng agent | x        |
| max_recall_count  | Số lượng cuộc gọi lại nếu không thành công       | x        |
| customers.id      | ID của khách hàng                                | x        |
| customers.mobiles | Danh sách các số điện thoại của khách hàng       | x        |

## Stop Queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v2/autodialer/queue/stop' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autodialer"
}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

> Error Response trả về:

```json
{
  "message": "queue not found"
}
```

API này nhằm mục đích yêu cầu tạm dừng một queue đang thực hiện.

### HTTP Request

`POST https://{{API_HOST}}/v2/autodialer/queue/stop`

### Body

> Sample data:

```json
{
  "queue_code": "Autodialer"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

## Delete Queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v2/autodialer/queue/delete' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autodialer"
}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

> Error Response trả về:

```json
{
  "message": "queue not found"
}
```

API này nhằm mục đích yêu cầu thu hồi (xoá) một queue sau khi đã tạm dừng.

### HTTP Request

`POST https://{{API_HOST}}/v2/autodialer/queue/delete`

### Body

> Sample data:

```json
{
  "queue_code": "Autodialer"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

## Start Queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v2/autodialer/queue/start' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autodialer"
}'
```

> Response trả về:

```json
{
  "message": "successfully"
}
```

> Error Response trả về:

```json
{
  "message": "queue not found"
}
```

API này nhằm mục đích yêu cầu tiếp tục một queue đang tạm dừng.

### HTTP Request

`POST https://{{API_HOST}}/v2/autodialer/queue/start`

### Body

> Sample data:

```json
{
  "queue_code": "Autodialer"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

# Autocall

## Nhận dữ liệu queue

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1",
    "template": "IVR-1",
    "concurrent_call" : 5,
    "max_recall_count": 3,
    "min_recall_duration": 3,
    "customers": [
        {
            "id": "TEL4VN_Test",
            "mobiles": [
                "0982596021"
            ],
            "contract_number": "HD123456",
            "upcoming_due_date": "2021-07-13",
            "upcoming_amount": 5000000,
            "due_date": "2021-07-13",
            "dpd": 5,
            "number_of_ovd_inst": 6,
            "total_ovd_amount": 3211100
        }
    ]
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": {
    "fail": [],
    "success": ["TEL4VN_Test"]
  }
}
```

> Error Response trả về:

```json
{
  "code": 400,
  "content": "import fail",
  "data": {
    "fail": [],
    "success": []
  }
}
```

API này nhằm mục đích nhận thông tin về queue để tiến hành tự động gọi ra theo kịch bản.

### HTTP Request

`POST http://{{API_HOST}}/v2/autocall/queue`

### Body

> Sample data:

```json
{
  "queue_code": "Autocall-Q1",
  "template": "IVR-1",
  "concurrent_call": 5,
  "max_recall_count": 3,
  "min_recall_duration": 3,
  "customers": [
    {
      "id": "TEL4VN_Test",
      "mobiles": ["0982596021"],
      "contract_number": "HD123456",
      "upcoming_due_date": "2021-07-13",
      "upcoming_amount": 5000000,
      "due_date": "2021-07-13",
      "dpd": 5,
      "number_of_ovd_inst": 6,
      "total_ovd_amount": 3211100
    }
  ]
}
```

| Parameter           | Description                                           | Required |
| ------------------- | ----------------------------------------------------- | -------- |
| queue_code          | Mã queue                                              | x        |
| template            | Kịch bản dùng để                                      | x        |
| concurrent_call     | Số lượng cuộc gọi đồng thời                           |          |
| max_recall_count    | Số lượng cuộc gọi lại nếu không thành công            |          |
| min_recall_duration | Nếu thời gian tính từ lúc nhấc máy nhỏ hơn sẽ gọi lại |          |
| customers.id        | ID của khách hàng                                     | x        |
| customers.mobiles   | Danh sách các số điện thoại của khách hàng            | x        |

## Import danh sách chặn

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue/dnc' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1",
    "customers": [
        {
          "id": "KH_01"
        },
        {
          "id": "KH_02"
        },
        {
          "id": "KH_03"
        }
    ]
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully"
}
```

> Error Response trả về:

```json
{
  "code": 404,
  "content": "queue not found"
}
```

API này nhằm mục đích cung cấp danh sách các khách hàng cần chặn cuộc gọi lên tổng đài. Tổng đài sẽ loại/bỏ qua các khách hàng này khi quay số nếu chưa quay đến. Nếu đã quay rồi hoặc đang trong cuộc gọi thì giữ nguyên.

### HTTP Request

`POST http://{{API_HOST}}/v2/autocall/queue/dnc`

### Body

> Sample data:

```json
{
  "queue_code": "Autocall-Q1",
  "customers": [
    {
      "id": "KH_01"
    },
    {
      "id": "KH_02"
    },
    {
      "id": "KH_03"
    }
  ]
}
```

| Parameter    | Description       | Required |
| ------------ | ----------------- | -------- |
| queue_code   | Mã queue          | x        |
| customers.id | ID của khách hàng | x        |

## Stop Queue

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue/stop' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1"
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully"
}
```

> Error Response trả về:

```json
{
  "code": 404,
  "content": "queue not found"
}
```

API này nhằm mục đích yêu cầu tạm dừng một queue đang thực hiện.

### HTTP Request

`POST http://{{API_HOST}}/v2/autocall/queue/stop`

### Body

> Sample data:

```json
{
  "queue_code": "Autocall-Q1"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

## Delete Queue

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue/delete' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1"
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully"
}
```

> Error Response trả về:

```json
{
  "code": 404,
  "content": "queue not found"
}
```

API này nhằm mục đích yêu cầu thu hồi (xoá) một queue sau khi đã tạm dừng.

### HTTP Request

`POST http://{{API_HOST}}/v2/autocall/queue/delete`

### Body

> Sample data:

```json
{
  "queue_code": "Autocall-Q1"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

## Start Queue

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue/start' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1"
}'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully"
}
```

> Error Response trả về:

```json
{
  "code": 404,
  "content": "queue not found"
}
```

API này nhằm mục đích yêu cầu tiếp tục một queue đang tạm dừng.

### HTTP Request

`POST http://{{API_HOST}}/v2/autocall/queue/start`

### Body

> Sample data:

```json
{
  "queue_code": "Autocall-Q1"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

# User

## Get User Status

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/user/{{user_id}}/status' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": {
    "agent_name": "Test User 01",
    "ext_loggedin": "false",
    "extension": "101",
    "user_uuid": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "username": "test.user01"
  }
}
```

> Error response:

```json
{
  "code": 404,
  "content": "user is not existed",
  "error": "Not Found"
}
```

API dùng để lấy trạng thái của người dùng trên hệ thống.

| Thông tin         | Mô tả                                            |
| ----------------- | ------------------------------------------------ |
| code              | Mã code HTTP trả về                              |
| content           | Thông báo                                        |
| data              | Data trả về                                      |
| data.agent_name   | Tên agent                                        |
| data.extension    | Extension của user                               |
| data.ext_loggedin | Extension này có đang được log in trên softphone |
| data.user_uuid    | Id của user                                      |
| data.username     | Username của user                                |

### HTTP Request

`GET http://{{API_HOST}}/v2/customer/user/{{user_id}}/status`

| Parameter | Description   | Example                                           |
| --------- | ------------- | ------------------------------------------------- |
| user_id   | Id người dùng | test.user01 hoặc aaaaaaaa-1111-2222-3333-eeeeeeee |

# Customer

## Get List Customers

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/customer' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": [
    {
      "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "contract_number": "ABC123",
      "created_at": "2021-07-15T12:50:23.805319Z",
      "customer_code": "KH01",
      "customer_id": "dddddddd-1111-2222-3333-eeeeeeee",
      "customer_name": "Khach Hang 01",
      "phone_number": "0899888998",
      "status": "NEW",
      "updated_at": "0001-01-01T00:00:00Z"
    },
    {
      "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "contract_number": "DEF456",
      "created_at": "2021-07-15T12:50:24.565573Z",
      "customer_code": "KH01",
      "customer_id": "gggggggg-1111-2222-3333-eeeeeeee",
      "customer_name": "Khach Hang 02",
      "phone_number": "0899888999",
      "status": "NEW",
      "updated_at": "0001-01-01T00:00:00Z"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 200
}
```

API dùng để lấy danh sách khách hàng đã upload.

| Thông tin            | Mô tả                            |
| -------------------- | -------------------------------- |
| code                 | Mã code HTTP trả về              |
| content              | Thông báo                        |
| data                 | Data trả về                      |
| data.campaign_id     | Id của chiến dịch                |
| data.contract_number | Mã hợp đồng                      |
| data.customer_code   | Mã khách hàng                    |
| data.customer_id     | Id khách hàng                    |
| data.customer_name   | Tên khách hàng                   |
| data.phone_number    | Số điện thoại khách hàng         |
| data.status          | Trạng thái khách hàng            |
| data.created_at      | Thời gian khởi tạo               |
| data.updated_at      | Thời gian được cập nhật gần nhất |

| Status      | Mô tả                                       |
| ----------- | ------------------------------------------- |
| new         | Khách hàng vừa được khởi tạo mới            |
| queue       | Đã được đưa vào hàng chờ để tiến hành gọi   |
| answered    | Có kết nối và nói chuyện với khách hàng     |
| busy        | Máy bận hoặc khách hàng bấm từ chối trả lời |
| no-answered | Khách hàng không nghe máy, để hết chuông    |
| cancel      | Phía extension, agent chủ động ngắt máy     |
| fail        | Khi tổng đài nhận mã lỗi từ telco           |
| unknown     | Trạng thái lỗi không xác định               |

### HTTP Request

`GET http://{{API_HOST}}/v2/customer`

### Query Parameters

| Parameter   | Description                       | Example                          |
| ----------- | --------------------------------- | -------------------------------- |
| campaign_id | Id Chiến dịch khách hàng thuộc về | aaaaaaaa-1111-2222-3333-eeeeeeee |
| limit       | Số lượng record trả về            | 50                               |
| offset      | Vị trí bắt đầu khi query          | 0                                |

## Get Specific Customer

```shell
curl -L -X GET 'http://{{API_HOST}}/v2/customer/dddddddd-1111-2222-3333-eeeeeeee' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "code": 200,
  "content": "successfully",
  "data": {
    "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "contract_number": "ABC123",
    "created_at": "2021-07-15T12:50:23.805319Z",
    "customer_code": "KH01",
    "customer_id": "dddddddd-1111-2222-3333-eeeeeeee",
    "customer_name": "Khach Hang 01",
    "phone_number": "0899888998",
    "status": "NEW",
    "updated_at": "0001-01-01T00:00:00Z"
  }
}
```

> Error response:

```json
{
  "code": 404,
  "content": "Not Found",
  "error": "Not Found"
}
```

API dùng để lấy thông tin của một khách hàng cụ thể.

| Thông tin            | Mô tả                            |
| -------------------- | -------------------------------- |
| code                 | Mã code HTTP trả về              |
| content              | Thông báo                        |
| data                 | Data trả về                      |
| data.campaign_id     | Id của chiến dịch                |
| data.contract_number | Mã hợp đồng                      |
| data.customer_code   | Mã khách hàng                    |
| data.customer_id     | Id khách hàng                    |
| data.customer_name   | Tên khách hàng                   |
| data.phone_number    | Số điện thoại khách hàng         |
| data.status          | Trạng thái khách hàng            |
| data.created_at      | Thời gian khởi tạo               |
| data.updated_at      | Thời gian được cập nhật gần nhất |

| Status      | Mô tả                                       |
| ----------- | ------------------------------------------- |
| new         | Khách hàng vừa được khởi tạo mới            |
| queue       | Đã được đưa vào hàng chờ để tiến hành gọi   |
| answered    | Có kết nối và nói chuyện với khách hàng     |
| busy        | Máy bận hoặc khách hàng bấm từ chối trả lời |
| no-answered | Khách hàng không nghe máy, để hết chuông    |
| cancel      | Phía extension, agent chủ động ngắt máy     |
| fail        | Khi tổng đài nhận mã lỗi từ telco           |
| unknown     | Trạng thái lỗi không xác định               |

### HTTP Request

`GET http://{{API_HOST}}/v2/customer/{{customer_id}}`

| Parameter   | Description   | Example                          |
| ----------- | ------------- | -------------------------------- |
| customer_id | Id khách hàng | aaaaaaaa-1111-2222-3333-eeeeeeee |

## Post Customer

```shell
curl --location --request POST 'http://{{API_HOST}}/v2/autocall/queue' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
    "customer_name": "Khach Hang 01",
    "customer_code": "KH01",
    "phone_number": "0899888998",
    "contract_number": "ABC123"
}'
```

> Response trả về:

```json
{
  "code": 201,
  "content": "successfully",
  "id": "dddddddd-1111-2222-3333-eeeeeeee"
}
```

> Error Response trả về:

```json
{
  "code": 400,
  "content": [
    {
      "phone_number": "Does not match pattern '^(84|0[3|5|7|8|9])+([0-9]{8})$'"
    }
  ],
  "error": "Bad Request"
}
```

API dùng để nhận thông tin của một khách hàng cụ thể. 7 ngày tính từ ngày thông tin được gửi sang, hệ thông sẽ thực hiện autodialer khách hàng cho agent.

### HTTP Request

`POST http://{{API_HOST}}/v2/customer`

### Body

> Sample data:

```json
{
  "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
  "customer_name": "Khach Hang 01",
  "customer_code": "KH01",
  "phone_number": "0899888998",
  "contract_number": "ABC123"
}
```

| Parameter       | Description                  | Required |
| --------------- | ---------------------------- | -------- |
| campaign_id     | Id chiến dịch                | x        |
| customer_name   | Tên khách hàng               |          |
| customer_code   | Mã khách hàng                | x        |
| phone_number    | Số điện thoại của khách hàng | x        |
| contract_number | Mã hợp đồng                  | x        |
