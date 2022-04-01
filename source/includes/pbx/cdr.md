# CDRs - Call Detail Records

Lịch sử cuộc gọi

### Attributes

| Attribute     | Description                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------- |
| id            | Id của CDR                                                                                  |
| sip_call_id   | call_id trong bản tin SIP                                                                   |
| cause         | Trạng thái cuộc gọi dựa theo mã phản hồi giao thức SIP. Vd: NORMAL_CLEARING, NO_ANSWER, ... |
| duration      | Thời hạn thực hiện cuộc gọi.                                                                |
| billsec       | Thời gian đàm thoại trong cuộc gọi.                                                               |
| direction     | Chiều cuộc gọi (inbound, outbound, local)                                                   |
| recording_url | Đường dẫn file ghi âm cuộc gọi                                                              |
| extension     | Extension nhận hoặc thực hiện cuộc gọi                                                      |
| from_number   | Cuộc gọi từ số nào                                                                          |
| to_number     | Cuộc gọi đến số nào                                                                         |
| receive_dest  | Ringroup hoặc queue của extension nhận cuộc gọi                                             |
| time_started  | Thời gian bắt đầu cuộc gọi                                                                  |
| time_answered | Thời gian khi cuộc gọi kết nối                                                              |
| time_ended    | Thời gian kết thúc cuộc gọi                                                                 |
| status        | Trạng thái cuộc gọi                                                                         |

## Get CDRs

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/cdr?' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Pagination response trả về:

```json
{
  "data": [
    {
      "id": "ad4c9b90-c071-405a-9723-980d2e5e1623",
      "sip_call_id": "112233aabbccddee..",
      "cause": "NORMAL_CLEARING",
      "duration": 15,
      "billsec": 11,
      "direction": 3,
      "recording_url": "https://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-17 17:30:35",
      "time_answered": "2021-02-17 17:30:43",
      "time_ended": "2021-02-17 17:30:46",
      "status": "ANSWERED"
    },
    {
      "id": "01b7d166-b564-42ec-80a1-4ad343225934 ",
      "sip_call_id": "aabbccddee112233..",
      "cause": "NORMAL_CLEARING",
      "duration": 45,
      "billsec": 39,
      "direction": 3,
      "recording_url": "",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-18 17:20:58",
      "time_answered": "",
      "time_ended": "2021-02-18 17:21:05",
      "status": "BUSY"
    },
    ...
  ],
  "limit": 10,
  "page": 1,
  "total": 22
}
```

> Scroll response trả về:

```json
{
  "data": [
    {
      "id": "ad4c9b90-c071-405a-9723-980d2e5e1623",
      "sip_call_id": "112233aabbccddee..",
      "cause": "NORMAL_CLEARING",
      "duration": 15,
      "billsec": 11,
      "duration": 11,
      "direction": 3,
      "recording_url": "https://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623.wav",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-17 17:30:35",
      "time_answered": "2021-02-17 17:30:43",
      "time_ended": "2021-02-17 17:30:46",
      "status": "ANSWERED"
    },
    {
      "id": "01b7d166-b564-42ec-80a1-4ad343225934",
      "sip_call_id": "aabbccddee112233..",
      "cause": "NORMAL_CLEARING",
      "duration": 45,
      "billsec": 39,
      "direction": 3,
      "recording_url": "",
      "extension": "101",
      "from_number": "19001919",
      "to_number": "0899888999",
      "receive_dest": "",
      "time_started": "2021-02-18 17:20:58",
      "time_answered": "",
      "time_ended": "2021-02-18 17:21:05",
      "status": "BUSY"
    },
    ...
  ],
  "scroll_id": "111222333444aaabbbcccddd=="
}
```

Trả về danh sách lịch sử cuộc gọi.
API CDRs sử dụng 2 cơ chế để trả về dữ liệu.
Pagination: Phân trang.
Scroll: Cuộn trang. (Mặc định)

Nếu user cung cấp trong param: page - Số trang, limit - số lượng trả về thì API sẽ trả về dữ liêu theo cơ chế Pagination.

### HTTP Request

`GET https://{{API_HOST}}/v1/cdr`

### Query Parameters

| Parameter        | Description                                                                  | Example                                   |
| ---------------- | ---------------------------------------------------------------------------- | ----------------------------------------- |
| start_date       | Tìm kiếm cdrs theo khoảng thời gian (Khởi tạo cuộc gọi)                      | 2021-02-18 hoặc 2021-02-18 17:20:58       |
| end_date         | Tìm kiếm cdrs theo khoảng thời gian (Khởi tạo cuộc gọi)                      | 2021-02-19 hoặc 2021-02-19 00:00:00       |
| duration         | Thời hạn của cuộc gọi                                                        | 10                                        |
| min_duration     | Thời hạn thực hiện cuộc gọi ít nhất.                                         | 5 - sẽ lấy CDR có duration lớn hơn 5 giây |
| extension        | Cuộc gọi từ extension nào                                                    | 101                                       |
| recordingfile    | File recording của cuộc gọi                                                  | abcd.mp3                                  |
| status           | Trạng thái cuộc gọi                                                          | ANSWERED                                  |
| phone            | Từ hoặc tới số điện thoại nào                                                | 0899888999                                |
| direction        | Chiều cuộc gọi (inbound, outbound, local)                                    | outbound                                  |
| start_date_ended | Tìm kiếm cdrs theo khoảng thời gian (Kết thúc cuộc gọi)                      | 2021-02-18 hoặc 2021-02-18 17:20:58       |
| end_date_ended   | Tìm kiếm cdrs theo khoảng thời gian (Kết thúc cuộc gọi)                      | 2021-02-19 hoặc 2021-02-19 00:00:00       |
| limit            | Số lượng record trả về                                                       | 50                                        |
| page             | Trang. (Pagination)                                                          | 1                                         |
| offset           | Vị trí bắt đầu khi query. (offset sẽ thay thế page nếu có data) (Pagination) | 0                                         |
| scroll_id        | Truyền vào sau lần query đầu tiên. (Scroll)                                  | abc123efsds...                            |

## Get a Specific CDR

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/cdr/01b7d166-b564-42ec-80a1-4ad343225934' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "id": "01b7d166-b564-42ec-80a1-4ad343225934",
  "sip_call_id": "aabbccddee112233..",
  "cause": "NORMAL_CLEARING",
  "duration": 45,
  "billsec": 39,
  "direction": 3,
  "recording_url": "",
  "extension": "101",
  "from_number": "19001919",
  "to_number": "0899888999",
  "receive_dest": "",
  "time_started": "2021-02-18 17:20:58",
  "time_answered": "",
  "time_ended": "2021-02-18 17:21:05",
  "status": "BUSY"
}
```

API dùng để lấy một cdr cụ thể.
Id có thể id của CDR hoặc sip_call_id trong bản tin

### HTTP Request

`GET https://{{API_HOST}}/v1/cdr/<ID>`

### URL Parameters

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| ID        | Id của CDR hoặc sip_call_id trong bản tin |
