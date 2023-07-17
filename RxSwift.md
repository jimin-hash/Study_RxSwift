# Study_RxSwift

RxSwift의 필요성
- 비동기 이밴트 처리를 할 수 있다.
- 비동기 이벤트의 흐름을 쉽게 파악하고 작성할 수 있다. (여러 스레드간의 클로저 이벤트 처리아 콜백 지옥에서 코드가 복잡하고 가독성이 떨어질 수 있다.)
- 비동기로 생성되는 데이터도 completion이 아니라 리턴값으로 관리할 수 있다. (코드의 depth가 깊어지지 않는다)
- MVVM과 같이 사용하여 이벤트 중심 프로그램에서 데이터 바인딩을 쉽게 처리할 수 있다.


### Rx의 구성 요소
1. Observable
    subscribed -> next -> completed / error -> disposabled

    combine, merge, zip stream 분리 병합할 수 있다.
2. Operator - create, subscribe, onNext, Sugar API ...
3. Schedulers
    DispatchQueue, OperationQueue를 매핑 
4. Dispose
5. Subject - observable 외부에서 값을 컨트롤할 수 있는 컴포넌트, 데이터를 넣어줄 수도 있고, subscribe 할 수 있는 양방향성이 있다.
- PublishSubject
- BehaviorSubject
- AsyncSubject
- ReplaySubject

Sugar API Examples
- just
- from
- map
- flapMap
- filter
- observeOn(MainScheduler.instance) - down stream
- subscribeOn(ConcurrentDuspatchQueueScheduler(qos: .default)) -> up stream 위치랑은 상관이 없음
- bind(to: ) -> 순환 참조 예방
- catchErrorJustError(" ")
- asDriver(onErrorJustError: "")
- drive(label.rx.text) -> 항상 main thread에서 실행, asDriver화 함께 써주어야함

RxReplay -> 에러가 난다고 하여도 stream이 끊어지지않는 subject


## ReactorKit
 ViewModel내에서 화면에 표시할 정보들을 struct로 묶어서 가지고 있고, 이 struct를 View로 던져서 필요한 데이터만 꺼내 화면에 적용하는 방식으로 처리할 수 있게 된다.
 상태 컴포넌트가 따로 관리되게끔 설계된 구조, 상태라는 컴포넌트는 추상화 개념이 적용되어있고 컴포넌트 간 결합도가 낮기 때문에 테스트하기에 용이
 MVVM 구현 방식이 가지각색이지만 ReactorKit을 사용함으로써 어느정도 제약을 두어서 정형화된 방식으로 구현할 수 있다.
 
 ![스크린샷 2023-07-17 오후 10 29 43](https://github.com/jimin-hash/Study_RxSwift/assets/62288773/eb770f03-0565-4a03-9648-193a8f4deae9)
 View는 Reactor에게 Action을 전달하고 Reactor는 View에게 State를 전달하는 단방향 구조 
 




---
참고하기 좋은 사이트
https://github.com/TDCIAN/StudyRxSwift/blob/main/Note.md


메모리 순한참조도 주위해야하므로 아래의 글을 읽어보는 것도 좋을듯 하다.
https://iamchiwon.github.io/2018/08/13/closure-mem/
