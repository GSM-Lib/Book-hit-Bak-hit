## 들어가며
이번 글에서는 클래스와 프로퍼티에 대해 알아봅니다, 본격적으로 알아보기 전에 코틀린의 클래스와 프로퍼티는 자바와 어떤 차이가 있는지 간단하게 알아보고 넘어가겠습니다.
```Java
public class Person {

    private final String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```
**name** 이라는 필드가 있는 **Person**클래스를 자바 코드로 작성해 보았습니다. 위 코드에서 필드가 둘 이상으로 늘어나게 되면 어떤 문제가 발생할까요??<br>
생성자의 본문에서 파라미터를 이름이 같은 필드에 대입해야 하는 **대입문**의 수가 늘어나게 됩니다. 이처럼 자바에서는 생성자 본문에 이와같은 반복적인 코드를 작성하는 경우가 많은데요, 코틀린에서는 이러한 필드 대입 로직을 훨씬 더 **적은 코드**로 작성할 수 있습니다.
```kotlin
class Person(val name: String)
```
위 코드는 **Person**클래스를 Kotlin으로 변환한것입니다. 보기만 해도 정말 간결해 지지 않았나요?
뒤의 내용에서 어떻게 하면 Kotlin에서 저런 코드가 가능한건지 알아보겠습니다. 

## 프로퍼티
클래스라는 개념의 목적은 데이터를 **캡슐화**하고 캡슐화 한 데이터를 다루는 코드를 한 주체 아래 가두는 것입니다.

자바에서는 데이터를 **필드**에 저장하며, 맴버 필드의 가시성은 보통 비공개(private) 입니다. 클래스는 클라이언트가 이런 필드의 데이터에 접근할 수 있도록 **접근자 메서드**를 제공합니다. 

필드를 읽기 위해서 **게터**를 제공하고 필드를 변경하기 위해서 **세터**를 제공합니다.

자바에서는 필드와 접근자를 한데 묶어 **프로퍼티**라고 부릅니다.

코틀린은 프로퍼티를 언어 기본 기능으로 제공하고 코틀린 프로퍼티는 자바의 필드와 접근자 메서드를 완전히 대신합니다. 

Kotlin에서 클래스의 프로퍼티를 선언할 때는 **val, var**을 사용합니다.
**val**로 선언한 프로퍼티는 읽기 전용, **var**로 선언한 프로퍼티는 변경이 가능합니다.
```kotlin
class(
    val name: String,
    var isMarried: Boolean
)
```
- name은 읽기 전용 프로퍼티로, 코틀린에서는 필드(비공개) 와 필드를 읽을 수 있는 개터(공개)를 만들어 냅니다.
- isMarried는 읽기 쓰기가 모두 가능한 프로퍼티로, 코틀린에서는 필드(비공개)와 읽기 쓰기를 위한 개터(공개), 세터(공개)를 만들어 냅니다. 

위 코드는 원래의 자바 코드와 똑같은 구현이 숨겨져 있습니다, Person에는 공개되지 않은 비공개 필드가 들어있고, 생성자가 그 필드를 초기화 하며 게터를 통해 비공개 필드에 접근합니다.

자바에서의 Person클래스 사용법
```Java
Person person = new Person("Bob", true);
System.out.println(person.getName()); //Bob
System.out.println(person.isMarried());//true
```
코틀린에서의 Person클래스 사용법
```kotlin
val person = Person("Bob", true)
println(person.name)//Bob
println(person.isMarried)//true
```
두 코드를 보며 알 수 있듯이 kotlin에서는 프로퍼티의 이름을 직접 입력해도 자동으로 getter를 호출해줍니다.

세터 또한 마찬가지로 person.setMarried(false)대신 person.isMarried = false를 사용합니다.

## 커스텀 접근자
대부분의 프로퍼티에는 프로퍼티의 값을 저장하기 위한 필드가 있는데요 이를 프로퍼티를 **뒷받침하는 필드**라고 부릅니다. 하지만 커스텀 게터를 사용하면 프로퍼티 값을 그때그때 계산할 수 있습니다.
```kotlin
class Rectangle(val height: Int, val width: Int) {
    val isSpaure: Boolean
        get() {
            return height == width
        }
}
```
위 코드는 자신이 직사각형인지를 반별해주는 코드입니다. 
위 코드에서는 커스텀 접근자를 사용하였는데요, 이로 인해 직사각형인지의 여부를 별도의 필드에 저장할 필요가 없어졌습니다. 또한 게터를 호출 할때마다 정사각형인지의 여부를 그때그때 계산하여 return 해줍니다.

만약 구문이 한줄인 경우에는 블록이 본문인 구문을 사용할 필요 없이 
```kotlin
get() = return height == width
```
위 코드처럼 사용이 가능합니다.