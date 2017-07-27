# 클로저(Closure)
클로저는 독립적인 변수를 참조하는 함수들이다.</br>
다른 말로 하면, 이 함수들은 그들이 생성된 환경을 '기억'한다.
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
