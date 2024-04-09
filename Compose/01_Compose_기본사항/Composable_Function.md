## [01. Composable Function](https://www.youtube.com/watch?v=fFLBCgoHHys)
### @Composable

```kotlin
@Composable
fun Button() { ... }
```

- 구성 가능한 함수, 컴포저블 함수
- UI를 구성하고자 할 때 사용
- 컴포저블은 사이드 이펙트를 허용하지 않음
  - 컴포저블 내부에서 속성이나 전역 변수를 수정하면 안 됨
- 사이드 이펙트를 허용하지 않으므로, 함수의 매개변수 자체가 UI를 구성하게 됨 -> 상태가 UI로 변환

### 재구성 (Recomposition)
- 상태는 곧, UI이기 때문에 상태(매개변수)가 변경될 때마다 UI를 새로 그리게 됨
  - 이를 **재구성(recomposition)**이라 함
- 재구성은 파라미터뿐만 아니라 컴포저블 내부 변수가 변경되도 발생함
  - 물론 변수를 그냥 정의하면 감지하지 못함, `remember`와 `MutableState`를 통해 객체로 래핑해주어야 함
  - `remember`은 값을 유지하는 역할, `MutableState`는 값 변경을 감지하고 재구성을 요청하는 역할을 함
    - 재구성은 기존에 컴파일된 함수를 그대로 재호출하기 때문에 값이 초기화될 수 밖에 없음(변경 사항이 저장되지 않음), `remember`은 그 값을 유지해주는 역할을 함
  ```kotlin
  @Composable
  fun CheckBox() {
    // 이게 정석이지만 실제론 별로 사용되지는 않는 형태
    // getValue, setValue를 호출해주어야 하기 때문에 사용에 불편
    var isChecked: MutableState<Boolean> = remember { mutableStateOf(false) }

    // 위임 프로퍼티를 사용한 형태로, 실무에선 이 형태로 사용
    // MutableState로 래핑되어 있지 않기 때문에 getValue, setValue를 제거할 수 있음! 편리!!
    var isChecked: Boolean by remember { mutableStateOf(false) }
    ...
  }
  ```

> **rememberSaveable**  
> `remember` 함수는 화면 회전 등으로 `Activity`가 파괴되어 재생성되는 경우 값을 보장하지 않는다. `rememberSaveable`의 경우 내부적으로 `Bundle`에 값을 저장하기 때문에 `Activity`가 재생성되어도 값이 보장된다.

### 특징
- Composable 함수는 순서대로 실행되지 않음
  - 우선순위에 따라 하위 목록이 먼저 실행될 수도 있음
- Composable 함수는 병렬로 실행될 수도 있음
- 재구성은 가능한 많이 건너 뜀
  - 그러니까, 최소한의 UI만 업데이트한다는 의미
  - state가 변경되더라도 해당 state에 의존하지 않은 뷰는 재렌더링되지 않음!
- 재구성은 낙관적
  - 기본적으로 재구성을 다음 파라미터 값이 바뀌기 전에 완료되고, 재구성이 완료되기 전에 다음 파라미터 값으로 바뀌게 된다면 재구성을 취소하고 다시 시작할 수 있음
- Composable 함수는 많이 실행될 수 있음
  - 애니메이션이 포함될 수 있기 때문에 프레임 드랍이 발생하지 않도록 Composable 함수가 빠른지 확인해야 함
