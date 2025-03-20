spring 프로젝트 -gradle

build.gradle
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'com.mysql:mysql-connector-j'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.security:spring-security-test'
	testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
	implementation 'io.jsonwebtoken:jjwt-api:0.12.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.12.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.12.5'
}
```

![[Pasted image 20240809140806.png]]

application.yml
```yml
spring:
  profiles:
    active: dev
    include: secret
  jpa:
    hibernate:
      ddl-auto: create
      # 자동으로 생겨나게 할거면 명시
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        use_sql_comments: true
    
    
```

application-secret.yml
```yml
custom:
    jwt:
      secretKey: dwadnbkjwanbjkdnwandkjawnkdjnawndkjawndkjnawdnawnjkawndjknawdn
      expiration: 604800
```

application-dev.yml
```yml
##개발용
server:
  port: 8080
spring:
  output:
    ansi:
      enabled: always
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/my_db
    username: test
    password: 1111
      
```

![[Pasted image 20240809151712.png]]
