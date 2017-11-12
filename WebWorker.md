# WebWorker
HTML 페이지에서 스크립트를 실행하면 그 페이지는 스크립트가 완료될 때 까지 응답하지 않게 된다. 
이를 해결 하기 위해서 web worker 를 사용할 수 있다.

Web worker 는 페이지의 퍼포먼스에 영향을 주지 않고 다른 스크립트와는 독립적으로 백그라운드에서 실행되는 javascript 이다.

기존의 웹은 멀티 스레드가 불가능했기 때문에 작업이 끝나기 전까지 UI 가 멈춰버리는 경우가 발생했다. 그러나 Web worker 덕분에 웹을 멀티 스레드 구동이 가능해졌다.
하지만 Web worker 는 워커를 생성시키는 메인 스레드와는 별도로 구동되기 때문에 window 나 document 객체에는 접근할 수 없다. 

### 브라우저의 웹 워커 호환 확인
```javascript
if (window.Worker) {
   // 웹 워커 사용가능
} else {
  // 웹 워커 사용 불가능
}
```

Web worker 는 HTML 페이지에서 Worker 라는 객체를 통해 실행된다.
Worker 객체를 생성 시 워커의 작업을 정의한 javascript 파일을 전달해주어야 한다.
```javascript
const worker = new Worker('worker.js');
```
> 워커를 위한 스크립트는 <body> 태크 하단에 들어가게 할 것을 권장한다. 
   

### Web worker methods
1. postMessage 
2. onmessage : 데이터를 수신
3. terminate : 워커의 작업을 중지
