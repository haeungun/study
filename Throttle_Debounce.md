
https://joshua1988.github.io/web_dev/javascript-interview-3questions/
https://css-tricks.com/debouncing-throttling-explained-examples/

# Throttle and Debounce
쓰로틀링과 디바운싱은 둘 다 연속적으로 실행되는 함수의 횟수를 제어하기 위해서 사용한다는 점에서 비슷하지만, 분명 다르다.</br>
함수의 디바운스 또는 스로틀 버전은 DOM 이벤트에 함수를 붙일 때 특히 유용하다.

## 디바운스
디바운스 기술을 통해 여러 개의 순차적인 호출을 하나의 그룹으로 "그룹화" 할 수 있다.
순차적인 빠른 이벤트는 단일 디바운싱 이벤트로 표현된다. 그러나 이벤트가 큰 격차로 촉발되면 디바운싱은 발생하지 않는다.

## 스로틀링
스로틀링은 디바운싱과는 다르게 연속적으로 이벤트가 계속 발생되어도 일정 시간 간격으로 함수의 실행을 보장한다. 
