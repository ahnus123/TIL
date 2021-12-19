### SOA(=Service Oriented Architecture, 서비스 지향 아키텍처)

```tex
▫️ 대규모 컴퓨터 시스템을 구축할 때의 개념. 업무 상에 일 처리에 해당하는 소프트웨어 기능을 서비스로 판단하여 그 서비스를 네트워크 상에 연동하여 시스템 전체를 구축해 나가는 방법론.
```

# SOAP(Simple Object Access Protocol)

```tex
▫️ 일반적으로 널리 알려진 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메세지를 컴퓨터 네트워크 상에서 교환하는 프로토콜
```

+ SOAP 아키텍처

  <img src="https://user-images.githubusercontent.com/33214969/146704310-dd5ef8bf-e47c-43b7-b2be-8ae02d352976.png" alt="soap2.png" width="50%;" />

+ SOAP 메세지 구조

  <img src="https://user-images.githubusercontent.com/33214969/146704314-52836f9e-46a3-4328-b500-8ff3d1ede6e0.jpeg" alt="soap1.jpeg" width="50%;" />

+ 특징

  + SOAP 기반 서비스는 SOA 개념을 실현하기 위한 기술임
  + 웹서비스 내의 모든 데이터는 XML로 표현됨
  + 그 데이터들과 이를 다룰 수 있는 오퍼레이션들이 WSDL로 정의되면 → UDDI라는 전역적 서비스 저장소에 등록(publish)되어 누구라도 서비스를 찾을 수 있도록 공개됨<br/> → SOAP는 UDDI 레지스트리라는걸 통해 웹 서비스를 등록(publish)+탐색(Find)+바인딩(Bind)해서 사용함
    * WSDL = XML, UDDI = 검색엔진
  + 공개된 웹 서비스가 이용될 때, 서비스 요청자와 서비스 제공자 간에 SOAP을 이용해 서비스를 호출 & 결과를 받게 됨
  + SOAP 메세지는 아래 그램과 같이 SOAP 봉투(envelope), SOAP 헤더(header), SOAP 바디(body)로 구성된 하나의 XML 문서로 표현됨<br/> → 복잡한 구성 때문에 HTTP 상에서 전달되기 무거움 + 메세지 인코딩/디코딩 과정 등 웹 서비스 개발의 난이도가 높아 개발 환경의 지원이 필요함
  + 보안, 메세지 전송 등에 있어서 REST 더 많은 표준들이 정해져 있음 → 좀 더 복잡함 & 보안, 트랜잭션, ACID(원자성, 일관성, 고립성, 지속성)을 준수해야 하는 보다 종합적인 기능이 필요한 조직에게는 적합함
  + SOAP 표준에는 성공/반복 실행 로직이 규정되어 있음 → SOAP API를 통해 통신할 때 처음~끝까지 신뢰성을 제공함

+ 프로세스

  1. Service requestor가 SOAP로 인코딩하여 웹 서비스를 Service provider에게 요청
  2. Service provider는 이걸 디코딩 + 요청에 맞는 비즤스 로직을 수행 + 그 결과를 다시 SOAP로 인코딩하여 return

+ 장점

  + 기존의 원격 기술 대비 프록시와 방화벽에 구애 받지 않음
  + 플랫폼 or 프로그래밍 언어에 독립적임
  + 에러 처리가 기본적으로 내장되어 있음
  + 분산환경에서 사용하기 적합함
  + 웹 서비스 표준(XSDL, UDDI, SW-*)이 잘 정립되어 있음

+ 단점

  + 복잡한 구조 때문에 어려움
  + REST에 비해 상대적으로 무거움 + 속도 느림

<br/>

## SOAP vs REST

+ [About REST](https://github.com/Sunha03/TIL/blob/main/CS/Web/REST%20API.md)
+ SOAP vs REST

|               | SOAP                                                         | REST                                               |
| ------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| 유형          | 프로토콜                                                     | 아키텍처 스타일                                    |
| 서버 접근     | 서비스 인터페이스를 통해 접근                                | URI를 이용해 접근                                  |
| 기능          | 기능 위주 : 구조화된 정보 전송                               | 데이터 위주 : 데이터를 위해 리소스에 접근          |
| 데이터 포멧   | XML만 사용                                                   | 일반 텍스트, HTML, XML, JSON 등 다양한 포멧을 허용 |
| 보안          | WS-Security, SSL 지원                                        | SSL, HTTPS 지원                                    |
| 대역폭        | 상대적으로 더 많은 리소스와 대역폭이 필요                    | 상대적으로 리소스가 적게 필요 + 무게가 가벼움      |
| 데이터 캐시   | 캐시 사용 X                                                  | 캐시 사용 O                                        |
| 페이로드 처리 | 엄격한 통신 규약을 갖고 있으며, 모든 메세지는 보내기 전에 알려져야 함 | 미리 알릴 필요 X                                   |
| ACID 준수     | 자체적인 ACID 기준이 있어서 데이터 손상을 줄여줌             | ACID 준수와 관련된 내용 X                          |

+ 웹 서비스 → REST 방식의 API를 선택하는 경우가 많음
+ 기업용 애플리케이션 → 보다 많은 리소스와 아주 엄격한 보안, 여러 다양한 요구사항들을 만족해야 하기 때문에 SOAP 방식을 선택하는 경우가 많음

<br/><br/>

[참고] https://mygumi.tistory.com/55<br/>[참고] https://devkingdom.tistory.com/12