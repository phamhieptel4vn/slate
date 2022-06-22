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
      "domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
      "campaign_uuid": "373854ae-0169-4a1f-b71b-e145b3579233",
      "campaign_name": "autocall_test_01",
      "template_uuid": "0dfd7a67-edf6-4ba8-9414-941b0b46fa5c",
      "template_name": "template_autocall_audio_v2",
      "dial_method": "autocall",
      "description": "",
      "status": true,
      "concurrent_call": 1,
      "ratio": 0,
      "created_at": "2022-02-03T12:05:00.518181Z",
      "updated_at": "2022-02-03T12:05:00.518181Z"
    },
    {
      "domain_uuid": "avavavav-1111-2222-3333-eeeeeeee",
      "campaign_uuid": "384243bd-5f8f-42a4-83c7-0f88670aea12",
      "campaign_name": "autodial_test_02",
      "template_uuid": "",
      "template_name": "",
      "dial_method": "autodial",
      "description": "",
      "status": true,
      "concurrent_call": 1,
      "ratio": 1,
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
  "dial_method": "autocall",
  "description": "",
  "status": true,
  "concurrent_call": 1,
  "ratio": 0,
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

## Tạo chiến dịch

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
