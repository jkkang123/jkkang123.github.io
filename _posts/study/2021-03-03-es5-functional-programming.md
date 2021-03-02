---
layout: post
title: ES5-functional Programming
subtitle: 함수형 프로그래밍
categories: study
tags: ES5
comments: true
---

# 함수형 프로그래밍에 대해서
---
**함수형 프로그래밍이랑 성공적인 프로그래밍을 위해 부수 효과를 미워하고 조합성을 강조하는 프로그래밍 패러다임**
* 부수효과를 미워한다. => 순수함수를 만든다.
* 조합성을 강조한다. => 모듈화 수준을 높인다.
<br><br>


> 순수함수 (동일한 인자를 넣으면 동일한 결과값)
>  
> ```
>  function add(a,b) {
>  return a+b;
>  }
>  
>  console.log( add(10,5)); //15
> ```
 
>  순수함수가 아닌 함수
>  
> ```
>  var c  = 10;
>  function add2(a, b) {
>  return a+ b+ c;
>  }
>  
>  console.log( add2(10,2) ); // 22
>  console.log( add2(10,3) ); // 23
>  console.log( add2(10,4) ); // 24
>  c = 20;
>  console.log( add2(10,2) ); // 32
>  console.log( add2(10,3) ); // 33
>  console.log( add2(10,4) ); // 34
> ```

**순수함수가 아닌 함수는 랜덤한 위치에서 함수를 실행시킬때 값이 변화되는 함수이다.**
 
**함수형 프로그래밍에서는 값을 변형해나가거나 값을 다룰때 원래 초기화되어있는 값을 건들지 않고 외부의 상태를 변화시키지 않으면서 값을 다루어 나가는 프로그래밍**

> 일급함수 : 변수에 함수를 값으로 다룰수 있는 함수
> 
> ```
> var f1 = function(a) { return a*a; };
> console. log(f1);
> ```
> 
> ```
> var f2 = add;
> console.log(f2);
> ```
> 
> ```
> function f3(f){ // 함수를 값으로 받는 함수
> return f();
> }
> 
> console.log( f3(function() {return 10;}) ); //10
> console.log( f3(function() {return 20;}) ); //20
> ```

> Add Maker
> ```
> function add_maker(a) {
> 	return function(b) { // closer
> 		return a + b;
> 	}
> }
> 
> var add10 = add_maker(10);
> 
> console.log( add10(20) ); //30
> ```

> 함수형 프로그래밍 예제
> 
> ```
> function f4( f1, f2, f3) {
> 	return f3(f1() + f2());
> }
> 
> console.log(
> 	f4(
> 		function() { return 2; } ,
> 		function() { return 1; } ,
> 		function(a) { return a * a; }));  // 3*3 = 9
> ```

`함수형 프로그래밍`은 애플리케이션, 함수의 구성요소, 더 나아가서 언어 자체를 함수처럼 여기도록 만들고, 이러한 `함수 개념을 가장 우선순위`에 놓는다.
`함수형 사고방식`은 문제의 해결방법을 `동사`(함수)들로 `구성`(조합) 해놓는것  
[*마이클 포거스 (클로저 프로그래밍의 즐거움)에서...*]
