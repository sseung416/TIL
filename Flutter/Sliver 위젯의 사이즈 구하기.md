# Sliver 위젯의 사이즈 구하기

- 기존 위젯의 사이즈 구하는 방법과 조금 다름, `RenderBox` 대신 `RenderSliver`로 형변환해야 함
- `RenderBox`와 마찬가지로, 레이아웃이 모두 완료된 후(그려진 후)에 `getAbsoluteSize()` 호출 가능

```dart

final _key = GlobalKey();

// 반드시 레이아웃이 완료된 후에 호출!!
double _getSliverHeight() {
  final sliver = key.currentContext?.findRenderObject;
  if (sliver == null || sliver is! RenderSliver) return 0;
  return sliver
      .getAbsoluteSize()
      .height;
}

@override
Widget build(BuildContext context) {
  return NestedScrollView(
      headerSliverBuilder: (_, __) =>
      [
        // 크기를 구하려는 위젯에 GolbalKey를 넣어줌
        SliverToBoxAdapter(key: _key, child: ...),
      ],
      ...
  )
}
```

- 좀 더 편하게 사용하기 위해 Extensions으로도 정의해보았음

```dart
extension SliverWidgetKeyExtension on GlobalKey {
  double getSliverHeight() {
    final object = currentContext?.findRenderObject();
    if (object == null || object is! RenderSliver) return 0;
    return object
        .getAbsoluteSize()
        .height;
  }
}
```

<br/>

> **참고!!**
>
> `getAbsoluteSize()` 함수에서 내부적으로 `assert(!debugNeedsLayout);`를 검증하고 있기 때문에 UI를 수정 후 핫 리로드할 때마다 오류가
> 띄워짐  
> 물론 한 번 더 리로드하면 고쳐지는 이슈이고 디버그 모드에서만 작동하는 검증이기 때문에 크게 신경쓸 필요는 없음  
> 다만.. 개발할 때 좀 불편하다는 점.. 이를 고칠 수 있는 방법은 좀 더 찾아봐야 할 듯함