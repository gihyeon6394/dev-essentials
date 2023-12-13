# Chapter 5 Representation Design

- Message Body Format
- Hypermedia Representation
- Media Type Representation
- Error Representation
- Recap

---

## Message Body Format

- REST API는 주로 entity body를 통해 resource 상태를 표현
- entity body는 text 기반으로 주로 XML과 JSON을 사용

### Rule: JSON should be supported for resource representation

- JSON은 REST API에서 가장 일반적으로 사용되는 표현
- resource의 표준 포맷이 없는 경우 JSON으로 표현해야함

### Rule: JSON must be well-formed

- JSON은 정렬되지 않은 name/value 쌍의 집합
- name은 항상 double quote로 둘러싸야 함
    - `"` 가 없어도 REST API는 JSON으로 인식할 수 있긴 함
- name에 특수문자가 가능하지만 지양

```json
{
  "firstName": "Osvaldo",
  "lastName": "Alonso",
  "firstNamePronunciation": "ahs-VAHL-doe",
  "number": 6,
  "birthDate": "1985-11-11"
}
```

### Rule: XML and other formats may optionally be used for resource representation

- JSON의 대체로 XML과 같은 다른 포맷을 사용할 수 있음
- WRML 스키마에 의해 다양한 markup, formmating 언어 사용 가능
    - JSON, XML : 다른 client 프로그램에서 쉽게 접근 사용 가능
    - HTML : 웹 브라우저에서 접근 가능
    - javascript : 런타임에 필요한 코드를 다운로드 가능

### Rule: Additional envelopes must not be created

- HTTP에서 제공하는 "envelope"을 사용해야 함

## Hypermedia Representation

- representation에 hypermedia를 포함
- link에는 추가적인 action이나 resource에 대한 정보를 포함

### Rule: A consistent form should be used to represent links

````
application/wrml;
    format="http://api.formats.wrml.org/application/json";
    schema="http://api.schemas.wrml.org/common/Link" 
````

```
{
    "href" : Text <constrained by URI or URI Template syntax>,
    "rel" : Text <constrained by URI syntax>,
    "requestTypes" : Array <constrained to contain media type text elements>,
    "responseTypes" : Array <constrained to contain media type text elements>,
    "title" : Text
}
```

- `href` : link의 target resource를 가리키는 URI
- `rel` : link와의 관계를 나타내는 문서
- `requestTypes` : link가 허용하는 request body media type
- `responseTypes` : link의 응답으로 가능한 response body media type
- `title` : link의 title

```json
{
  "href": "http://api.soccer.restapi.org/players/2113",
  "rel": "http://api.relations.wrml.org/common/self"
}
```

- `href` : link의 target resource를 가리키는 URI
- `rel` :  TODO. what?

````
{
  "href" : "http://api.soccer.restapi.org/players/2113",
  "rel" : "http://api.relations.wrml.org/common/self",
  "responseTypes" : [
    "application/wrml;
    format=\"http://api.formats.wrml.org/application/json\";
    schema=\"http://api.schemas.wrml.org/soccer/Player\"",
    
    "application/wrml;
    format=\"http://api.formats.wrml.org/application/xml\";
    schema=\"http://api.schemas.wrml.org/soccer/Player\"",
    
    "application/wrml;
    format=\"http://api.formats.wrml.org/text/html\";
    schema=\"http://api.schemas.wrml.org/soccer/Player\"",
    
    "application/json",
    "application/xml",
    "text/html"
  ],
  "title" : "Osvaldo Alonso"
}
````

- `responseTypes` : link의 응답으로 가능한 response body media type

### Rule: A consistent form should be used to represent link relations

- `rel` 은 현재 resource와 `href`로 가리키는 resource 사이의 관계를 나타냄

````
application/wrml;
 format="http://api.formats.wrml.org/application/json";
 schema="http://api.schemas.wrml.org/common/LinkRelation" 
````

````
{
 "name" : Text,
 "method" : Text <constrained to be choice of HTTP method>,
 "requestTypes" : Array <constrained to contain media type text elements>,
 "responseTypes" : Array <constrained to contain media type text elements>,
 "description" : Text,
 "title" : Text
}
````

- `name` : link relation의 이름
- `method` : link relation의 HTTP method (없으면, `GET`으로 간주)
- `requestTypes` : link relation이 허용하는 request body media type
- `responseTypes` : link relation의 응답으로 가능한 response body media type
- `description` : link relation의 설명
- `title` : link relation의 title

```Bash
# Request
GET /common/self HTTP/1.1
Host: api.relations.wrml.org

# Response
HTTP/1.1 200 OK
Content-Type: application/wrml;
 format="http://api.formats.wrml.org/application/json";
 schema="http://api.schemas.wrml.org/common/LinkRelation"
 
# NOTE: The description's line breaks must be omitted in well-formed JSON.
{
  "name" : "self",
  "method" : "GET",
  "description" : "Signifies that the URI in the value of the href
    property identifies a resource equivalent to the
    containing resource."
}
```

### Rule: A consistent form should be used to advertise links

- `links` : resource에 포함된 link의 배열

````json
{
  "firstName": "Osvaldo",
  "lastName": "Alonso",
  "links": {
    "self": {
      "href": "http://api.soccer.restapi.org/players/2113",
      "rel": "http://api.relations.wrml.org/common/self"
    },
    "parent": {
      "href": "http://api.soccer.restapi.org/players",
      "rel": "http://api.relations.wrml.org/common/parent"
    },
    "team": {
      "href": "http://api.soccer.restapi.org/teams/seattle",
      "rel": "http://api.relations.wrml.org/soccer/team"
    },
    "addToFavorites": {
      "href": "http://api.soccer.restapi.org/users/42/favorites/{name}",
      "rel": "http://api.relations.wrml.org/common/addToFavorites"
    }
  }
}
````

### Rule: A self link should be included in response message body representations

- identifiable resource을 담은 body가 포함된 response는 반드시 `self` link를 포함해야 함
- `href` 값으로 현재 resource를 가리키는 URI를 사용해야 함

```json
{
  "id": 123,
  "name": "Example Resource",
  "description": "This is an example resource.",
  "self": {
    "href": "/api/resources/123"
  }
}
````

### Rule: Minimize the number of advertised “entry point” API URIs

- TODO. what

### Rule: Links should be used to advertise a resource’s available actions in a state-sensitive manner

```json
{
  "links": {
    "self": {
      "href": "http://api.editor.restapi.org/docs/48679",
      "rel": "http://api.relations.wrml.org/common/self"
    },
    "cut": {
      "href": "http://api.editor.restapi.org/docs/48679/edit/cut",
      "rel": "http://api.relations.wrml.org/editor/edit/cut"
    },
    "copy": {
      "href": "http://api.editor.restapi.org/docs/48679/edit/copy",
      "rel": "http://api.relations.wrml.org/editor/edit/copy"
    }
  }
}
```

- 클립보드 프로그램에서 `paste` 메뉴에 사용할 json

```json
{
  "links": {
    "self": {
      "href": "http://api.editor.restapi.org/docs/48679",
      "rel": "http://api.relations.wrml.org/common/self"
    },
    "paste": {
      "href": "http://api.editor.restapi.org/docs/48679/edit/paste",
      "rel": "http://api.relations.wrml.org/editor/edit/paste"
    }
  }
}
```

## Media Type Representation

## Error Representation

## Recap