# Java의 메모리 구조 (Stack & Heap)

- Java의 메모리 영역은 메서드 영역(Method/Static Area), 힙 영역(Heap Area), 스택 영역(Stack Area), 네이티브 메서드 영역(Native Method Area), PC 레지스터로 구성됨

### Method Area (Static Area)
- 클래스 로더에 의해 로드된 클래스의 로딩 정보(메타데이터), 정적 변수, 상수 풀(Constant Pool), 클래스 메서드 및 생성자 코드가 저장되는 공간
- 모든 스레드에게 공유되는 영역이며, 런타임 시에 생성됨 
- JVM(프로세스)이 종료될 때까지 존재함

> **참고**  
> - [정적 변수와 상수의 차이점](https://github.com/sseung416/TIL/blob/main/Java/정적%20변수(static)와%20상수(static%20final)의%20차이.md)
> - [상수 풀(Constant Pool)]()

### Heap Area
- 객체와 배열이 저장되는 공간
- JVM 내에서 가장 큰 메모리 영역이며, 모든 스레드에게 공유됨
- 가비지 컬렉터가 관리하는 영역으로, 참조에 따라 사용되지 않는 객체를 자동으로 정리함
- 오버플로우 시, `java.lang.OutOfMemoryError : Java Heap Space` 발생

### Stack Area
- 각 스레드마다 개별적으로 존재하는 공간
- 로컬 변수나 호출된 메서드의 상태 등이 스택 프레임(Stack Frame)이라는 형태로 저장됨
- LIFO(Last In First Out) 구조로, 메서드가 호출되면 스택 프레임이 새롭게 생성되고 메서드가 종료되면 스택 프레임이 제거됨
- 오버플로우 시, `java.lang.StackOverFlowError` 발생

#### Stack Frame
- 자바 메서드 호출 시 생성되는 메모리 영역으로, 메서드의 실행 상태를 저장하고 관리하는 역할
- 지역 변수, 반환 값, 예외 처리 정보를 저장 

[//]: # (todo: 스택 프레임의 구성요소)

### Native Method Area
- 자바가 아닌 네이티브 메서드(C, C++ 등)를 호출할 때 사용되는 스택 영역
- 각 스레드마다 별도로 존재

### PC Register
- 현재 실행중인 자바 바이트코드 주소를 저장하는 레지스터
- 각 스레드마다 별도로 존재

### 기타
- 이해에 도움되는 글: [☕ 그림으로 보는 자바 코드의 메모리 영역(스택 & 힙)](https://inpa.tistory.com/entry/JAVA-☕-그림으로-보는-자바-코드의-메모리-영역스택-힙#)