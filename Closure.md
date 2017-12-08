# 클로저(Closure)
````
특정 함수가 참조하는 변수들이 선언된 렉시컬 스코프는 계속 유지되는 제, 그 함수와 스코프를 묶어서 클로저라고 한다.

클로저 = 함수 + 함수를 둘러싼 환경(Lexical environment)
````
클로저는 독립적인 변수를 참조하는 함수들이다. 다른 말로 하면, 이 함수들은 그들이 생성된 환경을 '기억'한다.<br>

자바스크립트에서 클로저는 함수가 생성되는 시점에 생성된다. 
> 클로저는 이너함수가 스코프 밖에 있는 변수에 접근하는 것</br>
> [MDN Closure](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

### 스코핑
```javascript
function init() {
   const name = "Haeun";
   function showName() {
      alert(name);
   }
   showName();
}

init();
```
`init()` 은 지역 변수 `name` 과 함수 `showName()` 을 생성한다.</br>
`showName()` 은 `init()` 안에 정의된 내부 함수이며 해당 함수 본문에서만 사용할 수 있다.</br>
`showName()` 자신은 지역변수를 가지지 않지만, 외부 함수의 변수들에 접근할 권한을 가지고 있다.</br>
그래서 **부모 함수에서 선언된 변수 `name` 을 사용할 수 있다.**

### 클로저
```javascript
function myFunction() {
   const name = "Haeun";
   function showName() {
      alert(name);
   }
   return showName;
}

const foo = myFunction();
foo();
```

### 클로저의 실제 활용 예
클로저를 가장 많이 사용하는 것은 자바스크립트 라이브러리나 모듈에서 private 으로 나의 변수를 보호하고 싶을 때나 static 변수를 이용하고 싶을 때이다. 
그리고 일상적으로 콜백 함수에 추가적인 값들을 넘겨줘서 활용하거나 처음에 초기화 했던 값을 계속 유지하고 싶을 때도 유용하게 사용할 수 있다. 
> 클로저는 보통 정보 은닉을 하거나, 함수 팩토리를 생성할 때 사용

``` html
<div id="wrapper">
   <button data-cd="1">Add div</button>
   <button data-cd="2">Add img</button>
   <button data-cd="delete">Clear</button>
   
   Adding below ... </br>
   
   <div id="appendDiv"></div>
</div>
<script>
(function() {
   // 클로저 활용 1: appendDiv 를 미리 가져와서 한번의 초기화만으로 이후의 함수들이 계속 접근할 수 있게 해준다. 
   var appendDiv = document.getElementById("appendDiv");
   document.getElementById("wrapper").addEventListener("click", append);
   
   function append(e) {
      var target = e.target || e.srcElement || event.srcElement;
      var callbackFunction = callback[target.getAttribute("data-cb")];
      appendDiv.appendChild(callbackFunction);
      
      var callback = {
         "1": (function() {
                 // 클로저 활용 2 : 콜백함수에서 추가할 HTML element 를 만들어주는 변수 활용
                 var div = document.createElement("div");
                 div.innerHTML = "Adding new div";
                 return function() {
                    return div.cloneNode(true);
                 }
              }()),
         "2": (function() {
                 // 클로저 활용 3 : 콜백함수에서 추가할 HTML element 를 만들어주는 변수 활용
                 var img = document.createElement("img");
                 img.src = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsXUtxNPqBET8CdLgZ-ByWd6pa9AQioyOl-Drf2G7dhaC65irp6Q";
                 return function() {
                    return div.cloneNode(true);
                 }
              }()),
         "delete": (function() {
                 appendDiv.innerHTML = "":
                 return document.createTextNode("Cleared");
              }())
      }
   }
})
</script>
```
위와 같이 최초에 초기화된 고정적인 값이나 변수를 자주 이용하는 경우, 클로저를 통해서 최초에 초기화해두고 콜백 함수에서 지속해서 참조하는 것이 퍼포먼스상 유리하게 작용할 수 있고, 객체의 속성이 자유로운 자바스크립트에서는 이러한 디자인 패턴이 아주 효율적이다. </br>
특히 같은 DOM element 를 적극적으로 이용할 때는 이러한 방법으로 사용하면 아주 좋다.



### 클로저의 단점

- 클로저는 메모리를 소모한다.
> `setTimeout()` 이나 이벤트에 대한 콜백 함수 등으로 등록했던 함수들이 메모리에 계속 남아있게 되면, 해당하는 클로저도 같이 메모리에 계속 남아있게 된다.
> 클로저를 생성할 때는 하나의 커다란 클로저를 생성하기보다는 각 변수나 함수들의 생명주기를 분석한 다음 효율적으로 나누면 좋다.
> 예를 들어, 지속 가능한 서로 다른 변수들이 있으면 해당 변수들은 묶어서 클로저를 별도로 관리하는 것이 메모리를 효율적으로 사용하는 방법이다. 

- 스코프 생성과 이후 변수 조회에 따른 퍼포먼스 손해가 있다. 
> 스코프 체인에 추가되는 스코프 때문에 퍼포먼스의 손해가 있을 수 있다. 
> 클로저의 하위에 있는 함수에서 상위에 있는 변수에 접근하고자 할 때 클로저로 생성한 스코프를 탐색해야 한다는 문제가 있다. 
> 스코프 체인이 하나나 두 개이면 큰 차이는 없지만, 과하게 사용하면 이러한 상황이 쌓여서 퍼포먼스에 영향을 미친다. 

### 반복문 클로저

*정수 값을 갖는 리스트를 반복문으로 접근하여 해당 요소마다 3초를 지연시키고 값을 출력하라*
```javascript
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
   setTimeout(function() {
      console.log("index number ::", i);
   }, 3000);
}
```


> 위 코드를 실행하면 각 인덱스에서 3초씩 지연된 후 `0,1,2,3` 이 찍히는 것이 아니라 모두 4가 찍힌다.</br>
> 문제의 원인은 `setTimeout`의 스코프와 `for` 반복문 안의 스코프가 다르기 때문이다.
> 이를 해결하는 방법 중 2 가지는 아래와 같다.

```javascript
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
   // i 값 을 setTime 함수 안에 전달하여 각 함수 호출마다 올바른 값에 접근하게 한다.
   setTimeout(function(i_local)) {
      return function() {
         console.log("index number ::", i_local);
      }
   }(i), 3000);
}
```
```javascript
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
   // ES6 의 let 은 함수가 호출 될 때 마다 인덱스 i 값이 바인딩 되는 새로운 바인딩 기법을 사용한다.
   setTimeout(function() {
      console.log("index number ::", i);
   }, 3000);
}
```
