## Bridge Pattern
> 추상클래스를 설계할 때, 개념적인 부분과 상세적인 부분을 분리하기 위한 패턴

#### 문제가 생긴 사례

![image](https://user-images.githubusercontent.com/21086676/102777461-f5724800-43d3-11eb-90dd-1c72f9567d81.png)

#### 왼쪽 그림
- Window 라는 추상클래스가 있음
- X운영체제에서 사용하기 위한 `XWindow` 구현
- PM운영체제에서 사용하기 위한 `PMWindow`구현

#### 오른쪽 그림
- 또다른 성격의 Window `IconWindow` 구현하게 됨
- 그런데 문제는 X운영체제에서 돌아가는 `IconWindow`와 PM운영체제에서 돌아가는 `IconWindow` 이 필요한 상황
- `IconWindow`의 기능을 유지해야하니깐, `IconWindow`를 상속받아 `XIconWindow`, `PMIconWIndow` 각각 개발
     - 각 클래스 내부에선 X운영체제, PM운영체제에서 돌아가기 위한 코드가 추가적으로 포함 됨

#### 이런 문제점이 생긴이유
- Window를 추상화 하긴 했지만 실제론 추상화를 더 할 수 있음
     - 어떠한 형태의(아이콘, 다이얼로그, 또다른 형태 등) 윈도우를 사용할 지 (개념)
     - 어떤 환경(X, PM 운영체제)에서 구현할 지 (구현)
> **결론** : 개념과 구현을 분리할 수 있다.

### Brige 패턴 적용된 모습
![image](https://user-images.githubusercontent.com/21086676/102779494-aa5a3400-43d7-11eb-8222-c3816534db6d.png)

- 새로운 윈도우가 생기면 Window를 상속받아 구현하면 된다.
- 새로운 운영체제가 생기면 WindowImp를 상속받아 구현하면 된다.
- 어떤 형태의 윈도우를 사용할지 (개념) 어떤환경에서 구현할지 (구현) 이 분리되어 있다.
