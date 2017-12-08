# ES6(ES2015(
ECMAScript 는 자바스크립트 프로그래밍 언어를 정의하는 국제 표준의 이름이다.

## ES6 에서 새롭게 등장한 내용들

### Arrow Function 
Arrow function 이 등장하면서 간결하게 함수를 표현하는 것이 가능해졌다. 
````javascript
// ES5
odds = evens.map(function (v) { return v + 1; });
pairs = evens.map(function (v) { return {evne: v, odd: v + 1}; });
nums = evens.map(function (v, i) { return v + i; });

// ES6
odds = evens.map(v => v + 1);
pairs = evens.map(v => ({even: v, odd: v + 1}));
nums = evens.map((v, i) => v + i);
````
Arrow function 의 가장 중요한 특징은 `this`값의 scope 변경이다. 

