# CallCenter

## Get CallCenter Queues

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/callcenter/queue' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
    "data": [
        {
            "call_center_queue_uuid": "7a514f33-f5f0-426a-97e7-c6cbe372b7a3",
            "domain_uuid": "26c3dd3e-4279-43e9-b122-51871b445306",
            "queue_name": "CALL_CENTER",
            "queue_extension": "3333",
            "queue_strategy": "ring-all"
        }
    ],
    "limit": 10,
    "offset": 0,
    "total": 1
}
```

### HTTP Request

`GET https://{{API_HOST}}/v1/callcenter/queue`

### Query Parameters

| Parameter     | Description               | Example    |
| ------------- | ------------------------- | ---------- |
| limit         | Số lượng record trả về    | 50         |
| offset        | Vị trí bắt đầu khi query  | 0          |
