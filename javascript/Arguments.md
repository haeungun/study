# Javascript 의 arguments 객체
자바스크립트는 굉장히 자유로운 특성의 언어이다. 
그러다보니 자바스크립트가 가지는 특이한 속성이 몇 가지 있는데, arguments 객체를 활용한 가변 인자 함수도 그 중의 하나이다.</br>
자바스크립트에서는 함수를 호출할 대 함수 형식에 맞춰 인자를 넘기지 않더라도 에러가 발생하지 않는다. 

```javascript
function foo(arg1, arg2) {
    console.log(arg1, arg2);
}

foo(); // undefined undefined
foo(1); // 1 undefined
foo(1,2); // 1 2
foo(1,2,3); // 1 2
```
> 함수 인자보다 적게 함수를 호출 했을 경우 넘겨지지 않은 인자에는 undefined 값이 할당 된다. 
> 인자 개수보다 많게 함수를 호출 했을 경우는 에러가 발생하지 않고 초과 인수를 무시한다. 

함수를 작성할 때, 호출된 인자릐 개수를 확인하고 이에 따라 동작을 다르게 구현을 하고 싶을 때, argumemts 객체를 사용할 수 있다.</br>
자바스크립트에서는 함수를 호출할 때 인수들과 함께 암묵적으로 arguments 객체가 함수 내부로 전달 된다. 
> arguments 객체 : 함수를 호출 할 때 넘긴 인자들이 배열 형태로 저장된 유사 배열 객체

### 가변인자 함수
매개 변수의 갯수가 변할 수 있는 함수

```javascript
function foo() {
   // 매개 변수의 갯수를 가져온다.
   const length = arguments.length;
   
   // 인자 갯수 조건에 따라 다른 동작을 한다. 
   if (length === 0) {
       // 인자 갯수가 0 일때, 
   } else if (length === 1) {
       // 인자 갯수가 1 일때, 
   } else {
       // 인자 갯수가 0 이나 1 이 아닐때, 
   }
}
```

### 파라미터 갯수가 동적인 함수
처음부터 함수에 들어올 인자의 갯수가 몇 개 인지 알 수 없는 경우가 있을 수 있다. 혹은 일일히 다 정의해주기 힘들거나 그럴 필요가 없는 경우도 있다.
```javascript
function sum() {
    var sum = 0;
    for (var i in arguments) {
        sum += arguments[i];
    }
    return sum;
}
```

### ES6/2015
ES6 에서는 다음과 같은 방법으로 가변 인자를 처리할 수 있다. 
```javascript
function printItems(...items) {
    items.forEach((item) => {
        console.log(item);
    })
}
```

```javascript
function printItems(category, ...items) {
    items.forEach((item) => {
        console.log(item);
    })
}

printItems("NAME") // undefined
printItems("NAME", "haeun") // haeun
printItems("NAME", "haeun", "donghee") // haeun, donghee
```

