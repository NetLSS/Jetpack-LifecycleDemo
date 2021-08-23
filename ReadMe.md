# Jetpack 생명주기-인식 예제 프로젝트

- Activity 와 Fragment 는 기본적으로 LifecycleOwner 인터페이스를 구현하고 있음

## Observer 정의 하기

```kotlin
class DemoObserver : LifecycleObserver {
    private val LOG_TAG = "LSS:DemoObserver"

    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    fun onResume() {
        Log.i(LOG_TAG, "ON_RESUME")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    fun onPause() {
        Log.i(LOG_TAG, "ON_PAUSE")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun onCreate() {
        Log.i(LOG_TAG, "ON_CREATE")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun onStart() {
        Log.i(LOG_TAG, "ON_START")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun onSTOP() {
        Log.i(LOG_TAG, "ON_STOP")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
    fun onDestroy() {
        Log.i(LOG_TAG, "ON_DESTROY")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_ANY)
    fun onAny(owner: LifecycleOwner, event: Lifecycle.Event) {
        Log.i(LOG_TAG, "ON_ANY : ${owner.lifecycle.currentState.name}")
    }
}
```

## Observer 달기

```kotlin
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        viewModel = ViewModelProvider(this).get(MainViewModel::class.java)

        lifecycle.addObserver(DemoObserver()) // 옵저버 달아버리기
    }
```

### Log Test

- 최초 실행

```shell
I/LSS:DemoObserver: ON_CREATE
I/LSS:DemoObserver: ON_ANY : CREATED
I/LSS:DemoObserver: ON_START
I/LSS:DemoObserver: ON_ANY : STARTED
I/LSS:DemoObserver: ON_RESUME
I/LSS:DemoObserver: ON_ANY : RESUMED
```

- 회전

```shell
DemoObserver: ON_PAUSE
DemoObserver: ON_ANY : STARTED
DemoObserver: ON_STOP
DemoObserver: ON_ANY : CREATED
DemoObserver: ON_DESTROY
DemoObserver: ON_ANY : DESTROYE
DemoObserver: ON_CREATE
DemoObserver: ON_ANY : CREATED
DemoObserver: ON_START
DemoObserver: ON_ANY : STARTED
DemoObserver: ON_RESUME
DemoObserver: ON_ANY : RESUMED
```

## 생명주기 소유자 정의해보기