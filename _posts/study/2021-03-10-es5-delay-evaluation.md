---
title: "[ES5] 자바스크립트에서의 지연평가,요약 "
layout: post
subtitle: 자바스크립트에서의 지연평가,요약
categories: study
tags: ES5
comments: true
related_posts:
- category/_posts/study/2021-03-07-es5-collectionprogramming
---

## 엄격한 평가와 지연평가

##### 지연평가를 통해 값을 전부 계산하지않고 원하는 값만 계산하여서 표현하는 방법은 순수함수를 이용하는 방법이 있다.
> ![1](https://cdn.discordapp.com/attachments/700628529968054284/818862199765991495/unknown.png)
> 
> ##### 위의 이미지는 엄격한 평가로써 val6번째의 값들을 map과 filter 그리고 reject의 함수들을 거쳐서 차례차례로 모든 값들을 실행시켜서 값을 반환 하는 방식이다.

<br><br><br>

> ![2](https://cdn.discordapp.com/attachments/700628529968054284/818862079763152917/unknown.png)
> 
> ##### 하지만 위의 그림은 지연평가를 이용해서 자신이 원하는 값인 2개의 값을 구하고 나면 더이상 연산을 하지않고 프로그램을 종료하는 방식이다. 이러한 방식이 지연평가이다.

<br><br>

##### 아래의 코드는 엄격한 평가와 지연평가를 비교하는 함수이다.
<br>
```

// 지연 평가를 시작 시키고 유지 시키는(이어 가는) 함수
  // 1. map
  // 2. filter, reject
var mi = 0;
var fi = 0;

_.go(
  _.range(100),
  _.map(function(val) {
    ++mi;
    return val * val;
  }),
  _.filter(function(val) {
    ++fi;
    return val % 2;
  }),
  _.some(function(val) {
    return val > 100;
  }),
  console.log);

console.log(mi, fi);


var mi = 0;
var fi = 0;

_.go(
  _.range(100),
  L.map(function(val) {
    ++mi;
    return val * val;
  }),
  L.filter(function(val) {
    ++fi;
    return val % 2;
  }),
  L.some(function(val) {
    return val > 100;
  }),
  console.log);

console.log(mi, fi);

// 끝을 내는 함수
  // 1. take
  // 2. some, every, find
```

<br><br>
### <center> 함수형 자바스크립트 요약 </center><br>


1. 함수를 되도록 **작게** 만들기

2. **다형성** 높은 함수 만들기

3. **상태**를 변경하지 않거나 **정확히** 다루어 **부수 효과를 최소화**하기

4. 동일한 인자를 받으면 항상 동일한 결과를 리턴하는 **순수함수** 만들기

5. 복잡한 객체 하나를 사용하기보다 되도록 **일반적인 값** 여러 개를 **인자**로 사용하기

6. 큰 **로직**을 **고차 함수**로 만들고 세부 로직을 **보조 함수**로 완성하기

7. 어느곳에서든 **바로 혹은 미뤄서 실행**할 수 있도록 일반 함수이자 순수함수로 선언하기

8. 모델이나 컬렉션 등의 커스텀 객체보다는 **기본 객체**를 이용하기

9. 로직의 흐름을 최대한 **단방향으로 흐르게** 하기

10. 작은 함수를 **조합**하여 큰 함수 만들기
