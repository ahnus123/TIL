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

        <img src="https://user-images.githubusercontent.com/33214969/146053011-1b9d512d-88a3-4c5f-92b3-f7eda2d4b236.png" style="zoom:50%;" />

        + 'protoc-gen-go-grpc: program not found or is not executable Please specify a program using absolute path or make sure the program is available in your PATH system variable' 에러 발생 시 → GOPATH 확인 & protobuf plugin 재설치(b번)

     4. 컴파일이 완료된 후 `./pb` 폴더에 `user.pb.go`가 생성됨

        <img src="https://user-images.githubusercontent.com/33214969/146053022-6977c0ce-2383-4cb5-b1a9-28c7229d9f38.png" style="zoom:33%;" />

     5. 정의한 protobuf로 GRPC 서버 구현

     + `user.proto`를 사용하여 user service를 담당하는 GRPC 서버를 구현
     
       + user 데이터 셋팅 - DB 조회 대신 static 선언으로 대체
     
         ```go
         // data/user.go
         package data
         
         import (
         	userpb "grpcserver/pb"
         )
         
         var UserData = []*userpb.UserMessage {
         	{
         		UserId: "1",
         		Name: "Henry",
         		PhoneNumber: "01012341234",
         		Age: 20,
         	},
         	{
         		UserId: "2",
         		Name: "Micheal",
         		PhoneNumber: "01056785678",
         		Age: 21,
         	},
         	{
         		UserId: "3",
         		Name: "Jessie",
         		PhoneNumber: "01090129012",
         		Age: 22,
         	},
         	{
         		UserId: "4",
         		Name: "Max",
         		PhoneNumber: "01045129012",
         		Age: 23,
         	},
         	{
         		UserId: "5",
         		Name: "Tony",
         		PhoneNumber: "01012345678",
         		Age: 24,
         	}
         }
         ```
     
       + `user.proto`를 사용하여 user service를 담당하는 GRPC 서버를 구현
     
         ```go
         // main.go
         type userServer struct {
         	userpb.UserServer
         }
         
         func main() {
         	...
         	grpcServer := grpc.NewServer()		// User service를 GRPC server에 등록
         	userpb.RegisterUserServer(grpcServer, &userServer{})
         	...
         }
         ```
     
         + GRPC 서버에 정의한 User 서비스를 사용하도록 만드는 함수가 이미 컴파일된 `user_grpc.pb.go` 파일에 존재함 → `RegisterUserServer` 함수를 가져와서 User 서비스를 등록하면 됨 ⇒ user 서비스를 담당하는 GRPC 서버 생성 완료
     
       + `user.proto`에 정의한 RPC 구현
     
         ```go
         // main.go
         // GetUser : user 상세 조회
         func (s *userServer) GetUser(ctx context.Context, req *userpb.GetUserRequest) (*userpb.GetUserResponse, error) {
         	userId := req.UserId
         
         	var userMessage *userpb.UserMessage
         	for _, u := range data.Users {
         		if u.UserId != userId {
         			continue
         		} else {
         			userMessage = u
         			break
         		}
         	}
         
         	return &userpb.GetUserResponse {
         		UserMessage: userMessage,
         	}, nil
         }
         
         // ListUsers : user 리스트 조회
         func (s *userServer) ListUsers(ctx context.Context, req *userpb.ListUsersRequest) (*userpb.ListUsersResponse, err) {
         	userMessages := make([]*userpb.UserMessage, len(data.Users))
         	for i, user := rangee data.Users {
         		userMessages[i] = u
         	}
         
         	return &userpb.ListUsersResponse {
         		UserMessages: userMessages,
         	}, nil
         }
         ```
     
     6. GRPC 서버 작동 확인
     
        - GRPC 서버는 HTTP API와 같이 `localhost:8000/users/:user_id`와 같은 curl or Postman으로 확인할 수 없음 → GRPC 서버로 curl할 수 있는 툴 설치 필요
     
          + [grpccurl](https://github.com/fullstorydev/grpcurl) : cli 툴
     
          + [bloomrpc](https://github.com/bloomrpc/bloomrpc) : gui 툴
     
        + bloomrpc 툴을 사용하여 GRPC 서버 작동 확인
     
          1. bloomrpc 설치 및 빌드
     
             ```bash
             # bloomrpc 설치
             $ brew install --cask bloomrpc
             
             # bloomrpc 빌드
             $ git clone <https://github.com/uw-labs/bloomrpc.git>
             $ cd bloomrpc
             $ yarn install && ./node_modules/.bin/electron-rebuild
             $ npm run package
             ```
     
             <img src="https://user-images.githubusercontent.com/33214969/146380738-8d543111-2cfb-4238-a42e-b7e7d2fd709e.png" alt="grpc-bloomrpc1.png" style="zoom:50%;" />
     
             <img src="https://user-images.githubusercontent.com/33214969/146380753-91a7578f-c27f-48b3-9f2a-b9a835cd324f.png" alt="grpc-bloomrpc2.png" style="zoom:50%;" />
     
          2. bloomrpc 실행
     
             ```bash
             $ cd bloomrpc
             
             # 아래 명령어 2개는 2개의 터미널에서 각각 실행
             $ npm run start-server-dev
             $ npm run start-main-dev
             ```
     
             <img src="https://user-images.githubusercontent.com/33214969/146380763-e4f85bac-a467-4606-89b2-c43af722b7e8.png" alt="grpc-bloomrpc3.png"  />
     
             <img src="https://user-images.githubusercontent.com/33214969/146380764-3dc112ef-1b67-4d18-8fed-e8930537e1b0.png" alt="grpc-bloomrpc4.png" style="zoom:50%;" />
     
             <img src="https://user-images.githubusercontent.com/33214969/146380769-73b302ab-334d-4ac2-9150-1bd126b94355.png" alt="grpc-bloomrpc5.png" style="zoom: 33%;" />
     
          3. proto 파일 import하기
     
             <img src="https://user-images.githubusercontent.com/33214969/146380770-45d9cd8a-a594-4cb7-a231-d88cfbd82493.png" alt="grpc-bloomrpc6.png" style="zoom: 33%;" />
     
          4. GRPC 서버 실행 후 RPC 실행
     
             + GRPC 서버 실행
     
               <img src="https://user-images.githubusercontent.com/33214969/146380771-ec4c16fd-b00e-4b2c-827c-b1a7fceb6178.png" alt="grpc-bloomrpc7.png" style="zoom:50%;" />
     
             + RPC 실행
     
             + (bloomrpc) RPC 클릭 → 자동으로 request를 보낼 수 있는 형식으로 포맷을 변경시켜줌 + request 형식에 맞게 데이터를 보내면 response를 출력
     
               <img src="https://user-images.githubusercontent.com/33214969/146380774-3ec5c40a-f97b-4e48-9a69-3d0fabe01123.png" alt="grpc-bloomrpc8.png" style="zoom: 33%;" />
     
               <img src="https://user-images.githubusercontent.com/33214969/146380779-611b27f5-a938-4a35-bfb9-d5518b476a1b.png" alt="grpc-bloomrpc9.png" style="zoom: 33%;" />
     
     </br></br>
     
     [참고] https://devjin-blog.com/golang-grpc-server-1/