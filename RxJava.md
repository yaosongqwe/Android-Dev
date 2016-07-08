# RxJava

特点: 异步, 响应式, 灵敏, 易泄露

### 基本概念:

Observable: 被观察者，事件源

Observer: 观察者

Subscriber: 观察者

CompositeSubscription: 可以维护Subscriptions列表, 方便在onDestroy()或者onDestroyView()里取消所有的订阅, 防止泄露

##### Subscriber 和 Observer 的关系

Subscriber是实现Observer接口的抽象类, 简单讲 
Subscriber 包含 Observer. 

>不仅基本使用方式一样，实质上，在 RxJava 的 subscribe 过程中，Observer 也总是会先被转换成一个 Subscriber 再使用。所以如果你只想使用基本功能，选择 Observer 和 Subscriber 是完全一样的。它们的区别对于使用者来说主要有两点：
>> 1. onStart(): 这是 Subscriber 增加的方法。它会在 subscribe 刚开始，而事件还未发送之前被调用，可以用于做一些准备工作，例如数据的清零或重置。这是一个可选方法，默认情况下它的实现为空。需要注意的是，如果对准备工作的线程有要求（例如弹出一个显示进度的对话框，这必须在主线程执行）， onStart() 就不适用了，因为它总是在 subscribe 所发生的线程被调用，而不能指定线程。要在指定的线程来做准备工作，可以使用 doOnSubscribe() 方法，具体可以在后面的文中看到。
>> 2. unsubscribe(): 这是 Subscriber 所实现的另一个接口 Subscription 的方法，用于取消订阅。在这个方法被调用后，Subscriber 将不再接收事件。一般在这个方法调用前，可以使用 isUnsubscribed() 先判断一下状态。 unsubscribe() 这个方法很重要，因为在 subscribe() 之后， Observable 会持有 Subscriber 的引用，这个引用如果不能及时被释放，将有内存泄露的风险。所以最好保持一个原则：要在不再使用的时候尽快在合适的地方（例如 onPause() onStop() 等方法中）调用 unsubscribe() 来解除引用关系，以避免内存泄露的发生。
>>3. 参考链接
https://gank.io/post/560e15be2dca930e00da1083

### 线程切换

```
Observable.subscribeOn(Schedulers.io())
Observable.subscribeOn(Schedulers.newThread())
Observable.observeOn(AndroidSchedulers.mainThread())
```

### 常用操作符

```
buffer: 重复收集指定时间内的元素一起发射
```

```
map: 变换每个元素
```

```
flatmap: 将一个Observable 转换为另一个 Observable
```

```
debounce: 发射产生元素后间隔时间内没有其他元素的元素
```

```
filter: 过滤
```

```
interval: 重复间隔时间发送元素
```

```
take: 取出前指定个数的元素
takeLast: 取出后指定个数的元素
```

```
just 从指定元素创建观察者
```

```
combineLatest: 当两个Observables中的任何一个发射了数据时，使用一个函数结合每个Observable发射的最近数据项，并且基于这个函数的结果发射数据
```

```
merg: 合并
```

```
timer: 延时发射
```

```
range: 范围
```

```
connect: 链接两个 Observable
```

### RxBus
定义 RxBus

```
public class RxBus {

    //private final PublishSubject<Object> _bus = PublishSubject.create();

    // If multiple threads are going to emit events to this
    // then it must be made thread-safe like this instead
    private final Subject<Object, Object> _bus = new SerializedSubject<>(PublishSubject.create());

    public void send(Object o) {
        _bus.onNext(o);
    }

    public Observable<Object> toObserverable() {
        return _bus;
    }

    public boolean hasObservers() {
        return _bus.hasObservers();
    }
}
```

定义某个事件
```
public static class TapEvent {}
```

注册事件
```
ConnectableObservable<Object> tapEventEmitter = _rxBus.toObserverable().publish();

....//处理事件

```

发送事件
```
if (_rxBus.hasObservers()) {
            _rxBus.send(new RxBusDemoFragment.TapEvent());
        }
```

### 参考链接:
[Awesome-RxJava] https://github.com/lzyzsd/Awesome-RxJava

[给 Android 开发者的 RxJava 详解] http://gank.io/post/560e15be2dca930e00da1083

[ReactiveX/RxJava文档中文版] https://mcxiaoke.gitbooks.io/rxdocs/content/

[RxJava-Android-Samples] https://github.com/kaushikgopal/RxJava-Android-Samples

[learning-rxjava] https://github.com/meddle0x53/learning-rxjava
