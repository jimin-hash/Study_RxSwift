# MVVM
## Model + View + View Model

전반적인 흐름
View에 들어온 Event를 View Model에게 노티하고 View Model은 Model을 업데이트 한다.
Model이 변화하면 이는 View Model에 알려지고, View Model과 바인딩되어있는 View가 업데이트 된다.

MVVM 구성하는 방법은 다양함으로 아래의 요소들을 모두 써도 되는것은 아니고 한가지의 예시이다.
개인적으로 Service와 Repository를 합쳐도 될거같은데 이것또한 Massive해질 수 있으므로 프로젝트 확장성을 생각하여 분리하는것도 나쁘지 않을듯하다.

## MVVM 요소
1. 3가지 Data
   - Entity: 서버로부터 받은 원천 데이터
   - Model: 비지니스 로직에서 사용하는 데이터
   - ViewModel: 화면에 보이기 위한 화면 데이터
2. ViewModel
   - 화면에 보이질 데이터를 Model로부터 만들고 가지고 있는다.
   - ViewModel은 View를 알아선 안된다.
3. Service
   - Model 데이터를 이용하여 비즈니스 로직을 담당한다.
4. Repository
   - API를 통해 데이터를 가져오는 로직을 포함한다.
5. View
   - ViewModel을 사용하기만 해야한다.
   - ViewModel의 데이터 변경을 스스로 알아채야하므로 DataBinding이 필요하다. 



---
장점
- View와 View Model의 의존성을 없애고, 각각의 부분을 독립적이게 모듈화하여 개발할 수 있다.
- View Model을 쉽게 테스트할 수 있도록 만들어준다.

단점
- 바인딩...
- Presentation Logic이 늘어나면 View Mdoel이 비대해질 수 있다.
---

참고 사이드
https://velog.io/@ictechgy/MVVM-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4
https://github.com/iamchiwon/mvvm_final/blob/master/README.md
