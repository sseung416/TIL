# Container

## Gradient

### 사용법
```dart
Container(
  decoration: BoxDecoration(
    gradient: LinearGradient(
      begin: Alignment.centerLeft,
      end: Alignment.centerRight,
      colors: [Colors.red, Colors.blue],
    ),
  ),
  child: ...
)
```

### 투명 그라디언트를 제공하는 경우에는?
- `Colors.transparent` 대신 `Colors.white.withOpacity(0)`을 사용하자
```dart
LinearGradient(
  begin: Alignment.centerLeft,
  end: Alignment.centerRight,
  colors: [Colors.white.withOpacity(0), Colors.blue],
),
```

## Border

### 위젯에 Border 지정하기
```dart
Container(
  decoration: BoxDecoration(
    border: Border.all( 
      width: 1,
      color: Colors.orange, 
    ),
  ),
  child: ...
)
```

### Border 한 쪽만 지정하기
- `BorderSide`를 사용
```dart
Container(
  decoration: BoxDecoration(
    border: Border( 
      bottom: BorderSide(
        color: Colors.black,
        width: 1,
      )
    ),
  ),
  child: ...
)
```

### 둥근 코너 Border 넣기
```dart
Container(
  borderRadius: BorderRadius.all(Radius.circular(8)),
  border: ...
)
```