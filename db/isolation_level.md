# Isolation Level
트랜잭션에서 isolation level 은 동시에 여러 트랜잭션이 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터에 대해 
어느 정도 허용할 것인지를 결정한다. 

isolation level 은 크게 4 가지로 나뉜다.
- `READ UNCOMMITED`
- `READ COMMITED`
- `REPEATABLE READ`
- `SERIALIZABLE`

위 isolation 레벨은 순서대로 뒤로 갈수록 각 트랜잭션 간의 데이터 격리 레벨 정도가 높아지며, 동시에 동시성도 저하 된다. 
> 약간의 차이는 있어도 사실상 `SERIALIZABLE` 수준이 아니라면 크게 성능의 개선이나 저하는 발생하지 않는다.

`READ UNCOMMITED` 는 일반적인 데이터베이스에서는 거의 사용되지 않고, `SERIALIZABLE` 도 동시성이 중요한 DB 에서는 거의 사용되지 않는다.


발생가능한 부정합|DIRTY READ|NON-REPEATABLE|PHANTOM READ
-------------|-------------|-------------|-------------
READ UNCOMMITED|O|O|O
READ COMMITED|X|O|O
REPEATABLE READ|X|X|O(InnoDB 는 X)
SERIALIZABLE|X|X|X

## READ UNCOMMITED
`READ UNCOMMITED` 수준에서는 각 트랜잭션에서의 변경 내용이 `COMMIT` 이나 `ROLLBACK` 여부에 상관없이 다른 트랜잭션에서 보여진다. 

어떤 트랜잭션에서 처리한 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있게 되는 현상은 **더티 리드(Dirty Read)** 라 한다. 
더티 리드를 유발하는 `READ UNCOMMITED` 는 정합성에 문제가 많고, 개발자와 사용자를 혼란스럽게 할 수 있으므로 최소 `READ UNCOMMITED` 이상의 
isolation level 을 사용하는 것이 좋다. 

## READ COMMITED
`READ COMMITED` 은 오라클 DBMS 에서 기본적으로 사용되는 격리 수준이며, 
이 레벨에서는 어떤 트랜잭션에서 데이터를 변경했더라고, `COMMIT` 이 완료된 데이터만 다른 트랜잭션에서 조회할 수 있기 때문에 더티리드가 발생하지 않는다.  

트랜잭션A가  어떤 레코드를 수정할 때, 새로운 값은 해당 테이블에 즉시 기록이 되고, 수정되기 이전의 값은 UNDO 영역에 기록된다. 
따라서 이때 다른 트랜잭션이 해당 레코드를 조회하게 되면, UNDO 영역에 백업된 레코드에서 가져온 수정 이전의 값을 읽게 되고, 최종적으로 
트랜잭션A가 변경된 내용을 커밋하게 되면 다른 트랜잭션에서도 백업된 UNDO 레코드가 아니라 변경된 새로운 레코드를 참조할 수 있게 된다. 

`READ COMMITED` 수준에서 더티 리드를 방지할 수는 있지만, **NON_REPEATABLE READ** 라는 부정합 문제가 존재한다. **REPEATABLE READ**는 
하나의 트랜잭션 내에서 똑같은 `SELECT` 쿼리를 실행했을 때는 항상 같은 결과를 가져와야 한다는 정합성이다. 
그렇지만, `READ COMMITED` 수준에서는 트랜잭션A가 값을 변경하고 커밋을 한다면 해당 데이터에 대해 조회하는 트랜잭션B에서는 
트랜잭션A의 커밋 전 후에 조회되는 데이터가 달라질 수 있다는 것이다. 이런 상황은 하나의 트랜잭션에서 동일한 데이터를 여러 번 
읽고 변경하는 작업이 금전적인 처리와 연결되면 문제가 될 수 있다. 

## REPEATABLE READ
`READ COMMITED` 격리 수준에서는 트랜잭션 내에서 실행되는 `SELECT` 문장과 트랜잭션 외부에서 실행되는 `SELECT` 문장의 차이가 별로 없다. 
하지만 `REPEATABLE READ` 수준에서는 기본적으로 `SELECT` 쿼리 문장도 트랜잭션 범위 내에서만 작동하게 된다. 
`REPEATABLE READ` 는 MySQL의 InnoDB 스토리지 엔진에서 기본적으로 사용되는 isolation level 이다. 

## SERIALIZABLE
