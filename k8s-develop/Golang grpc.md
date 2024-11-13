# personal-notes
Software defined network - Cloud computing - Kubernetes - Container

Sử dụng câu lệnh sau để install gRPC vào project:
```
$ go get -u google.golang.org/grpc
```
Cài đặt protocol plugin Go
```
$ go get -u github.com/golang/protobuf/protoc-gen-go
```
Generate ra ngôn ngữ tương ứng để sử dụng từ file .proto
```
protoc -I customer/ customer/customer.proto --go_out=plugins=grpc:customer
```
Lệnh chạy trên thì ```customer.pb.go``` sẽ được tạo ra

Link hướng dẫn:
https://medium.com/@pvcong11031994/go-l%C3%A0m-vi%E1%BB%87c-v%E1%BB%9Bi-grpc-92c6be80328f
