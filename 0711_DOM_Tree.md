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

## 웹 프론트엔드 개발의 본질
웹 프론드엔트 개발의 본질은 **웹 화면을 동적으로 제어**하는 것이다.
웹 화면을 동적으로 제어하는 것은 HTML 구조를 변경, 변형시키거나, 스타일 값을 변경하는 것이다.

DOM 조작은 jQuery 라이브러리를 사용한다고 하여도 상당히 느린 작업이다.</br>
사실 웹 성능을 최적화 하기 위해서는 React, Angular, jQuery 를 사용하는 것보다 
``document.querySelector()`` 를 통해 직접 제어하는 것이 가장 빠르다.
