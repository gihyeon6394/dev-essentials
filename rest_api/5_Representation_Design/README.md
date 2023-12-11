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

## Media Type Representation

## Error Representation

## Recap