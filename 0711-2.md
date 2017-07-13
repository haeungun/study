# DOM Manipulation Library

DOM 트리를 조작할 수 있는 방법은 굉장히 다양하다.</br>
여러 방법 중에서 몇 가지를 골라 DOM 을 조작하는 방법을 비교해보았다.

## jQuery 와 DOM API
jQuery 는 javascript DOM API 위에서 만들어진 응용 라이브러리이다.</br>
가능하면 jQuery 를 쓰지 않고 직접 DOM 을 조작하는 것이 성능 개선에 더 도움이 된다. 

### Creating Elements
새로운 HTML element 를 만들고자 할때 다음과 같은 문법을 쓸 수 있다. 
##### jQuery
```javascript
  $("<div></div>");
```
##### DOM API
```javascript
  document.creatElement("div");
```

### Inserting Elements Before & After
```html
<div id="box1"></div>
<div id="box2"></div>
<div id="box3"></div>
```
위와 같은 HTML element 에 아래처럼 새로운 element 를 삽입할 수 있다.
```html
<div id="box1"></div>
<div id="box2"></div>
<div id="box2.5"></div>
<div id="box3"></div>
```

##### jQuery
```javascript
  $('#2').after('<div id="2.5"></div>');
```
##### DOM API
```javascript
  document.getElementById('2')
    .insertAdjacentHTML('afterend', '<div id="2.5"></div>');
```
> insertAdjacentHTML 함수에 첫번째 인자로 전달될 수 있는 position 에는
> "beforebegin"(element 앞에), "afterbegin"(element 안 가장 첫번째), 
> "beforeend"(element 안 가장 마지막), "afterend"(element 뒤에) 가 있다.

### Inserting Elements As Children
```html
<div id="parent">
    <div id="oldChild"></div>
</div>
```
새로운 element 를 부모 element 안에 자식 element 로 생성할 수도 있다.
```html
<div id="parent">
    <div id="newChild"></div>
    <div id="oldChild"></div>
</div>
```

##### jQuery
```javascript
  $('parent').prepend('<div id="newChild"></div>');
```
> 마지막에 삽입하고 싶을 경우 ``$().append`` 를 쓸 수 있다.
##### DOM API
```javascript
  document.getElementById('2')
    .insertAdjacentHTML('afterbegin', '<div id="newChild"></div>');
```
> 마지막에 삽입하고 싶을 경우 ``insertAdjacentHTML()`` 의 첫번째 인자로 ``beforeend`` 를 준다.

### Removing Elements
Html element 를 삭제할 수 있는 방법은 다음과 같다.
##### jQuery
```javascript
  $("#ele").remove();
```
##### DOM API
```javascript
  document.getElementById("ele").parentNode
    .removeChild(document.getElementById("ele"));
```


### Removing Elements
Html element 에 class 를 추가하거나 삭제하기 위해 다음과 같은 방법을 쓸 수 있다.
##### jQuery
```javascript
  // class 추가
  $("#ele").addClass("myClass");
  // class 삭제
  $("#ele").removeClass("myClass");
```
##### DOM API
```javascript
  // class 추가
  // All modern browsers, with the exception of IE9
  document.getElementById("ele").classList.add("myClass");
  // All browsers
  document.getElementById("ele").classList += "myClass";
  
  // class 삭제
  // All modern browsers, with the exception of IE9
  document.getElementById("ele").classList.remove("myClass");
  // All browsers
  document.getElementById("ele").className = 
    document.getElementById("ele").className.replace(/^myClass$/,'');
```


