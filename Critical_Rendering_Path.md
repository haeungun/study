# Critical Rendering Path
브라우저가 페이지의 초기 출력을 위해 실행해야 하는 순서를 Critical Rendering Path(CRP)라고  
사이트의 성능을 향상 시키는 방법을 이해하는 데에 있어 CRP 에 대한 지식은 매우 유용하다.

CRP 는 다음과 같은 6단계로 구성된다.
- DOM 트리 구축
- CSSOM 트리 구축
- Javascript 실행
- 렌더링 츠리 구축
- 레이아웃 생성
- 페인팅
