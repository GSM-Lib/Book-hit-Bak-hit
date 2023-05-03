## 코틀린 기초 요약
1. 함수를 정의할 때는 fun 키워드를 사용한다
```kotlin
fun 함수이름() {
    내용
}
```

2. val과 var는 각각 읽기 전용 변수와 변경 가능한 변수를 선언할 때 쓰인다 
```kotlin
val 변수명 = 값 // 변경 불가능
var 변수명 = 값 // 변경 가능
```

3. 문자열 템플릿을 사용하면 문자열을 연결하지 않아도 되므로 코드가 간결해진다.
```kotlin
val name = "김현승"

print("hi!, $name")

// 출력: hi!, 김현승
```

4. 문자열 템플릿에 중괄호를 사용하면 식도 넣을 수 있다
```kotlin
val number = 5

print("hi!, ${ if(number == 5) "김현승" else "이현빈" })

// 출력: hi!, 김현승
```

5. 코틀린에서는 값 객체 클래스는 아주 간결하게 표현할 수 있다. 
```kotlin
class Person(val name: String)
```

6. 다른 언어에도 있는 if문은 kotlin에선 식이며 값을 만들어 낸다.
```kotlin
val handsome = true
val name = if (handsome) "김현승" else "이현빈"

print(name)

// 출력: 김현승
```

7. kotlin의 when은 자바의 switch와 비슷하지만 더 강력하다.
```kotlin
val name = "김현승"

when(name) {
    "김현승" -> { print("handsome") }
    "이현빈" -> { print("코뿔소") }
    else -> { print("누구냐 넌") }
}
```

8. 어떤 변수의 타입을 검사하고 나면 굳이 그 변수를 캐스팅 하지 않아도 검사한 타입의 변수처럼 사용할 수 있다, 이러한 경우 컴파일러가 스마트 캐스트를 활용해 자동으로 타입을 바꿔줍니다.
```kotlin
is 검사할 변수
```

9. for, while, do-while 루프는 자바가 제공하는 같은 키워드의 기능과 비슷하다. 하지만 코틀린의 for는 자바의 for보다 더 편리하다. 특히 맵을 이터레이션 하거나 이터레이션 하면서 컬렉션의 원소와 인덱스를 함께 사용해야 하는 경우 코틀린의 for가 더 편리하다.
```kotlin
for (i in 0..10) {
    //내용
}
```

10. 1..5와 같은 식은 범위를 만들어 냅니다. 범위와 수열은 코틀린에서 같은 문법을 사용하며, for루프에 대해 같은 추상화를 제공합니다. 어떤 값이 범위한에 들어있거나 들어있지 않은지 검사하기 위해서 in이나 !in을 사용한다.
```kotlin
(1..5).forEach { num ->
    print(num)
}

// 출력: 
1
2
3
4
5
```

11. kotlin의 예외 처리는 자바와 비슷합니다. 다만 코틀린에서는 함수가 던질 수 있는 예외를 선언하지 않아도 됩니다.
```kotlin
fun main() {
    val reader = BufferedReader(StringReader("239"))
    print(readNumber(reader))
}

fun readNumber(reader: BufferedReader): Int? {
    try {
        val line = reader.readLine()
        return Integer.parseInt(line)
    }
    catch (e: NumberFormatException) {
        return null
    }
    finally {
        reader.close()
    }
}

// 출력: 239
```