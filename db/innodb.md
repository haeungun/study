# InnoDB
MySQL 스토리지 엔진 중에 가장 많이 사용되며, MySQL 에서 사용할 수 있는 스토리지 엔진 중에서 거의 유일하게 레코드 기반 Lock 을 제공한다. 
때문에 높은 동시성 처리가 가능하고 안정적이며 성능이 뛰어나다. 

## InnoDB 의 특성
### 프라이머리 키에 의한 클러스터링
InnoDB의 모든 테이블은 기본적으로 프라이머리 키를 기준으로 클러스터링되어 저장된다. 즉, 프라이머리 키 값의 순서대로 디스크에 저장되고, 이로 인해 
프라이머리 키에 의한 레인지 스캔은 상당히 빨리 처리될 수 있다. 

### Lock 이 필요없는 일관된 읽기
InnoDB 스토리지 엔진은 MVCC(Multi Version Consurrency Control)라는 기술을 이용해서 Lock 을 걸지 않고 읽기 작업을 수행한다. 
Lock 을 걸지 않기 때문에 InnoDB 에서 읽기 작업은 다른 트랜잭션이 가지고 있는 Lock 을 기다리지도 않는다. 
`SEROALIZABLE` 격리 수준을 제외하고 읽기 작업이 가능하다.

### 외래키 지원
외래키에 대한 지원은 InnoDB 스토리지 엔진 레벨에서 지원하는 기능으로 MyISAM 이나 MEMORY 테이블에서 사용할 수 없다. 
InnoDB 에서 외래키는 부모 테이블과 자식 테이블 모두 해당 칼럼에 인덱스 생성이 필요하고, 변경시에는 반드시 부모테이블이나 자식 테이블에 데이터가 있는지 
체크하는 작업이 필요하므로 Lock 이 여러 테이블로 전파되고, 데드락이 발생할 때가 많다. 

### 자동 데드락 감지
InnoDB 는 그래프 기반의 데드락 체크 방식을 사용한다. 따라서 데드락이 발생함과 동시에 바로 감지가 되고, 감지된 데드락은 관련 트랜잭션 중에서 ROLLBACK 이ㅣ 
가장 용이한 트랜잭션을 자동적으로 강제 종료시킨다. 
> 가장 용이한 트랜잭션은 ROLLBACK 을 했을 때 복구 작업이 가장 작은 트랜잭션, 즉 레코드를 가장 적게 변경한 트랜잭션)

따라서 데드락 때문에 쿼리가 제한시간에 도달한다거나 슬로우 쿼리로 기록되는 경우가 많지 않다. 

### 자동화된 장애 복구
MySQL 서버가 시작될 때, 완료되지 못한 트랜잭션이나 디스크에 일부만 기록된 데이터 페이지 등에 대한 복구 작업이 자동으로 진행된다. 

### 오라클 아키텍처 적용
InnoDB 스토리지 엔진의 기능은 오라클 DBMS 의 기능과 상당히 비슷한 부분이 많다. 

## 버퍼풀
디스크의 데이터 파일이나 인덱스 정보를 메모리에 캐시해 두는 공간으로 쓰기 작업을 지연시켜서 일괄 작업으로 처리할 수 있는 버퍼 역할도 한다. 
> 일반적인 애플리케리션에서는 `INSERT`, `UPDATE`, `DELETE`와 같이 데이터를 변겨하는 쿼리는 데이터 파일의 이곳저곳에 위치한 레코드를 변경하기 때문에 
랜덤한 디스크 작업을 발생시킨다. 하지만 버퍼 풀이 이런 데이터들을 모아서 처리하게 되면 랜덤한 디스크 작업 횟수를 줄일 수 있다. 

MyISAM 키 캐시가 인덱스의 캐시만을 주로 처리하는 데 비해 InnoDB 의 버퍼풀은 데이터와 인덱스를 모두 캐시하고 쓰기 버퍼링의 역할까지 모두 처리한다. 
InnoDB의 버퍼풀은 많은 백그라운드 작업의 기반이 되는 메모리 공간이기 때문에 InnoDB의 버퍼풀 크기는 신중하게 설정하는 것이 좋다. 
> 물리 메모리 및 운영체제, 각 클라이언트 스레드가 사용할 메모리 모두 충분히 고려해야 함 

InnoDB 버퍼풀은 디스크에 기록되지 않은 변경된 데이터를 가지고 있으며 이러한 데이터를 가지고 있는 페이지를 더티페이지라고 한다. 
이러한 더티 페이지는 InnoDB 에서 주기적으로 또는 어떤 조건이 되면 체크 포인트 이벤트가 발생하는데, 이때 Write 스레드가 필요한 만큼의 
더티 페이지만 디스크로 기록한다. 
> 체크 포인트가 발생한다고 해서 버퍼풀의 모든 더티 페이지를 디스크로 기록하는 것은 아님

## 언두 로그
언두 영역은 `UPDATE` 문장이나 `DELETE` 와 같은 문장으로 데이털르 변경했을 때 변경되지 전의 데이터를 보관하는 곳이다. 
언두 데이터는 크게 두 가지 용도로 사용되는데, 첫 번째 용도가 트랜잭션에서의 롤백 대비용이고, 두 번째 용도는 트랜잭션의 isolation level 을 
유지하면서 높은 동시성을 제공하는 데 사용된다. 

## 인서트 버퍼
인덱스를 업데이트 하는 작업은 랜덤하게 디스크를 읽는 작업이 필요하므로 테이블에 인덱스가 많다면 상당히 많은 자원을 소모하게 된다. 
그래서 InnoDB 는 변경해야할 인덱스 페이지가 버퍼풀에 있으면 바로 업데이트를 수행하지만, 디스크로부터 읽어와서 업데이트를 해야 한다면 
이를 즉시 실행시키지 않고 임시 공간에 저장해주고 바로 사용자에게 결과를 반환하는 형태로 성능을 향상시키게 되는데, 
이때 사용되는 공간이 **인서트 버퍼**이다. 

MySQL 5.5 부터는 작업의 종류별로 인서트 버퍼를 활성화할 수 있으며, 인서트 버터가 비효율적일 때는 인서트 버퍼를 사용하지 않게 설정할 수 있다.

