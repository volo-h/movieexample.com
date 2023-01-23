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


    API Hanler
        |
    Business logic (controller)
        |
    Repository
        |
        DB


    • controller: Business logic
    • gateway: Logic for interacting with other services
    • handler: API handlers
    • repository: Database logic

