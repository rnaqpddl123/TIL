# MYSQL

## 날짜, 시간 계산
참고한 블로그 : https://ponyozzang.tistory.com/698 

SELECT * FROM addMate WHERE uid='messi' 
	AND TIMEDIFF(CURRENT_TIMESTAMP(), sendTime) < '12:00:00';
위와 같이 작성하면 addMate테이블에서  receiveUser='messi'이고
sendTime이 현재시간보다 12시간이내인 테이블만 보여준다.

