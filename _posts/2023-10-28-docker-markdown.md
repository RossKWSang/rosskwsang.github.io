---
layout: post
title: Docker로 MySQL 실행하기
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


1. Docker 이미지를 Pull한다.

```cmd
docker pull mysql:8.2.0
```
  
  
  
2. Docker 이미지 실행
```cmd
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:latest
```
  
  
  
3. Docker 컨테이너 배쉬 실행
```cmd
docker exec -it mysql-container bash
```
3을 실행하게 되면 컨테이너의 배쉬가 실행되어 새로운 커맨드 창을 볼 수 있다.
  
  
  
4. MySQL실행
```cmd
bash-4.4# mysql -u root -p
```
root password를 입력하고 들어가면 MySQL 콘솔에서 작업할 수 있다.

``` mysql
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

