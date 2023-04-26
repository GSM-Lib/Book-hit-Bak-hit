# 2.5 코틀린의 예외 처리
코틀린의 예외처리는 자바나 다른 언어와 동일합니다. 

요류가 발생하면 예외를 던질수 있습니다. 함수를 호출하는 쪽에서는 그 예외를 잡아 처리할 수 있습니다. 예외를 처리해주지 않는다면 함수 호출 스택을 거슬러 올라와 예외를 처리하는 부분이 나올 때까지 예외를 계속 던집니다.

```kotlin
if(percentage ! in 0..100) {
    throw IllegalArgumentException("A percentage value must be between 0 and 100: $percentage")
}
```

다른 클래스와 마찬가지로 예외 인스턴스를 만들 때도 new 클래스를 만들지 않아도 사용할 수 있습니다.

또한 throw는 식으로 이용되기에
```kotlin
val percent = if(number in 0..100) 
    number
else 
    throw IllegalArgumentException("A percentage value must be between 0 and 100: $percentage")
```

이와 같은 코드를 사용하면 조건이 참으로 동작하면 number의 값으로 초기화되고, 거짓이라면 초기화가 되지 않습니다.

## 2.5.1 try, catch, finally
자바와 마찬가지로 try와 catch, finally 절을 함께 사용할 수 있습니다.
예제를 들어 사용해보겠습니다.

```kotlin
fun readNumber(reader: BufferReader): Int? {
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
```
던지는 예외는 return 형식처럼 따로 지정해주지는 않습니다.
예외 타입은 모든 타입처럼 :의 오른쪽에 씁니다.
finally 자바와 같이 작동합니다.

자바 코드와 가장 큰 차이는 throws 절이 코드에 없다는 점이다.
자바에서는 함수를 작성할 때 함수 선언 뒤에 throws IOException을 붙여야합니다. 이유는 IOException이 체크 예외이기 때문입니다.

자바에서는 체크 예외를 명시적으로 처리해야합니다.

어떤 함수가 던질 가능성이 있는 예외나 그 함수가 호출한 다른 함수에서 발생할 수 있는 예외를 모두 catch 해야하며, 처리하지 않은 예외는 throws 절에 명시해야 합니다.

코틀린은 체크 예외와 언체크 예외를 구별하지 않습니다.
예외를 지정해도 되고, 지정하지 않아도 됩니다.

## 2.5.2 try를 식으로 사용
finally 절을 없애고 파일에서 읽은 수를 출력하는 코드를 추가하자.
```kotlin
fun readNumber(reader: BufferedReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch(e: NumberFormatException) {
        return
    }
    println(number)
}
```
코틀린의 try 키워드는 if나 when처럼 식으로 사용할 수 있기에 위처럼 사용할 수 있습니다. 하지만 try는 반드시 중과호{} 로 감싸줘야한다는 점이 if와 다른점입니다.
try의 본문도 내부에서 여러 문장이 있으면 마지막 식의 값이 전체 결과 값입니다.

이 예제는 catch 블록 안에서 return 문을 사용합니다. 따라서 예외사 발생한 경우 catch 블록 다른의 코드가 실행되지 않습니다. 하지만 계속 진행하고 싶다면 catch 블록 도 값을 만들어야합니다.

```kotlin
fun readNumber(reader: BufferedReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch(e: NumberFormatException) {
        null
    }
    println(number)
}
```
위와 같이 코드를 작성한다면 입력에서 예외가 발생하면 Null을 발생시킵니다.

try의 코드 블록의 실행이 정상적으로 끝나면 그 블록의 마지막 식의 값이 결과가 됩니다.