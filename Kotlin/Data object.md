# [Data object](https://kotlinlang.org/docs/object-declarations.html#data-objects) 

- 코틀린 1.9.0부터 추가된 타입
- 이름 그대로 object + data, 기존 object 한정자에 data 클래스의 함수를 제공함
  - `toString()`, `equals()`, `hashCode()` 제공
  - data object에서 제공하는 `equals()`, `hashCode()` 함수는 재정의할 수 없다고 함

### object vs data object

- 두 한정자의 가장 큰 차이점은 `toString()` 구현 여부
- data object로 선언한 클래스를 출력하면 클래스명만 출력됨

```kotlin
sealed class Result {
  object Success : Result() 
  
  data object Loading : Result()
}

fun main() {
  println(Result.Success) // com.example.test.Result$Success@2d294ee
  println(Result.Loading) // Result.Loading
}
```

### data class vs data object

- data object는 `componentN()`, `copy()` 함수는 지원하지 않음
  - data object는 싱글톤 패턴이며, 데이터 프로퍼티가 없기 때문에 굳이 두 함수의 지원이 필요 없음

### 왜 equals()가 필요할까..

- object 한정자는 싱글톤을 지원하기 위해 사용되기 때문에 항상 값이 동일할텐데 왜 `equals()`/`hashCode()`가 필요할까?
- 일반적인 경우에는 상관없지만, 리플렉션이나 직렬화와 같이 런타임 도중 같은 타입의 객체를 생성되는 상황에서 동등한 객체임을 보장하기 위해 구현됨

