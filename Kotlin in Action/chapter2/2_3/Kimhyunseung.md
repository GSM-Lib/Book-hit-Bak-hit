## 들어가며
오늘은 코틀린의 **when**에 대해 알아봅니다. when문을 설명하는 과정에서 코틀린의 **enum**과 **스마트 캐스트**에 대해서도 알아보겠습니다.

## enum 클래스
```kotlin
enum class Color {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```
코틀린에서의 enum은 **소프트 키워드**라 부르는 존재입니다, 이는 **enum**이라는 단어가 클래스 앞에 존재할 때는 특별한 의미를 지니지만 다른곳에서는 **enum**이라는 단어를 이름으로 사용할 수 있기 때문입니다.
반면 **class**는 키워드이기 때문에 이름으로 사용할 수 없습니다.

자바와 마찬가지로 enum은 단순히 값만 열거하는 존재가 아닙니다. enum클래스 안에도 **프로퍼티**나 **메서드**를 정의할 수 있습니다.

```kotlin
enum class Color (
    val r: Int, val g: Int, val b: Int 
) {
    RED(255, 0, 0), ORANGE(255, 165, 0),
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238);

    fun rgb() = (r * 256 + g) * 256 + b
}

>>>>print(Color.BLUE.rgb())
// 출력: 255
```
enum도 일반적인 클래스와 마찬가지로 **생성자와 프로퍼티**를 선언 할 수 있습니다. 각 enum상수를 정의할 때는 그 상수에 해당하는 프로퍼티의 값을 지정해야합니다.

enum안에서 메서드를 정의하는 경우에는 반드시 enum상수 목록과 메서드 정의 사이에 세미콜론을 넣어 분리시켜줘야 합니다.

## when으로 enum 클래스 다루기
```kotlin
enum class Color {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```
위와 같은 enum클래스가 있고 이 enum클래스를 이용하여 무지개의 각 색에 대해 상응하는 연상 단어를 짝지어주는 함수가 필요하다고 생각해봅시다. 
자바라면 **switch**문을 사용하여 그런 함수를 작성 할 수 있습니다. 하지만 코틀린에서는 **when**문을 사용하여 함수를 작성할 수 있습니다. 

만약 자바의 **switch**로 위와같은 함수를 작성하게 되면 어떻게 될까요?? 
switch의 각 케이스마다 return문을 적아줘야 할겁니다. 하지만 kotlin의 when은 kotlin의 if와 같이 값을 만들어내는 식이기 때문에 좀더 간결한 코드를 작성할 수 있습니다. 
```kotlin
fun getMnemonic(color: Color) = 
    when (color) {
        Color.RED -> "Richard"
        Color.ORANGE -> "Of"
        Color.YELLOW -> "York"
        Color.GREEN -> "Gave"
        Color.BLUE -> "Battle"
        Color.INDIGO -> "In"
        Color.VIOLET -> "Vain"
    }

>>>> print(getMnemonic(Color.BLUE))
//출력: Battle
```
kotlin의 when도 if문 처럼 **값을 만들어내는 식**이기 때문에 식이 본문인 함수에 when을 바로 사용할 수 있습니다. 

또한 when은 한 분기 안에서 여러 매치 패턴을 사용할 수도 있다. 이럴 경우에는 값 사이를 **콤마**로 분리하여 사용합니다.
```kotlin
fun getMnemonic(color: Color) = 
    when (color) {
        Color.RED, Color.ORANGE, Color.YELLOW -> "Wram"
        Color.GREEN -> "Netural"
        Color.BLUE, Color.INDIGO, Color.VIOLET -> "Cold"
    }

>>>> print(getMnemonic(Color.BLUE))
//출력: Cold
>>>> print(getMnemonic(Color.INDIGO))
//출력: Cold
```
지금까지 코드를 보면 상수 앞에 Color라는 코드들이 중복된다 이를 없애고 싶으면 클래스 자체를 모두 임포트 하면 됩니다.
```kotlin
import com.example.Color.*
```

## when과 임의의 객체를 함께 사용
자바의 switch와 다르게 when의 분기 조건은 임의의 객체를 허용합니다. 
```kotlin
when (setOf(c1, c2)) {
    setOf(RED, YELLOW) -> ORANGE
    setOf(YELLOW, BLUE) -> GREEN
    setOf(BLUE, VIOLET) -> INDIGO
    else -> throw Exception("Dirty color")
}
```
- when식의 인자로는 아무 객체나 사용할 수 있습니다. when은 인자로 받은 객체가 각 분기 조건에 있는 객체와 같은지 테스트 합니다.
- 매치되는 분기조건이 없으면 **else** 분기를 실행합니다

## 인자 없는 when 사용
위 when문의 분기 조건에서 색을 검사하기 위해 setOf 인스턴스를 생성합니다. 만약 이 when을 가지고 있는 함수가 여러번 호출될 경우에는 불필요한 가비지 객체가 늘어나게 됩니다. 이를 방지하기 위해 우리는 **인자가 없는 when식**을 사용할 수 있습니다.
```kotlin
when {
    (c1 == RED && c2 == YELLOW) || (c1 == YELLOW && c2 == RED) -> ORANGE
    (c1 == YELLOW && c2 == BLUE) || (c1 == BLUE && c2 == YELLOW) -> GREEN
    (c1 == BLUE && c2 == VIOLET) || (c1 == VIOLET && c2 == BLUE) -> INDIGO
    else -> throw Exception("Dirty color")
}
```
- 위 코드는 위에서 나온 when문과 동일한 동작을 합니다.
- when문에 아무 인자도 없으려면 각 분기의 조건이 **Boolean**결과를 리턴하는 식이어야 합니다. 
- 이러한 코드는 추가 객체를 만들지 않는다는 장점이 있지만 가독성은 더 떨어지게 됩니다.

## 스마트 캐스트
kotlin에서는 **is**를 통해 변수의 타입을 검사합니다. 이 is키워드는 자바의 **instanceof**와 비슷한데요, 하지만 자바에서 어떤 변수의 타입을 확인한 후에 그 타입에 속한 맴버에 접근하기 위해서는 명시적으로 변수의 타입을 캐스팅 해주어야 합니다.

kotlin에서는 이런 귀찮은 작업들을 막고자 스마트 캐스팅 기능을 지원합니다.
이 스마트 캐스팅이 무엇이냐면 어떤 변수가 원하는 타입인지 is로 검사하고 나면 굳이 변수를 직접 타입 캐스팅 해주지 않아도 자동으로 원하는 타입대호 컴파일러가 타입을 캐스팅 해줍니다.

이 스마트 캐스트 기능은 is로 타입을 검사한 후에 그 값이 바뀔 수 없는 경우에만 작동합니다. 스마트 캐스트를 사용하는 프로퍼티는 반드시 val이어야 하며 커스텀 접근자를 사용한 것이여도 안됩니다.

위의 조건을 만족하지 못할때 왜 스마트 캐스트를 사용할 수 없냐면 프로퍼티에 대한 접근이 항상 같은 값을 내놓는다고 확신할 수 없기 때문입니다.

추가로 변수를 원하는 타입으로 직접 타입캐스팅 하고싶을때는 **as**키워드를 사용합니다.
```kotlin
val e = "0"
val n = e as Int
```