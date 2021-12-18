# [ REST ]

## 1. REST(Representational State Transfer)

```tex
▫️ 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미함. 즉, 자원(resorce)의 표현(representation)에 의한 상태 전달
```

+ 자원(resource)의 표현(representation)

  + 자원 : 해당 소프트웨어가 관리하는 모든 것. ex) 문서, 그림, 데이터, ...
  + 자원의 표현 : 그 자원을 표현하기 위한 이름. ex) DB의 학생정보가 '자원'일 때, 'students'를 자원의 표현으로 정함

+ 상태(정보) 전달

+ 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달함

+ JSON or XML을 통해 데이터를 주고받는 것이 일반적임

+ 월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식

+ REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일임

+ REST는 네트워크 상에서 서버 <-> 클라이언트 사이의 통신 방식 중 하나임

+ REST의 구체적인 개념

  ```tex
  ▫️ HTTP URL(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것
  ```

  + 즉, **REST**는 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍처를 의미함
  + 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여함
  + CRUD Operation
    1. **POST** : 생성(Create)
    2. **GET** : 조회(Read)
    3. **PUT** : 수정(Update)
    4. **DELETE** : 삭제(Delete)
    5. **HEAD** : header 정보 조회(Head)

## 2. REST가 필요한 이유

+ 애플리케이션 분리 및 통합
+ 다양한 클라이언트의 등장
+ 최근의 서버 프로그램은 다양한 브라우저와 안드로이드, 아이폰과 같은 모바일 디바이스에서도 통신할 수 있어야 함
+ 이러한 멀티 플랫폼에 대한 지원을 위해 서비스 자원에 대한 아키텍처를 세우고 이용하는 방법을 모색한 결과 REST에 관심을 가지게 됨

## 3. REST의 장단점

+ 장점
  + HTTP 프로토콜의 인프라를 그대로 사용함 → REST API 사용을 위한 별도의 인프라를 구축할 필요가 없음
  + HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해줌
  + HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능함
  + Hypermedia API의 기본을 충실히 지키면서 범용성을 보장함
  + REST API 메세지가 의도하는 바를 명확하게 나타냄 → 의도하는 바를 쉽게 파악할 수 있음
  + 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화함
  + 서버와 클라이언트의 역할을 명확하게 분리함
+ 단점
  + 표준이 존재하지 않음
  + 사용할 수 있는 메소드가 4가지 밖에 없음 → HTTP Method 형태가 제한적임
  + 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 값이 더 어렵게 느껴짐
  + 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재함 (PUT, DELETE 사용 불가, pushState 지원 불가)

## 4. REST 구성 요소

1. **자원(Resource)** : URI
   + 모든 자원에 고유한 ID가 존재하고, 이 자원은 서버에 존재함
   + 자원을 구별하는 ID는 '/group/:group_id'와 같은 HTTP URI임
   + Client는 URI를 이용하여 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 서버에 요청함
2. **행위(Verb)** : HTTP Method
   + HTTP 프로토콜의 Method를 사용함
   + HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메소드를 제공함
3. **표현(Representation of Resource)**
   + 클라이언트가 자원의 상태(정보)에 대한 조작을 요청하면 서버는 이에 적절한 응답(Representation)을 보냄
   + REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어질 수 있음
   + JSON or XML을 통해 데이터를 주고 받는 것이 일반적임

## 5. REST의 특징

1. **Server-Client (서버-클라이언트) 구조**

   + 서버 : 자원이 있는 쪽 / 클라이언트 : 자원을 요청하는 쪽

     + REST 서버 : API를 제공하고 비즈니스 로직 처리/저장을 책임짐

     + 클라이언트 : 사용자 인증 or context(세션, 로그인 정보) 등을 직접 관리하고 책임짐

   + 서로 간 의존성이 줄어듦

2. **Stateless (무상태)**

   + HTTP 프로토콜은 Stateless Protocol임 → REST 역시 무상태성을 갖음
   + 클라이언트의 context를 서버에 저장하지 않음 → 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해짐
   + 서버는 각각의 요청을 완전히 별개의 것으로 인식하고 처리함
     + 각 API 서버는 클라이언트의 요청만을 단순 처리함
     + 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안됨
     + 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용함
     + 서버의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아짐

3. **Cacheable (캐시 처리 가능)**

   + 웹 표준 HTTP 프로토콜을 그대로 사용함 → 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있음
     + 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있음
     + HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그 or E-Tag를 이용하면 캐싱 구현이 가능함
   + 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구됨
   + 캐시 사용을 통해 응답시간이 빨라지고 REST 서버 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있음

4. **Layered System (계층화)**

   + 클라이언트는 REST API 서버만 호출함
   + REST 서버는 다중 계층으로 구성될 수 있음
     + API 서버는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있음
     + 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있음
   + PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있음

5. **Code-On-Demand (optional)**

   + 서버로부터 스크립트를 받아서 클라이언트에서 실행함 → 반드시 충족할 필요는 없음

6. **Uniform Interface (인터페이스 일관성)**

   + URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행함
   + HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능함
     + 특정 언어 or 기술에 종속되지 않음

<br/>

# [ REST API ]

## 1. REST API

+ API(Application Programming Interface)

  ```tex
  ▫️ 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램 간 상호작용을 촉진하며, 서로 정보를 교환가능하도록 하는 것
  ```

+ REST API

  ```tex
  ▫️ REST 기반으로 서비스 API를 구현한 것
  ```

  + 최근 OpenAPI, 마이크로서비스 등을 제공하는 업체 대부분은 REST API를 제공함

## 2. REST API 특징

+ 사내 시스템들도 REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있음
+ REST는 HTTP 표준을 기반으로 구현함 → HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있음
+ 즉, REST API를 제작하면 델파이 클라이언트 뿐만 아니라 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있음

## 3. REST API 설계 기본 규칙

+ 리소스 원형
  + **도큐먼트(Document)** : 객체 인스턴스 or DB 레코드와 유사한 개념
  + **컬렉션(Collection)** : 서버에서 관리하는 디렉토리라는 리소스
  + **스토어(Store)** : 클라이언트에서 관리하는 리소스 저장소

1. URI는 정보의 자원을 표현해야 함
   1. resource는 동사보다는 명사를, 대명사보다는 소문자를 사용함
   2. resource의 도큐먼트 이름으로는 단수 명사를 사용해야 함
   3. resource의 컬렉션 이름으로는 복수 명사를 사용해야 함
   4. resource의 스토어 이름으로는 복수 명사를 사용해야 함<br/> ex) GET /Member/1 → GET /members/1

2. 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현함
   1. URI에 HTTP Method가 들어가면 안됨<br/> ex) GET /members/delete/1 → DELETE /members/1
   2. URI에 행위에 대한 동사 표현이 들어가면 안됨(즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않음)<br/> ex) GET /members/show/1 → GET /members/1
   3. 경로 부분 중 변하는 부분은 유일한 값으로 대체함 (즉, :id는 하나의 특정 resource를 나타내는 고유 값임)<br/> ex) student를 생성하는 route : POST /students<br/> ex) id = 12인 student를 삭제하는 route : DELETE /students/12

## 4. REST API 설계 규칙

1. 슬래시 구분자(`/`)는 계층 관계를 나타내는데 사용함<br/> ex) http://restapi.example.com/houses/apartments

2. URI 마지막 문자로 슬래시(`/`)를 포함하지 않음

   + URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 함

   + REST API는 분명한 URI를 만들어 통신을 해야하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않음<br/>ex) http://restapi.example.com/houses/apartments (X)

3. 하이픈(`-`)은 URI 가독성을 높이는데 사용함

   + 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높임

4. 밑줄(`_`)은 URI에 사용하지 않음

   * 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 함 → 가독성을 위해 밑줄은 사용하지 않음

5. URI 경로에는 소문자가 적합함

   * URI 경로에 대문자 사용은 피함
   * RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문

6. 파일 확장자는 URI에 포함하지 않음

   * REST API에서는 메세지 바디 내용의 포맷을 나타내기 위한 팡리 확장자를 URI 안에 포함시키지 않음
   * Accept header를 사용함<br/>ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)<br/>ex) GET / members/soccer/345/photo HTTP/1.1 Host:restapi.example.com Accept: image/jpg (O)

7. 리소스 간에는 연관 관계가 있는 경우

   * `/(리소스명)/(리소스ID)/(관계가 있는 다른 리소스명)`<br/>ex) GET : /users/{userid}/devices (일반적으로 소유 'has'의 관계를 표현할 때 사용)

### REST API 설계 예시

| CRUD                        | HTTP verbs | Route         |
| --------------------------- | ---------- | ------------- |
| resource들의 목록을 표시    | GET        | /resource     |
| resource 하나의 내용을 표시 | GET        | /resource/:id |
| resource를 생성             | POST       | /resource     |
| resource를 수정             | PUT        | /resource/:id |
| resource를 삭제             | DELETE     | /resource/:id |

+ 응답 상태 코드
  + 1XX : 전송 프로토콜 수준의 정보 교환
  + 2XX : 클라이언트 요청이 성공적으로 수행됨
  + 3XX : 클라이언트는 요청을 완료하기위해 추가적인 행동을 취해야 함
  + 4XX : 클라이언트의 잘못된 요청
  + 5XX : 서버쪽 오류로 인한 상태 코드

<br/>

# [ RESTful ]

## 1. RESTful

```tex
▫️ 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어
```

+ 특징
  + 'REST API'를 제공하는 웹 서비스를 'RESTful'하다고 할 수 있음
  + RESTful은 REST를 REST 답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아님
  + 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭됨

<img src="https://user-images.githubusercontent.com/33214969/146705097-3d99c875-5e46-45eb-ac37-6b3f7b131247.png" alt="restful.png" width="70%;" />

## 2. RESTful의 목적

+ 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
+ RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기임 → 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없음

## 3. RESTful 하지 못한 경우

+ ex) CRUD 기능을 모두 POST로만 처리하는 API
+ ex) route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)

<br/><br/>

[참고] https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html