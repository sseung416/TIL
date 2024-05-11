# AppWidget onUpdate()에서 WorkManager enqueue 작업 시 무한 갱신되는 이슈

## 문제 상황

`AppWidget`은 브로드캐스트로 구성되어 있기 때문에 네트워크 통신과 같이 무거운 작업을 직접적으로 수행하지 못한다. 
따라서 `WorkManager`를 사용하여 네트워크 통신을 구현하였다.

<br/>

위젯의 토글 버튼을 클릭했을 때, 특정 서비스를 활성하거나 비활성하는 네트워크 요청을 날리고 위젯의 UI를 업데이트해주는 기능을 구현해야 했기 때문에,
`AppWidget`의 `onUpdate()` 콜백에 `WorkManager` 작업을 추가하였다.

```kotlin
class SampleAppWidget : AppWidgetProvider() {

  override fun onUpdate(
    context: Context,
    appWidgetManager: AppWidgetManager,
    appWidgetIds: IntArray,
  ) {
    Log.d(TAG, "onUpdate()")
    appWidgetIds.forEach { appWidgetId ->
      // WorkManager로부터 서비스 활성화 상태 조회를 요청하고 해당 값을 기준으로 위젯 업데이트
      val isEnabled = getServiceEnabled(context)
      val remoteView = constructRemoteViews(context, appWidgetId, isEnabled)
      appWidgetManager.updateAppWidget(appWidgetId, remoteView)
    }
  }
  
  private fun constructRemoteViews(context: Context, appWidgetId: Int, isEnabled: Boolean) =
    RemoteViews(context.packageName, R.layout.layout_sample_widget_provider).apply {
      val toggleButtonPendingIntent = PendingIntent.getBroadcast(
        context,
        0,
        Intent(context, SampleAppWidget::class.java).apply {
          action = ACTION_TOGGLE_BUTTON_CLICKED
          putExtra(EXTRA_APP_WIDGET_ID, appWidgetId)
        },
        PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_IMMUTABLE,
      )
      
      setOnClickPendingIntent(R.id.toggleButton, toggleButtonPendingIntent)
      
      // ... 
    }
  
  private fun getServiceEnabled(context: Context): Boolean {
    val workManager = WorkManager.getInstance(context)
    workManager.enqueue(...)
    
    val isEnabled = /* 어쩌구 저쩌구 처리를 과정을 거쳐서... */
    return isEnabled
  }

  // ...
}
```

[//]: # (todo: 예제 사진!!)

그러나 이를 실행하니 `onUpdate()` 콜백이 무한 호출되는 현상이 발생했다...

## 발생 원인

해당 현상이 발생하는 원인은 아래 Google Issue Tracker에 설명되어 있다.

[>> \[ Disabling component leads to AppWidgetProvider.onUpdate call \]](https://issuetracker.google.com/issues/115575872)

원인을 요약하면 다음과 같다.
- `WorkManager`는 더 이상 보류 중인 작업이 없을 때마다 리시버(`RescheduleReceiver`)를 비활성화함
  - 앱 부팅 속도 및 배터리 성능 향상을 위해서라고 함
- `PackageManager`는 컴포넌트가 활성화되거나 비활성화될 때마다 `PACKAGE_CHANGED` 브로드캐스트를 날림
- `AppWidget`의 구현체인 `AppWidgetServiceImpl`에서는 `PACKAGE_CHANGED`를 리시버에 등록 한 후에 `updateProviderForPackageLocked()`를 호출함
  - 이름에서 유추할 수 있듯이 해당 함수는 `AppWidget`의 `update()`를 요청하는 함수

그래서 위 작업들이 연쇄적으로 호출되면서 `update()` 함수가 무한히 호출되는 것!!  

## 해결 방법

이 문제를 해결하는 방법은 그냥 `onUpdate()` 콜백에서 `WorkManager` 작업 추가(`enqueue()`)를 하지 않으면 된다.  

2018년에 올라 온 이슈지만.. 아직까지 해결되지 않았기 때문에 `onUpdate()` 내부에서는 `WorkManager`에 작업을 추가할 수 없다..  
WorkManager에 작업을 추가하고자 한다면, `onEnabled()`나 `onReceive()`에서 호출하도록 하자.  

> 아니면.. `RescheduleReceiver`가 영원히 종료되지 않는 작업을 추가해주면 되긴 하는데, 이 방법은 앱 부팅 속도와 배터리 성능이 저하될 수 있기 때문에 비추한다.
> 정말 무조건 `onUpdate()`에서 `WorkManager`에 작업을 추가하는 방법 말고는 해결이 안 된다!! 이런 게 아니라면 지양하도록 하자.

[//]: # (todo: ### 코드 수정)
