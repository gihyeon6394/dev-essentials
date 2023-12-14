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

- `application/wrml ` media type은 `format`, `schema` 를 포함
- `format`, `schema`는 서로 다른 URI를 가질 수 있음

### Rule: A consistent form should be used to represent media type formats

- `application/wrml` 의 `format`은 content의 format을 가리키는 document

```
application/wrml;
  format="http://api.formats.wrml.org/application/json";
  schema="http://api.schemas.wrml.org/common/Format" 
```

````
{
  "mediaType" : Text <constrained by media type syntax>,
  "links" : {
    "home" : Link <form constrained by the Link schema>,
    "rfc" : Link <form constrained by the Link schema>
  },
  "serialize" : {
    "links" : {
      <Set of Link schema-constrained forms>
    }
  },
  "deserialize" : {
    "links" : {
      <Set of Link schema-constrained forms>
    }
  }
}
````

````
# Request
GET /application/json HTTP/1.1
Host: api.formats.wrml.org

# Response
HTTP/1.1 200 OK
Content-Type: application/wrml;
  format="http://api.formats.wrml.org/application/json";
  schema="http://api.schemas.wrml.org/common/Format"
{
  "mediaType" : "application/json",
  "links" : {
    "self" : {
      "href" : "http://api.formats.wrml.org/application/json",
      "rel" : "http://api.relations.wrml.org/common/self"
  },
    "home" : {
      "href" : "http://www.json.org",
      "rel" : "http://api.relations.wrml.org/common/home"
    },
    "rfc" : {
    "href" : "http://www.rfc-editor.org/rfc/rfc4627.txt",
    "rel" : "http://api.relations.wrml.org/format/rfc"
    }
  },
  "serialize" : {
    "links" : {
      "java" : {
        "href" : "http://api.formats.wrml.org/application/json/serializers/java",
        "rel" : "http://api.relations.wrml.org/format/serialize/java"
      },
      "php" : {
        "href" : "http://api.formats.wrml.org/application/json/serializers/php",
        "rel" : "http://api.relations.wrml.org/format/serialize/php"
      }
    }
  },
  "deserialize" : {
    "links" : {
      "java" : {
        "href" : "http://api.formats.wrml.org/application/json/deserializers/java",
        "rel" : "http://api.relations.wrml.org/format/deserialize/java"
      },
      "perl" : {
        "href" : "http://api.formats.wrml.org/application/json/deserializers/perl",
        "rel" : "http://api.relations.wrml.org/format/deserialize/perl"
      }
    }
  }
}
````

### Rule: A consistent form should be used to represent media type schemas

- _fields_ 를 디비 컬럼, 클래스 속성, 웹페이지 변수로 사용할 수 있음

````
application/wrml;
 format="http://api.formats.wrml.org/application/json";
 schema="http://api.schemas.wrml.org/common/Schema" 
````

````
{
  "name" : Text <constrained to be mixed uppercase>,
  "version" : Integer,
  "extends" : Array <constrained to contain (schema) URI text elements>,
  "fields" : {
    <Set of Field schema-constrained forms>
  },
  "stateFacts" : Array <constrained to contain mixed uppercase text elements>,
  "linkFormulas" : {
    <Set of LinkFormula schema-constrained forms>
  },
  "description" : Text
}
````

- `extends` : 스키마의 base 스키마

#### Field Representation

- Boolean : `true` or `false` (텍스트)
- Choice : Java의 enum과 비슷
- DateTime : ISO 8601 format의 날짜와 시간
- Double ": IEEE 754 format의 64-bit 부동소수점 (e.g. `3.141592653589793`)
- Integer : 32-bit signed integer (e.g. `42`), Java의 int
- List : 순서가 있는 값의 배열 (0부터 시작)
- Schema : schema의 URI를 지키는 텍스트 기반 값ㅣ
- Text : 0 이사으이 Unicode 문자열, `"` 로 감쌈, `\` 로 escape
- null : 텍스트 `null`을 사용

````
application/wrml;
  format="http://api.formats.wrml.org/application/json";
  schema="http://api.schemas.wrml.org/common/Field"
````

````
{
  "type" : Text <constrained to be one of the primitive field types>,
  "defaultValue" : <a type-specific value>,
  "readOnly" : Boolean,
  "required" : Boolean,
  "hidden" : Boolean,
  "constraints" : Array <constrained to contain (constraint) URI text elements>,
  "description" : Text
}
````

#### Constraint Representation

- `constraints` : field의 제약조건을 나타내는 URI
- 제약조건
    - value로 가능한 최소 최대값
    - 가능한 text literal
    - List의 element로 가능한 element type
    - syntax (e.g. URI , URI template, regex pattern)

````
application/wrml;
  format="http://api.formats.wrml.org/application/json";
  schema="http://api.schemas.wrml.org/common/Constraint"
````

````
{
  "name" : Text,
    "validate" : {
    "links" : {
      <Set of Link schema-constrained forms>
    }
  }
}
````

#### Link Formula Representation

- _link formula_ : response body의 state-sensitive link에 대한 formula
    - e.g. Game에 대한 응답일 때 게임이 종료되어야만 socre link가 포함됨

````
self = Identifiable
parent = Identifiable and not Docroot
update = Identifiable and not ReadOnly
recap = Final
scoreboard = InProgress or Final
````

- `self` : 현재 resource를 가리키는 link
- `parent` : 현재 resource의 parent를 가리키는 link
- `recap` : 게임이 종료되면 포함되는 link
- `scoreboard` : 게임이 진행중이거나 종료되면 포함되는 link

````
application/wrml;
  format="http://api.formats.wrml.org/application/json";
  schema="http://api.schemas.wrml.org/common/LinkFormula"
````

````
{
  "rel" : Text <constrained by URI syntax>,
  "condition" : Text <constrained to be a state fact-based Boolean expression>
}

````


## Error Representation

## Recap