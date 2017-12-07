# Webpack
빌드는 서버사이드 뿐 아니라 웹 프론트엔드에서도 필수적이다. javascript 와 css 를 축소하고 단위테스트를 수행하며 
프로젝트에 필요한 자산들(image, font)을 효율적으로 관리할 수 있을 뿐 아니라 패키지화까지 진행할 수 있다. 이러한 
사항들로 인해 결국 해당 프로젝트의 성능, 개발 편의, 개발속도가 향상될 수 있다. 물론 빌드를 실행하지 않고 직접적으로 
시스템 파일을 linking 하여 사용할 수도 있겠지만 performance 와 트레이드오프가 있을 수 있다. <br><br>

웹팩은 프론트엔드 빌드툴 중의 하나 이다. <br>


### Webpack vs Gulp
Webpack 은 Package Bundler 이며, Gulp 는 Task Runer 이다. 

- Package Bundler : 종속성을 가진 애플리케이션 모듈을 정적인 소스로 재생산
- Task Runner : 반복 가능한 특정 작업을 자동화

쉽게 말하면 Task Runner 는 그저 미리 정의해 놓은 어떤 작업을 실행하는 것이도 Package Bundler 는 말 그대로 
어떤 소스들을 하나로 패키지화 하는 것이다. 

### Webpack
Webpack 은 javascript 애플리케이션을 위한 Package Bundler 이고 목적은 종속성을 가진 애플리케이션 모듈을 
정적인 소스들로 생상하는 것이다. 애플리케이션을 처리할 때 필요한 모든 모듈을 종속성 그래프로 반복적으로 작성한 다음 
모든 모듈을 브라우저에서 로드 할 수 있는 하나의 Bundle 로 패키지화 한다. <br>
Webpack 은 다음과 같은 특징을 갖는다.
- Loader 를 통해 javascript, image file, font, css, scss 등과 같은 자산을 하나의 모듈로 취급
- Entry 별로 Bundle 생성 가능
> 자바스크립트가 로딩하는 모듈이 많아질수록 모듈간의 의존성은 증가한다. 의존성 그래프의 시작점을 웹팩에서는 엔트리(entry)라고 한다
- Bundle 에 대한 압축 및 난독화, 소스 맵에 대한 옵션을 제공
- plugin 사용을 통한 사용자 정의 기능 수행
- 비동기 I/O 와 다중 캐시 레벨을 사용하기 때문에 컴파일 속도가 매우 빠르다
- CommonJS(nodejs)와 AMD(requires) 스펙 지원

<br>
Webpack 은 크게 Entry, Output, Loader, Plug-In 4가지로 나눌 수 있다.
<br>
- Entry

````
Webpack 은 모든 애플리케이션에 대한 종속성 그래프를 작성하고 이 그래프의 시작점을 Entry Point 라고 한다. 
이 Entry Point 를 통해 모듈이 어디서부터 시작하는지를 명세하는 애플리케이션을 시작하는 첫번째 파일로 나타낼 수 있다.
````
- Output

````
Output 은 모든 애플리케이션의 자산(resource 또는 assets)을 하나의 Bundle 로 묶었으면 해당 Bundle 을 처리하는 
방법을 명세한다.
````
- Loader

````
Loader 는 사전에 처리할 작업을 나타내며 css, html, jpg, scss 등의 자산을 하나의 모듈로 취급하며 
이러한 파일들을 종속성 그래프에 추가할 때 모듈로 변환한다. 
````
- Plug-In 

````
Plug-In 은 일반적인 Complie 또는 모듈 처리에 필요한 작업 및 사용자 정의 기능을 수행하는데 사용한다.
````

##### Webpack 의 단점
초기 구축에 대한 시간적인 비용이 많이 투자 되며 Learning Curve 가 길다는 점이다. 



### Gulp
Gulp 는 Task Runner 이며, Work Flow 를 자동화 및 향상할 수 있는 도구이다. 
개발 Work Flow 에서 번거로운 작업들이나 시간적인 소모가 많이 들어가는 작업을 자동화하여 쉽게 처리할 수 있다.

- 반복 가능한 작업을 자동화
- javascript 테스트 실행 및 파일 병합
- js, cdd, html 등의 자산 파일을 압축
- Node stream 기반으로 빠른 빌드 속도를 제공
- 작업을 정의하고 실행하는 것이 수월

Gulp 는 Webpack 에 비해 Learning Curve 가 낮기 때문에 사용하기 쉬운 편이며, 코드에 대한 가독성이 좋다. 
대신 Webpack 과 같이 모든 모듈에 대한 종속성 관리가 이루어지지 않기 때문에 규모가 큰 프로젝트에서 패키지와 하기가 
쉽지가 않다. 
