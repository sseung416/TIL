# 조합 (Combination)

~~조합 문제 왜캐 못 풀어...~~

### 조합 
- **순서와 상관 없이** 나열한 경우의 수
  - 순열의 경우, `1 0`과 `0 1`은 다른 경우의 수이지만 조합의 경우, 동일한 경우의 수로 취급

### 점화식
![img.png](img/combination.png)

- (전체 경우의 수) = (특정 원소를 포함하고 뽑았을 때의 경우의 수) + (특정 원소를 포함하지 않고 뽑았을 때의 경우의 수)

### 구현
- [전체 코드는 여기로...](https://github.com/sseung416/AlgorithmStudy/blob/main/src/Combination.java)
#### 재귀함수
```java
/**
 * 재귀를 사용해 조합을 출력하는 함수
 *
 * @param arr     조합을 구할 원본 배열
 * @param visited 뽑은 여부, 최종 탐색 후 뽑은 조합을 출력할 때 사용
 * @param n       원본 배열의 길이
 * @param r       뽑아야 하는 원소의 개수
 * @param depth   원본 배열에서 뽑을 원소를 정하는 인덱스
 */
public static void combinationByRecursive(int[] arr, boolean[] visited, int n, int r, int depth) {
    // 뽑아야 하는 개수(r)가 0개라면 탐색 종료 후 값 출력
    if (r == 0) {
        print(visited, arr, n);
        return;
    }

    // 모든 원소를 탐색한 것이니 종료
    if (depth == n) {
        return;
    }

    // 특정 원소를 뽑은 경우
    visited[depth] = true;
    combinationByRecursive(arr, visited, n, r - 1, depth + 1);

    // 해당 원소를 뽑지 않은 경우
    visited[depth] = false;
    combinationByRecursive(arr, visited, n, r, depth + 1);
}
```

#### 백트래킹
```java
/**
 * 백트랙킹을 사용해 조합을 출력하는 함수
 *
 * @param start 원소를 뽑을지 말지의 기준점이 되는 인덱스, start보다 인덱스가 작으면 뽑지 않고 크면 뽑음
 * */
public static void combinationByBackTracking(int[] arr, boolean[] visited, int start, int n, int r) {
    // 뽑아야 하는 개수(r)가 0개라면 탐색 종료 후 값 출력
    if (r == 0) {
        print(visited, arr, n);
        return;
    }

    for (int i = start; i < n; i++) {
        // 원소를 뽑음, 현재 원소를 뽑았으니 다음 조합 때 start 인덱스 값을 증가
        visited[i] = true;
        combinationByBackTracking(arr, visited, i + 1, n, r - 1);

        // 원소를 뽑지 않고 다음 순회
        visited[i] = false;
    }
}
```

### 참고
- [조합 알고리즘](https://coding-food-court.tistory.com/117)
- [\[알고리즘\] 조합 (Combination)](https://velog.io/@soyeon207/알고리즘-조합-Combination)
- [조합 Combination (Java)](https://bcp0109.tistory.com/15)