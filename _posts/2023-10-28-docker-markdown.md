---
layout: post
title: Ubuntu22.04에서 Docker로 MySQL 실행하기
subtitle: 
categories: DevOps
tags: [백엔드, DevOps]
---

#### Ubuntu22.04에서 Docker를 사용하여 MySQL 이미지 구동하기



> ##### Version
> * Ubuntu : 22.04.3 LTS
> * Docker : 24.0.6  
> * MySQL : 8.2.0

---


**1. Docker 이미지 Pull**

```cmd
docker pull mysql:8.2.0
```



**2. Docker 이미지 실행**

처음 docker pull된 image를 시작한다면 다음과 같은 명령어 입력 
```cmd
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:8.2.0
```

재부팅 또는 다른 이유로 실행이 중지된 docker container를 다시 사용한다면 다음과 같은 명령어 입력
```cmd
docker start mysql-container
```

만약 3306 포트가 사용중이라면 다음과 같은 에러가 날 수 있음
```cmd
Error response from daemon: driver failed programming external connectivity on endpoint mysql-container (6ba46878ff00458d512e3dde5e779ffea6e290e722ef6eea30a9eb1b41771b4e): Error starting userland proxy: listen tcp4 0.0.0.0:3306: bind: address already in use
Error: failed to start containers: mysql-container
```

이런 경우 실행되고 있는 MySQL을 아래의 명령어를 통해 중지하고 위 run/start명령어를 입력
```cmd
mysqladmin -u root -p shutdown
```


**3. Docker 컨테이너 배쉬 실행**
```cmd
docker exec -it mysql-container bash
```
3을 실행하게 되면 컨테이너의 배쉬가 실행되어 새로운 커맨드 창을 볼 수 있다.
  
  
  
**4. 컨테이너 내에서 MySQL실행**
```cmd
bash-4.4# mysql -u root -p
```
root password를 입력하고 들어가면 MySQL 콘솔에서 작업할 수 있다.

```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| delivery           |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

