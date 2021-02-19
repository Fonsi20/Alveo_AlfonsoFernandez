# Proyecto FullStack

Model Project where the following technologies will be applied:

- FrontEnd Angular
- BackEnd JavaSpring
- DataBase MSSQL
- Docker Files
- Docker Compose

What you must have installed:

- Docker
- Docker-Compose
- JDK 8
- IDE desarrollo (En mi ejemplo utilice IntelliJ)
- NodeJS
- Angular CLI

To run all projects you must run the following command

```cmd
docker-compose up
```

To stop the execution of all projects you can run the following command

```cmd
docker-compose down
```

The Docker FRONTEND service exposes port 4200, when executing it must validate that this port is free.

```yaml
frontend:
  container_name: frontend
  build: ./frontend/website
  ports:
    - "4200:4200"
  command: >
    bash -c "npm install && ng serve --host 0.0.0.0 --port 4200 --disable-host-check"
  networks:
    - fullstack
  depends_on:
    - backend
```

The Docker BACKEND service exposes port 8080, when executing it must validate that this port is free, you can also check the DockerFile file that is located inside the microservices folder

```yaml
backend:
  container_name: backend
  build: ./backend/microservices
  ports:
    - "8080:8080"
  networks:
    - fullstack
  depends_on:
    - sqlserver
  environment:
    - DATABASE_HOST=sqlserver
    - DATABASE_USER=sa
    - DATABASE_PASSWORD=AQUI_LA_CONTRASENIA_DE_BD
    - DATABASE_NAME=springexample
    - DATABASE_PORT=1433
```

The Docker service DATABASE is the only one that contains VOLUMES, because we need keep the objects likes tables and data, if you dont wanna save anything you can comment the VOLUMES line.

```yaml
sqlserver:
  image: mcr.microsoft.com/mssql/server
  container_name: sqlserver2019
  ports:
    - 1433:1433
  environment:
    ACCEPT_EULA: Y
    SA_PASSWORD: AQUI_LA_CONTRASENIA_DE_BD
  networks:
    - fullstack
  volumes:
    - ./database:/var/opt/mssql/data
```

## BackEnd

This project contains services in Java with SpringBoot framework, it exposes 4 endpoints performing a CRUD:

```java
Entiti - Product:
- GET
- POST
- PUT
- DELETE
```

## FrontEnd

This project contains a site built in Angular + Bootstrap + LazyLoading, if you want to run only the FrontEnd project you must run the following commands

```javascript
npm install
```

## DataBase

This folder only registers the "volume" of the DB to maintain the persistence of the objects that can be created
