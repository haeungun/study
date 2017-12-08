# 프로토타입 
자바스크립트에서는 객체지향 개념을 지원하기 위해 프로토타입을 사용한다. 

## 자바스크립트의 객체 생성
`new` 키워드는 ES6 이전에 class 키워드가 없었던 자바스크립트 문법에는 적합하지 않았다. <br>
자바에서는 객체를 `class` 로 정의하지만, 자바스트립트에서는 `function` 으로 정의한다.

````javascript
function Person (name, age) {
  this.name = name;   
  this.age = age;
}

var haeun = new Person("haeun", 25);
console.log(haeun);   // Person {name: "haeun", age: 25}
````

ES6 에서는 `class` 키워드를 새로 만들었다.
````javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

var haeun = new Person("haeun", 25);
console.log(haeun);   // Person {name: "haeun", age: 25}
