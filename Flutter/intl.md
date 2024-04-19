## 화폐 단위 표시하기 ([intl](https://pub.dev/packages/intl/install))

- 여러 format 형식을 지원하는 다트 공식 패키지

### 설치
```
dart pub add intl
```

### 화폐 단위 표기
```dart
final formatter = NumberFormat.currency(locale: 'ko_KR', symbol: '₩');
print(formatter.format(10000)) // 출력: ₩10,000
```
- Extension
```dart
extension NumberFormatExtension on int {
  String formatCurrency({String locale = 'ko_KR', String symbol = ''}) =>
      NumberFormat.currency(locale: locale, symbol: symbol).format(this);
}
```
