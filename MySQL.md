# MYSQL

## 날짜, 시간 계산
참고 : https://ponyozzang.tistory.com/698 

SELECT * FROM addMate WHERE uid='messi' 
	AND TIMEDIFF(CURRENT_TIMESTAMP(), sendTime) < '12:00:00';
위와 같이 작성하면 addMate테이블에서  receiveUser='messi'이고
sendTime이 현재시간보다 12시간이내인 테이블만 보여준다.


## 테이블에 컬럼추가
참고 : https://zetawiki.com/wiki/MySQL_%ED%85%8C%EC%9D%B4%EB%B8%94_%EC%BB%AC%EB%9F%BC_%EC%B6%94%EA%B0%80

1. 맨뒤에 컬럼추가
ALTER TABLE `테이블명` ADD `컬럼명` 자료형

2. 맨앞에 컬럼추가
ALTER TABLE `테이블명` ADD `새컬럼명` 자료형 FIRST

3. 지정 컬럼 다음에 추가
ALTER TABLE `테이블명` ADD `새컬럼명` 자료형 AFTER `앞컬럼명`


## 테이블 제거

DROP TABLE IF EXISTS '테이블명';

## 테이블 추가

예시)
CREATE TABLE matchingCondition
(
	uid varchar(20) NOT NULL,
	bestExercise varchar(10) NOT NULL,
	minAge int DEFAULT 10 NOT NULL,
	maxAge int DEFAULT 100 NOT NULL,
	minDistance int DEFAULT 0 NOT NULL,
	maxDistance int DEFAULT 300 NOT NULL,
	pGender varchar(2) DEFAULT '모두' NOT NULL
);

## 테이블에 특정컬럼 foreign key로 설정

ALTER TABLE `테이블명`
	ADD FOREIGN KEY (`컬럼명`)
	REFERENCES users (`참조할 컬러명`)
	ON UPDATE RESTRICT
	ON DELETE RESTRICT
;

예시)
ALTER TABLE matchingCondition
	ADD FOREIGN KEY (uid)
	REFERENCES users (uid)
	ON UPDATE RESTRICT
	ON DELETE RESTRICT
;




