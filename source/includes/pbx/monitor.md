# Monitor

## Monitor Agent

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/monitor/agent' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "agent_name": "Test 01",
      "call_id": "6a65480c-8bc7-4d4a-9aae-4488d67bf711",
      "call_state": "RINGING",
      "call_time": "2021-07-01 09:30:00",
      "destination": "0899123456",
      "direction": "outbound",
      "domain_id": "9eb34970-dec2-466d-98c7-9532439be7eb",
      "domain_name": "test01.tel4vn.com",
      "extension": "101",
      "extension_id": "663dec3e-a405-4372-9991-8e6ae9f9788a",
      "network_ip": "10.0.0.15",
      "server_host": "10.0.0.1",
      "user_agent": "MicroSIP/3.20.6",
      "user_id": "41d0f364-79f8-4abb-9834-ee803320ea8d",
      "username": "test01"
    },
    {
      "agent_name": "Test 02",
      "call_id": "faf6a9ae-c475-4980-8a85-24710ab9d6cd",
      "call_state": "ONCALL",
      "call_time": "2021-07-01 09:31:00",
      "destination": "0899654321",
      "direction": "outbound",
      "domain_id": "9eb34970-dec2-466d-98c7-9532439be7eb",
      "domain_name": "test02.tel4vn.com",
      "extension": "102",
      "extension_id": "663dec3e-a405-4372-9991-8e6ae9f9788a",
      "network_ip": "10.0.0.16",
      "server_host": "10.0.0.1",
      "user_agent": "MicroSIP/3.20.6",
      "user_id": "56df3364-f377-43d3-b315-db1dc011e4de",
      "username": "test02"
    },
    ...
  ],
  "total": 2
}
```

API dùng để monitor agent đang login.

| Thông tin         | Mô tả                                  |
| ----------------- | -------------------------------------- |
| data              | Data trả về                            |
| data.user_id      | Id của user                            |
| data.username     | Username của extension                 |
| data.agent_name   | Tên của agent                          |
| data.extension_id | Id của extension                       |
| data.extension    | Extension                              |
| data.call_state   | Trạng thái cuộc gọi. (RINGING, ONCALL) |
| data.call_id      | Id của cuộc gọi                        |
| data.call_time    | Thời gian bắt đầu của cuộc gọi         |
| data.destination  | Số điện thoại nhận cuộc gọi            |
| data.direction    | Chiều cuộc gọi. (inbound, outbound)    |
| data.user_agent   | Tên, Id thiết bị                       |
| data.domain_id    | Id của domain                          |
| data.domain_name  | Tên của domain                         |
| data.network_ip   | IP của extension                       |
| data.server_host  | Host của tổng đài                      |

### HTTP Request

`GET https://{{API_HOST}}/v1/monitor/agent`
