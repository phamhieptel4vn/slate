# Template

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

## Get Templates

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/template' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "content": "Đây là đì mô âu tu con, xin chào {{customer_name}}",
      "description": "",
      "enable_option": false,
      "exit_sound": {},
      "invalid_sound": {},
      "max_timeout": 1,
      "name": "template_autocall_audio_v2",
      "press_options": [],
      "press_timeout": 1000,
      "speed_tts": 0.9,
      "template_uuid": "0dfd7a67-edf6-4ba8-9414-941b0b46fa5c",
      "voice_tts": "south-f-03",
      "voice_type": "tts"
    },
    {
      "audio_file": "bf039442-f297-476a-9d3e-7eb2fe7cdc64",
      "description": "",
      "enable_option": false,
      "exit_sound": {},
      "invalid_sound": {},
      "max_timeout": 1,
      "name": "template_autocall_audio_v1",
      "press_options": [],
      "press_timeout": 1000,
      "speed_tts": 1,
      "template_uuid": "ab9e2158-3b74-4adb-a227-cc034b3e2840",
      "voice_tts": "",
      "voice_type": "audio_file"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 2
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/template`

### Query Parameters

| Parameter | Description              | Example |
| --------- | ------------------------ | ------- |
| limit     | Số lượng record trả về   | 50      |
| offset    | Vị trí bắt đầu khi query | 0       |

## Get Template By Id

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/template/0dfd7a67-edf6-4ba8-9414-941b0b46fa5c' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "content": "Đây là đì mô âu tu con, xin chào {{customer_name}}",
  "description": "",
  "enable_option": false,
  "exit_sound": {},
  "invalid_sound": {},
  "max_timeout": 1,
  "name": "template_autocall_audio_v2",
  "press_options": [],
  "press_timeout": 1000,
  "speed_tts": 0.9,
  "template_uuid": "0dfd7a67-edf6-4ba8-9414-941b0b46fa5c",
  "voice_tts": "south-f-03",
  "voice_type": "tts"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/template/{{template_id}}`

### Query Parameters

| Parameter   | Description |
| ----------- | ----------- |
| template_id | Id kịch bản |

## Kịch bản TTS

```shell
curl --location --request POST ' https://{API_HOST}/v1/template' \
--header 'Authorization: Bearer {{TOKEN}}' \
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
        "press_num": "1",
        "voice_type": "tts",
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

`POST https://{API_HOST}/v1/template`

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
| key_field_1,2,3          | Các từ khoá trong kịch bản                        |
| voice_type               | Loại autocall : tts, audio_file                   |
| voice_tts                | Giọng đọc kịch bản (Đối với voice_type là tts)    |
| speed_tts                | Tốc độ đọc kịch bản (Đối với voice_type là tts)   |
| content                  | Nội dung của kịch bản (Đối với voice_type là tts) |
| audio_file               | File âm thanh (Đối với voice_type là audio_file)  |
| enable_option            | Xử lý kịch bản kèm phím bấm                       |
| press_options            | Danh sách các phím bấm cần xử lý                  |
| press_options.press_num  | Phím bấm cần xử lý                                |
| press_options.voice_type | Loại autocall : tts, audio_file                   |
| press_options.content    | Nội dung của kịch bản (Đối với voice_type là tts) |
| press_options.audio_file | File âm thanh (Đối với voice_type là audio_file)  |
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

## Khởi chạy autocall

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/ivr' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "carrier": "mobi",
    "template": "thong_bao_no_cuoc_01",
    "phone_number": "0899123456",
    "params": {
        "customer_name" : "Nguyễn Văn A",
        "contract_number": "HD123456",
        "due_date": "2021-07-13"
    }
}'
```

> Response trả về:

```json
{
  "message": "success"
}
```

> Error Response trả về:

```json
{
  "error": "missing tts key"
}
```

API này dùng để nhận thông tin và đẩy cuộc gọi autocall theo kịch bản và thông tin được truyền.

### HTTP Request

`POST https://{{API_HOST}}/v1/autocall/ivr`

### Body

> Sample data:

```json
{
  "carrier": "mobi",
  "template": "thong_bao_no_cuoc_01",
  "phone_number": "0899123456",
  "params": {
    "customer_name": "Nguyễn Văn A",
    "contract_number": "HD123456",
    "due_date": "2021-07-13"
  }
}
```

| Parameter                            | Description                         | Required |
| ------------------------------------ | ----------------------------------- | -------- |
| carrier                              | Đầu số, nhà mạng thực hiện cuộc gọi | x        |
| template                             | Kịch bản dùng để                    | x        |
| phone_number                         | Số điện thoại nhận cuộc gọi         |          |
| params.customer_name,contract_number | key_field                           | x        |

## Khởi chạy chiến dịch autocall

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/campaign' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_uuid": "avavavav-1111-2222-3333-eeeeeeee",
    "carrier": "mobi",
    "template": "thong_bao_no_cuoc_01",
    "concurrent_call": 2,
    "customers" : [
      {
        "phone_number": "0899123456",
        "params": {
            "customer_name" : "Nguyễn Văn A",
            "contract_number": "HD123456",
            "due_date": "2021-07-13"
        }
      }
    ]
}'
```

> Response trả về:

```json
{
  "fail": [],
  "message": "success",
  "success": ["0899123456"]
}
```

> Error Response trả về:

```json
{
  "error": "campaign is not found"
}
```

API này dùng để nhận thông tin chiến dịch và đẩy cuộc gọi autocall theo kịch bản và thông tin được truyền.

### HTTP Request

`POST https://{{API_HOST}}/v1/autocall/campaign`

### Body

> Sample data:

```json
{
  "campaign_uuid": "avavavav-1111-2222-3333-eeeeeeee",
  "carrier": "mobi",
  "template": "thong_bao_no_cuoc_01",
  "concurrent_call": 2,
  "customers": [
    {
      "phone_number": "0899123456",
      "params": {
        "customer_name": "Nguyễn Văn A",
        "contract_number": "HD123456",
        "due_date": "2021-07-13"
      }
    }
  ]
}
```

| Parameter                                      | Description                         | Required |
| ---------------------------------------------- | ----------------------------------- | -------- |
| campaign_uuid                                  | Chiến dịch                          | x        |
| carrier                                        | Đầu số, nhà mạng thực hiện cuộc gọi | x        |
| template                                       | Kịch bản dùng để                    | x        |
| customers                                      | Danh sách khách hàng                | x        |
| customers.phone_number                         | Số điện thoại nhận cuộc gọi         | x        |
| customers.params.customer_name,contract_number | key_field                           | x        |

Một số lưu ý:

<ul>
  <li>Nếu muốn đọc đúng khoản tiền (Không đọc từng số) - truyền dữ liệu theo format: “10410000”</li>
  <li>Nếu muốn đọc từng số (Ví dụ số điện thoại) - truyền dữ liệu theo format: 1 2 3 4 5 6 hoặc “0 9 8 7 6 5 4 3 2 1”</li>
  <li>Nếu muốn đọc thành số (Ví dụ 15 - mười lăm) - truyền dữ liệu theo format: “#15”</li>
  <li>Nếu giá trị của key field là ngày tháng năm - Yêu cầu format “dd/mm/yyyy” hoặc “yyyy-mm-dd”, ví dụ: “30/01/2021” hoặc "2021-01-30"</li>
</ul>

customers.key_field: tương ứng với đoạn text import vào campaign người dùng đã định nghĩa.

Ví dụ:

<ul>
  <li>Kịch bản : "Chào bạn {{customer_name}} vui lòng thanh toán khoản nợ {{due_amount}} trước ngày {{due_date}}"</li>
  <li>Nội dung kịch bản sẽ là: “Chào bạn Nguyễn Văn A vui lòng thanh toán khoản nợ mười triệu bốn trăm mười nghìn trước ngày ba mươi tháng một năm hai không hai mươi mốt”</li>
</ul>

## Tạo chiến dịch autocall

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/campaign' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_name": "autocall_campaign",
    "concurrent_call": 2
}'
```

> Response trả về:

```json
{
  "campaign_uuid": "avavavav-1111-2222-3333-eeeeeeee",
  "created": true
}
```

> Error Response trả về:

```json
{
  "error": "campaign_name is already taken"
}
```

API này dùng để tạo chiến dịch autocall.

### HTTP Request

`POST https://{{API_HOST}}/v1/campaign`

### Body

> Sample data:

```json
{
  "campaign_name": "autocall_campaign",
  "concurrent_call": 2
}
```

| Parameter       | Description                 | Required |
| --------------- | --------------------------- | -------- |
| campaign_name   | Tên chiến dịch              | x        |
| concurrent_call | Số lượng cuộc gọi đồng thời |          |

## Get Campaigns

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/campaign' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
      "campaign_uuid": "373854ae-0169-4a1f-b71b-e145b3579233",
      "campaign_name": "autocall_test_01",
      "template_uuid": "0dfd7a67-edf6-4ba8-9414-941b0b46fa5c",
      "template_name": "template_autocall_audio_v2",
      "description": "",
      "status": true,
      "concurrent_call": 0,
      "created_at": "2022-02-03T12:05:00.518181Z",
      "updated_at": "2022-02-03T12:05:00.518181Z"
    },
    {
      "domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
      "campaign_uuid": "384243bd-5f8f-42a4-83c7-0f88670aea12",
      "campaign_name": "autocall_test_02",
      "template_uuid": "",
      "template_name": "",
      "description": "",
      "status": true,
      "concurrent_call": 1,
      "created_at": "2022-02-02T17:32:15.495923Z",
      "updated_at": "2022-02-02T17:32:15.495923Z"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 2
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/campaign`

### Query Parameters

| Parameter     | Description               | Example    |
| ------------- | ------------------------- | ---------- |
| campaign_name | Tên chiến dịch            | campaign_a |
| template_name | Tên template được sử dụng | template_a |
| limit         | Số lượng record trả về    | 50         |
| offset        | Vị trí bắt đầu khi query  | 0          |

## Get Campaign By Id

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/campaign/373854ae-0169-4a1f-b71b-e145b3579233' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
  "campaign_uuid": "373854ae-0169-4a1f-b71b-e145b3579233",
  "campaign_name": "autocall_test_01",
  "template_uuid": "0dfd7a67-edf6-4ba8-9414-941b0b46fa5c",
  "template_name": "template_autocall_audio_v2",
  "description": "",
  "status": true,
  "concurrent_call": 0,
  "created_at": "2022-02-03T12:05:00.518181Z",
  "updated_at": "2022-02-03T12:05:00.518181Z"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/campaign/{{campaign_id}}`

### Query Parameters

| Parameter   | Description   |
| ----------- | ------------- |
| campaign_id | Id chiến dịch |

## Báo cáo chiến dịch autocall

```shell
curl --location --request GET 'https://{{API_HOST}}/v1/autocall/report?campaign_uuid=avavavav-1111-2222-3333-eeeeeeee&start_date=2022-01-01%2010:00:00&end_date=2022-01-01%2012:00:00' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": {
    "status": {
      "answered": 3,
      "busy": 1,
      "not_available": 0,
      "no_answer": 0,
      "cancel": 0,
      "failed": 0
    },
    "total_calls": 4,
    "total_duration": 43,
    "total_billsec": 10,
    "total_waitsec": 0
  }
}
```

> Error Response trả về:

```json
{
  "error": "campaign is not found"
}
```

API này dùng để nhận thông tin chiến dịch và đẩy cuộc gọi autocall theo kịch bản và thông tin được truyền.

### HTTP Request

`GET https://{{API_HOST}}/v1/autocall/report`

### Query Parameters

| Parameter     | Description                         | Example                          | Required |
| ------------- | ----------------------------------- | -------------------------------- | -------- |
| campaign_uuid | Chiến dịch thực hiện cuộc gọi       | aaaaaaaa-1111-2222-3333-eeeeeeee | x        |
| start_date    | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-18 17:20:58              |          |
| end_date      | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-18 17:20:58              |          |

# Audio

## Get Audios

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/audio' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "domain_uuid": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "audio_uuid": "793e2938-2d1e-4274-8dfd-ce6e2edbc4b0",
      "audio_name": "demo-01.wav",
      "created_at": "2022-01-01T13:16:32Z"
    },
    {
      "domain_uuid": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "audio_uuid": "793e2938-2d1e-4274-8dfd-ce6e2edbc4b0",
      "audio_name": "demo-02.wav",
      "created_at": "2022-01-01T14:16:32Z"
    }
  ],
  "limit": 10,
  "offset": 0,
  "total": 2
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/audio`

### Query Parameters

| Parameter  | Description              | Example  |
| ---------- | ------------------------ | -------- |
| audio_name | Tên file audio           | audio_01 |
| limit      | Số lượng record trả về   | 50       |
| offset     | Vị trí bắt đầu khi query | 0        |

## Upload Audio

```shell
curl --location --request POST 'https://api-pbx03.tel4vn.com/v1/audio' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--form 'file=@"/D:/tel4vn/demo.wav"'
```

> Response trả về:

```json
{
  "created": true,
  "id": "793e2938-2d1e-4274-8dfd-ce6e2edbc4b0"
}
```

> Error Response trả về:

```json
{
  "error": "audio is existed"
}
```

API này dùng để upload file audio lên server.

### HTTP Request

`POST https://{{API_HOST}}/v1/audio`

### Body

> Sample data:

| Parameter | Description | Required |
| --------- | ----------- | -------- |
| file      | file audio  | x        |
