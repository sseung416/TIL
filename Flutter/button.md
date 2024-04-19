# Button

## Button Style 지정하기 - styleFrom()
- `styleFrom()` 함수를 통해 Button의 style를 적용할 수 있음
- Button 종류에 따라 `[button type].styleFrom()` 함수를 정의하면 됨
  - `ElevatedButton.styleFrom()`, `OutlinedButton.styleFrom()`, `TextButton.styleFrom()`

### 주요 프로퍼티
- 자주 사용할 것 같은 것만 정리함

| name          | description |
|---------------|-------------|
| `shadowColor` |      그림자 색상 설정      |
| `backgroundColor`   |     버튼 background 색상 설정        |
| `shape`       |  버튼 shape 설정           |
| `fixedSize`   |  버튼 사이즈(width, height) 설정           |

### 예제
```dart
ElevatedButton(
  style: ElevatedButton.styleFrom(
    shadowColor: Colors.transparent,
    backgroundColor: const Color(0xffe30084),
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
    fixedSize: const Size(double.infinity, 52),
    padding: const EdgeInsets.symmetric(horizontal: 16),
  ),
  child: ...,
)
```

### 둥근 Button 만들기
```dart
ElevatedButton(
  style: ElevatedButton.styleFrom(
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
  )
  child: ...
)
```

### Button 사이즈 지정하기
```dart
ElevatedButton(
  style: ElvatedButton.styleFrom(
    fixedSize: const Size(double.infinity, 52), // width, height
  )
  child: ...
)
```
