# MapReduce
MapReduce 는 병렬처리에 있어서, 사용자가 `Map`과 `Reduce` 두 함수를 이용하여 데이터를 처리하도록 하는 프로그래밍 모델이다.
> MapReduce 모델을 구동하는 프레임워크도 MapReduce 라고 한다.

## Flow
### Input
MapReduce 에서 입력은 `key, value` 쌍으로 이루어진 리스트 형태이다. 
> MapReduce 의 입출력에서는 블록기반분산파일시스템을 이용한다. 
이 시스템에서 파일들은 고정 크기 블록 단위로 관리되며, fault-tolerance 지원을 위해 
기본적으로 복제된 replica 를 갖는다. 

### Mapper
Map 단계에서 입력파일의 각 블록은 `Map()` 함수를 수행하는 Mapper들에게 전달되고, 각 Mapper 는 블록내 key, value 레코드에 `Map()` 함수를 적용한다.
`Map()` 함수는 `key, value` 쌍의 리스트를 읽어서 다시 `new_key, new_value` 의 중간 결과를 반환한다.
> `Map()` 함수의 수행 결과는 각 노드의 로컬 디스크에 기록외어 시스템 장애시 fault-tolerance 를 지원한다.

### Intermediate Result
`Map()` 함수를 통해 반환된 중간 결과들은 반환된 새로운 `new_key` 값을 기준으로 묶인다.
따라서 동일한 키값을 가진 `new_value`들은 그룹화되어 `new_key, new_value[]` 로 구성된다.

### Reduce
각 Reducer 들은 모든 Mapper 가 종료된 시점에, 자신이 처리하도록 할당된 `new_key` 값에 대한 모든 리스트들은 Mapper 로부터 가져온다.
> 이 과정이 마치 카드를 섞는 과정과 유사하여 **셔플링(shuffling)** 이라고 한다.

`Reduce()` 함수는 각 `new_key`에 대해 aggregation 을 수행하여, 최종결과를 반환한다.
