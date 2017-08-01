# Event Capturing 과 Bubbling
 

이벤트를 추가할때 사용하는 `addEventListener` 함수에는 3개의 인자를 전달할 수 있다.<br>
첫번째 인자로 전달되는 값은 이벤트 타입("click", "keyup", "mousedown" 등)이고, <br>
두번째 인자는 이벤트가 발생하면 실행될 콜백함수이다.<br>
세번째 인자로 전달되는 true(이벤트 캡처), false(이벤트 버블) 값으로  이벤트 속성을 정할 수 있다. 

```javascript
element.addEventListener(이벤트타입, 핸들러, 캡처여부)
```
> 세번째 인자를 입력하지 않았을 때 전달되는 default 값은 브라우저마다 다르다. 

<br>
HTML 은 기본적으로 트리구조의 DOM 을 표현한다. <br>
따라서 자식 DOM element 에서 발생한 이벤트는 포괄적으로 보면 부모 DOM element 에서도 발생한 것으로 인지할 수 있다. 
이처럼 이벤트가 발생하였을 때, 부모와 자식 DOM 사이에 해달 이벤트를 전파할 때는 
`Capturing >> target >> Bubbling` 의 세 단계를 거치게 된다.


![이벤트 캡처링과 버블링](https://javascript.info/article/bubbling-and-capturing/eventflow.png)

> 캡처링과 버블링은 정반대로 동작한다.


### Capturing
캡처링은 이벤트가 최상위 부모 DOM 부터 가장 하위의 DOM 까지 부모에서부터 전파되는 것이다. 이벤트가 뭔가에 의해 발생하였다면 그 이벤트가 발생한  element 를 포함하는 부모 HTML 로부터 이벤트의 근원지인 자식 element 까지 이벤트를 검사하는 것이다. 이때, 캡처 속성의 이벤트 핸들러가 있다면 실행시키면서 이벤트 element 로 접근한다.
이렇게 이벤트의 근원을 아래로 내려가며 찾아가는 단계가 이벤트 캡처링이다. <br>

### Bubbling 
캡처링이 끝나고 target 에 도달하면 버블이 발생한다. 이벤트 버블링은 이벤트가 발생한 가장 하위의 특정 DOM 에서부터 상위의 부모 DOM 으로 한단계씩 전파된다. 즉, 다시 이벤트 element 로부터 이벤트 element 를 포함하고 있는 부모 element 까지 올라가며 이벤트를 검사하는 것이다. 이때 버블 속성의 이벤트 핸들러가 있다면 실행시킨다. 

### 요약

- 캡처링 단계: 이벤트 객체가 window 객체로부터 대상의 부모까지 순서대로 전달되는 단계</br>
- 대상 단계: 이벤트 객체가 이벤트 대상에 도달하는 단계.이 단계에서 이벤트가 버블링 단계를 거치지 않는 다고 명시하면, 이벤트 객체는 다음 단계를 생략</br>
> [이벤트 전파를 중단하는 네가지 방법](http://programmingsummaries.tistory.com/313)
- 버블링 단계: 이벤트 객체가 대상의 부모로부터 window 객체까지 역순으로 전파된다. 


### 실제 동작

아래 코드를 동작시켜보자.

[캡처링과 버블링 테스트](https://codepen.io/anon/pen/OgYZGP?editors=1011)
```html
<div id="box1" style="width: 300px; height: 200px; background-color: #FFAAAA;">
    <div id="box2" style="width: 200px; height: 150px; background-color: #AAFFAA;">
        <div id="box3" style="width: 100px; height: 100px; background-color: #AAAAFF;"></div>
    </div>
</div>
```
```javascript
document.querySelector("#box1").addEventListener("click", function() {
     console.log(`This is a ${this.id}`);
});
document.querySelector("#box2").addEventListener("click", function() {
     console.log(`This is a ${this.id}`);
});
document.querySelector("#box3").addEventListener("click", function() {
     console.log(`This is a ${this.id}`);
});
```
가장 안쪽의 box3 를 클릭하면 콘솔창에 아래와 같이 출력되는 것을 확인할 수 있다. 
```
"This is a box3"
"This is a box2"
"This is a box1"
```
만약 다음과 같이 box2 의 이벤트 캡처 여부를 true 로 설정한 후에 다시 box3 를 클릭하면,<br> box2 의 이벤트가 캡처링 단계에서 실행되게 된다.
```javascript
document.querySelector("#box2").addEventListener("click", function() {
     console.log(`This is a ${this.id}`);
}, true);
```
```
"This is a box2"  // 캡처링 단계에서 실행
"This is a box3"
"This is a box1"
```

이벤트 캡처링을 통해서 클릭 이벤트를 부모 DOM 에서 완료한 다음, 이벤트 전파를 `event.stopPropagation()` 함수로 막는다면 브라우저상에서 이벤트 대상 단계와 이벤트 버블링 단계의 추가적인 이벤트 전달로 인한 자원 소모를 조금이라도 아낄 수 있다.
