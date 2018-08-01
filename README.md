# RxSwift




Reactive Programming과 Rx
비동기 데이터 흐름
Reactive Programming을 한줄로 설명하자면 다음과 같습니다. 
(출처: https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

Reactive programming is prog

ramming with asynchronous data streams.
즉, 비동기적 데이터 흐름을 처리하는 프로그래밍이라는 것입니다. 이러한 관점에서 Reactive Programming은 Asynchronous Dataflow Programming이라고도 불리웁니다. 아래에서 더 자세히 다루겠지만, Reactive Programming의 핵심은 모든 것을 비동기적인 데이터의 Stream으로 간주하고, Observer 디자인 패턴을 활용해서 이러한 비동기 이벤트를 처리하는 것에 있습니다.

실시간화
위는 Reactive Programming을 구현하는 개발자의 입장에서 본 정의에 가깝습니다. 
사용자 입장에서 보자면 Reactive Programming이란 실시간으로 반응을 하는 프로그래밍을 말합니다. 예를 들면, 네이버 검색창에 단어를 하나씩 입력할 때마다 관련 검색어들이 자동완성으로 바로바로 제시되는 것이나, 페이스북 포스트에 '좋아요' 버튼을 누르면 해당 포스트를 보고 있는 다른 유저의 '좋아요' 카운트가 페이지 새로 고침이 없어도 실시간으로 올라가는 것을 말합니다.이는 '명령형' 프로그래밍을 뜻하는 Imperative Programming과 대비되는 개념입니다. 예를 들면, a = b + c라는 함수가 있을 때 명령형 프로그래밍에서는 a는 b + c 연산의 결과물이 되며, 차후 b와 c 값이 변하여도 재연산 명령이 새로 들어오지 않는 한 a 값은 첫번째 연산의 결과값으로 유지되게 됩니다. 하지만 Reactive Programming에서는 b와 c 값이 변할 때마다 바로바로 재연산이 수행되어 a값이 변하게 됩니다.
이런 관점에서 본다면 Reactive Programming은 Data Binding을 통해 Model과 View, Input과 Output이 서로서로 업데이트 상황을 실시간으로 공유받고 업데이트하는 프로그래밍이라고 할 수 있겠습니다.

Async와 Observer
위에서 언급하였듯이, Reactive Programming의 핵심은 비동기(Async) 이벤트와 Observer 디자인 패턴입니다. 
먼저 비동기(Asynchronous) 이벤트란 프로그램에서 다양한 작업들이 동시에 수행되는 중에 어떤 작업이 완료되는 것과 같은 이벤트를 말합니다. 앱에서 발생하는 비동기적인 이벤트는 정말 다양합니다. 즉, 유저가 앱에 로그인하는 행위, 로그인해서 유저가 올린 이미지를 조회하는 행위, 모바일 카메라를 작동시키는 행위, 다른 사람이 올린 이미지를 조회하는 행위, 다른 사람에게 글과 그림을 공유하는 행위 등이 일어나 앱이 서버와 교신할 때마다 알게 모르게 비동기적인 이벤트가 발생하게 됩니다. 
앱과 서버는 서로에게 끊임없이 정보를 요구하고 주고 받게 되며, 이 과정에서 유저의 인터페이스를 방해하지 않고 뒷단에서 데이터를 가져오는 작업이 바로 비동기적인 작업입니다. 그리고 요청한 데이터를 모두 가져오게 되서 앱이나 서버가 '니가 요청한 정보 다 가져왔어. 이제 그거 잘 활용해봐'라고 통보하는 것은 비동기적인 이벤트가 발생한 것이라고 볼 수도 있습니다.
Data Stream은 말 그대로 데이터의 물줄기 흐름입니다. Async 한 Data Stream은 아래처럼 표현할 수 있습니다. 참고로 Data Stream이 값을 생성해서 토해내는 것은 Emit(분출)이라고도 표현합니다.

Reactive Programming을 위해 유저가 입력할 때마다 즉각적으로 반응하려면, 프로그램이 지속적으로 값을 관찰(Observe)해야하고, 값에 변화가 일어날 때마다 특정 연산이 수행되어야 합니다. 이러한 관찰 패턴을 Observer 또는 Observation 디자인 패턴이라고 하며, 비동기 이벤트를 처리하는 Reactive Programming의 근간이 됩니다.
관찰(observe)한다는 것은 계속 듣고(listen) 있는다는 것과 같은 말입니다. Reactive Programming 상에서는 해당 스트림에 가입(subscribe)한 것이라고 표현하기도 합니다. 마치 라디오를 듣기 위해 주파수를 맞추거나, 웹사이트의 소식을 알기 위해 이메일 스트림에 가입하는 것과 비슷한 개념입니다.

Callback 보다 Observer
비동기 이벤트를 처리하기 위해 Observation 대신 Callback을 쓸 수도 있습니다. Callback은 함수의 인자에 들어간 함수로서, 본 함수의 작업이 완료된 다음 인자에 들어간 함수를 호출하는 방식으로 비동기 이벤트를 처리할 수 있습니다. 실제로 대부분 Async 작업을 처리하는 메서드들은 Callback 구조로 설계되어 있습니다. 하지만 Callback은 아래 예시처럼 굉장히 복잡한 구조가 될 소지가 높습니다. 이를 Callback Hell이라고도 부릅니다.
(예시)
이에 따라 Callback보다는 Observation이 비동기 작업을 처리하는 것에 있어서 더 효율적인 경우가 많습니다.

Functional Reactive Programming
Functional Reactive Programming(FRP)은 위에서 살펴본 Reactive Programming을 Functional Programming의 원리를 통해 구현하는 것을 말합니다. 쉽게 말하자면, 비동기적인 데이터 처리를 간단한 함수를 통해 수행하는 프로그래밍을 말합니다. FRP에 대해서 살펴보기 전에 먼저 Functional Programming에 대해 살펴보도록 하겠습니다.

Functional Programming이란 어떤 문제를 해결하는데 있어서 그 과정이나 공식에 매몰되기보다는 그냥 이미 만들어진 함수를 활용하는 방식을 말합니다. 다만, 무조건 활용하는 것이 아니라 함수 자체가 '숨겨진 input과 output'이 없도록 설계되어야 하는 것이 전제조건입니다.
일반적으로 함수는 input을 받아서 output을 산출합니다. 프로그래밍에서 input은 함수 parameter의 인자(argument)값으로 받아오고, output은 함수의 return 값이 되게 됩니다. 하지만 경우에 따라 parameter나 return에 명시되지 않은채, 함수 안에 숨어있는 input과 output이 있을 수 있습니다. 이렇게 숨겨져 있는 input과 output을 side-causes와 side-effects라고 부릅니다. 함수 안에 숨겨진 input과 output이 많을수록 해당 함수를 활용할 때 예상치 못한 일들이 일어날 수도 있고 디버깅도 힘들어집니다. 

// side-causes와 side-effect가 존재하는 함수
// 함수를 콜할 때 implementation detail을 살펴보지 못한다면 무엇이 어떻게 변할지 알 수 없음 
func add() {
    number = 5
    letter = "S"
    title = title + " \(number)" + letter
}

// 숨겨진 input/output이 없는 함수
func add(numberOne: Int, numberTwo: Int) -> Int {
    return numberOne + numberTwo
}
결국 Functional Programming이라는 것은, 결과에 집중하는 실용적인 함수를 정의하고 활용하되, 이러한 함수 안에 이러한 숨겨진 input과 output이 최대한 없도록 선언하는 프로그래밍 패러다임입니다. Swift에서 배열에 filter, map, reduce와 같은 메서드를 활용한 것이 대표적인 Functional Programming의 예입니다. 이들 메서드들은 자신을 호출하는 자가 의도하는 것을 정확히 구현할 수 있도록 투명하게 설계되어 있습니다. 예를 들면, 배열 [1, 2, 3, 4, 5]가 있다고 합시다. 이 배열 중 짝수만 필터링하거나, 각 값마다 5를 곱해주거나, 값들을 모두 합하여 새로운 배열을 만들고자 할 때, Swift Array가 제공하는 filter, map, reduce 메서드를 사용하지 않는다면 for loop을 사용해야 할 것이고 새로운 임시 배열도 만들어야 할 것입니다. 하지만 아래처럼 filter, map, reduce를 사용하면 아주 간단하게 문제는 해결됩니다.

let numbers = [1,2,3,4,5]


numbers.filter { $0 % 2 == 0 }
// [2, 4]

numbers.map { $0 * 5 }
// [5, 10, 15, 20, 25]

numbers.reduce(0, +)
// 15

위의 내용을 토대로 정리하자면 FRP는 위 filter, map, reduce와 같은 함수와 메서드를 활용(Functional)해서 비동기 이벤트를 처리(Reactive)하는 것을 말합니다.

ReactiveX
Rx는 ReactiveX의 약자로서 FRP의 원리를 활용해서 비동기적인 이벤트를 손쉽게 처리하기 위해 만들어진 API입니다. 즉, Rx의 목적은 '비동기 이벤트 계의 map, filter, reduce가 되자'는 것입니다. 이러한 Rx의 핵심은 다음과 같습니다. 

Rx에서는 모든 것이 Data Stream이다.
모든 것이 Stream이라는 핵심 개념을 잘 이해해야 Rx를 잘 활용할 수 있게 됩니다. Rx는 사실 Stream을 Observable이라고 표현하며 실제 라이브러리에서도 Observable의 객체로 선언됩니다. 그리고 해당 Data Stream, 즉, Observable이 언제(When), 그리고 무엇을(What) 분출하는지 듣고 있기 위해 Subscribe를 하게 됩니다.  

참조


Reactive Programming 개념 설명 (가장 좋은 설명)
https://gist.github.com/staltz/868e7e9bc2a7b8c1f754

Functional Programming
http://blog.jenkster.com/2015/12/what-is-functional-programming.html
Functional Programming in Swift 소개https://www.raywenderlich.com/114456/introduction-functional-programming-swiftFunctional Reactive Programming 기초와 기능
https://realm.io/kr/news/slug-max-alexander-functional-reactive-rxswift/
ReactiveX (Rx) 홈페이지
http://reactivex.io/
