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

Xin chào! Đây là bộ API Autocall, OTP, Voice call Text-to-speech của TEL4VN.

Nếu bạn cần thông tin để tích hợp hoặc cần hỗ trợ vui lòng liên hệ mail: support@tel4vn.com.

Các thông tin như {API_HOST}, {API_KEY}, tài khoản admin, tài khoản test sẽ được bên phía Tổng đài cung cấp.

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
    "user_id": "abeb99ea-7879-43ce-8d4e-a46ec0aca9ff",
    "access_token": "eyJhbGciOiJSU0EtT0FFUC0yNTYiLCJlbmMiOiJBMjU2R0NNIiwiaWF0IjoxNjEzNjMyNzc4fQ.dGhpcyBpcyB0ZXN0IGRhdGE=",
    "refresh_token": "",
    "expire_at": 1613636318,
    "token_type": "Bearer",
    "scope": ""
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

# Template - Kịch bản

Tốc độ đọc tăng dần từ 0 đến 2. Với 1 là tốc độ đọc bình thường và 2 là tốc độ đọc nhanh nhất.

Các giọng đọc kịch bản đang hỗ trợ

| Id         | Description            |
| ---------- | ---------------------- |
| north-m-01 | Miền Bắc 1 - Giọng nam |
| north-f-01 | Miền Bắc 1 - Giọng nữ  |
| north-m-02 | Miền Bắc 2 - Giọng nam |
| north-f-02 | Miền Bắc 2 - Giọng nữ  |
| south-m-03 | Miền Nam 1 - Giọng nam |
| south-f-03 | Miền Nam 1 - Giọng nữ  |

## Kịch bản OTP

```shell
curl --location --request POST 'http://{API_HOST}/v1/campaign/1/otp' \
--header 'Authorization: Bearer 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "otp_demo",
    "voice_tts" : "south-f-03",
    "speed_tts" : 1.2,
    "content": "Mã xác nhận của bạn là {{code_otp}} xin nhắc lại mã xác nhận của bạn là {{code_otp}}"
}'
```

> Response trả về:

```json
{
  "created": "true"
}
```

Tạo kịch bản OTP

### HTTP Request

`POST http://{API_HOST}/v1/campaign/{campaignId}/otp`

### URL Parameters

| Parameter  | Description          |
| ---------- | -------------------- |
| campaignId | Campaign Id được cấp |

### Body

> Sample data:

```json
{
  "name": "otp_demo",
  "voice_tts": "south-f-03",
  "speed_tts": 1.2,
  "content": "Mã xác nhận của bạn là {{code_otp}} xin nhắc lại mã xác nhận của bạn là {{code_otp}}"
}
```

| Parameter | Description                                    |
| --------- | ---------------------------------------------- |
| name      | Tên kịch bản muốn tạo. (Phải là duy nhất)      |
| content   | Nội dung của kịch bản                          |
| code_otp  | Là vị trí mã OTP sẽ đọc khi thực hiện cuộc gọi |
| voice_tts | Giọng đọc kịch bản                             |
| speed_tts | Tốc độ đọc kịch bản                            |

## Kịch bản TTS

```shell
curl --location --request POST ' http://{API_HOST}/v1/campaign/1/template' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "thong_bao_no_cuoc_01",
    "content": "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}, vui lòng bấm phím 1 nếu bạn đã thanh toán",
    "voice_type": "tts",
    "enable_option": true,
    "voice_tts": "south-f-03",
    "speed_tts": 1.2,
    "press_options": [
      {
        "press_num": 1,
        "type": "tts",
        "content": "Cám ơn {{customer_name}} đã thanh toán"
      }
    ],
    "exit_sound":{
        "enable": true,
        "voice_type":"tts",
        "content":"Cám ơn {{customer_name}} đã sử dụng"
    },
    "invalid_sound":{
        "enable": true,
        "voice_type":"tts",
        "content":"{{customer_name}} đã nhập sai"
    }
}'
```

> Response trả về:

```json
{
  "created": "true"
}
```

Tạo kịch bản Text-To-Speech

### HTTP Request

`POST http://{API_HOST}/v1/campaign/{campaignId}/template`

### URL Parameters

| Parameter  | Description          |
| ---------- | -------------------- |
| campaignId | Campaign Id được cấp |

### Body

> Sample data:

```json
{
  "name": "thong_bao_no_cuoc_01",
  "content": "Chào bạn {{key_field_1}} vui lòng thanh toán khoản nợ {{key_field_2}} trước ngày {{key_field_3}}",
  "voice_type": "tts",
  "voice_tts": "south-f-03",
  "speed_tts": 1.2,
  "enable_option": true,
  "press_options": [
    {
      "press_num": 1,
      "type": "tts",
      "content": "{{key_field_1}} đã bấm phím một"
    },
    {
      "press_num": 2,
      "type": "tts",
      "content": "{{key_field_1}} đã bấm phím hai"
    }
  ],
  "exit_sound": {
    "enable": true,
    "voice_type": "tts",
    "content": "Cám ơn {{key_field_1}} đã sử dụng"
  },
  "invalid_sound": {
    "enable": true,
    "voice_type": "tts",
    "content": "{{key_field_1}} đã nhập sai"
  }
}
```

| Parameter                | Description                                       |
| ------------------------ | ------------------------------------------------- |
| name                     | Tên kịch bản muốn tạo. (Phải là duy nhất)         |
| key_field_1,2,3          | Các từ khoả trong kịch bản                        |
| voice_type               | Loại autocall : tts, audio_file                   |
| voice_tts                | Giọng đọc kịch bản (Đối với voice_type là tts)    |
| speed_tts                | Tốc độ đọc kịch bản (Đối với voice_type là tts)   |
| content                  | Nội dung của kịch bản (Đối với voice_type là tts) |
| audio_file               | File âm thanh (Đối với voice_type là audio_file)  |
| enable_option            | Xử lý kịch bản kèm phím bấm                       |
| press_options            | Danh sách các phím bấm cần xử lý                  |
| press_options.press_num  | Phím bấm cần xử lý                                |
| press_options.type       | Loại autocall : tts, audio_file                   |
| press_options.type       | Loại autocall : tts, audio_file                   |
| exit_sound               | Xử lý khi kết thúc kịch bản                       |
| exit_sound.enable        | Bật xử lý khi kết thúc kịch bản                   |
| exit_sound.voice_type    | Loại autocall : tts, audio_file                   |
| exit_sound.content       | Nội dung của kịch bản (Đối với voice_type là tts) |
| exit_sound.audio_file    | File âm thanh (Đối với voice_type là audio_file)  |
| invalid_sound            | Xử lý khi bấm sai phím trong kịch bản             |
| invalid_sound.enable     | Bật xử lý khi bấm sai phím trong kịch bản         |
| invalid_sound.voice_type | Loại autocall : tts, audio_file                   |
| invalid_sound.content    | Nội dung của kịch bản (Đối với voice_type là tts) |
| invalid_sound.audio_file | File âm thanh (Đối với voice_type là audio_file)  |

key_field: tương ứng với đoạn text import vào campaign người dùng định nghĩa.

Ví dụ:

<ul>
	<li>key_field_1 là {{customer_name}}</li>
	<li>key_field_2 là {{due_amount}}</li>
	<li>key_field_3 là {{due_date}}</li>
</ul>

Đoạn content sẽ là : "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}"

# Autocall

API xử lý autocall.

Lưu ý:

Mỗi user tuỳ theo gói sử dụng mà có số lượng cuộc gọi đồng thời khác nhau.

Nếu cuộc gọi tiếp theo quá số lượng cuộc gọi đồng thời của gói, bạn có thể set `"queue" : true` để cuộc gọi đó được đẩy vào queue và tự động make call sau đó.

## Run OTP

```shell
curl --location --request POST 'http://{API_HOST}/v1/autocall/otp' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "caller": "0123456789",
    "callee": "0987654321",
    "param": {
        "template_name": "otp_demo",
        "code_otp": "123ABC"
    },
    "callback_url": "https://demo.webhook.com",
    "callback_apikey": "api_key_123"
}'
```

> Response trả về:

```json
{
  "call_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "status": "success"
}
```

API thực hiện cuộc gọi OTP

### HTTP Request

`POST http://{API_HOST}/v1/autocall/otp`

### Body

> Sample data:

```json
{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "caller": "0123456789",
  "callee": "0987654321",
  "param": {
    "template_name": "otp_demo",
    "code_otp": "123ABC"
  },
  "callback_url": "https://demo.webhook.com",
  "callback_apikey": "api_key_123",
  "queue": true
}
```

| Parameter           | Description                                |
| ------------------- | ------------------------------------------ |
| campaignId          | Campaign Id được cấp                       |
| caller              | Domain Url mà tổng đài sẽ hook dữ liệu tới |
| callee              | Số điện thoại nhận cuộc gọi                |
| param               |                                            |
| param.template_name | Kịch bản đọc code OTP                      |
| param.code_otp      | Code OTP                                   |
| callback_url        | SIP Call Event                             |
| callback_apikey     | API Key của webhook (optional)             |
| queue               | Đẩy cuộc gọi vào queue                     |

## Run TTS

```shell
curl --location --request POST 'http://{API_HOST}/v1/autocall/voice' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "caller": "0123456789",
    "callee": "0987654321",
    "param": {
        "template_name": "thong_bao_no_cuoc_01",
        "customer_name": "Nguyễn Văn A",
        "due_amount": "10.410.000",
        "due_date": "30/1/2021"
    },
    "callback_url": "https://demo.webhook.com",
    "callback_apikey": "api_key_123"
}'
```

> Response trả về:

```json
{
  "call_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "status": "success"
}
```

API thực hiện cuộc gọi Text-To-Speech

### HTTP Request

`POST http://{API_HOST}/v1/autocall/voice`

### Body

> Sample data:

```json
{
  "caller": "1234567890",
  "callees": ["0987654321", "1234567890"],
  "param": {
    "template_name": "thong_bao_no_cuoc_1",
    "customer_name": "Nguyễn Văn A",
    "due_amount": "10.410.000",
    "due_date": "30/1/2021"
  },
  "callback_url": "https://demo.webhook.com",
  "callback_apikey": "api_key_123",
  "queue": true
}
```

| Parameter           | Description                                                         |
| ------------------- | ------------------------------------------------------------------- |
| campaignId          | Campaign Id được cấp                                                |
| caller              | Domain Url mà tổng đài sẽ hook dữ liệu tới                          |
| callee              | Số điện thoại nhận cuộc gọi                                         |
| param               |                                                                     |
| param.template_name | Kịch bản đọc code OTP                                               |
| param.customer_name | Các thông tin sẽ được đọc dựa vào từ khoá bạn đã tạo trong kịch bản |
| callback_url        | SIP Call Event                                                      |
| callback_apikey     | API Key của webhook (optional)                                      |
| queue               | Đẩy cuộc gọi vào queue                                              |

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

# Audio File

## Upload audio file

```shell
curl --location --request POST 'http://{API_HOST}/v1/audio' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY' \
--form 'file=@"/audio_folders/AUDIO_1.wav"'
```

> Response trả về:

```json
{
  "created": "true"
}
```

API Upload file audio

### HTTP Request

`POST http://{API_HOST}/v1/audio`

### Form Data

| Parameter | Description |
| --------- | ----------- |
| file      | File Audio  |

# Call Detail Report

Lịch sử cuộc gọi

## Attributes

| Attribute    | Description                                | Type     |
| ------------ | ------------------------------------------ | -------- |
| call_id      | Call Id - Id cuộc gọi                      | string   |
| caller       | Số điện thoại thực hiện cuộc gọi           | string   |
| callee       | Số điện thoại nhận cuộc gọi                | string   |
| campaign_id  | Campaign Id                                | string   |
| create_time  | Thời gian khởi tạo cuộc gọi                | datetime |
| start_time   | Thời gian cuộc gọi bắt đầu                 | datetime |
| end_time     | ThờI gian cuộc gọi kết thúc                | datetime |
| answer_time  | ThờI gian cuộc gọi nhấc máy                | datetime |
| duration     | Thời lượng cuộc gọi                        | int      |
| billsec      | Thời lượng tính từ khi người nhận nhấc máy | int      |
| keypress     | Phím bấm . Default: NULL                   | string   |
| user_id      | User Id                                    | string   |
| param        | Các tham số nhận từ API                    | string   |
| type         | Call Type: OTP, Voice                      | string   |
| hangup_cause | Trạng thái cuộc gọi                        | string   |
| call_time    | Số lần thực hiện                           | string   |

## Get CDRs

```shell
curl --location --request GET 'http://{API_HOST}/v1/report?limit=10&offset=0' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY'
```

> Response trả về:

```json
{
  "data": [
    {
      "answer_time": "2021-05-05T08:12:40Z",
      "billsec": 9,
      "call_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "call_time": 1,
      "caller": "0123456789",
      "callee": "0987654321",
      "campaign_id": "eeeeeeee-dddd-cccc-bbbb-aaaaaaaa",
      "create_time": "2021-05-05T08:12:34Z",
      "duration": 12,
      "end_time": "2021-05-05T08:12:43Z",
      "hangup_cause": "NORMAL_CLEARING",
      "keypress": "2",
      "param": "{\"customer_name\":\"Van A\",\"template_name\":\"thong_bao\"}",
      "start_time": "2021-05-05T08:12:34Z",
      "type": "VOICE",
      "user_id": "11111111-bbbb-cccc-dddd-22222222"
    }
  ]
}
```

API dùng để lấy một cdr cụ thể.

### HTTP Request

`GET http://{API_HOST}/v1/report`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| limit     | Số lượng record trả về           |
| offset    | Vị trí record bắt đầu            |
| caller    | Số điện thoại thực hiện cuộc gọi |
| callee    | Số điện thoại nhận cuộc gọi      |

## Get a Specific CDR

```shell
curl --location --request GET 'http://{API_HOST}/v1/report/aaaaaaaa-bbbb-cccc-dddd-eeeeeeee' \
--header 'Authorization: 1ebe8f88MzA1MTNkZjMtZjc3My00MTmY'
```

> Response trả về:

```json
{
  "data": {
    "answer_time": "2021-05-05T08:12:40Z",
    "billsec": 9,
    "call_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "call_time": 1,
    "caller": "0123456789",
    "callee": "0987654321",
    "campaign_id": "eeeeeeee-dddd-cccc-bbbb-aaaaaaaa",
    "create_time": "2021-05-05T08:12:34Z",
    "duration": 12,
    "end_time": "2021-05-05T08:12:43Z",
    "hangup_cause": "NORMAL_CLEARING",
    "keypress": "2",
    "param": "{\"customer_name\":\"Van A\",\"template_name\":\"thong_bao\"}",
    "start_time": "2021-05-05T08:12:34Z",
    "type": "VOICE",
    "user_id": "11111111-bbbb-cccc-dddd-22222222"
  }
}
```

API dùng để lấy một cdr cụ thể.

### HTTP Request

`GET http://{API_HOST}/v1/report/{call_id}`

### URL Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| call_id   | call_id trong data trả về |
