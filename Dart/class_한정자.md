# class 한정자

|                | `abstract` | `base` | `interface` | `final` | `sealed`       |
|----------------|------------|--------|-------------|---------|----------------|
| 생성자            | X          | O      | O           | O       | X (subclass O) |
| 상속(extends)    | O          | O      | X           | X       | O              |
| 구현(implements) | O          | X      | O           | X       | O              |

<!-- https://jake-seo-dev.tistory.com/644 -->