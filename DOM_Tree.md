# DOM Tree

![DOM model](./images/dom-model.png)
> HTML element 는 트리 형태로 저장되며, 최상위에는 document node 가 존재한다.

HTML 태그는 실제로 어떠한 node 형태로 저장된다.
이렇게 저장된 정보를 DOM Tree 라고 한다.
> 세 가지 node 타입
> 1. document node
> 2. element node
> 3. text node

> document. 로 사용할 수 있는 APIs >>
[dom api](https://www.w3schools.com/jsref/dom_obj_document.asp) </br>
> element. 로 사용할 수 있는 APIs >>
[element api](https://www.w3schools.com/jsref/dom_obj_all.asp)

## 웹 프론트엔드 개발
웹 프론드엔트 개발은 **웹 화면을 동적으로 제어**하는 것이다.
웹 화면을 동적으로 제어하는 것은 HTML 구조를 변경, 변형시키거나, 스타일 값을 변경하는 것이다.

DOM 조작은 jQuery 라이브러리를 사용한다고 하여도 상당히 느린 작업이다.</br>
사실 웹 성능을 최적화 하기 위해서는 React, Angular, jQuery 를 사용하는 것보다 
``document.querySelector()`` 를 통해 직접 제어하는 것이 가장 빠르다.

## DOM 의 문제점
요즘 DOM API 는 수많은 플랫폼과 브라우저에서 사용되고 있는데, DOM 에는 치명적인 단점이 하나 있다. 바로 동적 UI 에 최적화 되어 있지 않다는 것 이다. 
물론 HTML 은 자체적으로는 정적이지만, javascript, jQuery 등을 사용하여 수정할 수는 있다. 
그러나 element 의 갯수가 몇백 개, 몇천 개 단위로 많아진다면 생각을 해 볼 필요가 있다. 이렇게 규모가 큰 웹 어플리케이션에서 DOM 에 직접 접근하여 변화를 주다보면, 성능 상의 이슈가 조금씩 발생하기 시작한다. (즉, 느려진다.)
일부는 이를 두고 요즘의 자바스크립트 엔진은 매우 빠르지만, DOM 은 느리다 라고 하는데 사실 DOM 자체는 빠르다.
DOM 자체를 읽고 쓸 때의 성능은 자바스트립트 객체를 처리할 때의 성능과 비교해서 다를게 없다. 
단, 브라우저 단에서 DOM 의 변화가 일어나면, 브라우저가 CSS 를 다시 연산하고, 레이아웃을 구성하고, 웹페이지를 리페인트 하는데, 이 과정에서 시간이 허비되는 것이다.
> 여기서 레이아웃을 새로 구성하면서 계산하는 것을 `reflow` 라고 하고, 색상변경과 같은 레이아웃에 관계없는 것들을 처리하는 것은 `repaint` 라고 한다. 
