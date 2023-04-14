# 2.3 선택 표현과 처리: enum과 when
오늘은 코틀린에서 when이란 무엇인지 알아보겠습니다. 
이는 자바로 비교하자면 switch 를 대치할 수 있습니다. 
또한 when에 대해서 설명하며, enum, 스마트 캐스트에 대해서도 간단하게 알아보겠습니다.

## 2.3.1 enum 클래스
먼저 enum 클래스는 무엇일까요?
이는 자바에서 사용하는 enum과 같은 의미로 사용하고 있습니다.
예시 코드로 색상을 정리해보겠습니다.

```kotlin
enum class Color {
    RED, ORANGE, YELLO, GREEN, BLUE, INDIGO, VIOLET
}
```

kotlin에서 enum은 자바보다 코틀린이 더 길게 작성해야하는 몇 안되는 문법입니다.
> kotlin 에서는 enum class를 사용하지만, <br>
java에서는 enum으로 사용합니다.

이 두 enum과 class가 하나로 사용되는 것이 아닌 
class 앞에 enum이 붙은 특별한 형태입니다. 
코틀린에서는 이러한 형태를 Soft Keyword라고 합니다.

또한 enum이라는 특수한 클래스로 지정해주더라도 클래스이기 때문에 클래스의 이름을 지정해주어야합니다.

> 마치 지우개가 달린 연필을 연필이라고 하는 것과 같죠

다시 돌아와서 enum은 단순히 값만 열거하는 존재가 아닙니다.
enum에 프로퍼티와 메서드를 정의할 수 있습니다.

```kotlin
enum class Color(
    val r: Int , val g: Int, val b: Int
) {
    RED(255, 0, 0), ORANGE(255, 165, 0), // 상수의 프로퍼티를 정의한다.
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238); // 이 부분에는 세미콜론을 사용해야합니다.

    fun rgb() = (r * 256 + g) * 256 + b
}

println(Color.BLUE.rgb())
//255
```

enum 클래스에서도 일반적인 클래스와 같이 생성자와 프로퍼티를 선언합니다. 
그리고 상수를 정의할 때에는 그 상수에 해당하는 프로퍼티 값을 지정해야만 합니다.
이 예제에서는 코틀린에서 세미콜론이 필수적으로 필요한 아마 유일한 부분을 볼 수 있습니다.

enum class 안에 메서드를 정의하는 경우 반드시 enum 상수 목록과 메서드 정의 사이에 세미콜론을 넣야합니다.

## 2.3.2 when으로 enum 클래스 다루기
무지개의 색을 기억하기 위해 연상법을 적용한 문장을 볼 수 있습니다.
"Richard Of Work Gave Battle In Vain!"이라는 문장이 있습니다. 
만약 무지개의 색과 해당 연상에 상응하는 색을 짝 지어주는 함수가 필요하다면 어떻게 작성해야할까요?

만약에 자바로 작성한다면 switch 문이나 if, else if 문을 이용하여 해당 함수를 구성할 수 있습니다.

그렇다면 kotlin에서는 when으로 사용할 수 있겠죠?
```kotlin
fun getMnemonic(color: Color) {
    when(color) {
        Color.RED -> "Richard"
        Color.ORANGE -> "Of"
        Color.YELLOW -> "York"
        Color.GREEN -> "Gave"
        Color.BLUE -> "Battle"
        Color.INDIGO -> "In"
        Color.VIOLET -> "Vain"
    }
}
print(getMnemonic(Color.BLUE))
// Battle
```

또한 Kotlin에서는 break를 넣지 않아도 됩니다.
성공적으로 매치되는 분기를 찾으면 그 분기를 실행하고, 한 분기 안에서 여러 값을 매치 패턴으로 ,(콤마)를 이용하여 사용할 수도 있습니다.

```kotlin
fun getMnemonic(color: Color) {
    when(color) {
        Color.RED, Color.ORANGE, Color.YELLOW -> "warm"
        Color.GREEN -> "neutral"
        Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
    }
}
print(getMnemonic(Color.BLUE))
// Battle
```

위와 같이 사용할 수 있습니다.

## 2.3.3 when과 임의의 객체를 함께 사용
코틀린에서 when은 java의 switch와 비교하자면 더욱 강력하게 사용할 수 있다는 것을 알 수 있습니다.

자바에서는 enum 상수나 숫자 리터럴만을 사용할 수 있는 것이 아닌, 임이의 객체 또한 허용합니다.

그렇다면 두 색을 혼합했을 때 미리 정해진 팔레트에 들어있는 색이 될 수 있는지 알려주는 함수를 작성해봅시다. 팔레트에 들어갈 수 있는 색이 위의 색들 선에서만 정리된다고 하고 작성해봅시다.

```kotlin
fun mix(c1: Color, c2: Color) =
    when(setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE
        setOf(YELLOW, BLUE) -> GREEN
        setOf(BLUE, VIOLET) -> INDIGO
        else throw Exception("Dirty color")
    }

println(mix(BLUE, YELLOW))
// GREEN
```
when 식의 인자로 아무 객체나 사용할 수 있고, when은 이런 인자로 받은 값과 같은지 확인합니다. 

두 색을 혼합하여 나오는 색을 열거합니다.

매치되는 분기 조건이 없는 경우에는 오류를 실행시킵니다.

위 코드를 실행시키면 BLUE와 YELLOW 값이 들어왔을때로 예상되는 결과인 GREEN 이 출력되는 것을 확인할 수 있습니다. 또한 setOf를 이용하여 여러 객체를 포함하는 Set 객체를 만들어줍니다.

그리고 각 원소의 순서를 중요치 않기에 RED, YELLOW와 YELLOW, RED가 같은 값을 반환함을 알 수 있습니다.

이러한 결과를 보며, 객체의 동등성을 확인하는 것임을 알 수 있습니다.
> 동일성이란 객체가 가르키고 있는 주소값이 같은 것, 동등성은 값을 비교하여 값이 값읕 것을 뜻합니다.

여기서는 when에 인자를 넣어 사용하였습니다.
그렇다면 인자를 넣지 않고는 사용이 불가능할까요??
코틀린에서는 인자없는 when 사용을 허락하고 있습니다. 그러면 어떻게 사용해야할지 확인해봅시다.
## 2.3.4 인자 없는 when 사용
```kotlin
fun mixOptimized(c1: Color, c2: Color) =
    when {
        (c1 == RED && c2 == YELLOW) -> ORANGE
        ... // 다른 수들은 같으니 스킵
        else -> throw Exception
    }


println(mixOptimized(RED, YELLOW))
// ORANGE
```

해당 코드를 보면 when에 아무 조건을 집어 넣지 않은 상황에서는 앞에서 나온 조건처럼 동등성을 비교하는 것이 아니라 boolean으로 if문을 사용하는 것처럼 사용할 수 있다는 것을 알 수 있습니다.

해당 코드는 추가적으로 객체를 만들지 않는다는 장점이 있지만, 가독성이 더 떨어집니다.

## 2.3.5 스마트 캐스트: 타입 검사와 타입 캐스트를 조합
이번 부분에서는 간단한 산술식을 계산하는 함수를 만들어서 스마트 캐스트에 대해 알아보겠습니다.

먼저 스마트 캐스트가 무엇일까요?

일단 예제 코드를 들어가며 설명하겠습니다.

```kotlin
interface Expr
class Num(val value: Int): Expr
class Sum(val left: Expr, val right: Expr): Expr
```

위 코드에(1 + 2) + 4라는 식을 대입하면
Sum(Sum(Num(1), Num(2)), Num(4)) 과 같은 구조의 객체가 생성됩니다.

해당 식에서 고려해야하는 경우들이 있습니다.
* 어떤 식이 수라면 그 값을 반환한다.
* 어떤 식이 합계라면 좌항과 우항의 값을 계산한 다음에 그 두 값을 합한 값을 반환한다.

이를 자바스타일로 코드를 작성해보자면
```kotlin
fun eval(e: Expr) Int {
    if(e is Num) {
        val n = e as Num
        return n.value
    }
    if(e is Sum) {
        return eval(e.right) + eval(e.left)
    }
    throw IllegalArgumentException("Unknown)
}
```

위와 같이 코드를 작성하게되면,
e가 Num일 때만 실행되는 것을 알고 있더라도, e를 Num으로 형변환 해줘야하는 불필요한 모습이 나옵니다.

하지만 코틀린에서는 컴파일러가 이를 대신 원하는 타입으로 지정해줄 수 있습니다. 
이것을 스마트 캐스트이라고 합니다.

eval 함수에서 e의 타입이 Num이라고 검사한 다음 부분에서는 e의 타입을 Num으로 해석합니다. 그렇기 때문에 사용할 때마다 명시적으로 사용해줄 필요가 없습니다.

스마트 캐스튼느 is로 변수에 든 값의 타입을 감사한 다음에 그 값이 바뀔 수 없는 경우에만 작동합니다.

이제 위에서 작성한 코드를 코틀린처럼 리팩토링 해보겠습니다.
```kotlin
fun eval(e: Expr): Int =
when(e) {
    is Num -> 
        e.value
    is Sum -> 
        eval(e.right) + eval(left)
    else -> throw IllegalArgumentException("Unknown)
}
```
위와 같이 is와 스마트 캐스트를 이용하여 바로 e.value를 가져오고,
if를 when으로 조건을 주어 확인하고, 식을 본문으로 사용하도록 작성하여 위와 같이 간결한 코드를 작성할 수 있습니다. 

## 2.3.7 if와 when의 분기에서 블록 사용
또한 분기에서 블록을 사용할 수 있습니다. 그러한 경우에는 블록의 마지막 문장이 전체의 결과가 됩니다. 만약 앞의 함수에 로그를 찍어주는 함수를 작성하고 싶다면 이렇게 변경하면됩니다.

```kotlin
fun evalWithLogging(e: Expr): Int =
when(e) {
    is Num -> {
        println("num: ${e.value}")
        e.value
    }
    is Sum -> {
        val left = evalWithLogging(e.left)
        val right = evalWithLogging(e.right)
        println("sum: $left + $right")
        eval(e.right) + eval(left)
    }
    else -> throw IllegalArgumentException("Unknown)
}
```

이를 출력하면 
```
num: 1
numL 2
sum: 1 + 2
num: 4
sum: 3 + 4
7
```
이라는 결과를 확인할 수 있습니다.

블록의 마지막 식이 블록의 결과라는 규칙은 블록이 값을 만들어내야하는 경우에 항상 성립합니다. 식이 본문인 함수는 블록을 본문으로 가질 수 없고, 블록이 본문인 함수는 return 문을 반드시 내부에 있어야한다는 것을 알 수 있습니다.