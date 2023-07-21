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

## 데이버 바인딩 방법
### Input / Output 방법
Input: 뷰로 부터 전달받게 되는 데이터  view -> vm
Output: 입력받은 데이터를 변경하여 뷰에 뿌려주는 데이터 vm -> view
- Rx에서 데이터 바인딩을 하는 하나의 방법으로 비동기 처리를 간단하게 작업할 수 있고, 통일된 방식으로 UI 바인딩과 이벤트에도 사용할 수 있는 장점이 있다.
  단점으로는 Rx의 러닝커브가 높음으로 어디까지 Rx로 구현할지 규정 지어야한다.
- 의존성 역전을 위하여 ViewModelType을 이용하는게 좋다

  [SOLID] (https://velog.io/@harinnnnn/OOP-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EB%8C%80-%EC%9B%90%EC%B9%99SOLID-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%97%AD%EC%A0%84-%EC%9B%90%EC%B9%99-DIP#:~:text=%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%97%AD%EC%A0%84%20%EC%9B%90%EC%B9%99%EC%9D%B4%EB%9E%80%20%EA%B0%9D%EC%B2%B4,%EC%BD%94%EB%93%9C%EB%A5%BC%20%EC%9E%91%EC%84%B1%ED%95%A0%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.)

  [인터페이스 주입] (https://ios-daniel-yang.tistory.com/71)
  
### Helper Class 이용 방법
- 간단한 헬퍼 클라스로 구현이 가능하지만 여러개의 연속적인 네트워크 호출이 발생한다면 depth가 깊어지는 문제가 있다.

## ReactorKit
 ViewModel내에서 화면에 표시할 정보들을 struct로 묶어서 가지고 있고, 이 struct를 View로 던져서 필요한 데이터만 꺼내 화면에 적용하는 방식으로 처리할 수 있게 된다.
 상태 컴포넌트가 따로 관리되게끔 설계된 구조, 상태라는 컴포넌트는 추상화 개념이 적용되어있고 컴포넌트 간 결합도가 낮기 때문에 테스트하기에 용이
 MVVM 구현 방식이 가지각색이지만 ReactorKit을 사용함으로써 어느정도 제약을 두어서 정형화된 방식으로 구현할 수 있다.
 
 ![스크린샷 2023-07-17 오후 10 29 43](https://github.com/jimin-hash/Study_RxSwift/assets/62288773/eb770f03-0565-4a03-9648-193a8f4deae9)
 View는 Reactor에게 Action을 전달하고 Reactor는 View에게 State를 전달하는 단방향 구조 

 https://velog.io/@sso0022/iOS-ReactorKit-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0

## Compose



---
참고하기 좋은 사이트
https://www.youtube.com/watch?v=iHKBNYMWd5I
https://github.com/TDCIAN/StudyRxSwift/blob/main/Note.md


메모리 순한참조도 주위해야하므로 아래의 글을 읽어보는 것도 좋을듯 하다.
https://iamchiwon.github.io/2018/08/13/closure-mem/
