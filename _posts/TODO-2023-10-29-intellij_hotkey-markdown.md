---
layout: post
title: IntelliJ 단축키 정리
subtitle: 
categories: 백엔드
tags: [백엔드, 개발환경]
---


[신버전] 한 번에 끝내는 Spring 완.전.판 초격차 패키지 Online.



> ##### Version
> * Ubuntu : 22.04.3 LTS
> * Docker : 24.0.6  
> * MySQL : 8.2.0

---


**1. Docker 이미지를 Pull**

```cmd
docker pull mysql:8.2.0
```



**2. Docker 이미지 실행**
```cmd
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:8.2.0
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

