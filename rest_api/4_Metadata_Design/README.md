# Chapter 4 Metadata Design

- HTTP Headers
- Media Types
- Media Type Design
- Recap

---

#### REST API headers

| code             | description                        |
|------------------|------------------------------------|
| `Content-Type`   | entity body의 media type을 나타냄       |
| `Content-Length` | entity body의 bytes 사이즈를 나타냄        |
| `Last-Modified`  | resource가 마지막으로 수정된 timestamp      |
| `ETag`           | 응답 message entity의 버전을 식별하는 임의 문자열 |
| `Cache-Control`  | TTL 기반의 캐시 값 (초 단위)                |
| `Location`       | resource의 URI를 나타냄                 |

## HTTP Headers

- _entity headers_ : HTTP request/response에 대한 metadata
- HTTP 는 표준 header를 정의해둠

### Rule: Content-Type must be used

- `Content-Type` : request, response body의 data _type_ 을 나타냄
- `media type` : `Content-Type` header의 value

### Rule: Content-Length should be used

- `Content-Length` : entity body의 bytes 사이즈
- Client에게 유용한 2가지 정보 제공
    - connection으로부터 정상적인 크기를 읽었는지
    - `HEAD` 요청으로 entity body 크기를 알 수 있음

### Rule: Last-Modified should be used in responses

- `Last-Modified` : entity가 마지막으로 수정된 timestamp
- response에만 포함 가능
- client나 캐시는 신선도 검사를 위해 사용 가능
- `GET` 요청에 대한 응답에 항상 포함되어야 함

### Rule: ETag should be used in responses

- `ETag` : entity의 버전을 식별하는 임의 문자열
- `GET` 요청에 대한 응답으로 항상 포함되어야 함
- Client는 `If-None-Match` request header로 사용 가능

### Rule: Stores must support conditional PUT requests

- store resource에 대한 `PUT` 은 insert, udpate로 모두 사용됨 (모호함)
- `PUT` with `If-Unmodified-Since` reqeust : API에게 resource가 특정 시각 이후로 수정되지 않았다면 PUT 요청을 수행하라고 알려줌
- `PUT` with `If-Match` request : API에게 resource가 특정 ETag와 일치한다면 PUT 요청을 수행하라고 알려줌

#### 예시

- client A, B
- REST API `/objects` store resource에 접근

1. client A가 `PUT /objects/2113` 요청
    - REST API가 resource `2113` create -> `201 Created` response
2. client B가 `PUT /objects/2113` 요청
    - REST API가 resource가 이미 존재하니 `409 Conflict` response
3. client B가 `PUT /objects/2113` 요청 (`If-Match` header 포함)
    - REST API는 _current_ entity tag value와 비교하여 다르면 `412 Precondition Failed` response
    - 같으면 resource를 update -> `200 OK` response (`Last-Modified`, `ETag` header 포함)

### Rule: Location must be used to specify the URI of a newly created resource

- `Location` response header : client가 원하는 resource의 URI
- `201 Created` response에 생성된 resource의 URI를 포함
- `202 Accepted` response에 비동기 controller resource URI를 포함

## Media Types

- message body에 담긴 data의 형태를 식별
  -_Content-Type_ header의 value가 참조하는 값

### Media Type Syntax

````
type "/" subtype *( ";" parameter )

Content-type: text/html; charset=ISO-8859-4
Content-type: text/plain; charset="us-ascii"
````

- _type_ 의 value : `application`, `audio`, `image`, `message`, `multipart`, `text`, `video` 중 하나
    - REST API는 주로 `application` type을 사용
- _subtype_ 의 value : type에 따라 다름 (계층구조)
- _parameter_ : media type의 특정 instance를 식별하는 추가 정보
    - `attribute=value` 쌍, `;`로 구분

### Registered Media Types

- Internet Assigned Numbers Authority (IANA) : media type을 관리
- 누구든지 새로운 media type을 등록할 수 있음
- https://www.iana.org/form/media-types

| media type               | description     |
|--------------------------|-----------------|
| _text/plain_             | 텍스트             |
| _text/html_              | HTML            |
| _image/jpeg_             | JPEG 이미지        |
| _application/xml_        | XML 문서          |
| _applicaiton/atom+xml_   | Atom _feed_     |
| _application/javascript_ | JavaScript 프로그램 |
| _application/json_       | JSON 문서         |

### Vendor-Specific Media Types

````
application/vnd.ms-excel
application/vnd.lotus-notes
text/vnd.sun.j2me.app-descriptor
````

- prefix `vnd.` : vendor-specific media type

## Media Type Design

### Rule: Application-specific media types should be used

- body에 단순히 파싱하는 것을 넘어서는 과정이 필요한 타입 (JSON)
- `Content-Type` 에 미디어 타입 (e.g. `application/json`)을 사용하는 것이 좋음
- 대체로 WRML을 사용할 수 있음

````
application/wrml;
 format="http://api.formats.wrml.org/application/json";
 schema="http://api.schemas.wrml.org/soccer/Player" 
````

#### Media Type Format Design

- `format` 을통해 _cacheable_ 한 문서로 이동가능하게 함
- 런타임에 필요한 여러 프로그램 언어를 지원할 수 있음
    - message body의 직(역)렬화 코드를 다운로드

#### Media Type Schema Design

- `schema` 을통해 _cacheable_ 한 문서로 이동가능하게 함
- resource type의 필드와 링크를 설명하는 스키마를 참조할 수 있음

#### Media Type Schema Versioning

````
schema="http://api.schemas.wrml.org/soccer/Player"
schema="http://api.schemas.wrml.org/soccer/Player-2"
````

- `-2` : major version
- 버전 정보로 식별하는 URI는 영구적으로 같은 스키마를 참조
    - e.g. `http://api.schemas.wrml.org/soccer/Player-2 `는 항상 같은 스키마

### Rule: Media type negotiation should be supported when multiple representations are available

````
Accept: application/wrml;
 format="http://api.formats.wrml.org/text/html";
 schema="http://api.schemas.wrml.org/soccer/Team" 
````

- `Accept` request header : client가 원하는 media type을 지정

### Rule: Media type selection using a query parameter may be supported

```Bash
GET /bookmarks/mikemassedotcom?accept=application/xml
````

- `accept` 쿼리 파라미터로 `Accept` header 미러링

## Recap

