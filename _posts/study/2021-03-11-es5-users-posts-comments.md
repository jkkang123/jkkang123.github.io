---
title: "[ES5] 실전코드 "
layout: post
subtitle: ES5 실전코드
categories: study
tags: ES5
comments: true
related_posts:
- category/_posts/study/2021-03-10-es5-delay-evaluation
---

### 어플리케이션을 개발할때 필요한 함수형 프로그래밍들을 코딩하기위해 연습해보자


##### 데이터
```
var users = [
  { id: 101, name: 'ID' },
  { id: 102, name: 'BJ' },
  { id: 103, name: 'PJ' },
  { id: 104, name: 'HA' },
  { id: 105, name: 'JE' },
  { id: 106, name: 'JI' }
];

var posts = [
  { id: 201, body: '내용1', user_id: 101 },
  { id: 202, body: '내용2', user_id: 102 },
  { id: 203, body: '내용3', user_id: 103 },
  { id: 204, body: '내용4', user_id: 102 },
  { id: 205, body: '내용5', user_id: 101 },
];

var comments = [
  { id: 301, body: '댓글1', user_id: 105, post_id: 201 },
  { id: 302, body: '댓글2', user_id: 104, post_id: 201 },
  { id: 303, body: '댓글3', user_id: 104, post_id: 202 },
  { id: 304, body: '댓글4', user_id: 105, post_id: 203 },
  { id: 305, body: '댓글5', user_id: 106, post_id: 203 },
  { id: 306, body: '댓글6', user_id: 106, post_id: 204 },
  { id: 307, body: '댓글7', user_id: 102, post_id: 205 },
  { id: 308, body: '댓글8', user_id: 103, post_id: 204 },
  { id: 309, body: '댓글9', user_id: 103, post_id: 202 },
  { id: 310, body: '댓글10', user_id: 105, post_id: 201 }
];
```

```
// 1. 특정인의 posts의 모든 comments 거르기

_.go(
	_.where(posts, { user_id : 101}),
	_.pluck('id')
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
	console.log); //user_id가 101인 post에서의 post_id를 comments에서 찾아서 출력
```
```
// 2. 특정인의 posts에 comments를 단 친구의 이름들 뽑기
_.go(
	_.where(posts, { user_id : 101}),
	_.pluck('id')
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
	_.map(function(comment) {
		return _.find(users, function(user) {
			return user_id == comments.user_id;
		}).name;
	}),
	_.uniq,
	console.log); //특정인의 이름을 출력하면서 중복인 이름 제거
```

##### 위에서 중복된 내용의 코드를 제거하고 합치면 다음과 같다.

```
// 1. 특정인의 posts의 모든 comments 거르기

function posts_by(attr) {
  return _.where(posts, attr);
}

var comments_by_posts = _.pipe(
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  });
	
	var f1 = _.pipe(posts_by, comments_by_posts);
	
console.log(f1({ user_id: 101 }));

	
// 2. 특정인의 posts에 comments를 단 친구의 이름들 뽑기

var comments_to_user_names = _.map(function(comment) {
  return _.find(users, function(user) {
    return user.id == comment.user_id;
  }).name;
});

var f2 = _.pipe(f1, comments_to_user_names, _.uniq);

console.log(f2({ user_id: 101 }));

```

##### 위에있는 f1 함수와 f2 함수를 이용해서 밑의 카운트 정보를 나타내는 표현도 간단하게 코드를 작성할 수 있다.

```
// 3. 특정인의 posts에 comments를 단 친구들 카운트 정보

var f3 = _.pipe(f1, comments_to_user_names, _.count_by);

console.log(f3({ user_id: 101 }));
```



```
// 4. 특정인이 comment를 단 posts 거르기
_.go(
  _.where(comments, { user_id: 105 }),
  _.pluck('post_id'),
  _.uniq,
  function(post_ids) {
    return _.filter(posts, function(post) {
      return _.contains(post_ids, post.id);
    });
  },
  console.log);
```

##### 유저들의 정보를 가져오고 그 유저의 포스트와 달려있는 코멘트를 추출하는 코드를 작성해보자 

```
// 5. users + posts + comments (index_by와 group_by로 효율 높이기)


var users2 = _.index_by(users, 'id');

var comments2 = _.go(
  comments,
  _.map(function(comment) {
    return _.extend({
      user: users2[comment.user_id]
    }, comment);
  }),
  _.group_by('post_id'));

console.log(comments2);

var posts2 = _.go(
  posts,
  _.map(function(post) {
    return _.extend({
      comments: comments2[post.id] || [],
      user: users2[post.user_id]
    }, post);
  }));

var posts3 = _.group_by(posts2, 'user_id');
console.log(posts3);

var users3 = _.map(users2, function(user) {
  return _.extend({
    posts: posts3[user.id] || []
  }, user);
});

console.log(users3);
```

##### 위의 만들어진 user2와 user3를 이용해서 특정한 값을 뽑아내는 코드를 작성해보자

```
// 5.1. 특정인의 posts의 모든 comments 거르기
var user = users3[0];

_.go(user.posts,
  _.pluck('comments'),
  _.flatten,
  console.log);
	
console.log(_.deep_pluck(user, 'posts.comments'));
	
// 5.2. 특정인의 posts에 comments를 단 친구의 이름들 뽑기
_.go(user.posts,
  _.pluck('comments'),
  _.flatten,
  _.pluck('user'),
  _.pluck('name'),
  _.uniq,
  console.log);
	
_.go(user, _.deep_pluck('posts.comments.user.name'), _.uniq, console.log);

// 5.3. 특정인의 posts에 comments를 단 친구들 카운트 정보

_.go(user.posts,
  _.pluck('comments'),
  _.flatten,
  _.pluck('user'),
  _.pluck('name'),
  _.count_by,
  console.log);

_.go(user, _.deep_pluck('posts.comments.user.name'), _.count_by, console.log);

// 5.4. 특정인이 comment를 단 posts 거르기

console.log(
  _.filter(posts2, function(post) {
    return _.find(post.comments, function(comment) {
      return comment.user_id == 105;
    })
  })
);
```
