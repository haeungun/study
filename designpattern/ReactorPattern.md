# Reactor Pattern
[The reactor design pattern is an event handling pattern for handling service requests delivered concurrently 
to a service handler by one or more inputs. The service handler then demultiplexes the incoming requests and 
dispatches them synchronously to the associated request handlers.](https://en.wikipedia.org/wiki/Reactor_pattern#Implementations)
> 하나 이상의 입력이 서비스 핸들러에 동시에 전달되는 서비스 요청을 처리하기 위한 이벤트 처리 패턴 

Reactor 는 이벤트가 발생하기를 기다리고, 이벤트가 발생하면 Reactor 는 이벤트를 적절한 이벤트 핸들러에 넘겨주는 역할을 한다.
동작방식은 크게 다섯 과정을 거친다. 
- Initiate: 이벤트에 반응하는 reactor 를 만들고 reactor 에 이벤트를 처리할 이벤트 핸들러를 등록
- Receive: reactor 는 이벤트 발생을 수신
- **Demultiplex**: 이벤트를 처리할 이벤트 핸들러 단위로 분할
- Dispatch: 분할된 이벤트를 해당 이벤트 핸들러에 발송
- Process event: 이벤트핸들러에서 이벤트 처리
