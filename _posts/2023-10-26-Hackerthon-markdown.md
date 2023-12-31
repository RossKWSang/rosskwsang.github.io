---
layout: post
title: Kernel360 해커톤에 대한 회고
subtitle: 스프링부트를 사용하여 백엔드를 직접해보다.
categories: Kernel360
tags: [백엔드, 해커톤, Kernel360, 패스트캠퍼스]
---



#### 요약 - kernel360 프로젝트 중 하나인 해커톤을 마치고 그에 대한 회고와 공부한 기술스택, 기여도에 대한 글

* 일시    : 2023-10-16 ~ 2013-10-20

* 결과    :
  
  * 아이디어 기획 - [kernel360의 스터디 개설 서비스 - studyUp](https://github.com/RossKWSang/hackerthon1-studyUp)
  
  * 아이디어 기획 - [나의 작은 블로그 - ministory](https://github.com/RossKWSang/hackerthon1-ministory/tree/main)


---
#### *<center>"회고: 해커톤은 내가 가진 모든 것을 쏟는 것"</center>*

해커톤은 짧은 시간동안 내가 할 수 있는 모든 역량을 ***총동원*** 하여 일정 수준의 결과물을 얻어내는 것이다.

해커톤도 boot-up과 마찬가지로 팀을 한번씩 옮기면서 진행하였다. boot-up과 다른점은 해커톤은 주어진 시간동안 코딩을 활용하여 시연가능한 산출물을 내어야한다는 것이었다. 모든 조에서 백엔드 기술을 구현할 프레임워크로 Spring Boot를 선정했다. 나의 경우 자바 프로그래밍의 입문을 겨우 마친 상태였으므로 리딩하는 조원 또는 그들이 남긴 코드를 분석하며 구현방식을 익히기 시작했다. 엔터티, DTO, 리포지토리, 서비스, 컨트롤러 등 기초적인 CRUD 기능이 나올 수 있도록 빠르게 코드를 모방하기 시작했고, 하나의 기능에 동작하는 간단한 코드를 작성할 수 있었다.

내가 가진 모든 것을 쏟는다는 해커톤의 취지에 맞추기에는 나의 백엔드 실력이 그렇게 높지는 않았다. 하지만 현업에서는 갑자기 새로운 일을 맡는일이 자주 일어나기 때문에 무언가를 신속하게 배우는 능력을 얻었다고 생각하면 나름 보람이 있었을 것이라 생각한다.

해커톤의 가장 좋았던 점은 다른 사람들의 레거시 코드를 분석해서 첨가하거나 수정할 수 있다는 것이었다. 좋은 코드를 썼다고 자부할 수는 없지만 스프링부트와 타임리프를 사용하여 구현된 몇가지 기능들이 기 코딩이 되어있는 프로젝트에 추가되는 것이었다. 코딩(또는 프로그래밍)은 계속할 수록 의문이 늘어가는 분야인 것 같다. 내가 학습에 사용할 수 있는 시간은 한정되어있고 나는 그것을 활용해 최고의 결과를 얻어내야한다. 어떤 것이 핵심인지 어떤 것에 집중해야 하는지 지금도 많은 고민이 있다. 

해커톤을 마친 시점은 kernel360에 합류한지 10일이 되는 날이었다.

---
#### *<center>"기술스택: 타인에게 제공할 지식이 없다면 시간을 써야한다."</center>*

협업툴은 이전과 마찬가지로 github, notion, slack, figma, erdcloud를 사용하였다. boot-up에서 기획 단계의 역할을 맡아보았기 때문에 조금 더 빠른 속도로 많은 일들을 할 수 있었다. 

코딩에서는 스프링부트와 타임리프를 사용하여 카테고리를 작성하였다. 데이터베이스는 MySQL을 사용하고 JPA로 이를 연동하였다. 기술스택이 가지는 다른 선택지 대비 장점은 다음과 같다.

* 스프링부트(Spring Boot) : 생산성, 보안성, 활성화된 커뮤니티, 손쉬운 데이터베이스 매핑
  
* 타임리프(Thymeleaf) : 템플릿 재사용, 커스텀 태그 라이브러리, html과 Java의 결합
  
* MySQL : 뛰어난 성능, 보안성, 다양한 기능 등 

아래는 해커톤 기간동안 작성한 코드의 일부분

> <center>CategoryController.java</center>

---

```java
package com.example.ministory.controller;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.example.ministory.dto.CategoryDto;
import com.example.ministory.entity.Category;
import com.example.ministory.service.CategoryService;

import lombok.AllArgsConstructor;

@AllArgsConstructor
@Controller
@RequestMapping("/api/category")
public class CategoryController {
	private CategoryService categoryService;

	// TODO: 유저의 카테고리를 전부 받아서 리스팅하는 함수
	@GetMapping("")
	public String getCategoryList(Model model, Long userId) {
		List<Category> categories = categoryService.findUserCategory(1L);
		model.addAttribute("categories", categories);
		return "category/categoryList";
	}

	// TODO: 카테고리를 입력하는 함수
	@PostMapping("")
	public String postCategory(CategoryDto categoryDto, Long userId) {
		categoryDto.setTitle("test");
		// TODO: 1번 유저가 생성한 카테고리로 우선 분류
		categoryService.saveCategoryOnUser(categoryDto, 1L);
		return "redirect:category";
	}

	@PostMapping("/delete")
	public String deleteCategory(Long categoryId) {
		categoryService.deleteCategory(categoryId);
		return "redirect:/api/category";
	}
}
```

> <center>CategoryDto.java</center>
---

```java
package com.example.ministory.dto;

import com.example.ministory.entity.Category;
import com.example.ministory.entity.User;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
@NoArgsConstructor
public class CategoryDto {
    private Long categoryId;
    private String title;
    public Category toEntity(User user) {
        return Category.builder()
                .categoryId(categoryId)
                .user(user)
                .title(title)
                .build();
    }
}
```

> <center>CategoryService.java</center>

---
 
```java
package com.example.ministory.service;

import com.example.ministory.dto.CategoryDto;
import com.example.ministory.entity.User;
import com.example.ministory.entity.Category;

import com.example.ministory.exception.NotFoundException;
import com.example.ministory.repository.UserRepository;
import com.example.ministory.repository.CategoryRepository;


import lombok.AllArgsConstructor;
import org.aspectj.weaver.ast.Not;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.transaction.Transactional;
import java.util.List;

@Service
@AllArgsConstructor
public class CategoryService {
    @Autowired
    private final UserRepository userRepository;
    @Autowired
    private final CategoryRepository categoryRepository;

    @Transactional
    public List<Category> findUserCategory(Long userId) {
        User user = userRepository.findById(userId).orElseThrow(() -> new NotFoundException("해당하는 User가 없습니다."));
        return categoryRepository.findCategoriesByUser(user);
    }

    @Transactional
    public void saveCategoryOnUser(CategoryDto categoryDto, Long userId) {
        User user = userRepository.findById(userId).orElseThrow(()-> new NotFoundException("해당하는 User가 없습니다."));
        System.out.println(categoryDto.getTitle());
        Category category = categoryDto.toEntity(user);

        categoryRepository.save(category);
    }

    @Transactional
    public void updateCategory(Long categoryId, String title) {
        Category category = categoryRepository.findById(categoryId).orElseThrow(() -> new NotFoundException("해당하는 Category가 없습니다."));
        category.setTitle(title);
    }

    @Transactional
    public void deleteCategory(Long categoryId) {
        categoryRepository.deleteById(categoryId);
    }
}
```

> <center>CategoryList.html</center>

---

```html
<!DOCTYPE HTML>
<html xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      xmlns:th="http://www.thymeleaf.org"
      layout:decorate="~{layout/defaultLayout}">
<div layout:fragment="content" class="container my-3 border-bottom">
    <div>
        <h4>카테고리 생성</h4>
    </div>
    <div/>
    <form th:action="@{/api/category}" th:object="${categoryDto}" method="post">
        <div class="mb-3">
            <label for="categoryTitle" class="form-label">카테고리명</label>
            <input type="text" id="categoryTitle" name="categoryTitle" class="form-control" placeholder="카테고리명을 입력하세요." required="required"/>
        </div>
        <button type="submit" class="btn btn-primary">등록하기</button>
    </form>

    <div>
        <table class="table">
            <thead class="table">
            <tr>
                <th>카테고리명</th>
                <th>삭제</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="category : ${categories}">
                <td th:text="${category.title}"></td>
                <td>
                    <form th:action="@{/api/category/delete}" th:method="POST">
                        <input type="hidden" name="categoryId" th:value="${category.categoryId}" />
                        <button type="submit" name="_method" class="btn btn-danger" value="DELETE">삭제하기</button>
                    </form>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</div> <!-- /container -->
</html>
```

---

#### *<center>"기여도: 몇 가지 성취는 있었지만 배부른 첫술은 아니었다."</center>*

카테고리를 추가하고 삭제하는 기능이 작동하는 것은 확인했으나, 촉박한 일정 안에 완벽한 CRUD를 구현할 수는 없었다. 상당한 아쉬움으로 남았으며 이를 E2E 프로젝트를 진행하며 해소하길 원한다.

카테고리는 사용자로부터 입력을 받아 카테고리 엔티티로 저장되며 삭제버튼을 누를 경우 삭제된다. 사용자의 메인페이지에서 카테고리란을 열람할 경우 테이블 형식으로 나열되도록 코딩했다. 또한 몇가지 엔드포인트를 연결할 수 있도록 화면에 링크를 구현하였다. (아래 그림)

<img src="https://user-images.githubusercontent.com/147565215/276836064-8df8866c-cd8d-44ec-befb-514652509ea5.png" width="700px" height="300px" title="main image"/>


