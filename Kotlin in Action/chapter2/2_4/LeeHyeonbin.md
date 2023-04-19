# 2.4 대상을 이터레이션: while과 for 루프
이번에는 Kotlin에서 while과 for과 같은 반복문을 사용하는 방법에 대해 알아보겠습니다.<br>
while은 자바 거의 흡사하기 때문에 크게 다루지 않고 넘어가겠습니다.<br>
Kotlin에서의 for는 아래와 같이 사용할 수 있습니다.
```kotlin
for '아이템' in '원소들' 
```

간단하게 예시를 들어보자면
```kotlin
for student in school
```
처럼 사용할 수 있습니다.

## 2.4.1 while
Kotlin에는 while과 do while이 있습니다.
```kotlin 
while('조건') {
    /* ... */
}

do{
    /* ... */
} while('조건')
```

위처럼 사용할 수있으며, 자바와 동일하게 while은 조건이 참인동안 실행되고, 
do while은 본문을 가장 처음에 무조건 실행시키고, 이후에 조건이 참인 동안 실행됩니다.

## 2.4.2 수에 대한 이터레이션: 범위 수열
코틀린에서는 자바나 C에서 진행하던 방식인 변수를 하나 생성하고, 변수를 증가시키고, 최종 값을 지정해주는 방식으로 진행하지 않고 코틀린에서는 범위를 지정하여 반복문을 지정합니다.

범위를 지정할 때는 ..이나 until을 이용할 수 있습니다.
이 두 키워드의 차이는 마지막 원소를 포함하는가에 차이가 있습니다.

```kotlin
val oneToTen = 1..10
```

```kotlin
for i in oneToTen {
    println(i)
}
```
>1<br>
2<br>
3<br>
4<br>
5<br>
6<br>
7<br>
8<br>
9<br>
10<br>

.. 연산자를 사용하는 경우에는 시작 값과 끝 값을 연결해서 범위를 만듭니다.

그렇다면 마지막 원소가 포함되지 않게 하려면 어떻게 해야할까요??

```kotlin
for i in 1 until 10 {
    println(i)
}
```
>1<br>
2<br>
3<br>
4<br>
5<br>
6<br>
7<br>
8<br>
9<br>

until 연산자를 이용하면 해당 원소 전까지만 돌아가게됩니다.

이런 식으로 어떤 범위에 속한 값을 일정한 순서로 이터레이션하는 경우는 수열이라고 부릅니다.

그렇다면 이를 이용하여 피즈버즈 게임을 만들어보겠습니다.
해당 게임은 3으로 나누어 떨어지는 수에는 피즈, 5로 나누어 떨어지는 수에는 버즈, 
3과 5로 모두 나누어 떨어지는 수에는 피즈버즈라고 외치면 되는 게임입니다.

```kotlin
fun fizzBuzz(i: Int) = when {
    i % 15 == 0 -> "FizzBuzz"
    i % 3 == 0 -> "Fizz"
    i % 5 == 0 -> "Buzz"
    else -> "$i"
}

for (i in 1..100) {
    println(fizzBuzz(i))
}
```

> 1 <br>
2 <br>
Fizz <br>
4 <br>
Buzz <br>
Fizz <br>
7 ...

계속 똑같이가면 조금 지켜우니 이번에는 100부터 1로 가도록 하되 짝수만 하도록 개보겠습니다.

```kotlin
for (i in 100 downTo 1 step 2 ) {
    println(fizzBuzz(i))
}
```

> Buzz<br>
98 <br>
Fizz <br>
94 <br>
92<br>
FizzBuzz <br>
88 ...

해당 코드를 확인해보면 증가값인 step을 가지도록하여, 이터레이션합니다. 
증가 값을 사용하면, 모든 수를 하는 것이 아닌 중간을 건너뛸 수 있습니다.

## 2.4.3 맵에 대한 이터레이션
앞 부분에서 for .. in 루프를 자주 사용한다고 하였습니다. 하지만 이러한 부분은 자바와 사용법만 다르고 크게 다르지 얂은 반복문이기 때문에 다른 내용을 설명해보겠습니다.

코틀린에서 맵에 대한 이터레이션을 사용하는 방법을 알아보겠습니다.

예제로 문자열에 대한 2진 표현을 출력하도록 코드를 작성해보겠습니다.

```kotlin
val binaryReps = TreeMap<Char, String>()

for (c in 'A' .. 'F') {
    val binary = Integer.tobinaryString(c.toInt())
    binaryReps[c] = binary
}

for ((letter, binary) in binaryReps) {
    println("$leffer = $binary")
}
```

.. 연산자는 숫자 타입 뿐만 아니라 문자 타입에도 사용할 수 있는 것을 알 수 있습니다.
'A' .. 'F'는 F에 이르는 문자를 모두 포함하는 범위를 만듭니다.

위 코드에서는 for 루프를 이용하여 이터레이션하려는 컬렉션의 원소를 푸는 방법을 보여줍니다. 원소를 풀어서 letter와 binary라는 두 변수에 저장합니다.

letter에는 키가 들어가고, binary에는 2진 표현이 들어갑니다.

위처럼 사용하늡 방법을 구조 분해 문법이라 하고, 위처럼 사용할 수 있지만 나중에 더 자세히 다루겠습니다.

코틀린에서는 get과 put을 이용하여 값을 사용하는 대신에 map[key]나 map[key] = value 와 같이 간단하게 설정하고 가져올 수 있습니다.
이를 출력하면
> A = 1000001<br>
B = 1000010<br>
C = 1000011<br>
D = 1000100<br>
E = 1000101<br>
F = 1000111<br>

로 출력되는 것을 알 수 있습니다.

이러한 구조 분해 구문을 사용하면 원소의 현재 인덱스를 유지하면서 컬렉션을 이터레이션할 수 있습니다.

인덱스를 저장하기 위한 변수를 별도로 선언하고 루프에서 매번 증가시킬 필요가 없습니다.

```kotlin
val list = arrayListof("10", "11", "1001")
for ((index, element) in list.withIndex()) {
    println("$index: $element")
}
```

이 코드는 예상대로 다음과 다음과 같이 이용할 수 있습니다.

> 0: 10<br>
1: 11<br>
2: 1001

withIndex와 관련된 내용은 다음에 더 자세하게 알려줍니다.


## 2.4.4 in으로 컬렉션이나 원소 검사
in 연산자를 이용하면 어떤 값이 해당 범위에 속하는지 알 수 있습니다.
반대로 !in 키워드를 사용하면 어떤 값이 해당 범위에 속하지 않는지 알 수 있습니다.
```kotlin
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'

println(isLetter('q'))
// true
println(isNotDigit('x'))
// false
```

위 코드는 해당 글자가 문자인지 글자인지 검사하는 코드입니다. 이런 식으로 || 을 이용해도 전혀 문제 없이 처음부터 마지막 코드 사이에 있는지를 검사합니다. 

또한 in과 !in 연산자를 when 식에서도 사용할 수 있습니다.

```kotlin
fun recognize(c: Char) = when(c) {
    in '0'..'9' -> "It's a digit!"
    is 'a'..'z', in 'A'..'Z' -> "It's a letter!"
    else -> "I don't know..."
}
```
> println(recognize('8'))
It's a digit!

또한 범위는 문자에서만 국한되는 것이 아니라 비교가 가능한 객체라면 (java.lang.Comparable 인터페이스를 구현한 클래스라면) 그 클래스의 인스턴스 객체를 사용해 범위를 지정할 수 있습니다.

그렇다면? "Java"와 "Kotlin" 사이의 모든 문자열을 이터레이션 할 수 있을까요?

여기에는 조금 무리가 있습니다. 하지만 in 연산자를 사용하면 값이 범위 안에 속하는지를 알 수 있습니다.

```kotlin
println("Kotlin" in "Java".."Scala")
// true
```

 String에서 Comparable 구현은 두 문자열을 알파벳 순서로 비교하기 때문에 여기서 in 검사에서도 알파벳 순서로 비교합니다. 또한 컬렉션에서도 마찬가지고

 ```kotlin
 println("Kotlin" in setOf("Java", "Scala"))
 // false
 ```

 위 집합에는 Kotlin이 들어가있지 않기때문에 false가 뜨는 것을 확인할 수 있습니다.