---
title: "[ES5] 함수형 프로그래밍으로 전환(map, filter, each, curry)"
layout: post
subtitle: 함수형 프로그래밍으로 전환
categories: study
tags: ES5
comments: true
related_posts:
- category/_posts/study/2021-03-03-es5-functional-programming.md
---

### 회원목록

```
var users = [
  { id: 1, name: 'ID', age: 36 },
  { id: 2, name: 'BJ', age: 32 },
  { id: 3, name: 'JM', age: 32 },
  { id: 4, name: 'PJ', age: 27 },
  { id: 5, name: 'HA', age: 25 },
  { id: 6, name: 'JE', age: 26 },
  { id: 7, name: 'JI', age: 31 },
  { id: 8, name: 'MP', age: 23 }
];
```

```
// 1. 명령형 코드
  // 1. 30세 이상인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age >= 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);
```

```
  // 2. 30세 이상인 users의 names를 수집한다.
var names = [];
for (var i = 0; i < temp_users.length; i++) {
  names.push(temp_users[i].name);
}
console.log(names);
```

```
  // 3. 30세 미만인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age < 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);
```

```
  // 4. 30세 미만인 users의 ages를 수집한다.
var ages = [];
for (var i = 0; i < temp_users.length; i++) {
  ages.push(temp_users[i].age);
}
console.log(ages);
```

### 위의 코드를 필터와 맵으로 리팩토링

```
// 2. _filter, _map으로 리팩토링
function _filter(list, predi) {
var new_list = [];
for (var i = 0; i < list.length; i++) {
  if (predi(users)) {
    new_list.push(users[i]);
		}
	}
	return new_list;
}

function _map(list, mapper) {
var new_list = [];
for (var i = 0; i < list.length; i++) {
    new_list.push(mapper(users[i]));
        }
    }
    return new_list;
}
```

```
var over_30 = _filter(users, function(user) { return user.age >= 30; });
console.log(over_30);

var names = _map(over_30, function(user) {
  return user.name;
});
console.log(names);

var under_30 = _filter(users, function(user) { return user.age < 30; });
console.log(under_30);

var ages = _map(under_30, function(user) {
  return user.age;
});
console.log(ages);
```

```
console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    function(user) { return user.name; }));

console.log(
  _map(
    _filter(users, function(user) { return user.age < 30; }),
    function(user) { return user.age; }));
```

##### Filter같은 함수는 응용형 함수라고 하며 응용형 함수는 함수가 함수를 받아서 원하는 시점에  해당하는 함수를 알고있는 인자를 적용하는 식으로 하는 것이 응용형 프로그래밍 혹은 적응형 프로그래밍이라고 한다.

##### 밑에는 each 로 _map과 _fileter의 중복을 제거하는 과정이다.
```
// 3. each 만들기
  // 1. _each로 _map, _filter 중복 제거
function _filter(list, predi){
	var new_list = [];
	_each(list, function(val){
		if(predi(val)) new_list.push(val);
	});
	return new_list;
}

function _map(list, mapper){
	var new_list = [];
	_each(list, function(val){
		new_list.push(mapper(val));
	});
	return new_list;
}

function _each(list, iter) {
	for(var i = 0; i<list.length; i++) {
		iter(list[i]);
	}
	return list;
}
```

```
  // 2. 외부 다형성
    // 1. array_like, arguments, document.querySelectorAll
console.log(
  [1, 2, 3, 4].map(function(val) {
    return val * 2;
  })
); // [2, 4, 6]

console.log(
  [1, 2, 3, 4].filter(function(val) {
    return val % 2;
  })
); //[1,3]
```
```

console.log(document.querySelectorAll('*')); // [html, head, ...]
```

##### 이 코드의 나타나는 배열이 아니다 왜냐하면 

```
console.log(
 document.querySelectorAll('*'), function(node) {
    return node.nodeName;
  })
); 
```
##### 위의 코드는 오류가 난다 왜냐하면 document.querySelectorAll의 결과는 Array가 아니기 때문이다. 

##### 이것이 객체지향의 특징이다. 메서드는 이렇게 해당하는 클래스의 준비되어있지 않은 메서드는 사용이 불가능하다. 즉 다형성을 지원하기가 어렵다.

```
console.log(
  _map(document.querySelectorAll('*'), function(node) {
    return node.nodeName;
  })
); 
```
위의 코드로 document.querySelectorAll를 함수형으로 바꿔주면 오류가 나지 않는다. 

```
// 3. 커링
  // 1. _curry, _curryr
	function _curry(fn) {
		return function(a,b) {
			return arguments.length == 2 ?
			fn(a,b) : function(b) {return fn(a,b); };
		}
	}
```
##### 본체함수의 결과를 미뤄뒀다가 결과에 반영하는 방식이 커링함수이다.

##### 밑에 코드는 위의 커링함수를 이용하여 add 변수만들었다.
```
var add = _curry(function(a, b) {
  return a + b;
});

var add10 = add(10);
var add5 = add(5); 
console.log( add10(5) ); //10
console.log( add(5)(3) ); //8
console.log( add(1,2) ); //3
```
##### 그리고 위의 _curry함수를 이용해서 빼기 함수를 적용시킬 _curryr 함수를 만들었다.
```
function _curryr(fn) {
	return function(a,b) {
		return arguments.length == 2 ?
		fn(a,b) : function(b) {return fn(b,a); };
	}
}

var sub = _curryr(function(a, b) {
return a - b;
});

console.log( sub(10, 5) ); //5

var sub10 = sub(10); 
console.log( sub10(5) ); //-5
```

```
// 2. _get 만들어 좀 더 간단하게 하기
var _get = _cuuryr(function _get(obj, key) {
	return obj == null ? undefined : obj[key];
});

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    _get('name')));
		
console.log(
  _map(
    _filter(users, function(user) { return user.age < 30; }),
    _get('age')));


var user1 = users[0] ;
console.log(user1.name); //ID
console.log(_get(user1, 'name')); //ID
console.log(_get('name')(user1)); //ID

console.log(_get(users[10], 'name')); //undefined
```
