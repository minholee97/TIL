## 학습 내용

* 함수

```kotlin
fun main(args: Array<String>) {
  println("Hello, World!")
}
```
  * 함수 선언에 fun 키워드를 사용
  * 파라미터 이름 뒤에 타입 명시
  * 함수를 최상위 수준에 정의 가능 (클래스안에 넣을 필요 없음)
  
* 식이 본문인 함수
```kotlin
fun max(a: Int, b: Int): Int = if (a>b) a else b

fun max(a: Int, b: Int) = if (a>b) a else b // 반환 타입 생략, 타입 추론
```
  * 식 : 값을 만들어 내며 다른 식의 하위 요소로 계산에 참여함
  * 문 : 자신을 둘러싸고 있는 가장 안쪽 블록의 최상위 요소로 존재, 값을 만들어 내지 않음
  * 자바에서는 모든 제어 구조가 문, 코틀린에서는 루프를 제외한 대부분의 제어 구조가 식
  * 식이 본문인 함수의 경우 컴파일러가 식을 분석해서 타입을 지정함
  
* 변수
  * val (값을 뜻하는 value에서 따옴)
    * 변경 불가능한(immutavle) 참조를 저장하는 변수다. val로 선언된 변수는 일단 초기화하고 나면 재대입이 불가능하다. (자바의 final 변수에 해당)
    * 참조 자체는 불변이지만 그 참조가 가리키는 객체의 내부 값은 변경될 수 있음
    
  * var (변수를 뜻하는 variable에서 따옴)
    * 변경 가능한(mutable) 참조다. 이런 변수의 값은 바뀔 수 있다. (자바의 일반 변수에 해당)
  
### 클래스

* java
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
//...
Person person = new Person("Bob", true);
System.out.println(person.getName());
-> Bob
System.out.println(person.isMarried());
-> true
```

* kotlin
```kotlin
class Person(val name: String)
//...
val person = Person("Bob", true) //new 키워드를 사용하지 않음
println(person.name)
-> Bob
println(person.isMarried)
-> true //프로퍼티 이름을 직접 사용, 자동으로 게터를 호출
```
  * 코틀린의 기본 가시성은 public

* 커스텀 접근자
```kotlin
class Rectangle(val height: Int, val width: Int) {
	val isSquare: Boolean
		get() {
			return height == width
		}
	// 정사각형인지 직사각형인지 별도 필드에 저장할 필요가 없음
}
```
* import의 경우 자바 방식을 따를 것을 권장

* enum
  * 단순히 값을 열거만 하지 않고 프로퍼티나 메소드를 정의할 수 있음
```kotlin
enum class Color (
    val r: Int, val g: Int, val b: Int //상수의 프로퍼티 정의
) {
    //각 상수를 생성할 때 그에 대한 프로퍼티 값을 지정
    RED(255, 0, 0), ORANGE(255, 165, 0),
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238); //세미콜론 반드시 사용

    fun rgb() = (r * 256 + g) * 256 + b //enum 클래스 안에서 메소드를 정의

    fun getMnemonic(color: Color) {
        when (color) {
            Color.RED -> "Richard"
            Color.ORANGE -> "Of"
            Color.YELLOW -> "York"
            Color.GREEN -> "Gave"
            Color.BLUE -> "Battle"
            Color.INDIGO -> "In"
            Color.VIOLET -> "Vain"
        }
    }
}
```
  * enum 상수 목록과 메소드 사이에 세미콜론 필수
  * 인자 없이 when을 사용할 수 있음
  * 

