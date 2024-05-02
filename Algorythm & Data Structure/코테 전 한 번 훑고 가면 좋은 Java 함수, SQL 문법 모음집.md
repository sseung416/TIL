# 코테 전 한 번 훑고 가면 좋은 Java 함수, SQL 모음집

## Java
### 문자열
#### 대문자, 소문자 변환
- `"string".toUpperCase()`
- `"string".toLowerCase()`

#### 문자열 뒤집기
- `StringBuilder(str).reverse().toString()`
- 위 방식 혹은 직접 반복문을 통해 뒤집어야 함

### 진수 변환
- 10진수 -> N진수: `Integer.toString(number, radix)`
- N진수 -> 10진수: `Integer.parseInt(str, n)`

### 내림차순 정렬
- `Arrays.sort(arr, Collections.reverseOrder());`

### Array to List, List to Array
#### Array to List
- `Arrays.asList(arr)` 
- 위 방식으로 변환 시 ImmutableList로 초기화되기 때문에 변경이 필요하면 아래 방식을 사용하자
  ```java
  List<T> list = new ArrayList<>();
  list.addAll(Arrays.asList(arr));  
  ```
#### List to Array
- List to Array는 대부분 스트림써서 처리하니, 그냥 단순 반복문 사용하자

### 이것만 알면 자바 자료구조 문법은 끝
#### Queue
```
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);  - O(1)
queue.poll();  - O(1)
queue.peek();  - O(1)
queue.clear();
queue.isEmpty();
queue.size();
queue.contains(0);
```
#### Deque
```
Deque<Integer> dq = new LinkedList<>();
dq.offer(1); / dq.offerFirst(1); / dq.offerLast(1); - O(1)
dq.poll(); / dq.pollFirst(); / dq.pollLast(); - O(1)
dq.peek(); / dq.peekFirst(); / dq.peekLast(); - O(1)
dq.clear();
dq.isEmpty();
dq.size();
dq.contains(0);
```
#### PriorityQueue
```
PriorityQueue<Integer> pq = new PriorityQueue<>();
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
pq.offer(1); - O(log n)
pq.poll(); - O(log n)
pq.peek(); - O(1)
pq.clear();
pq.isEmpty();
pq.size();
pq.contains(0);
```
#### Stack
```
Stack<Integer> s = new Stack<>();
s.push(0); - O(1)
s.pop(); - O(1)
s.peek(); - O(1)
s.clear();
s.empty();
s.size();
s.contains(0);
```
#### HashMap
```
HashMap<Integer, String> map = new HashMap<>();
map.put(1, "1"); 평균 - O(1), 최악 - O(N)
map.remove(1); 평균 - O(1), 최악 - O(N)
map.clear();
map.get(1); 평균 - O(1), 최악 - O(N)
map.isEmpty();
map.containsKey(1);
map.containsValue("2");
```
#### Map entry
```
Iterator<Entry<Integer, String>> entries = map.entrySet().iterator();
while(entries.hasNext()){
Map.Entry<Integer, String> entry = entries.next();
System.out.println("[Key]:" + entry.getKey() + " [Value]:" +  entry.getValue());
}

Iterator<Integer> keys = map.keySet().iterator();
while(keys.hasNext()){
int key = keys.next();
System.out.println("[Key]:" + key + " [Value]:" +  map.get(key));
}
```
#### ArrayList
```
List<Integer> lst = new ArrayList<>();
lst.get(1); - O(1)
lst.set(1, 1); - O(1)
lst.add(1); - O(1)
lst.remove(1); - O(n)
lst.add(1, 1); - O(n)
lst.contains(1); - O(n)
lst.indexOf(1); - O(n)
lst.clear();
list.size();
```
#### LinkedList
```
List<Integer> lst = new LinkedList<>();
lst.offer(0);  - O(n)
lst.add(0, 0);  - O(n)
lst.offerFirst(0); - O(1)
lst.offerLast(0); - O(1)
lst.pollFirst(); - O(1)
lst.pollLast(); - O(1)
lst.poll();  - O(n)
lst.get(0); - O(n)
lst.contains(1); - O(n)
lst.indexOf(1); - O(n)
lst.clear();
list.size();
```
#### HashSet
```
HashSet<Integer> set = new HashSet<>();
set.add(1); - 여러개면 추가는 한번만
set.remove(); - 없어도 에러 없음
set.contains(1);
set.iterator();
```
#### TreeSet
```
TreeSet<Integer> set = new TreeSet<>();
set.add(value);
set.remove();
set.clear();
set.iterator();
set.first(); - 정렬 순서 기준 맨 앞 원소
set.last(); - 정렬 순서 기준 맨 뒤 원소
set.pollFirst(); - 제일 앞 원소 삭제 후 해당 원소 반환
set.pollLast(); - 제일 뒤 원소 삭제 후 해당 원소 반환
set.higher(value); - value보다 큰 원소들 중 최솟값 없으면 null
set.lower(value); - value보다 작은 원소들 중 최댓값 없으면 null
set.isEmpty();
set.subSet(value1, value2) - value1 ~ value2 범위 내 검색
set.contains(value);
```

<br/>

## SQL
### 문법 순서
- SELECT ~ FROM
- JOIN ~ ON ~
- WHERE ~
- GROUP BY ~
- HAVING ~
- ORDER BY ~

### LIKE: 문자열 패턴 검색
- %: 모든 문자, _: 한 글자
- '_강원도%', '%강원도%' 등 조합 ㄱㄴ
- SELECT * FROM TABLE WHERE NAME LIKE '강원도%'; (강원도로 시작하는 NAME 칼럼의 데이터를 출력)

### DATE_FORMAT: 날짜 형식 변경
- 날짜 형식을 설정하지 않을 때의 default=0000-00-00 00:00:00
- DATE_FORMAT(값, 형식)
- `SELECT DATE_FORMAT(date_col, '%Y-%m-%d') FROM table;`: 날짜 값만 출력
  - 반드시 **년도의 Y는 대문자**로 작성해야 함, 아니면 뒤 2자리만 잘려서 출력됨
- 자세한 형식은 [해당 블로그](https://bramhyun.tistory.com/28) 확인 
- DATE(값): '연도-월-일'로 변환 
- YEAR(값): 연도 반환 
- MONTH(값): 월 반환 
- TIME(값): 시간 반환 
- HOUR(값)

### 날짜 비교하기
- DATE_ADD(기준, 값, 날짜): 날짜 더하기
- DATE_SUB()
- DATEDIFF(날짜1, 날짜2): 두 기간 사이 일수 계산 (날짜1 - 날짜2)
- TIMEDIFF(날짜+시간1, 날짜+시간2): 두 기간 사이 일수 계산, 시간 포함 (1 - 2)

### NULL 처리하기
#### IS NULL
- NULL 값인지 비교
- `~ WHERE id IS NULL`
#### IS NOT NULL
- NULL 값이 아닌지 비교
- `~ WHERE id IS NOT NULL`
#### IFNULL 
- 해당 컬럼이 NULL일 때, 다른 값으로 출력하도록 하는 함수
- `SELECT IFNULL(컬럼, 대체 값) FROM table`

### IF, CASE: 조건문
- IF(컬럼, 참일 때 값, 거짓일 때 값)
- CASE
    ```
    CASE
      WHEN 조건1 THEN 반환값1
      WHEN 조건2 반환값2
      ELSE 반환값
    END
    ```

### 문자열 자르기
- LEFT(값, 자를 길이)
- SUBSTR(값, 시작 위치, 자를 길이)
- RIGHT(값, 길이)
- ex) `SELECT LEFT(name, 3) FROM student;`

### ORDER BY: 데이터 정렬
- WHERE문 뒤에 위치
- `SELECT * FROM TABLE ORDER BY ID;`: ID 칼럼 기준으로 오름차순 정렬
- `SELECT * FROM TABLE ORDER BY ID DESC;`: ID 칼럼 기준으로 내림차순 정렬
- 다중 조건은 쉼표로 구분함
- `~ ORDER BY ID, SCORE;`: ID 기준 오름차순 정렬 -> 값이 같다면 SCORE 기준 정렬
- `~ ORDER BY ID, SCORE DESC;`: ID 기준 오름차순 -> 값 같을 때 SCORE 기준 내림차순 정렬, ID, SCORE 둘 다 내림차순이 아니니 헷갈리지 말 것

### 집계함수
- SUM, AVG, COUNT, MAX, MIN
- 함수명(컬럼명)의 문법 형태
- SELECT COUNT(ID) FROM TABLE;
- COUNT(*)은 NULL(빈 값)을 포함하여 계산하기 때문에 주의, 되도록 COUNT(컬럼명)을 사용하자

### GROUP BY
- 그룹, 주로 집계함수 구현시 같이 사용됨
- GROUP BY는 MAX 함수 지원이 안 됨 [관련 글](https://school.programmers.co.kr/questions/38854)
- 여러 개의 속성 지정이 가능
- `SELECT stu, AVG(score) FROM table GROUP BY stu;`: 학생별 평균 점수 구하기
- `SELECT stu, AVG(score) FROM table GROUP BY stu, sub;`: 학생별 과목의 평균 점수 구하기
#### HAVING
- 그룹의 조건, GROUP BY와 함께 사용함
- `SELECT AVG(score) FROM table GROUP BY id HAVING MAX(score) > 100;`: 최고 점수가 100 이상인 사람의 평균 점수 구하기

### WITH
- 임시 결과 저장, 주로 서브 쿼리를 위해 사용
- 어려운 GROUP BY 문제에 다수 출제됨
- 문제를 쪼개서 풀기 위함
- 형식: WITH(칼럼) as (SELECT ~ )
- [WITH절 사용법 예시](https://codingdog.tistory.com/entry/mysql-with-절-임시-결과를-정의하는-with-절을-알아봅시다), [WITH 활용 추천 문제](https://school.programmers.co.kr/learn/courses/30/lessons/151139?language=mysql)

### 소수점 관리
- ROUND(숫자, [반올림할 소수점 자리]): 반올림, 소수점 기준은 생략 ㄱㄴ(default=1)
- CEILING(숫자): 올림
- FLOOR(숫자): 내림

### 조인
- (INNER) JOIN: 두 테이블 값이 모두 일치하는 데이터만 출력
- [LEFT/RIGHT/FULL] OUTER JOIN: 선택한 테이블의 모든 값 출력 + 조인 조건에 부합하는 상대 테이블의 데이터 출력 (default=FULL)
- CROSS JOIN
- SELF JOIN
- 형식: `SELECT * FROM TABLE1 t1 JOIN TABLE2 t2 ON t1.id = t2.id;`
#### USING
- 두 테이블의 칼럼명이 동일할 때, 두 칼럼이 따로 작성되지 않고 하나로 통합해 작성되는 것
- `~ FROM table1 JOIN table2 USING (id)`

### 기타
- 컬럼명이 띄워쓰여야 할 경우 백틱(``) 사용
  - 특히 ORDER BY는 따옴표('')는 먹히지 않음, 반드시 백틱 사용해야 함!!!