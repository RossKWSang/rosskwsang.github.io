---
layout: post
title: Kernel360 boot-up에 대한 회고
subtitle: 백엔드 개발자가 되는 첫발은 아이디어에서부터
categories: Kernel360
tags: [백엔드, Boot-up, Kernel360, 패스트캠퍼스]
---



#### 요약 - kernel360 프로젝트 중 하나인 Boot-up을 마치고 그에 대한 회고와 공부한 기술스택, 기여도에 대한 글

* 일시    : 2023-10-11 ~ 2013-10-13

* 결과    :
  
  * 아이디어 기획 01(링크) - [음식점 정보 매칭 서비스](https://github.com/Kernel360/boot-up1-evereat)
  
  * 아이디어 기획 02(링크) - [음식점 위치 정보 제공 서비스](https://github.com/RossKWSang/boot-up1-flavor-match)

---
#### *<center>"회고: kernel360 boot-up은 나의 개발자 전원 버튼을 누르는 계기"</center>*

kernel360은 프로젝트 위주의 백엔드 부트캠프이다. 내가 이 과정에 합류한 이유는 회사에서 일하는 방식으로 배운다는 철학이 마음에 들어서였고, 그러한 내 생각은 boot-up부터 현실화되기 시작했다. boot-up은 컴퓨터가 부팅되듯이 조단위로 기획된 아이디어를 백엔드 단에서 어떻게 실현시킬지 그것을 구체화하기 위한 몇가지 작업을 하는 것으로 구성되었다. 대체로 작업은 기능명세서, 와이어프레임, API명세서, ER 다이어그램 작성 등이었다.


boot-up은 나의 부족함을 철저하게 느끼게되는 시기이기도 했다. 내가 원하는 서비스를 기획해보는 것은 일견 누구나 할 수 있는 일처럼 느껴진다. 하지만, 나는 백엔드 기획을 어떻게 하는지에 대한 경험이 전무했다. 반면, 과정을 함께하는 팀원들은 너무나 당연하다는 듯 업무를 진행해나가기 시작했다. 팀윈들과 함께하면서 나는 협업이라는 것에 허겁지겁 따라가고 있었다. 하지만 이러한 과정으로 아이디어 기획과 데이터스키마 설계 등을 여러가지 협업툴(github, notion, figma, erdcloud 등)로 진행할 수 있다는 사실을 알게되었는데, 실제로 글을 쓰는 시점인 개발을 진행하는 과정에서 협업을 하는데 많은 도움이 되고있다.


4일간의 boot-up기간을 반으로 나누어 팀을 바꾸는 방식으로 협업을 했다. 각 조에있는 조장을 제외한 모든 인원이 랜덤하게 조에 배속이 되고 팀이 바뀌고 난 이후 업무를 파악하고 그것을 더 구체화시키는 과정에서 새로운 업무에 적응한다는 것이 무엇인지 알게되었다.

---
#### *<center>"기술스택: 기술의 힘을 빌어 기획단계의 협업을 하기 시작했다."</center>*

boot-up기간에는 아이디어 기획이 주가 되었으므로 특별한 기술 스택을 사용하지는 않았다. 하지만, 개발에 진입하기 전 초기단계에서 사용할 것 같은 많은 협업툴을 사용해볼 수 있었다. 대부분의 협업툴은 구성원에게 변경되는 내용이 실시간으로 공유될 수 있고, 시각화 툴을 제공하고 있었다. 대부분 협업하기에 무리없는 수준의 기능을 무료로 사용할 수 있다.


* **github** - git의 기초적인 사용과 kernel360 Organization의 upstream 리포지토리를 fork하고 변경사항을 적용한 이후 pull request를 올리는 것을 배울 수 있었다. 그 외 프로젝트 생성, 이슈 제기 등으로 세분화된 To-do list를 어떤식으로 배분하는지 등을 배우게 되었다.


* **notion** - 패스트캠퍼스 운영진 측에서 제공하는 안내 페이지와 템플릿을 활용하여 팀별 회고와 문서작성등에 활용, notion은 실시간 내용 공유와 수많은 블록의 선택지를 제공하기 때문에 유연하게 협업에 사용될 수 있다.


* **figma** - 와이어프레임 제작을위해 화면을 구성할 수 있는 협업 툴로 디자이너들이 주로 사용하는 것으로 알고 있었던 협업툴이다. 이전에 창업 부트캠프를 다닐때도 figma를 써서 디자인된 화면을 구현한 적이 있지만, 내가 이것을 직접 써서 화면을 구현하는 것은 처음이었다.
 

* **erdcloud** - 데이터 관계도를 그리는 협업툴 ER-Diagram은 처음 그려보는 것이었고 팀에 기여를 하기위해서는 남아서 따로 그리는 법을 공부를 해야했었다.


---
#### *<center>"기여도: 팀을 위해 노력하는 것은 개인을 위한 노력이 선행되어야한다."</center>*

나는 두 팀을 거쳐가면서 위의 서술된 대부분의 내용에 기여를 하였다. 함께 배우는 사람들 중에 경험이 많은 사람이 보통 팀장이 되어 이끌어주는 모습을 많이 보았다. 기능명세서와 API명세서는 하나의 기능을 3-Depth의 세부목록과 각각의 URL을 작성하였고, ER-Diagram의 경우 한 기능에 핵심적으로 필요한 테이블을 그리고 그것과 연계된 관계를 그릴 수 있었다. 와이어프레임은 몇 가지 화면에 수정 보완이 필요한 부분에 대한 작업을 진행했다.

이 모든일은 github 리포지토리 readme.md에 올라가야하므로 내가 기여한 부분에 대하여 pull request를 보내고 merge되는 경험까지 할 수 있었다.

kernel360은 협업을 강조하는 부트캠프이다. 협업이 그냥 사이가 좋다는 것을 의미하는 것이 아니라는 말을 많이 들었는데 boot-up기간동안 실감이 날 수 있었다. 협업툴을 사용하는 것부터 나 스스로가 제대로 익혀놓지 않으면 협업이라는 것은 불가능했다. 그럼에도 조원을 잘 이끌고 도와준 팀원들과 리딩 그룹에 고마운 마음이 있다. 아래는 이 과정에서 내가 기여한 부분의 예시를 보여준다.

---

#### 음식점 정보 매칭 서비스 - ERD작성

음식점 정보 매칭 서비스는 점심 시간에 밥먹을 피어가 없는 사람들을 커뮤니티와 채팅을 통해 이어주는 서비스이다. 이를 실현시키기 위한 회원과 파티, 식당 등의 개념을 테이블로 모델링하고 관계를 설정하는 일을 하였다.

<img src="https://user-images.githubusercontent.com/68376744/274828994-3b16de8a-2ff2-40cc-b7e6-66af29cd2e4d.png" width="500px" height="500px" title="main image"/>

---

#### 음식점 정보 매칭 서비스 - 와이어프레임 작성

모바일 어플리케이션으로 출시된다는 가정하에 상세한 와이어프레임을 figma로 제작하는데 기여했다.

<img src="https://user-images.githubusercontent.com/86637372/275299682-71eec721-0994-456b-97e1-4285856a618d.png" width="500px" height="100px" title="main image"/>


---

#### 음식점 위치 정보 제공 서비스 - API 명세서

음식점 위치 정보 제공 서비스의 API 명세를 마크다운 문서에 json형식으로 작성하는데 기여했다.

> **회원 가입**

> **엔드포인트** : /api/sign-up

> Request
```json
{
"id": "flavor-match",
"password": "#######",
"username": "Kim",
"age": "25",
"sex": "male",
"religion": "No",
"language": "kr"
}
```
> Response
```json
{
  "status": "success",
  "message": "회원가입이 성공적으로 완료되었습니다.",
  "user_id": 12345,
  "registration_date": "2023-09-20"
}
```
---

> **회원 로그인**

> **엔드포인트** : /api/login

> Request
Method: POST
```json
{
    "email": "kkk@gmail.com",
    "password": "12345678",
}
```
> Response

> 201 Created

> Body:
```json
{
    "status": "success",
    "message": "User successfully signed in.",
    "data": {
        "id": "아이디",
        "username": "닉네임",
        "name": "이름",
        "email": "kkk@gmail.com",
        "age": 30,
        "gender": "Other",
        "nation_code": "US"
	}
}
```
---
> 내 리뷰 리스트 조회

> 엔드포인트  /api/myreviews/{user-id}

> Request

> Method : GET

> Parameter : user_id

> Response

```json
{
  "status": "success",
  "message": "회원별 리뷰 리스트 호출이 완료되었습니다.",
  "reviews": [
    {
      "restaurant_id": 305,
      "restaurant_review_id": 123498723,
      "star_coount": 5,
      "review_text": "점심시간에 잠깐 다녀온 곳인데 사장님이 친절하고 맛있었어요 ....."
    },
    {
      "restaurant_id": 501,
      "restaurant_review_id": 4564898723,
      "star_coount": 4,
      "review_text": "직장인 가성비 맛집 ....."
    },
    {
      "restaurant_id": 6745,
      "restaurant_review_id": 8797898723,
      "star_coount": 5,
      "review_text": "분위기 좋은 곳 ....."
    }
  ]
}
```