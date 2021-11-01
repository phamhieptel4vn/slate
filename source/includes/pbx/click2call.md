# Click-to-call

## Synchronous

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/click2call?ext=101&phone=0899098899' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

Nếu Extension đã login thì API Click-to-call Synchronous sẽ chờ tới khi extension nhấc máy hoặc ngắt máy.

<div class="alert alert-success alert-dismissible fade show" role="alert">
Đầu số hotline dùng để gọi ra ngoài bạn vui lòng liên hệ team TEL4VN để được cung cấp và cài đặt.
</div>

### HTTP Request

`GET https://{{API_HOST}}/v1/click2call?ext=<EXTENSION>&phone=<PHONE>`

### URL Parameters

| Parameter       | Description                                               | Required |
| --------------- | --------------------------------------------------------- | -------- |
| ext             | Extension thực hiện cuộc gọi                              | true     |
| phone           | Số điện thoại sẽ được gọi tới                             | true     |
| src_cid_name    | Tên hiển thị trên máy extension. Default: Click2Call      | false    |
| src_cid_number  | Số hiển thị trên máy extension. Default: Số của extension | false    |
| dest_cid_name   | Tên của số điện thoại sẽ chèn vào bản tin SIP             | false    |
| dest_cid_number | Số điện thoại sẽ chèn vào bản tin SIP                     | false    |
| auto_answer     | Tự động nhấc máy phía extension. Default: false           | false    |
| hotline         | Đầu số hotline để thực hiện cuộc gọi ra ngoài             | false    |

## Asynchronous

```shell
curl -L -X GET 'https://{{API_HOST}}/v1/click2call/async?ext=101&phone=0899098899' \
-H 'Authorization: Bearer {{TOKEN}}'
```

> Response trả về:

```json
{
  "status": "success",
  "call_id": "01b7d166-b564-42ec-80a1-4ad343225934"
}
```

> Error Response trả về:

```json
{
  "status": "fail",
  "message": "User not registered extension with Softphone or IP Phone",
  "error": "USER_NOT_REGISTERED"
}
```

API dùng để thực hiện click-to-call.

Sau khi thực hiện click-to-call, hệ thống sẽ gọi vào số extension của domain, sau khi extension pickup cuộc gọi thì có một cuộc gọi đẩy ra số mobile dựa vào parameter của API.

API Click-to-call Asynchronous sẽ không chờ tới khi extension nhấc máy hoặc ngắt máy, mà sẽ trả về call_id nếu extension đã login và trả về mã lỗi nếu extension không login.

<div class="alert alert-success alert-dismissible fade show" role="alert">
Đầu số hotline dùng để gọi ra ngoài bạn vui lòng liên hệ team TEL4VN để được cung cấp và cài đặt.
</div>

### HTTP Request

`GET https://{{API_HOST}}/v1/click2call/async?ext=<EXTENSION>&phone=<PHONE>`

### URL Parameters

| Parameter       | Description                                               | Required |
| --------------- | --------------------------------------------------------- | -------- |
| ext             | Extension thực hiện cuộc gọi                              | true     |
| phone           | Số điện thoại sẽ được gọi tới                             | true     |
| src_cid_name    | Tên hiển thị trên máy extension. Default: Click2Call      | false    |
| src_cid_number  | Số hiển thị trên máy extension. Default: Số của extension | false    |
| dest_cid_name   | Tên của số điện thoại sẽ chèn vào bản tin SIP             | false    |
| dest_cid_number | Số điện thoại sẽ chèn vào bản tin SIP                     | false    |
| auto_answer     | Tự động nhấc máy phía extension. Default: false           | false    |
| hotline         | Đầu số hotline để thực hiện cuộc gọi ra ngoài             | false    |

## Error

| Parameter           | Description                                   |
| ------------------- | --------------------------------------------- |
| USER_BUSY           | Người dùng hiện tại đang bận                  |
| USER_NOT_REGISTERED | Người dùng chưa login Softphone hoặc IP Phone |
