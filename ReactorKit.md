# ReactorKit
 ViewModel내에서 화면에 표시할 정보들을 struct로 묶어서 가지고 있고, 이 struct를 View로 던져서 필요한 데이터만 꺼내 화면에 적용하는 방식으로 처리할 수 있게 된다.
 상태 컴포넌트가 따로 관리되게끔 설계된 구조, 상태라는 컴포넌트는 추상화 개념이 적용되어있고 컴포넌트 간 결합도가 낮기 때문에 테스트하기에 용이
 MVVM 구현 방식이 가지각색이지만 ReactorKit을 사용함으로써 어느정도 제약을 두어서 정형화된 방식으로 구현할 수 있다.
 
 ![스크린샷 2023-07-17 오후 10 29 43](https://github.com/jimin-hash/Study_RxSwift/assets/62288773/eb770f03-0565-4a03-9648-193a8f4deae9)
 View는 Reactor에게 Action을 전달하고 Reactor는 View에게 State를 전달하는 단방향 구조 

 https://velog.io/@sso0022/iOS-ReactorKit-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0
