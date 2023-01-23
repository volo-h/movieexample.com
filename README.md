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
 