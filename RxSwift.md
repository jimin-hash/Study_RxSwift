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
    이벤트 구독자, 이벤트가 발생하면 처리하는 역할, onNext, onCompleted, onError를 통해 이벤트 전달 받은 것을 확인할 수 있다.
2. Observable -  Observer에게 이벤트(next, completed, error) 전달
3. Operator - create, subscribe, onNext, Sugar API ...
4. Schedulers
    DispatchQueue, OperationQueue를 매핑 
5. Dispose
6. Subject - observable 외부에서 값을 컨트롤할 수 있는 컴포넌트, 데이터를 넣어줄 수도 있고, subscribe 할 수 있는 양방향성이 있다. Observable과 Observer역할을 동시에 수행.
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
AnyObserver -> 단순히 구독에 관련된 메소드(on, onNext, onError, onCompleted)를 구현, 단순히 전달되는 이벤트에 대해서 처리하는 기본적인 Observer
AnyObserver를 뷰에서 발생한 이벤트를 뷰모델로 전달할 때 뷰모델의 setter로 활용합니다.
뷰에서 throttle 등 연산자를 통해 다중 발생을 방지했기 때문에 뷰모델에서 연속적으로 이벤트를 받지 않기 때문에 이렇게 사용합니다

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

### Signal vs driver
공통점
- error를 return 하지 않는다
- main thread에서 작동한다.

차이점
![스크린샷 2023-07-24 오전 12 06 27](https://github.com/jimin-hash/Study_RxSwift/assets/62288773/5b667819-d179-4578-b4e1-2eeb2d5babf4)
- Signal은 새로운 구독자에게 reply 하지 않는다. (Driver처럼 구독하는 순간 초기값이나 최신값을 주지 않음. 구독한 이후에 발행되는 값을 받음)


```
   signal.emit(onNext: { (element) in 
   
   }
   
   driver.drive(onNext: { (element) in 
   
   }
```

#### Public vs Behavior and 
BehaviorRelay/BehaviorSubject는 상태를 나타낸다. 
PublishRelay/PublishSubject는 이벤트를 나타낸다.
따라서 사실상 PublishRelay/PublishSubject는 Signal을 사용하는 것이 맞는 것이다.

그러면 Publish*를 사용할지 Behavior*를 사용할지 구분하고, 적절한 Signal과 Driver를 사용하면 됩니다. 이것의 구분은 마지막 상태 '값'을 저장해야하는지와 초기값이 필요한지 안한지에 대하여 필요하지 않다면, PublishRelay를 사용하였습니다. BehaviorRelay를 무분별하게 사용한다면 초기 값 이벤트를 방출하기 때문에 원하지 않는 플로우가 진행 될 수 있습니다 !


### Clean Architecture
Massive ViewModel이 되지 않고 ViewModel의 부담을 줄여지기 위해서 클린 아키텍쳐가 생겨낫다.
비지니스 로직의 분리와 의존성을 도메인쪽으로 향하게 할 수 있다.
3 Layers
    1. Presentation Layer
    2. Domain Layer
    3. Data Layer
이해가 안가는데 좀 더 공부해서 업데이트 해야 할 듯...
    
---
참고하기 좋은 사이트
https://www.youtube.com/watch?v=iHKBNYMWd5I
https://github.com/TDCIAN/StudyRxSwift/blob/main/Note.md


메모리 순한참조도 주위해야하므로 아래의 글을 읽어보는 것도 좋을듯 하다.
https://iamchiwon.github.io/2018/08/13/closure-mem/
