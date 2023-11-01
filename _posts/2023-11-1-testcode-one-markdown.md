---
layout: post
title: 테스트 코드 작성기 01
subtitle: 
categories: Java
tags: [Java, 테스트]
---

#### Junit5와 AssertJ를 활용한 간단한 기능구현과 테스트 코드 작성기



> ##### Version
> * JUnit5 : 5.8.2
> * AssertJ : 3.22.0
> * Gradle : 6.8.1

---


**1. build.gradle에 의존성 추가**

```gradle
dependencies {
    testImplementation 'org.assertj:assertj-core:3.22.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.8.2'
}
```

build.gradle을


**2. 간단한 기능과 테스트 코드 작성**

![트리구조](/assets/images/tree_dir.png)

의존성 설정이 완료되면 다음과 같이 디렉터리와 클래스를 생성한다.




**3. 문자열 분리 기능 구현 및 테스트**

* 기능 클래스 : StringSplitter.java
* 테스트 클래스 : StringSplitterTest.java
* 요구사항 명세 
  * ㅇㅇㅇ

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

