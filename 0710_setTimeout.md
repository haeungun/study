# Javascript 에서 setTimeout 의 활용

```javascript
setTimeout(function() {
  console.log("Hello!!");
}, 3000);
```
```javascript
// bad
// setTimeout 의 첫번째 인자로 문자열이 넘어가면 돌아가기는 하지만 느리다. 
setTimeout(console.log("Hello!!"), 3000);
```
## 1. 시간 간격을 둔 함수 실행

기본적으로 특정 함수나 기능을 바로 실행시키지 않고, 약간의 시간 간격을 두고 실행되게 하고 싶을 경우 사용



## 2. javascript 의 비동기 실행을 마치 동기적인 것처럼 실행

setTimeout 에 의해 호출되는 함수는 자체 호출 스택을 가지므로 
setTimeout(function(){}, n) 이 실행되면 현재 스택은 비워지고 n ms 후에 실행된다.timeout 을 0 으로 설정한다면(지연시킬 시간이 없다면), 
setTimeout 은 스택을 푸는 즉시 실행되게 된다.</br>
이는 싱글스레드에서 비동기 코드를 실행시키는 데에 유용한 트릭이다.</br>
`setTimeout(callback, 0)`을 통해 함수를 실행시키면 알고리즘이 non-blocking, asynchronous 지만, 실행은 효율적인 선형 순서로 차단된다.  
> 사실 해결은 되지만, 좋은 방법은 아니다.
> 내가 생각한대로 비동기 함수 진행이 안될수도 ... 


## 3. 함수의 호출 스택 비우기

setTimeout 은 호출스택을 비운다.</br>
(setTimeout 은 실행 큐에서 함수를 제거하고 javascript 가 현재 실행 큐에서 완료된 후에 실행되기 때문)

즉 timeout 을 0 으로 설정하고 recursive 함수를 호출할 경우 호출 스택이 쌓이는 것을 막을 수 있다. 
