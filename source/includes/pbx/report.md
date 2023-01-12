# Report

## List Call Id

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/report/call_id?end_date=2021-06-01%2000:00:00&start_date=2021-06-01%2023:59:59' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": [
      {
          "call_id": "181ba2e3-4528-4e8c-80b9-2b998af93c5b",
          "sip_call_id": "0.4cCkoEATg8DPFKD7jSOgueuiDCwuxH"
      },
      {
          "call_id": "bc018081-0451-4d3b-bb4c-98154b35962f",
          "sip_call_id": "4Kh4ppE5xoBrPzqpFiJkVAtZ2BXb2F-s"
      },
      ...
    ],
    "total": 10
}
```

> Error Response trả về:

```json
{
  "error": "date range must be in 24 hours"
}
```

API dùng để lấy danh sách các call_id và sip_call_id trong khoảng thời gian.

### HTTP Request

`GET https://{{API_HOST}}/v1/report/call_id`

### Query Parameters

| Parameter  | Description                         | Example             |
| ---------- | ----------------------------------- | ------------------- |
| start_date | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-18 17:20:58 |
| end_date   | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-19 00:00:00 |

## List Call Status

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/report/call_status?end_date=2021-06-01%2000:00:00&start_date=2021-06-01%2023:59:59' \
-H 'Authorization: Bearer {{TOKEN}}'
-H 'Content-Type: application/json'
```

> Response trả về:

```json
{
  "data": [
    {
      "count": 15225,
      "status": "ANSWERED"
    },
    {
      "count": 7161,
      "status": "BUSY"
    },
    {
      "count": 1515,
      "status": "FAILED"
    },
    {
      "count": 59,
      "status": "UNKNOWN"
    },
    {
      "count": 700,
      "status": "NO ANSWER"
    }
  ],
  "total": 24660
}
```

> Error Response trả về:

```json
{
  "error": "start_date must be after end_date"
}
```

API dùng để lấy các trạng thái và số lượng cuộc gọi các trạng thái trong khoảng thời gian.

### HTTP Request

`GET https://{{API_HOST}}/v1/report/call_status`

### Query Parameters

| Parameter  | Description                         | Example             |
| ---------- | ----------------------------------- | ------------------- |
| start_date | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-18 17:20:58 |
| end_date   | Tìm kiếm cdrs theo khoảng thời gian | 2021-02-19 00:00:00 |
