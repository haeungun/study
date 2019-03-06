# SQL 인젝션

`Statement` 대신 `PreparedStatement` 를 쓰는 이유는 `PreparedStatement` 가 쿼리를 파싱한 뒤 파싱 결과를 캐싱해놓고 사용하기 때문에
성능상 이점이 있다는 것도 있지만, **SQL 인젝션**과 관련된 보안상 이슈도 있다. 

다시말해 `PreparedStatement` 를 쓰면 SQL 인젝션 공격이 불가능하다. 

## SQL 인젝션 예시
SQL 인젝션은 악의적인 SQL 문을 실행되게 함으로써 데이터베이스를 비정상적으로 조작하는 인젝션 공격방법이다. 
````SQL
SELECT * FROM users WHERE username='test' and password='password' OR 1=1
````
일반적인 쿼리에 `OR 1=1`을 삽입한 경우이다. 
`1=1` 은 항상 `true` 이기 때문에 위의 쿼리를 이용하면 비밀번호를 입력하지 않아도 로그인이 가능하다. 

## PreparedStatement
`PreparedStatement` 를 사용하면 사용자의 입력이 쿼리로부터 분리된다. 

일반적인 `Statement` 를 사용하여 쿼리를 입력할 경우, 매번 parse 에서 fetch 까지 모든 과정을 수행하지만, `PreparedStatement` 를 사용하는 경우, 
파싱 결과를 저장해두고 필요할 때마다 사용한다. 이렇게 반복적인 재사용을 위해 자주 변경되는 부분은 변수로 선언하고 매번 다른 값을 **바인딩**하여 
사용한다. 

이렇게 바인딩된 데이터는 내부의 인터프리터나 컴파일 언어로 처리하기 때문에(SQL 문법이 아님) 문법적인 의미를 가질 수 없고, 따라서 바인딩 변수에는 SQL 공격 쿼리를 
입력하더라도 의미있는 쿼리로 동작되지 않는다. 
