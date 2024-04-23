---
layout: post
title: 파이썬 상식
subtitle: 파이썬 상식 100개
categories: Python
tags: [Python]
---

1. 파이썬 List의 len()은 시간복잡도가 O(1)이다.
    - len()은 __len__()을 호출하며, __len__()은 카운터로 작동하며 데이터가 정의되고 저장되면 자동적으로 증가한다. 
    - 따라서 데이터를 순회하며 깊이를 가져오는 명령(O(n)) 대신에 이미 저장된 value를 가지고 오게 된다.


