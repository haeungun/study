# Event Delegation Pattern
이벤트 델리게이션 패턴은 굉장히 실용적으로 사용할 수 있는 디자인 패턴이다.<br>
여러 element 에 이벤트를 걸어야 할때, 활발한 이벤트 처리가 필요할 때 구성하면 좋다.

```html
<ul class="menu">
    <li id="home">home</li>
    <li id="introduce">introduce</li>
    <li id="member">member</li>
    <li id="contact">contact</li>
</ul>
```
위와 같은 html 코드가 있고 각 하위 메뉴 마다 이벤트를 설정해줘야 할때, 다음과 같은 방법을 쓸 수 있다.
```javascript
document.querySelector("#home").addEventListener("click", function() {
    console.log(`This is ${this.id}`); // This is home
});
document.querySelector("#introduce").addEventListener("click", function() {
    console.log(`This is ${this.id}`); // This is introduce
});
document.querySelector("#member").addEventListener("click", function() {
    console.log(`This is ${this.id}`); // This is member
});
document.querySelector("#contact").addEventListener("click", function() {
    console.log(`This is ${this.id}`); // This is contact
});
```
일반적으로 간단한 사이트를 개발할 때는 위처런 개별적으로 이벤트 리스너를 할당하여 관리할 수 있다.
하지만 DOM 이 아주 많고, 이벤트가 복잡해진다면 일일히 모든 DOM 에 이벤트 리스너를 할당하는 것은 초기화 단계에서 컴퓨팅 자원을 많이 소모하는 문제가 있을 수 있다. 

따라서 하나의 부모 DOM 을 만들어서 이벤트 처리를 하는 것이 바로 **이벤트 델리게이션 패턴**이다. 

이벤트 델리게이션 패턴의 작동원리는 HTML 에서 이벤트 버블링을 통해 이벤트를 상위 DOM 으로 전달할 수 있다는 데 기인한다. <br>
> [이벤트 버블링과 캡처링](./Event_Capturing_and_Bubbling.md)

### Event Delegation 사용법
이벤트 델리게이션 패턴 사용법은 매우 간단하다. 이벤트를 걸어줄 element 들을 감싸는 하나의 상위 element 에 이벤트를 걸어주고, 이벤트의 타겟을 확인하여 이벤트가 발생하도록 해준다. 
```javascript
document.querySelector("#menu").addEventListener("click", function(e) {
    if (e.target.nodeName === "LI") {
        console.log(`This is ${e.target.id}`);
    }
});
```
훨씬 코드가 간결해졌다. 하나의 리스너가 element 들을 옮겨(?) 다니며 작동하므로, 기존의 모든 element 들에 이벤트 리스너를 달아주어야했던 코드보다 효율적으로 작동된다. 
