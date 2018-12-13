# User Lock
MySQL 에서 사용되는 잠금은 크게 스토리지 엔진 레벨과 MySQL 엔진 레벨 두가지가 있다.  
MySQL 엔진은 MySQL 서버에서 스토리지 엔진을 제외한 나머지 부분이라고 볼 수 있는데, MySQL 엔진 레벨의 잠금은 모든 스토리지 엔진에 영향을 
미치게 되지만, 스토리지 엔진 레벨의 잠금은 스토리지 엔진 간에 서로 영향을 미치지 않는다.

그 중에서도 테이블 데이터 동기화를 위해 사용할 수 있는 `USER LOCK` 에 대한 내용이다. 


## What
MySQL 에서는 `GET_LOCK()` 함수를 이용해서 임의로 잠금을 설정할 수 있다. 이 Lock 은 대상이 테이블이나, 레코드와 같은 데이터베이스 객체가 아니다. 
유저락은 사용자가 지정한 문자열에 대해 Lock 을 걸고, 해제 한다. 
````SQL
SELECT GET_LOCK("mlock", 5);  # "mlock" 이라는 문자열에 대해 Lock 을 걸고, 이미 걸려있다면 5초간 대기

SELECT IS_FREE_LOCK("mlock"); # "mlock" 이라는 문자열에 대해 Lock 이 설정되어있다면 0 을 반환, 사용중이지 않다면 1 반환

SELECT IS_USED_LOCK("mlock"); # "mlock" 에 락이 걸려있다면 Lock 을 건 클라이언트 pid 반환, 사용중이지 않다면 0 반환

SELECT RELEASE_LOCK("mlock"); # "mlock" 이라는 문자열에 대해 획득했던 잠금을 해제, 성공적으로 해제시 1 반환
````

## When
한 대의 데이터베이스에 여러 대의 애플리케이션이 접속해서 서비스를 하고, 
각 클라이언트에서 데이터베이스의 정보를 동기화해서 사용해야 하는 경우에 사용할 수 있다. 

특히 많은 레코드를 한 번에 변경해야 할 때 `SELECT ... FOR UPDATE` 를 사용하면 데드락에 취약해진다는 문제점이 있다. 
이런 경우에 동일한 데이터를 변경하거나 참조하는 프로그램끼리 분류해서 유저락을 걸고 쿼리를 실행하면 간단히 해결할 수 있다. 

## Etc
- 클라이언트A 가 유저락 X 를 획득했다면, 클라이언트A는 유저락을 중복해서 획득하는 것이 가능하다. 
````SQL
SELECT GET_LOCK("X", 1);  # return 1
SELECT GET_LOCK("X", 1);  # return 1 (동일한 클라이언트 일 경우)

SELECT RELEASE_LOCK("X"); # return 1
SELECT RELEASE_LOCK("X"); # return null (동일한 클라이언트에서 여러번 잠금을 획득했더라도 해제는 한번만 수행)
````
