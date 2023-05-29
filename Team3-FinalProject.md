# 운동 mate 프로젝트

## MySQL alias(별칭)한 데이터 가져오기
현재 내가 사용하는 방식의 mybatis에서는 mysql에서 alias한 데이터 자료들을 가져올때
dao에 적는 sql문에서 as를 생략해서 쓰면 alias한 데이터들을 가져올수 있다.
이때 entity에서는 당연히 alias한 데이터의 정보도 적어줘야한다.

## 알고리즘 작성
### 거리계산
1. 유저간 거리를 계산하기 위해서 회원가입시 입력한 주소의 위도와 경도를 데이터베이스에 저장
2. 함수(MapUtill)를 만들어서 거리를 계산하였음

### 일치하는 관심운동
1. 회원가입시 관심운동을 3~5개 저장하게 하여서 이것을 숫자로 저장함
2. 숫자로 저장한걸 2진수로 풀어서 앤드와이즈 연산자(&)를 이용하여 운동목록이 하나라도 겹치는 데이터들을 뽑아냄
3. 1의 위치에 따라서 어떤 운동인지 보여주고 1이 몇개인지 보여준다.

### score로 리스트 만들기
1. 거리,관심운동개수, 평점에 각각 가중치를 넣어서 score를 매긴다.
2. score를 기준으로 내림차순 정렬을해서 매칭 리스트에 보여준다.

### 필터링
1. 매칭조건에서 현재 관심운동, 나이, 거리, 성별을 입력한다.
2. 매칭된 유저가 조건에서 벗어날경우 목록에서 제거한다.


## 스프링 다룰때 error발생한 부분들

1. 함수(Utill)을 만들어서 사용할때도 위에 @Service를 붙여야함
2. 데이터 길이를 맞추기 위해서 String.format를 활용해 공백을 맞춰서 길이를 맞춰줌
3. List 정렬할때 내가 만든 객체로 정렬할경우
	Collections.sort(matchingList, (m1, m2) -> (int)(m2.getScore()- m1.getScore()));
	위와 같이 만들어준다.

## 네이버 클라우드 연결
1. mavenbuild를 하면 target폴더에 project-0.0.1-SNAPSHOT.war파일이 생성되는데 이 파일을 mobXterm에 옮긴다.
2. .war파일이 있는곳에서 java –jar project-0.0.1-SNAPSHOT.war를 입력하면 네이버 클라우드에서 서버를 열어준다.
3, mobaXterm을 종료한 뒤에도 서버를 유지하고 싶다면 nohup java –jar project-0.0.1-SNAPSHOT.war > project.log 2>&1 & 입력하면
mobaXterm을 종료한뒤에도 서버가 유지된다.


## 스프링에서 데이터 보낸것들을 자바스크립트에서 다룰때 나온 문제점들
1. 스크립트에서 ${data}는  document.getElementById(${data})로 인식한다.
2. ${List}로 보낼경우에는 for문 으로 돌리면 각각을  document.getElementById(${data})로 인식해준다.
3. List내에서 E-sports같은경우는 '-' 때문에 인식을 제대로 못해준다. E와 sports를 각각 인식하는 문제가 생겼다.
	a. hidden으로 value값으로 List를 따로 입력해줘서 그 value값을 가져와서 array형태로 다시 만들어준다.
	b. var likeExerList = document.getElementById('likeExerList').value.replace(/\s/g,'').replace(/\[/g,'').replace(/\]/g,''); 
	c. let ExerList = likeExerList.split(',');	
	d. 그후에 각각을 for문으로 결과를 처리하면된다.


## 스프링 에러페이지 만들기
1. application.properties에서 
	server.error.include-exception = true
	server.error.path=/error
	작성해서 에러페이지로 이동하는 페이지와 exception도 error페이지로 보내게 처리해줘야함
2. 내가 실수로 @GetMapping로 경로를 설정해 줬었는데 post에서 exception문제가 일어나서 exception처리를 못해줬었다.
	@RequestMapping로 경로를 설정해서 get,post방식 모두에서 에러가 났을때 에러페이로 이동할수있게 경로설정을 해줘야한다.

## 스프링 필터 페이지 만들기
1. 1:1 매칭 이나 글작성같은경우 로그인이 필요한데 이것들을 위한 필터링 페이지 필요

## 속도 향상
###현재 스프링 프레임워크에서 처리한 필터링을 MySQL에서 처리하게 하기

###기존 방식: DB에서 데이터들을 전부 가져와서 스프링 내에서 if문을 걸어서 필터링을 하였음

### 개선 방식: DB에서 조건을 걸어서 필터링한 데이터들을 가져는 방식으로 변경  
필터링 목록 : 성별, 나이, 좋아하는 운동목록 
	개선전 : 데이터 23개기준 34 ~ 31밀리초
	개선후 : 데이터 23개기준 11 ~ 10밀리초
	걸리는 시간이 1/3이하로 줄어들음


### 아이디,닉네임 중복체크
자바스크립트에서 id가 "uid" a태그의 값을 가져오고 싶다면
document.getElementById('uid').text; 또는 $('#uid').text();로 텍스트 값을 가져올수있다.
해당 텍스트 값을 변경하고싶다면
document.getElementById('uid').innerText = '변경할 내용'; 또는
 $('#uid').text('변경할 내용') ; 으로 변경가능하다


## 로그인 이후에 이전페이지로 돌아가는법 
 String prevPage = (String) req.getHeader("REFERER");
 prevPage에 이전 페이지의 주소가 저장된다.
 다만 이 방법은 주소를 직접입력하거나 다른 컨트롤러에서 redirect방법으로 들어왔을경우에는
주소를 받을수가 없다.
 이 두가지 예외의 경우에는 다른방식으로 처리를 따로 해주어야한다.
 해당 프로젝트에서는 prevPage가 null일때는 대문으로 가게하였고 
redirect방식으로 왔을때는 특정 파라미터값을 넘겨주어서 파라미터값에 따라 페이지이동을 시켯다.


