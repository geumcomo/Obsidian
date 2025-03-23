## ubuntu에 접속한다.

```
sudo systemctl status mysql
```

- mysql 서버를 실행한 경우 
	정상적으로 실행중이라고 뜰 것이다.
  
- mysql 공식 이미지를 사용한 경우
	Unit mysql.service could not be found.

두가지에 대해서 알아볼 예정이다.

### 일단 백업 디렉도리와 sql파일을 만들자.
1. mkdir backup
2. vim backup.sql
## mysql 서버를 실행한 경우
 
```
 mysqldump -u root -p jobgem_db > /home/ubuntu/JobgemDeploy/backup/backup.sql
 <!-- 로컬 호스트와 데이터베이스를 입력하면 자동으로 복사가 될 것이다. -->
```

> 복사될 경로는 무조건 상대경로가 아닌 절대경로로 해야한다.
{: .prompt-info}
> 

## mysql 공식 이미지를 사용한 경우
```
docker ps
<!-- 현재 실행중인 도커 컨테이너중 mysql서비스의 names를 확인한다. -->

docker exec -i mysql mysqldump -u root -p jobgem_db > /home/ubuntu/JobgemDeploy/backup/backup.sql
<!-- names와 데이터베이스를 입력한다. -->
```

backup.sql를 확인하면 데이터베이스 데이터 구조를 볼 수 있을 것이다.

## 마치며

`docker exec -i`
사용자 입력을 받지 못할 시 즉시 종료된다.

`docker exec -it`
사용자 입력을 받으면서 서로 상호작용한다.

linux의 `cron`을 사용하면 지정된 시간에 자동 백업을 할 수 있는 스크립트를 만들 수 있다.
