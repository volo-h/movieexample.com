internal - can be imported and used only by packages stored within the same directory or one of the directories it includes.
Putting code into an internal directory can ensure your code is not exported and used by external packages.

Services:

• Movie metadata service: 
  Store and retrieve the movie metadata records by movie IDs.

• Rating service: 
  Store ratings for different types of records and retrieve aggregated ratings for records.

• Movie service: 
  Provide complete information to the callers about a movie or a set of movies, including the movie metadata and its rating.

Logic structure:

                  Movie service
                  |           |
Movie metadata Service     Rating service


Folder structure:

                    movie/
           metadata/      rating/

    API Handler
        |
    Business logic (controller)
        |
    Repository
        |
        DB

    • handler: API handlers
    • gateway: Logic for interacting with other services
    • controller: Business logic
    • repository: Database logic

 All code that we are not going to export will be stored in the internal directory and this will include most of our applications.

 The exported structures will reside in the pkg directory.
 
Detailed services structure:

  movieexample.com/

    metadata/                                               :8081
      cmd/
        main.go
      internal/
        controller/
          metadata/
            controller.go
          handler/
            http/
              http.go
          repository/
            memory/
              memory.go
            error.go
      pkg/
        model/
          metadata.go

    movie/                                                  :8083
      cmd/
        main.go
      internal/
        controller/
          movie/
            controller.go
        gateway/
          metadata/
            http/
              metadata.go
          rating/
            http/
              rating.go
          error.go
        handler/
          http/
            http.go
      pkg/
        model/
          movie.go

    rating/                                                 :8082
      cmd/
        main.go
      internal/
        controller/
          rating/
            controller.go
        handler/
          http/
            http.go
        repository/
          memory/
            memory.go
          error.go
      pkg/
        model/
          rating.go

#### run serveices
```sh
go run metadata/cmd/main.go
  http://localhost:8081/metadata
  http://localhost:8081/?id=1
```
```sh
go run rating/cmd/main.go
  http://localhost:8082/rating
  http://localhost:8082/?id=1&type=2
```
```sh
go run movie/cmd/main.go
  http://localhost:8083/
  http://localhost:8083/?id=1
```

#### runs Hashicorp Consul inside Docker in development mode, exposing its ports 8500 and 8600 for local use.
```shell
  docker run -d -p 8500:8500 -p 8600:8600/udp --name=dev-consul consul agent -server -ui -node=server-1 -bootstrap-expect=1 -client=0.0.0.0
```
#### Consul web U
http://localhost:8500/

go run *.go --port <PORT>

Protocol Buffers format

```shell
  go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
  export PATH="$PATH:$(go env GOPATH)/bin"

  protoc -I=api --go_out=. movie.proto
```

serializing data --------> 
      deserialized data  <--------------

ecoding ------->
       decoding <-----------

schema definition language:
  - XML
  - YAML
  - Apache Thrift
  - Apache Avro
  - Protocol Buffer  

go mod tidy

cd cmd/sizecompare/

go run *.go
```shell
  go test -bench=.
```

synchronous communication (request-response model)
Protocols:
  - HTTP


gRPC is an RPC framework that was created at Google. 
gRPC uses HTTP/2 as the transport protocol and Protocol Buffers as a serialization format. 
It provides an ability to define RPC services and generate the client and server code for the services.

define a service API using the Protocol Buffers format and generate the client 
and server gRPC code for communication with each of our services using a proto compiler.

#### install protoc-gen-go-grpc
```shell
 go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

##### generating clients and plug them into our applications.
```shell
  protoc -I=api --go_out=. --go-grpc_out=. movie.proto
```

In formats such as Protocol Buffers, all fields are always optional.
In generated code, this results in lots of fields that can have nil values.
For an application developer, this means doing more nil checks across all applications to prevent possible panics.

add some mapping logic to translate the internal data structures and their generated counterparts.

Message broker:
• At-least-once: The message gets delivered at least once, but may be delivered multiple times in case of failures.
• Exactly-once: The message broker guarantees that the message gets delivered and it will be delivered exactly once.
• At-most-once: The message can be delivered 0 or 1 time.

Apache Kafka:
Producer ---> Topic <--- Consumer
