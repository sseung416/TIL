# TextInputEditText에 Drawable Click Event 적용하기

- `TextInputEditText`에서 drawable를 적용할 때는 `drawableEnd`/`drawableStart` 대신, `endIconDrawable`/`startIconDrawable`를 사용해야 함
```xml
<com.google.android.material.textfield.TextInputLayout
    ...
    app:endIconDrawable="@drawable/ic_search"
    app:endIconMode="custom">
```
- 클릭 이벤트는 아래와 같이 정의하면 됨
```kotlin
textInputLayout.setEndIconOnClickListener { ... }

textInputLayout.setStartIconOnClickListener { ... }
```