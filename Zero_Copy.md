# Zero Copy

## Without Zero Copy

1. `read system call` 은 유저 모드에서 커널 모드로 context switch 를 발생시키고, </br>
읽은 파일은 디스크 저장소로부터 커널 버퍼로 복사된다. 
2. 데이터는 커널 버퍼에서 유저 버퍼로 복사되고, `read system call` 이 return 된다. </br>
return 되면서 다시 커널 모드에서 유저 모드로 context switch 가 발생하고, 데이터는 유저 버퍼에 저장되어 있는 상태가 된다. 
3. `write system call` 은 유저 모드에서 커널 모드로 context switch 가 발생시킨다.</br>
그리고 데이터를 다시 커널 버퍼로 복사한다. 이때는 이전과 달리 socket 과 관련된 버퍼로 데이터가 들어간다.
4. `write system call` 이 return 되면서 context switch 가 발생하고, </br>
DMA 엔진이 커널 버퍼에서 프로토콜 엔진으로 데이터를 전달하면서 copy 가 발생한다. 

`zero copy` 없이 I/O 작업을 수행하면, read/write 과정에서 각각 두번의 context switch 와 데이터 복사과정이 발생한다.

## Zero Copy
//@TODO
