# docker-go-neo4j

1. download the image for Neo4j and start the container in the background.
```
docker-compose up -d
```

2. verify that the database is working by going to http://localhost:7474/

3.  In the Neo4j UI, enter neo4j as password and set testing as a new password. 

4.  Then, when you're logged in, type :play movie-graph in the editor and press the play button on the right.
```:play movie-graph```

5. Add the backend service to the docker-compose.yml file
```
backend:
    container_name: 'api-go'
    build: './backend'
    ports:
      - '8080:8080'
    volumes:
      - './backend:/go/src/app'
    depends_on:
      - 'neo4j'
    networks:
        - neo4j_go_net

```

6.  Create a directory call *backend*. Inside that backend folder, create a file called Dockerfile
```
FROM golang:latest

WORKDIR /go/src/app
COPY . .

RUN go get github.com/pilu/fresh

CMD [ "fresh" ]
```

This file fetches the golang image from Docker Hub, sets up a working directory, copies everything from the backend folder into the working directory, installs the required packages, and runs the fresh command for hot reloading.

7. Intialize the backend

go mod init github.com/Nditah/docker-go-neo4j/backend

8. Reload docker-compose and you can view the app running at http://localhost:8081, where you'll see a list of movies in JSON format.
