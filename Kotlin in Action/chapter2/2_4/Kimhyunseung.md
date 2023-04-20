## 이터레이션?
이터레이션이란 결과를 생성하기 위한 프로세스의 반복을 의미합니다.
 
 ## while과 do-while 루프
 코틀린의 while문과 do-while의 문법은 자바와 다르지 않습니다
 ```kotlin
 while(조건) {
    //내용
 }
 ```
 - 조건이 참인 동안에 본문을 반복 실행합니다.
 ```kotlin
 do {
    //내용
 } while(조건)
 ```
 - 맨처음에 무조건 본문을 한 번 실행한 다음, 조건이 참인 동안 본문을 반복 실행합니다.

 ## for문
 ### 1. 수에 대한 이터레이션: 범위와 수열
 코틀린에는 자바의 for문 처럼 어떤 변수를 초기화 하고 그 변수를 루프를 실행할 때마다 갱신하고 루프 조건이 거짓이 될 때 반복을 마치는 형태의 루프가 존재하지 않습니다. 이런 방식의 루프를 대신하기 위해 코틀린에서는 범위를 사용합니다.
 범위는 기본적으로 두 값으로 이뤄진 구간이며 **".."** 연산자로 시작값과 끝 값을 연결해서 범위를 만듭니다.
 ```kotlin
 val oneToTen = 1..10
 ```
 이 범위는 항상 마지막 값을 포함합니다.
 정수 범위로 수행할 수 있는 가장 단순한 작업은 범위에 속한 모든 값에 대한 이터레이션 입니다. 이런 식으로 어떤 범위에 속한 값을 일정한 순서로 이터레이션 하는 경우를 **수열** 이라고 부릅니다. 

 코틀린의 **for**문에서는 **step**키워드를 통해 증가 값을 설정할 수 있고, 이에 들어가는 값을 음수로 바꾸면 역방향 수열을 만들 수 있습니다. 음수를 사용하지 않고 역방향 수열을 만들고 싶다면 **downTo**키워드를 사용할 수 있습니다. 
```kotlin
for (i in 100 downTo 1) {
    //내용
}
```
- 역방향 수열

downTo를 사용한 역방향 수열에서도 마찬가지로 **step**키워드를 사용하여 감소값을 조정할 수 있습니다.

범위는 앞에서 언급한대로 범위의 끝 값을 항상 포함합니다. 하지만 끝 값을 포함하지 않는 범위를 만들고 싶다면 until함수를 사용할 수 있습니다.
```kotlin
(x in 0 until size)

(x in 0 .. size - 1)
```
- 위 두 구문은 같은 동작을 합니다.
### 2. 맵에 대한 이터레이션
```kotlin
val binaryReps = TreeMap<Char, String>()

for (c in 'A'..'F') { // A 부터 F 까지의 문자의 범위를 사용해 이터레이션 진행
    val binary = Integer.toBinaryString(c.toInt()) // 아스키 코드를 2진 표현으로 바꾼다.
    binaryReps[c] = binary // c를 키로 c의 2진 표현을 맵에 넣는다.
}

for((letter, binary) in binaryReps) { // 맵에 대해 이터레이션 한다. 맵의 키와 값을 두 변수에 대입한다.
    println("$letter = $binary")
}
```
**".."** 연산자는 숫자 뿐만 아니라 문자 타입에도 적용할 수 있습니다다. 위 예제코드의 'A'..'F'는
A부터 F까지에 이르는 문자를 모두 포함하는 범위를 만듭니다.

위 예제코드의 for문 에서는 원소를 풀어서 letter와 binary라는 두 변수에 저장합니다.

또한 위 예제코드에서 키를 사용해 맵의 값을 가져오거나 키에 해당하는 값을 설정하는 멋진 코틀린 기능을 보여줍니다. get과 put대신 **map[key]** 나 **map[key] = value** 를 사용해 값을 가져오고 설정할 수 있습니다.
```kotlin
binaryReps[c] = binary //kotlin
```
```java
binaryReps.put(c, binary) //java
```
- 위의 두 구문은 동일한 기능을 수행합니다.

맵에 사용했던 구조 분해 구문을 맵이 아닌 컬렉션에서도 활용할 수 있습니다. 이러한 구조 분해 구문을 사용하면 원소의 현재 인덱스를 유지하면서 컬렉션을 이터레이션할 수 있습니다, 또한 인덱스를 저장하기 위한 변수를 별도로 선언하고 루프에서 매번 그 변수를 증가시킬 필요가 없습니다. 
```kotlin
val list = arrayListOf("10", "11", "1001")
for((index, element) in list.withIndex()) {
    println("$index: $element")
}

>>> 출력

0: 10
1: 11
2: 1001
```
- **withIndex()** 를 사용하면 value와 index를 모두 받을 수 있다

### 3. in키워드로 컬렉션이나 범위의 원소 검사
**in** 연산자를 사용하면 어떤 값이 지정된 범위에 속하는지 검사할 수 있습니다. 반대로 **!in**을 사용하면 어떤 값이 지정된 범위에 속하지 않는지를 검사할 수 있습니다.
```kotlin
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'

print(isLetter('q'))
>>> 출력: true
print(isNotDigit('x'))
>>> 출력: true
```
추가로 **in**과 **!in**연산자를 when문에서도 사용할 수 있습니다.
```kotlin
fun recognize(c: Char) = when(c) {
    in '0'..'9' -> "it's a digit!"
    in 'a'..'z', in 'A'..'Z' -> "it's a letter!"
    else -> "what?"
}
print(recognize('8'))
>>> 출력: it's a digit!
```
범위는 문자에만 국한되지 않습니다. 비교가 가능한 클래스(java.lang.Comparable 인터페이스를 구현한 클래스 ex) Stirng클래스)라면 그 클래스의 인스턴스 객체를 사용해 범위를 만들 수 있습니다. 

하지만 Comparable을 사용하는 범위의 경우 그 범위 내의 모든 객체를 항상 이터레이션 하지는 못합니다. 예를 들어 'Java'와 'Kotlin'사이의 모든 문자열을 이터레이션할 수 있을까요? 그럴수는 없습니다. 하지만 in연산자를 사용하면 값이 범위안에 속하는지 항상 확인할 수 있습니다.
```kotlin
println("Kotlin" in "Java".."Scala")
>>> 출력: true
```
String에 있는 Comparable구현이 두 문자열을 알파벳 순서로 비교하기 때문에 여기있는 in 검사에서도 문자열을 알파벳 순서로 비교합니다
```kotlin
println("Kotlin" in setOf("Java", "Scala"))
>>> 출력: false
```
컬렉션에서도 마찬가지로 in연산을 사용할 수 있습니다.


