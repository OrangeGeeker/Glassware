# Singleton 模式

## 相关模式

在以下模式种，通常只会生成一个实例

- AbstractFactory 模式
- Builder 模式
- Facade 模式
- Prototype 模式

## 使用场景考虑

- 该类占用较多的资源，如线程、IO、网络请求等
- 该类的数据是全局的、共享的
- 该类的实例生命周期应该是全局的，在application整个生命周期中都会用到的

## Double Check Lock 单例

```java
private volatile static Singleton singleton = null;

private Singleton() {
}

public static Singleton getInstance() {
    if (singleton == null) {//第一次检查
        synchronized (Singleton.class) {//lock
            if (singleton == null) {//第二次检查
                singleton = new Singleton();
            }
        }
    }
    return singleton;
}
```

- volatile 关键字，保证 singleton 对象每次从主内存读取，避免由于 java 内存模型带来不必要的麻烦
- 双校验 null 单同步，避免每次调用都需要同步，同时保证线程安全。

## 静态内部类单例

```java
private Singleton() {
}

private static class SingletonHolder{
    private static Singleton singleton = new Singleton();
}

public static Singleton getInstance() {
    return SingletonHolder.singleton;
}
```

- 私有内部静态类，利用了加载外部类的时候内部类不会立即被加载的特性延迟加载
- 线程安全，延迟加载，建议使用

## 普通 static field

```java
public class Singleton{
    private static final Singleton INSTANCE = new Singleton()

    private Singleton(){
        
    }

    public static Singleton getInstance(){
        return INSTANCE;
    }
}
```
