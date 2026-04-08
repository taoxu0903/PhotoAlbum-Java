# Architecture Diagram

This diagram illustrates the current architecture of the Photo Album application, a Spring Boot web application for uploading and managing photos backed by an Oracle database.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nClient"]

    subgraph AppLayer["Spring Boot Application (Java 8, Spring Boot 2.7.18, Port 8080)"]
        subgraph Presentation["Presentation Layer"]
            TH["Thymeleaf Templates\nindex.html / detail.html / layout.html"]
            Static["Static Assets\nCSS / JavaScript"]
        end

        subgraph Controllers["Controller Layer"]
            HC["HomeController\nGET / POST /upload"]
            DC["DetailController\nGET /photos/{id}"]
            FC["PhotoFileController\nGET /photos/{id}/image"]
        end

        subgraph Services["Service Layer"]
            PS["PhotoService\nPhotoServiceImpl\nUpload / Retrieve / Validate"]
        end

        subgraph Repository["Data Access Layer"]
            PR["PhotoRepository\nSpring Data JPA"]
        end
    end

    subgraph DataLayer["Data Layer"]
        OracleDB["Oracle Database\nOJDBC8 / Hibernate ORM\nPhotos table (BLOB storage)"]
    end

    subgraph FileSystem["Local File System"]
        FS["Static Uploads Directory\nsrc/main/resources/static/uploads"]
    end

    Browser -- "HTTP Requests" --> HC
    Browser -- "HTTP Requests" --> DC
    Browser -- "HTTP Requests" --> FC
    HC -- "renders" --> TH
    DC -- "renders" --> TH
    TH -- "includes" --> Static
    HC -- "calls" --> PS
    DC -- "calls" --> PS
    FC -- "calls" --> PS
    PS -- "CRUD operations" --> PR
    PR -- "JDBC / JPA" --> OracleDB
    PS -- "writes file" --> FS
```
