# go-grpc

```bash
brew install protobuf

protoc --version

go get -u github.com/golang/protobuf/protoc-gen-go

protoc --go_out=plugins=grpc:. ./protobuf/user.proto

更换protoc-gen-go生成器:

go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2

protoc --go_out=./protobuf --go-grpc_out=./protobuf ./protobuf/user.proto

```
