# Spring Boot app & Postgresql & Docker compose

## 0. launch Play with Docker

go to https://labs.play-with-docker.com/
select 3 manager 2 worker
goto to the first manager

## 1. Build app & Dockerfile

`git clone https://github.com/magneval/spring-boot-postgresql-docker-compose.git`

`mvn clean install`

OR:

`docker run -it --rm --name my-maven-project -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven -v /var/run/:/var/run/ maven mvn clean install`

## 2. Run docker-compose

`cd src/main/docker`

`docker-compose up`

`docker pull dockersamples/visualizer`

`docker run -it -d -p 8081:8080 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer`

`docker service create   --name=viz   --publish=5000:8080/tcp   --constraint=node.role==manager   --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock   dockersamples/visualizer`


`docker-compose pull mypostgres`

`docker deploy --compose-file docker-compose.yml` demo

`docker service ls`

`docker service scale demo_app=3`

`docker service ls`

**What happens:**

1. Starts Postgresql and waits up to 15 seconds for it to finish ([using wait-for-it](https://github.com/vishnubob/wait-for-it))
2. Starts Spring boot application which populates database with some test data

## 3. Test

Navigate to <http://localhost:8080> and you should see: `[{"id":1,"name":"A"},{"id":2,"name":"B"},{"id":3,"name":"C"}]`
