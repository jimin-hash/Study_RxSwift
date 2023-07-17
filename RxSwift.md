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
- filter
- observeOn(MainScheduler.instance) - down stream
- subscribeOn(ConcurrentDuspatchQueueScheduler(qos: .default)) -> up stream 위치랑은 상관이 없음
- bind(to: ) -> 순환 참조 예방
- catchErrorJustError(" ")
- asDriver(onErrorJustError: "")
- drive(label.rx.text) -> 항상 main thread에서 실행, asDriver화 함께 써주어야함

RxReplay -> 에러가 난다고 하여도 stream이 끊어지지않는 subject

메모리 순한참조도 주위해야하므로 아래의 글을 읽어보는 것도 좋을듯 하다.
https://iamchiwon.github.io/2018/08/13/closure-mem/

---
참고하기 좋은 사이트
https://github.com/TDCIAN/StudyRxSwift/blob/main/Note.md
