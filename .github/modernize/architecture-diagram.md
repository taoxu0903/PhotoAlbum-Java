# Architecture Diagram

This diagram illustrates the high-level architecture of the PhotoAlbum Java application, a Spring Boot web application for storing and viewing photo galleries backed by Oracle Database.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nUser Interface"]

    subgraph App["Spring Boot Application (Java 8, Port 8080)"]
        subgraph Presentation["Presentation Layer"]
            TH["Thymeleaf Templates\nHTML Views"]
            HC["HomeController\nGallery listing"]
            DC["DetailController\nPhoto detail view"]
            FC["PhotoFileController\nFile upload / download"]
        end

        subgraph Business["Business Layer"]
            PS["PhotoService\nUpload, retrieve, delete photos"]
        end

        subgraph DataAccess["Data Access Layer"]
            PR["PhotoRepository\nSpring Data JPA"]
        end
    end

    subgraph Storage["Storage"]
        ODB[("Oracle Database\nPhoto metadata")]
        FS["Local File System\nPhoto binary files"]
    end

    Browser -- "HTTP requests" --> HC
    Browser -- "HTTP requests" --> DC
    Browser -- "Multipart file upload" --> FC
    HC -- "renders" --> TH
    DC -- "renders" --> TH
    FC -- "delegates" --> PS
    HC -- "delegates" --> PS
    DC -- "delegates" --> PS
    PS -- "CRUD operations" --> PR
    PS -- "reads / writes files" --> FS
    PR -- "JDBC via ojdbc8" --> ODB
```
