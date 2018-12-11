# JVM Heap 의 구조
> Hotspot JVM 과 IBM JVM 은 구조에 차이가 있다. 본 내용은 Hotspot JVM 을 기준으로 한다.

## Young Generation
Young Generation 은 Eden 영역과 Survivor 영역으로 구성된다. Minor GC 와 관련된다.

#### Eden
- Object 가 Heap 에 최초로 할당되는 장소
- Eden 영역이 꽉 차게 되면 Object 의 참조 여부를 확인하여 참조가 되어 있는 Live Object 이면 Survivor 영역으로 넘어가면 Eden 영역을 모두 Scavenge 한다. 

#### Survivor
- Eden 영역에서 살아남은 Object 들이 잠시 머무르는 곳
- Survivor0, Survivor1 두 개로 구성되며, Live Object 는 하나의 Survivor 영역만 사용

## Old Generation
Young Generation 에서 Live Object 로 오래 살아남아 어플리케이션에서 특정 횟수 이상 참조되어 기준 age 를 초과한 Object 는 Old Generation 으로 이동한다. 
즉, 비교적 오래 참조가 되어 이용되고 있고 앞으로도 계속 사용될 확률이 높은 Object 를 저장하는 영역이다. 
Old Generation 의 메모리에서 GC 가 발생하는 것을 FUll GC(Major GC) 라고 한다. 

#### Perm
- Perm 영역은 보통 Class 나 Method 의 메타 정보, static 변수와 상수 정보들이 저장되는 공간으로 메타 데이터 저장 영역이라고도 함
- Java8 부터는 Native 영역으로 이동하여 Metaspace 영역으로 변경됨
> 단, 기존 Perm 영역에 존재하던 static object 는 Heap 영역으로 옮겨져서 최대한 GC 의 대상이 될 수 있도록 함
````
Java8 에서 JVM 메모리의 구조적인 개선 사항으로 Perm 영역이 metaspace 영역으로 전환, 기존의 perm 영역은 사라지게 되었다.
metaspace 영역은 Heap 이 아닌 Native 메모리 영역으로 취급된다.
(Heap 영역은 JVM 에 의해 관리되고, Native 메모리는 OS 레벨에서 관리)
````

