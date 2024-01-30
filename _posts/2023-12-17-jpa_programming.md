---
layout: post
title: 자바 ORM 표준 JPA 프로그래밍 정리하며 읽기 - 01
subtitle: 연관관계 부분 (4장 ~ 7장)
categories: JPA
tags: [Spring Boot, JPA]
---

이 문서는 김영한 개발자의 [자바 ORM 표준 JPA 프로그래밍]의 4장 ~ 7장 엔티티 매핑을 정리한 문서입니다. 책 내용을 읽어나가면서 정리할 내용을 가지치기하는 방식으로 정리를 시도했습니다.

대표적인 어노테이션

- 객체와 테이블 매핑 : @Entity, @Table
- 기본 키 매핑 : @Id
- 필드와 컬럼 매핑 : @Column
- 연관관계 매핑 : @ManyToOne, @JoinColumn

---

#### 엔티티 정의 어노테이션

**@Entity**

- JPA는 Entity객체를 만들때 기본생성자도 자동으로 만든다.
  - (기본생성자가 아닌) 생성자를 만들 경우 위 기능은 동작하지 않으므로 기본생성자를 따로 만들어야 한다.

- final, enum, interface, inner클래스에는 사용할 수 없다.
- 저장할 필드에 final을 사용하면 안된다.
  - 필드에 final을 사용하는 것의 의미?
    - 상수 값이 되거나 한번만 쓸 수 있는 필드가 됨
    - 이러한 final이 붙은 멤버 변수들은 생성자 메서드가 끝나기 전에 초기화를 마쳐야함

**@Table**

- 엔티티와 매핑할 테이블을 지정

**@Id 기본키 매핑 전략**

세가지 데이터베이스 기본 키 생성 전략

1. 직접할당: 기본 키를 어플리케이션에서 직접 할당

```java
@Entity
public class Member {
  @Id
  @Column(name="id")
  private Long id;
  ...
}
```

2. 자동생성: 대리 키 사용 방식

- IDENTITY 전략
  - 기본 키 생성을 데이터베이스에 위임하는 전략(MySQL, PostgreSQL, SQL Server, DB2)

```java
@Entity
public class Member {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
  ...
}
```

- SEQUENCE 전략

  - SEQUENCE는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트
  - 시퀀스를 지원하는 데이터베이스 (오라클, PostgreSQL, DB2, H2)에서 사용

- 아래와 같이 시퀀스를 생성해야한다.

```sql
CREATE SEQUENCE BOARD_SEQ START WITH 1 INCREMENT BY 1;
```

- 다음과 같이 엔티티를 정의한다.

```java
@Entity
@SequenceGenerator( // BOARD_SEQ_GENERATOR라는 시퀀스 생성기를 등록
  name = "BOARD_SEQ_GENERATOR",
  sequenceName = "BOARD_SEQ", // 매핑할 데이터베이스 시퀀스 이름
  initialValue = 1,
  allocationSize = 1) // 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용)
public class Member {
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "BOARD_SEQ_GENERATOR")
  private Long id;
  ...
}
```

JPA는 데이터베이스 스키마를 자동으로 생성하는 기능을 지원한다.

---

#### 객체와 테이블 사이의 연관관계의 차이점

- 테이블은 외래 키 하나로 두 테이블의 연관관계를 관리함
- 연관관계의 주인만이 연관관계와 매핑되고 외래 키를 관리(등록, 수정, 삭제)할 수 있음

| 내용            | 객체 (엔티티 객체 X)                      | 테이블                             |
| --------------- | ----------------------------------------- | ---------------------------------- |
| 방향            | 단방향                                    | 양방향 (JOIN 사용)                 |
| 매개            | 참조                                      | 외래 키                            |
| 다중성          | 다중성 연관관계 있음                      | 다중성 연관관계 있음               |
| 연관관계의 주인 | 양방향 연관관계로 만들면 주인을 따로 선정 | 있음 (외래키를 가지고 있는 테이블) |

#### 객체관계 매핑 (단방향 연관관계)

- 아래는 Member 엔티티내 Team 엔티티를 참조하는 코드임
- 흔한 Java클래스와는 다르게 두가지 어노테이션이 추가되어있음

```java
@ManyToOne
@JoinColumn(name="TEAM_ID")
private Team team;
```

@ManyToOne : 다중성을 나타내는 어노테이션, 이 경우는 다대일 관계를 나타냄
@JoinColumn : 외래키를 매핑할 때 사용하는 어노테이션

---

#### 객체관계 매핑 (양방향 연관관계)

- 위 Member엔티티의 단방향 연관관계를 포함하여 다음과 같은 코드를 Team엔티티에 작성

```java
@OneToMany(mappedBy="team")
private List<Member> members = new ArrayList<Member>();
```

양방향 연관관계의 장점은 반대방향으로 객체 그래프 탐색 기능이 추가되는 것
따라서 위 필요에 의하지 않는다면 모든 관계를 양방향으로 정의할 필요 없음

#### 상속관계 매핑

- 관계형 데이터베이스에는 상속이라는 개념이 없으나, 슈퍼타입 서브타입 관계가 가장 유사함
- ORM에서 상속 관계 매핑은 슈퍼타입 서브타입 논리 모델을 물리 모델인 테이블로 구현한다는 것을 의미 (3가지 방식이 존재)

  - 조인 전략
  - 통합 테이블 전략
  - 서브타입 테이블로 변환
    - @MappedSuperClass사용 - 테이블과는 관계 없고 진정한 의미에서 슈퍼타입 서브타입 매핑은 아님
