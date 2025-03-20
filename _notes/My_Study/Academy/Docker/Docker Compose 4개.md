src에 .env파일 생성
```.env
TZ= Asia/Seoul
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_ROOT_PASSWORD=1111
MYSQL_USER=test
MYSQL_PASSWORD=1111
MYSQL_DATABASE=my_db
```
Docker 파일 생성
```dockfile\

FROM gradle:7.6.1-jdk as builder
 
WORKDIR /app 
# 작업위치
COPY ./ ./ 
RUN gradle clean build --no-daemon

FROM openjdk:17-alpine
WORKDIR /app
COPY --from=builder /app/build/libs/backend-0.0.1-SNAPSHOT.jar .
# 현재 위치에 복사 
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "backend-0.0.1-SNAPSHOT.jar"]

```

여러 컨테이너 연결 4개정도 통합
관리를 하나의 앱처럼 하려면 docker compose를 해야됨

![[Pasted image 20240826120828.png]]

