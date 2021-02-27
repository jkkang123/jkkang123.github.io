---
title: 졸업논문 프로젝트
layout: post
subtitle: StreetViewProject
date: '2021-02-28 01:00:00'
categories:
- project
---

# <center>기존의 스트리트뷰의 시스템의 <br>문제점과 해결방안</center>
<br><br>

## 1. 서론
---

### 1. 연구의 배경 및 목적

현재 살고 있는 대한민국을 기준으로 사람들은 자신이 가고 싶은 지역이 있거나 혹은 다른 곳으로 여행을 가서 그 지역에서 길을 찾을 때 핸드폰에 있는 지도 어플리케이션을 이용한다. <br>
그리고 지도에서 원하는 위치를 정하고 스트리트뷰 시스템을 통해서 그 지도의 주변 모습을 볼 수 있다.

하지만 현재의 지도 어플리케이션의 스트리트뷰는 거리의 모습을 찍어서 제공하기는 하지만 그것은 대부분 차가 지나갈 수 있는 도로주변이나 커다란 거리만 가능하다. <br>
차가 다닐 수 없는 등산로나 골목길 같은 길의 모습을 보기 위해서는 그 주변의 지역을 직접 발로 찾아가서 봐야 될 필요가 있다. <br>
이 글에서 다루는 어플리케이션은 서로 이용하는 사람들이 차가 다닐 수 없는 시골길이나 등산로 같은 길을 사람들끼리 서로 사진이나 동영상을 촬영해서 지도에 표시함으로써 가보지 못한 사람들에게 현재 그 공간이 어떤 장소인지 정보를 제공받을 수 있다.

---
## 2. 본론

---
### 1. 요구 분석
* #### 사용자의 요구사항분석
먼저 개발하기 전에 사용자들에게 요구사항을 분석한다. 사용자들은 이 어플리케이션을 통해서 사람들과 자신이 찍은 스트리트 뷰를 공유하기를 원하고 또한 그러한 찍은 스트리트 뷰를 지도에서 볼 수 있기를 원한다. <br>
이렇게 두 가지 요구사항을 바탕으로 어플리케이션의 개발환경과 기능들을 미리 설계하였다.
* #### 개발환경
개발할 어플리케이션의 가명은 대동여지도로 구조는 크게 유저와 웹페이지, 서버 그리고 미디어 촬영기기로 구분될 수 있다. <br>
웹페이지는 유저들이 대동여지도의 기능을 실행하기 위한 수단으로 Firebase서버와 연동이 되어있으며 유저들의 정보를 전달 및 수용할 수 있다. <br>
서버는 Firebase를 사용하는데 유저들은 Firebase를 통해서 자신의 계정 정보나 미디어 파일들 같은 데이터들을 저장 및 관리가 가능하다. <br>
유저는 미디어 촬영기기를 가지고 미디어를 촬영하거나 저장하여서 이 저장한 데이터들을 가지고 웹페이지에 접속해서 서로 정보를 공유할 수 있다.
* #### 기능
대동여지도의 기능에는 지도기능, 업로드기능 그리고 마이페이지로 나뉘어 진다. <br>
지도기능에는 장소검색, 마커설정, 라인표시기능, 현재위치 표시기능 그리고 스트리트 뷰 감상기능이 있다. <br>
업로드기능에는 사진업로드와 동영상업로드 기능이 있고 유저에는 로그인 회원가입 그리고 소셜 로그인 기능이있다. <br>
마지막으로 마이페이지에는 자신이 업로드한 컨텐츠 관리기능과 검색했던 장소 관리 기능이 있다.
<br><br>![appFunc](https://cdn.discordapp.com/attachments/700628529968054284/815246688545407036/unknown.png?raw=true)

---
### 2. 개발 도구<br>
* ####  개발언어
개발언어로는 HTML/CSS 와 Javascript를 사용한다. HTML/CSS는 웹과 모바일 제작을 위한 공식 통합개발환경이고 Javascripts는 객체기반의 스크립트 프로그래밍 언어이다.
* #### 개발환경
개발환경은 Firebase와 Kakaomap API를 사용한다. Firebase는 모바일, 웹 어플리케이션 개발 플랫폼으로 백엔드를 대신해줘서 프론트엔드에 집중할 수 있게 서비스를 지원해주는 서버시스템이다. <br>
그리고 Kakaomap API는 Kakaomap에서 지원하는 API시스템으로 사이트와 모바일 애플리케이션에서 지도를 이용한 서비스를 제작할 수 있도록 다양한 기능을 제공한다.
* #### 개발 라이브러리
개발 라이브러리는 metadata-extractor과 Adobe XD를 사용한다. metadata-extractor는 JPEG, TIFF등 이미지 파일에 각종 메타정보를 저장하는 포맷이다. <br>
Adobe XD는 웹사이트, 모바일 앱 등을 디자인하기 위한 올 인원 UX./UI 솔루션 라이브러리 이다.

---
### 3. 개발 및 설계<br>
#### 1) 프로그래밍 순서
어플리케이션을 Firebase 서버에 등록하고 호스팅을 할 예정이기 때문에 먼저 Firebase프로젝트에 어플리케이션 서버를 추가 하였다. <br>
그리고 호스팅을 위한 커스텀 도메인과 제작하였다. 그 뒤에 Index페이지를 제작하고 그곳에서 기능들을 다룰 수 있는 페이지로 연결한 뒤 각각의 페이지들의 기능들을 구성하였다. <br>
하단에는 페이지들의 구성방법과 기능구현 방법을 설명하였다.
#### 2) 페이지 구성
- ##### Index페이지
헤더의 네비게이션에 Home 과 Map 그리고 로그인 버튼을 제작해서 Home에는 Index페이지의 링크를, Map에는 카카오 MapAPI를 이용한 지도페이지의 링크를 첨부한다. <br>
그리고 간단한 페이지 소개문구와 하단에는 이 웹페이지를 통해서 할 수 있는 기능들을 설명이 되어있고 화면에는 나타나지 않지만 코드에는 하단에 Firebase서버의 스크립트가 등록이 되어있어 현재 자신의 로그인이 되어있는 아이디가 우측 상단에 뜰 수 있도록 구현되어있다.
- ##### 로그인페이지
Index페이지의 로그인 버튼은 누르게 되면 로그인 페이지가 뜨게 링크를 첨부하였는데, 로그인에는 자신의 이메일과 패스워드를 입력하게 되면 회원가입이 되고 그 회원가입한 정보들은 자동으로 Firebase서버에 등록이 되어서 로그인을 할 때 사용할 수 있다. <br>
그리고 로그인 위에는 소셜로그인을 가능한 Facebook과 Google 마크가 그려진 버튼이 있다.
이 버튼을 통해 자신의 Facebook과 Google의 회원정보를 통해 인덱스 페이지에 접속이 가능하다. 
- ##### Map페이지
카카오 MapAPI를 이용해서 만든 Map 페이지에는 MapAPI에서 제공하는 기능들로 구현이 되어있다. <br>
<br>처음에 Map 페이지에 접속하게 되면 자신의 현재위치정보의 검색을 허용하는 팝업이 뜨게 되며 허용 버튼을 누르면 현재위치로 마커가 이동이 된다. <br>
그리고 마커를 클릭하게 되면 인포윈도우가 뜰 수 있게 버튼이벤트를 구현하였고 인포윈도우에는 그 마커의 등록이 되어있는 미디어의 목록버튼과 등록을 할 수 있는 등록 버튼이 구현되어있다. <br>
목록버튼을 클릭하게 되면 팝업창이 뜨게 되고 그 팝업창에는 마커에 등록 되어있는 미디어의 목록이 있다. <br>
그리고 등록버튼을 클릭하면 파일 선택 이벤트 처리가 발생하고 해당 어플리케이션을 실행한 전자기기에서 미디어 파일을 등록할 수 있게 하였다.<br>
또한 등록이 성공적으로 되면 Firebas의 Storage서버와 Realtime Database서버에 자동적으로 등록한 파일과 파일의 정보와 업로드된다.

---
## 3. 결론
---
### 1. 기존의 스트리트 뷰와의 차이점

사람들의 편의성을 추구하기 위해서 기존의 있는 스트리트뷰 시스템들의 불편한 점인 차로만 갈 수 있는 길이거나 현재 시간대의 불확실성한 정보를 가진 스트리트 뷰를 정기적으로 갱신하지 않고 그대로 사용하는 것은 유저들에게 비 확실한 정보를 전달하는 것과 같다. <br>
하지만 이 어플리케이션을 이용하는 유저들은 자신의 발로 가고 싶은 장소에 직접 가지 않고도 그 주변의 풍경이나 장소의 정보들을 직접 제공받을 수 있다. <br>
이러한 점들은 그 지역의 정보제공도 가능할 뿐만 아니라 그 지역을 홍보 할 수 있는 광고효과도 볼 수 있을 것이다.
 
### 2. 한계점 및 향후 연구 방향
개발을 진행함에 따라 몇 가지 아쉬운 점들이 있었다.<br>
첫째는 사진을 업로드 하고나서 그 파일의 진정성에 대한 문제이다.<br>
유저들이 그 장소에 관련이 있는 파일을 업로드 하는데 파일에 대한 정보는 그 파일을 직접 봐야 확인이 가능하다.<br>
혹시 자신이 촬영한 파일이 아니라 다른 파일 일 수도 있기에 이러한 점은 차후 다른 유저들이 파일의 진정성을 판단 할 수 있는 리뷰시스템의 기능추가를 할 것이다.<br>
두 번째로 라이브러리의 사용성이다. 파일을 업로드 할 때 파일의 메타데이터를 추출하는 metadata-extractor는 기존의 파일의 데이터를 추출하는 데에는 문제가 없었지만 그 메타데이터를 어떻게 Map API와 연동시킬지에 대한 점이다.<br>
이러한 점은 개발자의 역량에 따라서 달라지긴 하지만 현재로써는 라이브러리의 적절한 사용은 좀더 연구를 진행해 봐야 알 수 있을 듯하다.<br>
 종합적으로 앞서 말했던 장점들과 개선해야하는 부분을 점진적으로 보완해 간다면 누구든지 서로의 아는 장소를 공유하고 그 장소의 정보를 주고받는 시스템으로 발전 가능성이 있을 것이다.