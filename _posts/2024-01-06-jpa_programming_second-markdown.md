---
layout: post
title: 자바 ORM 표준 JPA 프로그래밍 정리하며 읽기 - 06
subtitle: 연관관계 부분 6장
categories: JPA
tags: [Spring Boot, JPA]
---

이 문서는 김영한 개발자의 [자바 ORM 표준 JPA 프로그래밍]의 6장 []을 정리한 문서입니다. 책 내용을 읽어나가면서 정리할 내용을 가지치기하는 방식으로 정리를 시도했습니다.


| 내용            | 객체 (엔티티 객체 X)                      | 테이블                             |
| --------------- | ----------------------------------------- | ---------------------------------- |
| 방향            | 단방향                                    | 양방향 (JOIN 사용)                 |
| 매개            | 참조                                      | 외래 키                            |
| 다중성          | 다중성 연관관계 있음                      | 다중성 연관관계 있음               |
| 연관관계의 주인 | 양방향 연관관계로 만들면 주인을 따로 선정 | 있음 (외래키를 가지고 있는 테이블) |

#### 다대일 단방향

- 회원은 Member.team으로 팀 엔티티를 참조할 수 있지만 반대로 팀에는 회원을 참조하는 필드가 없다.

```java
@Entity
public class Member {
  
  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;
  
  private String username;
  
  @ManyToOne
  @JoinColumn(name = "TEAM_ID")
  private Team team;
}
```

```java
@Entity
public class Team {

  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;

  private String name;
}
```

---

#### 다대일 양방향

- 양방향은 외래 키가 있는 쪽이 연관관계의 주인이다.
  - 일대다의 경우 "다"의 경우에서 항상 외래키를 가지고 있음.
  - Member와 Team의 관계에서는 Member.team이 연관관계의 주인임.
- 양방향 연관관계는 항상 서로를 참조해야 한다.
  - setTeam또는 addMember와 같은 메서드를 사용.
  - 무한루프에 빠지지 않도록 검사하는 로직.


```java
@Entity
public class Member {
  
  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;
  
  private String username;
  
  @ManyToOne
  @JoinColumn(name = "TEAM_ID")
  private Team team;

  public void setTeam(Team team) {
    this.team = team;
    if(!team.getMember().contains(this)) {
      team.getMembers().add(this);
    }
  }
}
```


```java
@Entity
public class Team {

  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;

  private String name;

  @OneToMany(mappedBy = "team")
  private List<Member> members = new ArrayList<Member>();

  public void addMember(Member member) {
    this.members.add(member);
    if (member.getTeam() != this) {
      member.setTeam(this)
    }
  }
}
```

#### 일대다 단방향

- 한 엔티티가 여러 엔티티를 조회할 수 있지만 반대의 경우는 성립하지 않는 경우
- @JoinColumn을 명시해야함
- 단점 : 매핑한 객체가 관리하는 외래 키가 다른 테이블에 있다는 것 -> 연관관계 처리를 INSERT하나로 할 수 있는 것이 아니라, UPDATE도 추가로 실행해야함


```java
@Entity
public class Member {
  
  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;
  
  private String username;
  
}
```

```java
@Entity
public class Team {

  @Id @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long Id;

  private String name;

  @OneToMany(mappedBy = "team")
  @JoinColumn(name = "TEAM_ID") // MEMBER 테이블의 TEAM_ID(PK)
  private List<Member> members = new ArrayList<Member>();
}
```

일대다 매핑의 단점 - Persist시 매핑된 개수 만큼 추가 UPDATE 쿼리가 발생한다.

```java
public void testSave() {
  Member member1 = new Member("member1"):
  Member member2 = new Member("member2"):

  Team team1 = new Team("team1");
  team1.getMembers().add(member1):
  team1.getMembers().add(member2):

  em.persist(member1);  // INSERT member1
  em.persist(member2);  // INSERT member2
  em.persist(team1);    // INSERT team1, UPDATE member1.fk, UPDATE member2.fk

  transaction.commit();
}
```

#### 일대다 양방향

- 일대다 단방향 매핑 반대편에 다대일 단방향 매핑을 읽기 전용으로 추가하여 일대다 양방향처럼 보이도록 함
- 일대다 단방향 매핑이 가지는 단점(쿼리가 추가발생)을 그대로 가진다
- 쓰지 말자


#### 일대일

- 일대일 관계는 그 반대도 일대일 관계
- 다대일 관계에서는 외래키를 꼭 다(N) 쪽에서 가지지만 일대일 관계에서는 어디서든 가질 수 있음
- MEMBER가 주 테이블, LOCKER가 대상 테이블

##### 일대일 단방향

##### 일대일 양방향


```java
@ManyToOne
@JoinColumn(name="TEAM_ID")
private Team team;
```

@ManyToOne : 다중성을 나타내는 어노테이션, 이 경우는 다대일 관계를 나타냄
@JoinColumn : 외래키를 매핑할 때 사용하는 어노테이션
