# JVM
JVM은 크게 3 개의 주요 하위 시스템을 갖는다.

1. 클래스 로더 
2. 런타임 데이터 영역(Runtime data area)
3. 실행 엔진(Execution engine)

## Classloader
클래스로더는 클래스가 요청될 때 파일로부터 읽어 메모리로 로딩하는 역할을 한다. 클래스로더는 컴파일 타임이 아닌 런타임에 동적으로 클래스를 처음 참조하는 시점에서 
클래스를 로딩하고 링킹, 초기화를 실행한다. 
- Loading
- Linking
- Initialization

## Runtime Data Area
클래스로더가 읽어들인 클래스 파일은 Runtime Data Area 에 적재된다. 
- Method Area
> 메소드 영역은 JVM 내에 하나만 존재하며 공유자원이다. `static` 변수와 같은 클래스 레벨의 데이터가 저장된다. 
- Heap Area
> 모든 객체, 인스턴스 변수나 배열을 저장한다. 메소드 영역과 힙영역은 메모리를 공유하기 때문에 멀티스레드 환경에서 thread-safe 하지 않다. 
- Stack Area
> 스레드는 각각 별도의 런타임 스택을 갖는다. 로컬 변수는 스택 메모리에 생성된다. 
- PC Registers
> 현재 실행중인 명령의 주소를 저장. 각 스레드는 별도의 PC 레지스터를 갑는다. 
- Native Methods Stacks
> 모든 스레드는 별도의 네이티브 메소드 스택을 갖는다. 

## Execution Engine
- Interpreter
- JIT Compiler
- Garbage Collector
