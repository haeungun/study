# Event Capturing 과 Bubbling

HTML 은 기본적으로 트리구조의 DOM 을 표현한다. <br>
따라서 자식 DOM element 에서 발생한 이벤트는 포괄적으로 보면 부모 DOM element 에서도 발생한 것으로 인지할 수 있다. 
이처럼 이벤트가 발생하였을 때, 부모와 자식 DOM 사이에 해달 이벤트를 전파할 때는 
**"Capturing" - target - "Bubbling"** 의 세 단계를 거치게 된다.


![이벤트 캡처링과 버블링](https://javascript.info/article/bubbling-and-capturing/eventflow.png)
