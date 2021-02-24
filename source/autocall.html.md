---
title: Autocall API Reference

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

Xin chào! Đây là bộ API Autocall, OTP, Voice call Text-to-speech của TEL4VN.

Nếu bạn cần thông tin để tích hợp hoặc cần hỗ trợ vui lòng liên hệ mail: support@tel4vn.com.

Các thông tin như {API_HOST}, {API_KEY}, tài khoản admin, tài khoản test sẽ được bên phía Tổng đài cung cấp.

# Authentication

## Get Access Token

```shell
curl --location --request POST 'http://{API_HOST}/api/v1/users/generate_access_token' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{ 
"secret_key": "30511f666-f888-411b-a6e0-11111fb6gg7a"
 }'
```

> Response trả về:

```json
{ 
  "data": { 
  "id": "30511f666-f888-411b-a6e0-11111fb6gg7a",
  "user_id": "f461", 
  "access_token": "1ebe8f88MzA1MTNkZjMtZjc3My00MTmY", 
  "refresh_token": "", 
  "expire_at": 15552000, 
  "token_type": "Bearer", 
  "scope": "member"
  } 
}
```

Autocall API sử dụng API Token để xác thực truy cập tới API. API Token bạn lấy từ service thông qua {API_KEY} được Tổng đài cung cấp.

Tất cả các API của Autocall đều yêu cầu user cung cấp Token trong header giống phía dưới.

`Authorization: {TOKEN}`

<aside class="notice">
Bạn vui lòng thay đổi <code>{TOKEN}</code> bằng token đã lấy được.
</aside>

### HTTP Request

`POST http://{API_HOST}/api/v1/users/generate_access_token`

### Body

| Parameter  | Description         |
| ---------- | ------------------- |
| secret_key | Secret key được cấp |

# OTP

## Run OTP

```shell
curl --location --request POST 'http://{API_HOST}/api/v2/campaigns/1/otp' \ 
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{ 
    "caller": "0123456789", 
    "callees": [ "0987654321" ], 
    "params": { 
    "template_name": "template_otp_1", 
    "code_otp": "M43TUI" 
  }, 
  "callback_url": "https://demo.webhook.com", 
  "callback_apikey": "api_key_123" 
}'
```
> Response trả về:

```json
{ 
  "data": { 
    "campaign_id": "1",
    "results": { 
    "fail": [
        { 
        "1234567890": "Wrong Format" 
        }
      ], 
    "success": [ 
      "1_1_1_5_cb76e372-b798-4a88-9c8a-410740f0300d_0987654321" 
      ] 
    } 
  } 
}
```

API thực hiện cuộc gọi OTP

### HTTP Request

`GET http://{API_HOST}/api/v2/campaigns/{campaignId}/otp`

### URL Parameters

| Parameter  | Description          |
| ---------- | -------------------- |
| campaignId | Campaign Id được cấp |

### Body

> Sample data:

```json
{ 
  "caller": "0123456789", 
  "callees": [ 
    "0987654321",
    "1234567890"
  ],
  "params": {
    "template_name": "template_otp_1", 
    "code_otp": "M43TUI" 
  }, 
  "callback_url": "https://demo.webhook.com", 
  "callback_apikey": "api_key_123" 
}

```

| Parameter            | Description                                |
| -------------------- | ------------------------------------------ |
| caller               | Domain Url mà tổng đài sẽ hook dữ liệu tới |
| callees              | Số điện thoại nhận cuộc gọi                |
| params               |                                            |
| params.template_name | Kịch bản đọc code OTP                      |
| params.code_otp      | Code OTP                                   |
| callback_url         | SIP Call Event                             |
| callback_apikey      | API Key của webhook (optional)             |

# Text-To-Speech

## Import TTS Voice

```shell
curl --location --request POST 'http://{API_HOST}/api/v2/campaigns/1/voice/import' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "thong_bao_no_cuoc_01",
    "content": "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}"
}'
```
> Response trả về:

```json
{ 
  "data": { 
    "campaign_id": "1",
    "results": { 
    "fail": [
        { 
        "1234567890": "Wrong Format" 
        }
      ], 
    "success": [ 
      "1_1_1_5_cb76e372-b798-4a88-9c8a-410740f0300d_0987654321" 
      ] 
    } 
  } 
}
```

Tạo kịch bản Text-To-Speech

### HTTP Request

`GET http://{API_HOST}/api/v2/campaigns/{campaignId}/voice/import`

### URL Parameters

| Parameter  | Description          |
| ---------- | -------------------- |
| campaignId | Campaign Id được cấp |

### Body

> Sample data:

```json
{
    "name": "thong_bao_no_cuoc_01",
    "content": "Chào bạn {{key_field_1}} vui lòng thanh toán khoản nợ {{key_field_2}} trước ngày {{key_field_3}}"
}
```

| Parameter       | Description                               |
| --------------- | ----------------------------------------- |
| name            | Tên kịch bản muốn tạo. (Phải là duy nhất) |
| content         | Nội dung của kịch bản                     |
| key_field_1,2,3 | Các từ khoả trong kịch bản                |


key_field: tương ứng với đoạn text import vào campaign người dùng định nghĩa.

Ví dụ:
<ul>
	<li>key_field_1 là {{customer_name}}</li>
	<li>key_field_2 là {{due_amount}}</li>
	<li>key_field_3 là {{due_date}}</li>
</ul>

Đoạn content sẽ là : "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}"

## Run TTS

```shell
curl --location --request POST 'http://{API_HOST}/api/v2/campaigns/1/voice/import' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "caller": "1234567890",
    "callees": [
        "0987654321",
        "1234567890"
    ],
    "params": {
        "template_name": "thong_bao_no_cuoc_1",
        "customer_name": "Nguyễn Văn A",
        "due_amount": "10.410.000",
        "due_days": "#15"
    },
    "callback_url": "https://demo.webhook.com",
    "callback_apikey": "api_key_123"
}
```
> Response trả về:

```json
{ 
"data": { 
    "campaign_id": "1",
    "results": { 
    "fail": [
      { 
      "1234567890": "Wrong Format" 
      }
    ], 
    "success": [ 
      "1_1_1_5_cb76e372-b798-4a88-9c8a-410740f0300d_0987654321" 
      ] 
    } 
  } 
}

```

API thực hiện cuộc gọi Text-To-Speech

### HTTP Request

`GET http://{API_HOST}/api/v2/campaigns/{campaignId}/voice`

### URL Parameters

| Parameter  | Description          |
| ---------- | -------------------- |
| campaignId | Campaign Id được cấp |

### Body

> Sample data:

```json
{
    "caller": "1234567890",
    "callees": [
        "0987654321",
        "1234567890"
    ],
    "params": {
        "template_name": "thong_bao_no_cuoc_1",
        "customer_name": "Nguyễn Văn A",
        "due_amount": "10.410.000",
        "due_date": "30/1/2021"
    },
    "callback_url": "https://demo.webhook.com",
    "callback_apikey": "api_key_123"
}
```

| Parameter            | Description                                                         |
| -------------------- | ------------------------------------------------------------------- |
| caller               | Domain Url mà tổng đài sẽ hook dữ liệu tới                          |
| callees              | Số điện thoại nhận cuộc gọi                                         |
| params               |                                                                     |
| params.template_name | Kịch bản đọc code OTP                                               |
| params.customer_name | Các thông tin sẽ được đọc dựa vào từ khoá bạn đã tạo trong kịch bản |
| callback_url         | SIP Call Event                                                      |
| callback_apikey      | API Key của webhook (optional)                                      |


Một số lưu ý:
<ul>
  <li>Nếu muốn đọc đúng khoản tiền (Không đọc từng số) - truyền dữ liệu theo format: “10.410.000”</li>
  <li>Nếu muốn đọc từng số (Ví dụ số điện thoại) - truyền dữ liệu theo format: 123456789 hoặc “0987654321”</li>
  <li>Nếu muốn đọc thành số (Ví dụ 15 - mười lăm) - truyền dữ liệu theo format: “#15”</li>
  <li>Nếu giá trị của key field là ngày tháng năm - Yêu cầu format “dd/mm/yyyy”, ví dụ: “30/01/2021”</li>
</ul>

params.key_field: tương ứng với đoạn text import vào campaign người dùng đã định nghĩa.

Ví dụ:
<ul>
  <li>Kịch bản : "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}"</li>
  <li>Nội dung kịch bản sẽ là: “Chào bạn Nguyễn Văn A vui lòng thanh toán khoản nợ mười triệu bốn trăm mười nghìn trước ngày ba mươi tháng một năm hai không hai mươi mốt”</li>
</ul>

# Call Detail Report

Lịch sử cuộc gọi

## Attributes

| Attribute    | Description                             | Type     |
| ------------ | --------------------------------------- | -------- |
| id           | Call Id                                 | string   |
| campaign_id  | Campaign Id                             | string   |
| code_otp     | Code OTP - Null with failed calls       | object   |
| created_at   | Initiate the call at                    | datetime |
| started_at   | The time that callee has picked up call | datetime |
| ended_at     | The time that callee has hang up call   | datetime |
| duration     | Call duration                           | int      |
| keypress     | Keypress. Default: 127 - Hangup         | string   |
| phone_number | Phone numbers was received              | string   |
| user_id      | User Id                                 | string   |
| param        | Param that api call is received         | string   |
| type         | Call Type: OTP,TTS,DEFAULT              | string   |
| status       | Call Disposition.                       | string   |


| Call Type     | Description                                                                   |
| ------------- | ----------------------------------------------------------------------------- |
| FAIL          | Mã lỗi mặc định                                                               |
| ANSWERED      | Cuộc gọi thành công và được người dùng nhấc máy                               |
| NOANWSER      | Người dùng gác máy hoặc không nhấc máy                                        |
| BUSY          | Cuộc gọi bị báo bận                                                           |
| CONGESTION    | Cuộc gọi bị nghẽn                                                             |
| INVALIDNUMBER | Số điện thoại gọi ra không phù hợp                                            |
| ERROR         | Máy chủ nhận được yêu cầu không khớp với bất kỳ hộp thoại hoặc giao dịch nào. |
| CANCEL        | Người dùng gác máy (Trường hợp riêng của nhà mạng Mobiphone gọi Mobiphone)    |

## Get a Specific CDR

```shell
curl --location --request GET 'http://{API_HOST}/api/v2/report/1_1_1_5_cb76e372-b798-4a88-9c8a-410740f0300d_0987654321' \ 
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY'
```

> Response trả về:

```json
{ 
  "data": { 
    "campaign_id": "1", 
    "code_otp": "M43TUI", 
    "created_at": "2021-01-11 11:28:45", 
    "duration": 15, 
    "ended_at": "2021-01-11 11:29:09", 
    "id": "1_1_1_5_cb76e372-b798-4a88-9c8a-410740f0300d_0987654321", 
    "keypress": "127", 
    "param": "{\"customer_name\":\"Nguyễn Văn A\",\"due_amount\":\"10.410.000\",\"due_days\":\"#15\",\"template_name\":\"thong_bao_no_cuoc_1\"}",
    "phone_number": "0987654321", 
    "started_at": "2021-01-11 11:28:54", 
    "status": "ANSWERED", 
    "type": "TTS",
    "user_id": "5" 
  } 
}
```

API dùng để lấy một cdr cụ thể.

### HTTP Request

`GET http://{API_HOST}/api/v2/report/{callId}`

### URL Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| ID        | Call_id trong data trả về |
