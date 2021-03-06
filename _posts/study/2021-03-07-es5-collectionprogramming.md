---
title: "[ES5] 콜렉션 중심 프로그래밍 (reject,find,reduce... )"
layout: post
subtitle: 콜렉션 중심 프로그래밍
categories: study
tags: ES5
comments: true
related_posts:
- category/_posts/study/2021-03-04-es5-translatetofunctionalprogramming
---


### 컬렉션 중심 프로그래밍

##### 컬렉션 중심 프로그래밍의 4가지 유형과 함수

 1. 수집하기 - map, values, pluck 등
2. 거르기 - filter, reject, compact, without 등
3. 찾아내기 - find, some, every 등
4. 접기 - reduce, min, max, group_by, count_by

```
var users = [
  { id: 10, name: 'ID', age: 36 },
  { id: 20, name: 'BJ', age: 32 },
  { id: 30, name: 'JM', age: 32 },
  { id: 40, name: 'PJ', age: 27 },
  { id: 50, name: 'HA', age: 25 },
  { id: 60, name: 'JE', age: 26 },
  { id: 70, name: 'JI', age: 31 },
  { id: 80, name: 'MP', age: 23 },
  { id: 90, name: 'FP', age: 13 }
];
```


##### Values 함수
```
function _values(data) {
	return _map(data, function(val) {return val;});
}

var _values = _map(_identity);

function _identity(val) {
  return val;
}

console.log(users[0]); //[ id: 10, name: 'ID', age: 36 ]
console.log(_keys(users[0])); //[ "id", "name", "age" ]
console.log(_values(users[0])); //[ 10, "ID", 36 ]

```

##### Pluck 함수

```
function _pluck(data, key) {
	return _map(data, _get(key));
}
console.log( _pluck(users, 'age') );
console.log( _pluck(users, 'name') );
console.log( _pluck(users, 'id') );
```

##### Reject 함수

```
//   1. reject
//      reject 함수와 같은함수
function _negate(func) {
  return function(val) {
    return !func(val);
  }
}

function _reject(data, predi) {
	return _filter(data, _negate(predi));
	});
}


console.log(
  _filter(users, function(user) {
    return user.age > 30;
  })
);

console.log(
  _reject(users, function(user) {
    return user.age > 30;
  })
);

//   2. compact

var _compact = _filter(_identity);

console.log(
  _compact([1, 2, 0, false, null, {}]));
```
##### Find함수

```
// 3. 찾아내기 - find
//   1. find 만들기
var _find = _curryr(function(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    var val = list[keys[i]];
    if (predi(val)) return val;
  }
});

console.log(
	_find(users, function(user) {
		return user.id == 20:
		})
	);
	
	
_go(users, _find_index(function(user) { return user.id == 50; }),
  console.log); //4

//   2. find_index
var _find_index = _curryr(function(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    if (predi(list[keys[i]])) return i;
  }
  return -1;
});

console.log(
  _find_index(users, function(user) {
    return user.id == 50;
  })
); // 4
```

##### Some 함수, Every 함수

```
//3. some

function _some(data, predi) {
  return _find_index(data, predi || _identity) != -1;
}

//   4. every
function _every(data, predi) {
  return _find_index(data, _negate(predi || _identity)) == -1;
}

//some과  every 예제
console.log(_some([1, 2, 5, 10, 20], function(val) {
  return val > 20;
})); //false

console.log(_some([1, 2, 5, 10, 20], function(val) {
  return val > 10;
})); //true


console.log(_every([1, 2, 5, 10, 20], function(val) {
  return val > 10;
}));
// false
console.log(_every([12, 24, 5, 10, 20], function(val) {
  return val > 3;
}));
// true

console.log(
  _some([1, 2, 0, 10])
)
// true
console.log(
  _some([null, false, 0])
)
// false
console.log(
  _some([null, false, 1])
)
// true

console.log(
  _every([1, 2, 0, 10])
)
// false
console.log(
  _every([null, false, 0])
)
// false
console.log(
  _every([null, false, 1])
)
// false
console.log(
  _every([1, 2, 10])
)
// true

console.log(
  _some(users, function(user) {
    return user.age < 20;
  })
); //true
```
