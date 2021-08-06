# Autodialer

## Nhận dữ liệu queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autodialer/queue' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autodial",
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

`POST https://{{API_HOST}}/v1/autodialer/queue`

### Body

> Sample data:

```json
{
  "campaign_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeee",
  "queue_code": "Autodial",
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
curl --location --request POST 'https://{{API_HOST}}/v1/autodialer/queue/stop' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autodial"
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

`POST https://{{API_HOST}}/v1/autodialer/queue/stop`

### Body

> Sample data:

```json
{
  "queue_code": "Autodial"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |

## Start Queue

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autodialer/queue/start' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "queue_code": "Autodial"
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

`POST https://{{API_HOST}}/v1/autodialer/queue/start`

### Body

> Sample data:

```json
{
  "queue_code": "Autodial"
}
```

| Parameter  | Description | Required |
| ---------- | ----------- | -------- |
| queue_code | Mã queue    | x        |
