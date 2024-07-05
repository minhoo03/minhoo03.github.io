---
layout: single
title: "함수형 프로그래밍이란"
published: true
---

# 함수형 프로그래밍이란

함수형 프로그래밍은 프로그래밍 패러다임 중 하나로, 프로그램의 상태 변경을 최소화하고 예측 가능한 코드를 작성하는 것을 목표로 한다.

> 프로그래밍 패러다임(Programming Paradigm)은 프로그래밍을 하는 방식이나 접근법을 의미한다. 코드를 어떻게 작성하고 구조화할지 결정하는 방식이다.


함수형 프로그래밍에는 몇 가지 중요한 원칙이 있다.

* 함수의 입출력은 순수해야 한다
* 부작용이 없어야 한다
* 함수와 데이터를 중심으로 사고해야 한다

첫 번째와 두 번째 원칙은 동일한 개념을 다루고 있다.

함수가 순수하다는 것은 주어진 인자에 대해 항상 동일한 결과를 반환해야 하며, 외부 상태에 의존하거나 이를 변경하지 않아야 한다는 의미다. 즉, 순수 함수는 어떠한 인자를 받아 이를 처리한 후 항상 일관된 결과를 반환하는 함수이다.

<!-- 추가 설명 -->

# FxTS란

FxTS는 국내에서 만들어진 TypeScript 기반 함수형 프로그래밍 라이브러리다. 

타입 추론을 쉽게 할 수 있고, 동시성 프로그래밍을 핸들링하기 좋고, 자바스크립트 표준 객체인 iterable/asynciterable을 다루기 때문에 언어의 발전과 함께 갈 수 있고, 자바스크립트 표준 에러 핸들링을 지원한다.

즉, FxTS를 사용하면 쉽고 선언적인 비동기 프로그래밍과 타입 추론, 에러 처리에 강점을 가져가 코드의 가독성과 유지 보수성을 높일 수 있다.
