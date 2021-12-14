
## **RPC**(Remote Procedure Call)

```````tex
▫️ 한 프로그램이 네트워크의 세부 정보를 이해하지 않고도 네트워크 안의 다른 컴퓨터에 있는 프로그램에서 서비스를 요청하는 프로토콜
```````

<img src="https://user-images.githubusercontent.com/33214969/145026118-4b9feea9-6eb9-41a6-8076-f2d6fd1618f9.jpg" alt="rpc.jpg" width="50%"/>

+ 특징
  + 서로 다른 컴퓨터 프로그램들이 서로 다른 주소에서 서로를 호출하지만, 마치 같은 주소에서 호출하는 것처럼 작동하게 하는 원격 프로시져 프로토콜 → 프로그램들은 서로가 누구인지 알 필요 없이 정해진 방식대로 단순히 함수 호출만 하면 됨
  + 클라이언트에서 서비스를 요청(function call)하면, 서버에서 서비스를 제공함
  + IDL(Interface Definication Language) 기반으로 다양한 언어를 가진 환경에서도 쉽게 확장이 가능하며, 인터페이스 협업에도 용이함

# GRPC(google Remote Procedure Call)

```tex
▫️ google 사에서 개발한 protobuf라는 방식을 사용해 RPC라는 프로토콜을 데이터로 주고 받는 오픈소스 RPC 플랫폼
```

<img src="https://user-images.githubusercontent.com/33214969/145026218-b272ef47-c4be-4915-8ce6-8c5a62b4a4b9.png" alt="grpc.png" width="50%" />

+ 특징

  + 전송을 위해 TCP/IP 프로토콜과 `HTTP 2.0` 프로토콜을 사용함
  + IDL(Interface Definition language)로 `protobuf(protocol buffer)`를 사용함 → IDL만 정의하면 높은 성능을 보장하는 서비스와 메세지에 대한 소스 코드가 각 언어에 맞게 자동 생성됨 → 개발자들은 사용 언어에 구애받지 않고 사용하기만 하면 됨
  + SSL/TLS를 사용하여 서버를 인증함 + 클라이언트 ↔ 서버 간에 교환되는 모든 데이터를 암호화함
  + 서버에서 클라이언트 응용 프로그램의 함수를 바로 호출 할 수 있음 → 분산 MSA를 쉽게 구현할 수 있음
  + 서버 측에서는 서버 인터페이스를 구현 + GRPC 서버를 실행 → 클라이언트 호출을 처리함
  + [GRPC 사양](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) : GRPC 서비스가 따라야 하는 형식에 대한 지침

+ 지원 언어

  <img src="https://user-images.githubusercontent.com/33214969/145026282-6477e9d3-2d42-4b36-acd2-2e43d0f8f080.png" alt="grpc_official_support.png" width="50%" />

+ 장점

  + 높은 생산성과 효율적인 유지보수
  + 다양한 언어와 플랫폼 지원
  + 높은 메세지 압축률 및 성능
  + HTTP/2 기반의 양방향 스트리밍
  + 다양한 GRPC 생태계 → 필요에 따라 Authentication, Tracing, Load Balancing, Health Checking, API Gateway 등의 다양한 도구를 지원함
  + 빠름

+ 단점

  + 간단한 RESTful API를 제공을 목적으로 한다면 부적합함
  + protobuf 및 HTTP/2에 대한 아주 약간의 커닝 러브가 존재함
  + 메세지가 일반 REST API와 다르게 바이너리로 전달됨 → 테스트가 쉽지 않음
  + 브라우저와 서버간 통신 불가 → grpc-gateway를 통해 protobuf 형식으로 데이터를 변환한 뒤 사용함

+ REST와의 차이점

  - HTTP/2 사용
    + HTTP/1.1 - 기본적으로 클라이언트의 요청이 올때만 서버가 응답하는 구조 -> 매 요청마다 connection을 생성해야 함

  - 프로토콜 버퍼로 데이터를 전달함 -> Proto File만 배포하면 환경과 프로그램 언어에 구애받지 않고 서로 간의 테이터 통신이 가능함

+ 사용하는 경우

  + MSA(마이크로 서비스)인 경우
  + 지점 간 실시간 통신하는 경우
  + Polyglot 환경인 경우
  + 네트워크 제한 환경인 경우
  + IPC(프로세스 간 통신)인 경우

### GRPC vs HTTP

|                      | GRPC                     | HTTP API(with JSON)             |
| -------------------- | ------------------------ | ------------------------------- |
| 계약                 | 필수(.proto)             | 선택(OpenAPI)                   |
| 프로토콜             | HTTP/2                   | HTTP                            |
| Payload              | Protobuf(소형, 이진)     | JSON(대형, 사람이 읽을 수 있는) |
| 규범                 | 엄격한 사양              | 느슨함, 모든 HTTP가 유효함      |
| 스트리밍             | 클라이언트, 서버, 양방향 | 클라이언트, 서버                |
| 브라우저 지원        | X (웹 필요)              | O                               |
| 보안                 | 전송 (TLS)               | 전송 (TLS)                      |
| 클라이언트 코드 생성 | O                        | OpenAPI + 타사 도구             |

</br>

## HTTP 2.0(=HTTP/2)

```````tex
▫️ Hypertext Transfer Protocol Version 2의 약자로서, 2015년 IETF에 의해 공식적으로 발표된 HTTP/1.1의 차기 버전
```````

+ 특징

  + HTTP/1.1은 기본적으로 클라이언트의 요청이 들어올 때만 서버가 응답함 →  매 요청마다 connnection을 생성해야 함
  + HTTP/2는 1개의 connection으로 동시에 여러 개의 메세지를 주고 받음
  + header를 압축하여 중복 제거 후 전달함 → HTTP/1.1에 비해 훨씬 효율적임
  + 필요 시 클라이언트 요청 없이도 서버가 리소스를 전달할 수 있음 → 클라이언트 요청을 최소화 할 수 있음

  <img src="https://user-images.githubusercontent.com/33214969/145026741-751be423-1134-47bc-bb6c-5f826d3c05b5.png" alt="http_2.png.png" width="30%" />

</br>

## ProtoBuf(Protocol Buffer)

``````tex
▫️ google 사에서 개발한 구조화된 데이터를 직렬화(Serialization)하는 기법
``````

+ 직렬화 : 데이터 표현을 바이트 단위로 변환하는 작업

+ 아래와 같이 작성된 proto 파일이 protoc 컴파일러를 통해 각 언어로 소스 코드가 생성됨. 아래와 같은 예시로 protocol buffer가 선언되었고, 이는 통신하는 시점에 아래 이미지와 같이 인코딩되어 송수신됨 → 사이즈가 훨씬 작아짐

  <img src="https://user-images.githubusercontent.com/33214969/145026773-23ab799a-7b2d-48bb-b51e-ca915a132a52.png" alt="porotobuf.png" width="60%" />

</br></br></br>

[참고] https://grpc.io/docs/languages/go/, https://devjin-blog.com/golang-grpc-server-1/, https://velog.io/@kyusung/grpc-web-example, https://chacha95.github.io/2020-06-15-gRPC1/