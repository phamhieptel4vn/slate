# Campaign

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
      "domain_id": "dddddddd-1111-2222-3333-eeeeeeee",
      "campaign_id": "aaaaaaaa-1111-2222-3333-eeeeeeee",
      "campaign_name": "Autodial",
      "type": "autodialer",
      "description": "Autodial",
      "status": "active",
      "created_at": "2021-07-22T09:56:08.411641Z"
    },
    {
      "domain_id": "dddddddd-1111-2222-3333-eeeeeeee",
      "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "campaign_name": "Autocall",
      "type": "autocall",
      "description": "Autocall",
      "status": "active",
      "created_at": "2021-07-21T08:45:46.910886Z"
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

| Parameter | Description              | Example  |
| --------- | ------------------------ | -------- |
| type      | Loại                     | autocall |
| limit     | Số lượng record trả về   | 50       |
| offset    | Vị trí bắt đầu khi query | 0        |

## Get a Specific Campaign

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/campaign/aaaaaaaa-bbbb-cccc-dddd-eeeeeeee' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "domain_id": "dddddddd-1111-2222-3333-eeeeeeee",
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "campaign_name": "Autocall",
  "type": "autocall",
  "description": "Autocall",
  "status": "active",
  "created_at": "2021-07-21T08:45:46.910886Z"
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/campaign/{{campaign_id}}`

### Query Parameters

| Parameter   | Description     |
| ----------- | --------------- |
| campaign_id | Id của campaign |

## Create Campaign

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/campaign' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "campaign_name" : "Autodial",
    "type" : "autodialer"
}'
```

> Response trả về:

```json
{
  "created": true,
  "id": "dddddddd-1111-2222-3333-eeeeeeee"
}
```

### HTTP Request

`POST https://{{API_HOST}}/v1/campaign`

### Body

| Parameter     | Description                 | Required |
| ------------- | --------------------------- | -------- |
| campaign_name | Tên campaign                | x        |
| type          | Loại (autocall, autodialer) | x        |
| description   | Mô tả                       |          |

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

## Kịch bản TTS

```shell
curl --location --request POST ' https://{API_HOST}/v1/template' \
--header 'Authorization: {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_id" : "663dec3e-a405-4372-9991-8e6ae9f9788a",
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

`POST https://{API_HOST}/v1/template`

### Body

> Sample data:

```json
{
  "campaign_id": "663dec3e-a405-4372-9991-8e6ae9f9788a",
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
| campaign_id              | Kịch bản được gán vào campaign                    |
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

## Nhận dữ liệu queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/queue' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autocall",
  "template": "Test Autocall",
  "concurrent_call": "5",
  "customers": [
    {
      "id": "TEL4VN_Test",
      "mobiles": ["0899123456"],
      "contract_number": "HD123456",
      "due_date": "2021-07-13"
    }
  ]
}'
```

> Response trả về:

```json
{
  "data": {
    "fail": [],
    "success": ["TEL4VN_Test"]
  }
}
```

> Error Response trả về:

```json
{
  "data": {
    "fail": [],
    "success": []
  }
}
```

API này nhằm mục đích nhận thông tin về queue để tiến hành tự động gọi ra theo kịch bản.

### HTTP Request

`POST https://{{API_HOST}}/v1/autocall/queue`

### Body

> Sample data:

```json
{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autocall",
  "template": "Test Autocall",
  "concurrent_call": "5",
  "customers": [
    {
      "id": "TEL4VN_Test",
      "mobiles": ["0899123456"],
      "contract_number": "HD123456",
      "due_date": "2021-07-13"
    }
  ]
}
```

| Parameter                          | Description                                | Required |
| ---------------------------------- | ------------------------------------------ | -------- |
| campaign_id                        | Id của campaign                            | x        |
| queue_code                         | Mã queue                                   | x        |
| template                           | Kịch bản dùng để                           | x        |
| concurrent_call                    | Số lượng cuộc gọi đồng thời                |          |
| customers.id                       | ID của khách hàng                          | x        |
| customers.mobiles                  | Danh sách các số điện thoại của khách hàng | x        |
| customers.contract_number,due_date | key_field                                  | x        |

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

## Import danh sách chặn

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/queue/dnc' \
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
  "message": "successfully"
}
```

> Error Response trả về:

```json
{
  "message": "queue not found"
}
```

API này nhằm mục đích cung cấp danh sách các khách hàng cần chặn cuộc gọi lên tổng đài. Tổng đài sẽ loại/bỏ qua các khách hàng này khi quay số nếu chưa quay đến. Nếu đã quay rồi hoặc đang trong cuộc gọi thì giữ nguyên.

### HTTP Request

`POST https://{{API_HOST}}/v1/autocall/queue/dnc`

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
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/queue/stop' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1"
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

`POST https://{{API_HOST}}/v1/autocall/queue/stop`

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
curl --location --request POST 'https://{{API_HOST}}/v1/autocall/queue/start' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autocall-Q1"
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

`POST https://{{API_HOST}}/v1/autocall/queue/start`

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
