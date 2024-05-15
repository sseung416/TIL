# 정적 변수(static)와 상수(static final)의 차이

### 정적 변수 (static)
```java
public class Student {
    public static int age = 0; 
}
```
- 클래스 로드 시점에 초기화되어, 메서드(static) 영역에 저장됨
- 모든 스레드에서 접근이 가능
- 변경이 가능

### 상수 (static final)
```java
public class Student {
    public static final String FOO_SCHOOL = "푸~학교";
    public static final String BOO_SCHOOL = "부~학교";
}
```
- 컴파일 시점에 초기화되어, 상수 풀(Constant Pool)에 저장됨
- 모든 스레드에서 접근 가능
- `final`이 함께 선언되었기 때문에 변경이 불가능

### 정리
|        | `static`    | `static final`      |
|--------|-------------|---------------------|
| 초기화 시점 | 클래스 로드 시    | 컴파일 타임              |
| 저장 위치  | 메서드 영역      | 상수 풀(Constant Pool) |
| 변경 여부  | O           | X                   |