# [Dio](https://pub.dev/packages/dio)

## Interceptor
### Interceptor
- 기본 인터셉터
- `onRequest`, `onResponse`, `OnError`의 함수로 구성된 인터페이스
  - `onResponse`는 Response의 status coder가 200일 때, `onError`는 요청에 실패했을 때(200을 제외한 나머지) 호출됨
- 인터셉터의 코드가 길지 않을 때 `InterceptorWrapper`를 사용하면 더욱 간결하게 구현할 수 있음

### QueuedInterceptor
- 요청의 순서를 보장하는 인터셉터
- 기본 `Interceptor`와 동일하게 `QueuedInterceptorWrapper`를 제공, 사용 방법은 기존 `Interceptor`와 동일
- 기존에는 `Interceptors.lock`을 통해 동시성을 보장했으나 이는 deprecated되고 `QeuedInterceptor`이 새롭게 등장 ([>> Dio Gihub Issue 참고](https://github.com/cfug/dio/issues/1308))

### InterceptorHandler
- Interceptor 구현 시, 응답의 흐름을 제어하기 위해 제공하는 핸들러
- 구성 함수
  - `resolve(response)`: 오류가 해결되었을 때 호출
  - `reject(dioException)`: 오류를 발생하고자 할 때 호출
  - `next(T)`: 다음 작업 진행, 아무것도 정의하지 않았을 때 디폴트로 실행됨