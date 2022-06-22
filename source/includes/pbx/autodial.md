# Autodial

## Khởi chạy chiến dịch autodial

```shell
curl --location --request POST 'https://{{API_HOST}}/v1/autodial/campaign' \
--header 'Authorization: Bearer {{TOKEN}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "campaign_uuid": "avavavav-1111-2222-3333-eeeeeeee",
    "carrier": "mobi",
    "ratio": 2,
    "customers" : [
      {
        "phone_number": "0899123456"
      }
    ]
}'
```

> Response trả về:

```json
{
  "fail": [],
  "message": "success",
  "success": {
    "0899123456": "fae84fef-bdf4-42b0-a64a-d623b707c550"
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

`POST https://{{API_HOST}}/v1/autocall/campaign`

### Body

> Sample data:

```json
{
  "campaign_uuid": "avavavav-1111-2222-3333-eeeeeeee",
  "carrier": "mobi",
  "ratio": 2,
  "customers": [
    {
      "phone_number": "0899123456"
    }
  ]
}
```

| Parameter              | Description                         | Required |
| ---------------------- | ----------------------------------- | -------- |
| campaign_uuid          | Chiến dịch                          | x        |
| carrier                | Đầu số, nhà mạng thực hiện cuộc gọi | x        |
| customers              | Danh sách khách hàng                | x        |
| customers.phone_number | Số điện thoại nhận cuộc gọi         | x        |
