---
layout: "post"
title: "Unified Modeling Language(UML) basic"
author: "mingi hong"
date: 2020-01-26 08:24:58 -0700
categories: "Computer Knowledge"
permalink: /:categories/:title.html
---

## Unified Modeling Language

Class간의 관계

관계성 - SW 동작


1. Generalization  부모쪽으로 화살표 표시
2. Association       직선 하지만 dependency를 고려하여 화살표를 그려줌 (  )
3. Aggregation     마름모 직선 ( 클래스를 멤버변수로 가지고 있다 )


- Public +
- Protected #
- Private -



설계자 -> 개발자



instance 갯수가 있을 경우 숫자를 적어줌

????????????????????????????????????????

상속 사용

중복성 ->

동물원 프로그램

동물원 클래스

사자 / 호랑이 / 펭귄 / 독수리 / 원숭이 


포유류 ( 공통점 ) 일반적인 기능 끄집어 내서 사용 
- 다리가 4개다


“IS’관계

“UPCAST” 언제나 허용 - 밑에서 위로 형변환

“DOWN CAST” 안됨   - 위에서 밑으로 형변환
