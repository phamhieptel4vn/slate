# Event

## Get Events

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/event' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "data": [
    {
      "id": "avavavav-1111-2222-3333-eeeeeeee",
      "event": "hangup",
      "callback_url": "https://webhook.demo/",
      "callback_apikey": "foo"
    },
    ...
  ],
  "total": 1
}
```

Trả về các call events của tenant.

### HTTP Request

`GET https://{{API_HOST}}/v1/event`

## Create Events

```shell
curl -L -X POST 'https://{{API_HOST}}/v1/event' \
-H 'Authorization: Bearer {{TOKEN}}' \
-H 'Content-Type: application/json' \
--data-raw '{
    "callback_url" : "https://webhook.demo/",
    "callback_apikey" : "foo",
    "event" : "create"
}'
```

> Response trả về:

```json
{
  "created": true,
  "id": "avavavav-1111-2222-3333-eeeeeeee"
}
```

Tạo Event Hook, mỗi lần bắt được {event} tổng đài sẽ hook dữ liệu về {callback_url}.

### HTTP Request

`POST https://{{API_HOST}}/v1/event`

### Body

| Parameter       | Description                                    |
| --------------- | ---------------------------------------------- |
| callback_url    | Domain Url mà tổng đài sẽ hook dữ liệu tới     |
| callback_apikey | ApiKey hoặc access token của domain (optional) |
| event           | SIP Call Event                                 |

### SIP Call Event

| Parameter    | Description                                                               |
| ------------ | ------------------------------------------------------------------------- |
| hangup       | Sự kiện khi cuộc gọi bị ngắt, huỷ                                         |
| ringing      | Sự kiện khi cuộc gọi được khởi tạo                                        |
| answered     | Sự kiện khi cuộc gọi được nhấc máy                                        |
| cdr          | Sự kiện sau khi cdr được tạo xong                                         |
| missed       | Sự kiện khi có cuộc gọi inbound tới nhưng không có extension nào nghe máy |
| cdr_autocall | Sự kiện sau khi cdr của autocall được tạo xong                            |

<div class="alert alert-danger alert-dismissible fade show" role="alert">Nếu bạn tạo event nằm ngoài các event ở trên, hệ thống sẽ không nhận diện được nên sẽ không hook data về.</div>
<div class="alert alert-warning alert-dismissible fade show" role="alert">Nếu bạn cần hook event ngoài các event ở trên, vui lòng gửi mail hỗ trợ.</div>

## Delete Event

```shell
curl -L -X DELETE 'https://{{API_HOST}}/v1/event/eeeeeeee-1111-2222-3333-eeeeeeee' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "deleted": true
}
```

API dùng để xoá một event_domain.

### HTTP Request

`DELETE https://{{API_HOST}}/v1/event/<ID>`

### URL Parameters

| Parameter | Description  |
| --------- | ------------ |
| ID        | ID của event |

## Event Hook Data

> Response trả về:

```json
{
  "call_id": "avavavav-1111-2222-3333-eeeeeeee",
  "sip_call_id": "dddddddd-1111-2222-3333-eeeeeeee",
  "domain": "test.tel4vn.com",
  "direction": "outbound",
  "from_number": "0899888999",
  "to_number": "101",
  "hotline": "19001919",
  "state": "hangup",
  "duration": 10,
  "billsec": 5,
  "recording_url": "https://recording.demo/ad4c9b90-c071-405a-9723-980d2e5e1623"
}
```

### Description

| Parameter     | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| call_id       | Id định danh cuộc gọi                                             |
| sip_call_id   | SIP Call Id                                                       |
| domain        | Domain nhận hoặc thực hiện cuộc gọi                               |
| direction     | Hướng cuộc gọi (inbound / outbound)                               |
| from_number   | Số gọi. Sẽ là số ext nếu cuộc gọi là outbound                     |
| to_number     | Số nhận. Sẽ là số ext nếu cuộc gọi là inbound                     |
| hotline       | Đầu số nhận hoặc thực hiện cuộc gọi                               |
| state         | ringing / answered / hangup / cdr                                 |
| duration      | Tổng thời lượng cuộc gọi. (Riêng sự kiện hangup)                  |
| billsec       | Thời lượng tính từ khi hai bên kết nối. (Riêng sự kiện hangup)    |
| recording_url | URL public để play file ghi âm. (Riêng sự kiện cdr)               |
| press_key     | Phím bấm của mobile nếu có (Riêng sự kiện cdr_autocall)           |
| customer_id   | Id của customer khi import vào queue (Riêng sự kiện cdr_autocall) |

## Note\*

Một số thông tin cần lưu ý khi tích hợp event:

- Event ringing, answered, hangup là các event theo luồng của một cuộc gọi.
- Event cdr là event sẽ gửi tới webhook sau khi tổng đài cập nhật CDR.
- Đối với các case outbound: call_id của các event sẽ map giống nhau do tồn tại trên một cuộc gọi.
- ĐốI với các case inbound:
  - Trường hợp 1 mobile - 1 extension: call_id của các event sẽ map giống nhau do tồn tại trên một cuộc gọi.
  - Trường hợp 1 mobile - nhiều extension (Ringing, Queue, IVR): call_id của các event ringing, answered, hangup sẽ giống nhau. call_id của event cdr sẽ khác nhau. Do call_id của các event ringing, answered, hangup là call_id xử lý của cuộc gọi trên từng extension, call_id của event cdr là call_id của cả một luồng inbound (Ringing, Queue, IVR).
