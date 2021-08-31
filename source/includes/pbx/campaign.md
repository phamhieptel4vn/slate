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
      "max_concurrent_call": 1,
      "default_concurrent_call": 1,
      "default_ratio": 100,
      "max_ratio": 100,
      "gateway_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "default_queue_agent": "",
      "queue_agent_next_call": "",
      "scheduled_recall": "default",
      "recall_hours_block": 0,
      "answer_callback_url": "",
      "local_start_time": "00:00:00",
      "local_end_time": "23:59:59",
      "customer_order": "id",
      "created_at": "2021-08-01T02:36:29.509713+07:00",
      "updated_at": "0001-01-01T00:00:00Z"
    },
    {
      "domain_id": "dddddddd-1111-2222-3333-eeeeeeee",
      "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "campaign_name": "Autocall",
      "type": "autocall",
      "description": "Autocall",
      "status": "active",
      "max_concurrent_call": 1,
      "default_concurrent_call": 1,
      "default_ratio": 100,
      "max_ratio": 100,
      "gateway_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
      "default_queue_agent": "",
      "queue_agent_next_call": "",
      "scheduled_recall": "default",
      "recall_hours_block": 0,
      "answer_callback_url": "",
      "local_start_time": "00:00:00",
      "local_end_time": "23:59:59",
      "customer_order": "id",
      "created_at": "2021-08-01T02:36:29.509713+07:00",
      "updated_at": "0001-01-01T00:00:00Z"
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
  "max_concurrent_call": 1,
  "default_concurrent_call": 1,
  "default_ratio": 100,
  "max_ratio": 100,
  "gateway_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "default_queue_agent": "",
  "queue_agent_next_call": "",
  "scheduled_recall": "default",
  "recall_hours_block": 0,
  "answer_callback_url": "",
  "local_start_time": "00:00:00",
  "local_end_time": "23:59:59",
  "customer_order": "id",
  "created_at": "2021-08-01T02:36:29.509713+07:00",
  "updated_at": "0001-01-01T00:00:00Z"
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
    "type" : "autodialer",
    "max_concurrent_call": 1,
    "default_concurrent_call": 1,
    "default_ratio": 100,
    "max_ratio": 100,
    "gateway_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "default_queue_agent": "",
    "queue_agent_next_call": "",
    "scheduled_recall": "default",
    "recall_hours_block": 0,
    "local_start_time": "00:00:00",
    "local_end_time": "23:59:59"
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

| Parameter               | Description                                                                                        | Required |
| ----------------------- | -------------------------------------------------------------------------------------------------- | -------- |
| campaign_name           | Tên campaign                                                                                       | x        |
| type                    | Loại (autocall, autodialer)                                                                        | x        |
| status                  | Trạng thái (active, deactive)                                                                      |          |
| description             | Mô tả                                                                                              |          |
| max_concurrent_call     | Số lượng cuộc gọi đồng thời tối đa (áp dụng cho autocall). Mặc định: 1.                            |          |
| default_concurrent_call | Số lượng cuộc gọi đồng thời mặc định (áp dụng cho autocall) . Mặc định: 1.                         |          |
| max_ratio               | Tỉ lệ thực hiện cuộc gọi đựa trên số lượng agent tối đa (áp dụng cho autodialer). Mặc định: 100.   |          |
| default_ratio           | Tỉ lệ thực hiện cuộc gọi đựa trên số lượng agent mặc định (áp dụng cho autodialer). Mặc định: 100. |          |
| gateway_id              | Id gateway để thực hiện cuộc gọi. Liên hệ để được cung cấp.                                        | x        |
| default_queue_agent     | Nhóm agent sẽ nhận cuộc gọi mặc định.                                                              |          |
| scheduled_recall        | Lên lịch gọi lại ['default','hours']                                                               |          |
| recall_hours_block      | Nếu như lên lịch gọi lại là hours, thì ứng với cách bao nhiêu giờ sẽ gọi lại                       |          |
| local_start_time        | Thời gian hoạt động (bắt đầu). Format : HH:MM:SS                                                   |          |
| local_end_time          | Thời gian hoạt động (kết thúc). Format : HH:MM:SS                                                  |          |

## Update Campaign

```shell
curl -L -X PUT 'https://{{API_HOST}}/v1/campaign/aaaaaaaa-bbbb-cccc-dddd-eeeeeeee' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
--data-raw '{
    "campaign_name" : "Autodial",
    "type" : "autodialer",
    "max_concurrent_call": 1,
    "default_concurrent_call": 1,
    "default_ratio": 100,
    "max_ratio": 100,
    "gateway_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
    "default_queue_agent": "",
    "queue_agent_next_call": "",
    "scheduled_recall": "default",
    "recall_hours_block": 0,
    "local_start_time": "00:00:00",
    "local_end_time": "23:59:59"
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

`PUT https://{{API_HOST}}/v1/campaign/{{campaign_id}}`

### Query Parameters

| Parameter   | Description     |
| ----------- | --------------- |
| campaign_id | Id của campaign |

### Body

| Parameter               | Description                                                                                        | Required |
| ----------------------- | -------------------------------------------------------------------------------------------------- | -------- |
| campaign_name           | Tên campaign                                                                                       | x        |
| type                    | Loại (autocall, autodialer)                                                                        | x        |
| status                  | Trạng thái (active, deactive)                                                                      |          |
| description             | Mô tả                                                                                              |          |
| max_concurrent_call     | Số lượng cuộc gọi đồng thời tối đa (áp dụng cho autocall). Mặc định: 1.                            |          |
| default_concurrent_call | Số lượng cuộc gọi đồng thời mặc định (áp dụng cho autocall) . Mặc định: 1.                         |          |
| max_ratio               | Tỉ lệ thực hiện cuộc gọi đựa trên số lượng agent tối đa (áp dụng cho autodialer). Mặc định: 100.   |          |
| default_ratio           | Tỉ lệ thực hiện cuộc gọi đựa trên số lượng agent mặc định (áp dụng cho autodialer). Mặc định: 100. |          |
| gateway_id              | Id gateway để thực hiện cuộc gọi. Liên hệ để được cung cấp.                                        | x        |
| default_queue_agent     | Nhóm agent sẽ nhận cuộc gọi mặc định.                                                              |          |
| scheduled_recall        | Lên lịch gọi lại ['default','hours']                                                               |          |
| recall_hours_block      | Nếu như lên lịch gọi lại là hours, thì ứng với cách bao nhiêu giờ sẽ gọi lại                       |          |
| local_start_time        | Thời gian hoạt động (bắt đầu). Format : HH:MM:SS                                                   |          |
| local_end_time          | Thời gian hoạt động (kết thúc). Format : HH:MM:SS                                                  |          |
