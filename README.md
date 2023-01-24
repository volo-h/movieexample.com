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
  docker run -d -p 8500:8500 -p 8600:8600/udp --name=dev-consul consul agent -server -ui -node=server-1 -bootstrap-expect=1
-client=0.0.0.0
```
#### Consul web U
http://localhost:8500/

go run *.go --port <PORT>
