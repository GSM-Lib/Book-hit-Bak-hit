## 코틀린의 예외처리
코틀린의 예외처리는 자바나 다른 언어의 예외처리와 비슷합니다. 함수에서 예외를 던지고 함수를 사용하는 곳에서 그 예외를 잡아 처리할 수 있습니다. 또한 발생한 예외를 함수 호출 단에서 처리하지 않으면 예외를 처리하는 부분이 나올 때까지 예외를 다시 던집니다. 

코틀린의 기본 예외처리 구문은 자바와 비슷합니다. 
```kotlin
throw 예외이름(메시지)
```
코틀린의 예외도 다른 클래스들과 마찬가지로 **new**키워드를 붙일 필요가 없습니다. 또한 코틀린의 **throw**는 **식**이므로 또 다른 식에 포함될 수 있습니다. 
```kotlin 
val percentage = 
    if (number in 0..100)
        number
    else
        throw IllegalArgumentException("number의 값은 0에서 100 사이여야 합니다.")
```
- 조건이 참일 경우엔 변수가 number의 값으로 초기화 됩니다.
- 조건이 참이 아닐 경우엔 변수가 초기화되지 않습니다.

## try, catch, finally
코틀린에서도 자바와 마찬가지로 예외를 처리하려면 **try와, catch, finally** 절을 함께 사용합니다. 
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

>>> 출력: 239
```
- 파일에서 각 줄을 읽어 수로 변환하되 그 줄이 올바른 수 형태가 아니면 null을 반환하는 예제

자바코드에는 예외를 던지는 **throws**절이 코드에 있습니다.
예외를 던진다는건 예외처리를 자신이 진행하는것이 아닌 이 메소드를 사용하는 곳으로 **책임을 전가**하는 행위를 말합니다. 하지만 코틀린에서는 throws키워드를 사용하지 않습니다.

또한 자바에서는 함수 선언 뒤에 **체크 예외**를 throws를 사용하여 명시적으로 처리해야합니다. 

```markdown
## 체크 예외란?
체크 예외란 클래스를 상속받지 않은 예외 클래스들을 말합니다.
이 체크 예외는 복구 가능성이 있는 예외이므로 반드시 예외를 처리하는 코드를 작성해야 합니다. 
예외를 처리하기 위해서는 catch문으로 예외를 잡거나 throws를 통해 밖으로 예외를 던져 함수를 호출하는곳에 책임을 전가합니다. 
만약 예외를 처리하지 않는다면 컴파일 에러가 발생합니다.
```

하지만 코틀린은 다른 최신 JVM언어와 마찬가지로 **체크 예외와 언체크 예외**를 구분하지 않습니다. 코틀린에서는 throws를 사용하지 않기 때문에 함수가 던지는 예외를 지정할 필요가 없습니다. 또한 발생한 예외를 잡아내도 되고 잡아내지 않아도 됩니다.

이러한 방식은 실제 자바 프로그래머들이 체크 예외를 사용하는 방식을 고려해 설계했습니다. 

예를 들어 위의 예제코드에서 **NumberFormatException**은 체크 예외가 아닙니다. 따라서 자바 컴파일러 에서는 이 예외를 잡아내게 강제하지 않습니다. 그에 따라 실행 시점에 **NumberFotmatException**이 자주 발생하는걸 볼 수 있습니다. 하지만 입력값이 잘못 입력되는 경우는 흔히 있는 일이므로 그런 문제가 발생한 경우 부드럽게 다음 단계로 넘어가도록 프로그램을 설계해야 한다는 점에서 이는 불행한 일입니다.

또한 **BufferedReader.close**는 **IOException**을 던질 수 있는데, 이 예외는 체크 예외이므로 반드시 예외를 처리해줘야 합니다. 하지만 실제 스트림을 닫다가 실패하는 경우 특히 스트림을 사용하는 클라이언트 프로그램이 취할 수 있는 의미있는 동작은 없습니다. 그러므로 이 IOException을 잡아내는 코드는 불필요한 코드가 되어버리는 것입니다. 

## try를 식으로 사용
```kotlin
fun main() {
    val reader = BufferReader(StringReader("not a number))
    readNumber(reader)
}

fun readNumber(reader: BuffredReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        return
    }
    print(number)
}
>>> 아무것도 출력되지 않는다.
```
코틀린의 **try**문은 **if, when**과 마찬가지로 **식**입니다. 따라서 try의 값을 변수에 대입할 수 있습니다. 
if와 달리 try문은 본문을 반드시 **중괄호**로 둘러싸야 합니다. 
다른 문장들과 마찬가지로 try의 본문도 내부가 여러 문장으로 이루어져 있다면 **마지막 식의 값**이 전체 결과 값이 됩니다.

위의 예제는 **catch블록** 안에서 return문을 사용합니다. 따라서 예외가 발생한 경우엔 catch블록의 다음 코드는 실행되지 않습니다. 만약 계속 진행하고 싶다면 catch블록도 **값**을 만들어야 합니다. catch블록 또한 내부의 **마지막 식**이 블록 전체의 값이 됩니다. 
```kotlin
fun main() {
    val reader = BufferReader(StringReader("not a number))
    readNumber(reader)
}

fun readNumber(reader: BuffredReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        null
    }
    print(reader)
}

>>>출력: null
```
- 예외가 발생했으므로 함수가 null을 출력합니다.

try코드 블록의 실행이 정상적으로 끝나면 그 블록의 마지막 식의 값이 결과가 됩니다. 에외가 발생하고 잡히면 그 예외에 해당하는 catch블록의 값이 결과가 됩니다. 