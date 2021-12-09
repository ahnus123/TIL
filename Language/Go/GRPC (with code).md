# GRPC (with code)

## 1. GRPC 서버 구현

1. Go 모듈 생성

   ```bash
   # go mod init (모듈명)
   $ go mod init
   ```

   - go.mod에 다른 모듈들을 import할 수 있고, import 하게 되면 해당 모듈들을 프로젝트 내에서 자유롭게 사용할 수 있음
   - `go.mod` 파일 생성 후 grpc 모듈을 import 해야 함
   - Go로 구현된 GRPC의 코드는 오픈 소스로 [grpc-go repository](https://github.com/grpc/grpc-go)에 있음
   - `go get -u` 모듈 이름으로 `grpc-go` 패키지를 import 해야 함

   ```bash
   $ go get -u google.golang.org/grpc
   ```

   - grpc 모듈 다운로드

2. GRPC 서버 구현

   ```go
   package main
   
   import (
   	"log"
   	"net"
   
   	"google.golang.org/grpc"
   )
   
   const portNumber = "8000" // port 번호
   
   func main() {
   	listen, err := net.Listen("tcp", ":"+portNumber)
   	if err != nil {
   		log.Fatalf("Failed to listen : %v", err)
   	}
   
   	grpcServer := grpc.NewServer()
   
   	log.Printf("Start GRPC server on %s port", portNumber)
   	if err := grpcServer.Serve(listen); err != nil {
   		log.Fatalf("Failed to Serve : %s", err)
   	}
   }
   ```

   <img src="https://user-images.githubusercontent.com/33214969/146026614-5c38b022-5280-4df6-acf4-6ef8c538bae5.png" style="zoom: 50%;" />

   1. net 표준 라이브러리 패키지([net package](https://pkg.go.dev/net))를 이용해 어떤 네트워크에 어떤 port 번호로 서버를 실행할지 정의함
   2. import한 grpc 모듈([google.golang.org/grpc](https://pkg.go.dev/google.golang.org/grpc)) 모듈에서 `NewServer()`함수를 호출하여 GRPC 서버 구현
   3. net 패키지를 이용해 구현한 listener connection을 Serve()라는 함수의 인자로 넣어줌
   4. `go run main.go` 로 실행 → port 8000으로 서버 실행됨

3. protobuf 서비스 정의하기

   + 마이크로서비스에 맞게 특정 서비스를 담당하는 GRPC 서버를 구현

   + GRPC 서버 - protobuf라는 방식을 사용하여 정해진 양식대로 데이터를 주고 받음 → protobuf로 만들 GRPC 서버의 메세지를 먼저 정의해야 함

     + protobuf 사용법1([Google Protobuf Buffer Developer Guide](https://developers.google.com/protocol-buffers/docs/proto3))
     + protobuf 사용법2([Protocol Buffer Golang Github Repo](https://github.com/protocolbuffers/protobuf-go/tree/v1.25.0))

   + 아래 구현될 서버는 user의 정보를 담당하는 마이크로서비스 형태임

     ```protobuf
     // user.proto
     syntax = "proto3";
     
     package proto;
     
     option go_package = "/pb";
     
     service User {
         rpc GetUser(GetUserRequest) returns (GetUserResponse);
         rpc ListUsers(ListUsersRequest) returns (ListUsersResponse);
     }
     
     message UserMEssage {
         string user_id = 1;
         string name = 2;
         string phone_number = 3;
         int32 age = 4;
     }
     
     message GetUSerRequest {
         string user_id = 1;
     }
     
     message GetUserResponse {
         UserMessage user_message = 1;
     }
     
     message ListUsersRequest{}
     
     message ListUsersResponse {
         repeated UserMessage user_messages = 1;
     }
     ```

     1. `GetUser` : user id 전달 → 그에 맞는 유저의 정보를 return하는 rpc
     2. `ListUsers` : 서비스에 존재하는 모든 user들의 user 정보를 return하는 rpc

4. proto 컴파일

   + proto 정의 후 실제로 GRPC 서버에서 사용할 수 있도록 protoc 컴파일러를 이용해 컴파일 → https://developers.google.com/protocol-buffers/docs/gotutorial#compiling-your-protocol-buffers

     1. protoc 컴파일러 다운로드(protoc-3.19.1-osx-x86_64.zip)

        + 설치 방법

          1. homebrew로 설치

             ```bash
             $ brew install protobuf
             
             # 버전 정보 확인
             $ protoc --version
             ```

             <img src="https://user-images.githubusercontent.com/33214969/146026632-b4db933c-f06a-4bb8-8509-967abe806256.png" style="zoom: 50%;" />

             <img src="https://user-images.githubusercontent.com/33214969/146026771-df3e5949-576e-495f-a150-97ae1de9d9e9.png" style="zoom:50%;" />

          2. https://developers.google.com/protocol-buffers/docs/downloads

        + 어디서든 호출할 수 있는 PATH에 추가 (ex. GOPATH > bin / include)

     2. Golang protobuf plugin 설치

        ```bash
        $ go get google.golang.org/grpc/cmd/protoc-gen-go-grpc
        ```

     3. proto 파일을 GRPC 서버에서 사용할 수 있도록 컴파일

        ```bash
        $ protoc --proto_path=./proto ./proto/*.proto --plugin=$(go env GOPATH)/bin/protoc-gen-to-grpc --go-grpc_out=.
        ```

        - —proto_path : proto 파일 위치
        - —go-grpc_out=. : grpc 호환 코드(.pb.go)를 생성할 위치를 지정

        <img src="https://user-images.githubusercontent.com/33214969/146026779-3424ab01-c5bc-4b4d-b957-784fd6f9663e.png" style="zoom:50%;" />

     4. 컴파일이 완료된 후 `./pb` 폴더에 `user.pb.go`가 생성됨

        <img src="https://user-images.githubusercontent.com/33214969/146026785-f51ddd9e-9b29-48da-ae4b-d119d6216ed5.png" style="zoom:50%;" />

     

     </br></br>

     [참고] https://devjin-blog.com/golang-grpc-server-1/