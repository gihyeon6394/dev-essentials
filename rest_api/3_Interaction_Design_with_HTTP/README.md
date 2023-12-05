# Chapter 3. Interaction Design with HTTP

- HTTP/1.1
- Request Methods
- Response Status Codes
- Recap

---

## HTTP/1.1

- REST APIs 는 HTTP/1.1을 모두 사용 (request methods, response status codes, headers)

#### curl

- command-line open-source web client tool
- curl을 통해 _scriptable_ 하게 HTTP request를 보낼 수 있음

## Request Methods

```Bash
## RFC 2616 (HTTP/1.1) 에서 정의된 HTTP Request-Line
Request-Line   = Method SP Request-URI SP HTTP-Version CRLF
````

- `GET` : retrieve resource state
- `HEAD` : retrieve resource metadata
- `PUT` : add a new resource to store or update an existing resource
- `DELETE` : remove a resource from its parent
- `POST` : create a new resource (by controller)

### Rule: GET and POST must not be used to tunnel other request methods

- _Tunneling_ : HTTP를 남용해서 메시지의 의도를 숨기거나 잘못 나타내는 것
- 항상 HTTP request mehtod 목적에 맞게 사용할 것

```Bash
## Bad
GET /users/register
````

### Rule: GET must be used to retrieve a representation of a resource

- `GET`은 resource의 state를 retrieve하는데 사용
- header만 있고, body가 없는 request
- cache : origin server와 통신하지 않고 응답하는 것

```Bash
$ curl -v http://api.example.restapi.org/greeting
> GET /greeting HTTP/1.1
> User-Agent: curl/7.20.1
> Host: api.example.restapi.org
> Accept: */*

< HTTP/1.1 200 OK
< Date: Sat, 20 Aug 2011 16:02:40 GMT
< Server: Apache
< Expires: Sat, 20 Aug 2011 16:03:40 GMT
< Cache-Control: max-age=60, must-revalidate
< ETag: text/html:hello world
< Content-Length: 130
< Last-Modified: Sat, 20 Aug 2011 16:02:17 GMT
< Vary: Accept-Encoding
< Content-Type: text/html

<!doctype html><head><meta charset="utf-8"><title>Greeting</title></head>
<body><div id="greeting">Hello World!</div></body></html>
````

- `curl -v` : verbose mode (request, response 모두 출력)
- `GET /greeting` : greeting resource의 state를 retrieve

### Rule: HEAD should be used to retrieve response headers

- `HEAD`는 `GET`과 동일한 response를 반환하지만 body가 없음
- body 없이 header만 retrieve
- 목적 : resouce 존재 여부 check, meadtadata retrieve

```Bash
$ curl --head http://api.example.restapi.org/greeting

HTTP/1.1 200 OK
Date: Sat, 20 Aug 2011 16:02:40 GMT
Server: Apache
Expires: Sat, 20 Aug 2011 16:03:40 GMT
Cache-Control: max-age=60, must-revalidate
ETag: text/html:hello world
Content-Length: 130
Last-Modified: Sat, 20 Aug 2011 16:02:17 GMT
Vary: Accept-Encoding
Content-Type: text/html
````

### Rule: PUT must be used to both insert and update a stored resource

- `PUT`은 store에 resource를 추가하거나 업데이트하는데 사용

````Bash
PUT /accounts/4ef2d5d0-cb7e-11e0-9572-0800200c9a66/buckets/objects/4321
````

### Rule: PUT must be used to update mutable resources

- `PUT`은 mutable resource를 update하는데 사용
- body에는 resource의 새로운 state를 담아서 전송

### Rule: POST must be used to create a new resource in a collection

- `POST`는 collection에 새로운 resource를 생성하는데 사용
- body에는 _suggested_ state를 담아서 요청
- 은유적으로 게시판에 message를 "포스팅"

```Bash
POST /leagues/seattle/teams/trebuchet/players
```

### Rule: POST must be used to execute controllers

- `POST`는 반드시 controller를 실행하는데 사용
- `POST` 의 header와 body는 controller의 input으로 전달
- **`POST`는 리소스를 가져오거나, 저장하거나, 삭제하는데 사용하면 안됨**
- 가져옴(GET), 저장(PUT), 삭제(DELETE)는 모두 `POST`로 처리하면 안됨
- _unsafe_ , _non-idempotent_ : 결과가 항상 같음을 보장하지 않음

````
POST /alerts/245743/resend
````

### Rule: DELETE must be used to remove a resource from its parent

- `DELETE`는 resource를 parent로부터 삭제하는데 사용 (parent : store, collection)
- `DELETE` 요청이 완료되면 더이상 client는 resource를 retrieve할 수 없음
    - 따라서 다음 `GET`, `HEAD` 요청에 대한 응답으로 `404 Not Found`
- 오버로딩하면 안됨
    - "soft" delete : resource를 삭제하지 않고, 삭제된 것처럼 표시
    - "hard" delete : resource를 삭제하고, 삭제된 것처럼 표시
    - "soft" delete는 `DELETE`로 처리하면 안됨 (`POST`로 처리)

```Bash
DELETE /accounts/4ef2d5d0-cb7e-11e0-9572-0800200c9a66/buckets/objects/4321
```

### Rule: OPTIONS should be used to retrieve metadata that describes a resource’s available interactions

- resource metadata를 retrieve하는데 사용
    - 응답 header에 `Allow` 헤더를 포함해서 반환

````Bash
Allow: GET, PUT, DELETE
````

- 응답 body에 각 method에 대한 설명을 포함해서 반환
    - e.g. 각 method 별로 관련된 link를 포함해서 반환

## Response Status Codes

- RFC 2616에서 정의한 `Status-Line` : HTTP response status code

````Bash
Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF
````

| Category            | Description            |
|:--------------------|:-----------------------|
| 1xx : Informational | protocol-level의 정보 제공  |
| 2xx : Success       | request 성공             |
| 3xx : Redirection   | client가 추가 동작을 취해야함    |
| 4xx : Client Error  | client의 잘못된 요청으로 인한 에러 |
| 5xx : Server Error  | server의 잘못된 요청으로 인한 에러 |

### Rule: 200 (“OK”) should be used to indicate nonspecific success

- client가 요청한 것을 완벽히 처리했음을 의미
- `200` 응답에는 response body가 있어야함

### Rule: 200 (“OK”) must not be used to communicate errors in the response body

- `200` 응답 response body에는 error message를 포함해서는 안됨

### Rule: 201 (“Created”) must be used to indicate successful resource creation

- collection 생성, store에 resource 추가, controller 실행을 통한 resource 생성에 사용

### Rule: 202 (“Accepted”) must be used to indicate successful start of an asynchronous action

- client의 요청이 비동기적으로 처리됨을 의미
- request가 유용하지만 최종적으로 처리될때까지 문제발생 가능성은 있는 상태
- 처리 시간이 오래 걸리는 작업에 대해 사용
- controller에서만 사용

### Rule: 204 (“No Content”) should be used when the response body is intentionally empty

- `PUT`, `POST`, `DELETE`에 대한 응답
- response body에 상태 메시지나 representation을 포함하지 않으려할 때 사용

### Rule: 301 (“Moved Permanently”) should be used to relocate resources

- 요청한 resource가 새로운 URI로 이동했음을 의미
- 응답 header에 `Location` 헤더를 포함해서 반환

### Rule: 302 (“Found”) should not be used

- HTTP/1.1부터 `303("See Other")`, `307("Temporary Redirect")`로 대체해서 사용할 것

### Rule: 303 (“See Other”) should be used to refer the client to a different URI

- controller가 실행이 완료되었고, client에게 resource에 대한 URI를 알려줄 때 사용

### Rule: 304 (“Not Modified”) should be used to preserve bandwidth

- `204("No Content")`와 유사, 응답 body가 비어있음
- `204("No Content")` : body가 비어있을 때
- `304("Not Modified")` : body가 비어있고, client의 cache가 최신 상태일 때

### Rule: 307 (“Temporary Redirect”) should be used to tell clients to resubmit the request to another URI

- client에게 다른 URI로 요청을 다시 보내라고 알려줄 때 사용
- REST API가 cleint 요청대로 수행할 수없고, 응답의 `Location` header로 요청해야할 것

### Rule: 400 (“Bad Request”) may be used to indicate nonspecific failure

- client의 잘못된 요청으로 인해 실패했음을 의미
- 다른 4xxx error 중 적절한 것이 없을 때

### Rule: 401 (“Unauthorized”) must be used when there is a problem with the client’s credentials

- client가 부적절한 인증정보를 가지고 resource에 접근
- 불충분한 권한

### Rule: 403 (“Forbidden”) should be used to forbid access regardless of authorization state

- REST API가 접근을 허용하지 않음
- application-level permission (e.g. application 권한 밖의 리소스에 접근하는 것)
- `401` : 불충분한 권한
- `403` : 접근을 허용하지 않음

### Rule: 405 (“Method Not Allowed”) must be used when the HTTP method is not supported

- resource에 대해 HTTP method가 지원되지 않음을 의미
- e.g. read-only resource에 대해 `PUT`, `POST`, `DELETE`를 사용할 때
- `405` 응답에는 `Allow` header 필수

````
Allow: GET, POST
````

### Rule: 406 (“Not Acceptable”) must be used when the requested media type cannot be served

- 요청한 media type이 지원되지 않음을 의미
- `Accept` header에 지원되지 않는 media type이 포함되어 있을 때
- e.g. `Accept: application/xml` 인데, API가 응답으로 `application/json`만 지원할 때

### Rule: 409 (“Conflict”) should be used to indicate a violation of resource state

- resource를 불가능한 상태로 변경하려 할때
- e.g. 비어있지 않은 resrouce에 대한 `DELETE` 요청

### Rule: 412 (“Precondition Failed”) should be used to support conditional operations

- client가 request header에 사전조건을 지정하여 요청 -> 조건이 충족되지 않음

### Rule: 415 (“Unsupported Media Type”) must be used when the media type of a request’s payload cannot be processed

- request payload의 media type이 지원되지 않음을 의미
- request header `Content-Type` 에 지원되지 않는 media type이 포함되어 있을 때

### Rule: 500 (“Internal Server Error”) should be used to indicate API malfunction

- 대부분의 web framework는 request handler 내에서 발생한 exception을 `500`으로 처리
- `500` 에러는 절대 client의 fault가 아님
- client가 동일한 request를 다시 보내면 hop에서 적절히 처리하도록 수정하여 응답해야함

## Recap