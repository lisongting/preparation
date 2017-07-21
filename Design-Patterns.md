# 23种设计模式 

<h3 id="index">目录</h3>

[六大设计原则](#六大设计原则)

1.创建型



* [单例模式](#单例模式)
* [工厂方法模式](#工厂方法模式)
* [抽象工厂模式](#抽象工厂模式)





<h2 id="六大设计原则">六大设计原则</h2> 

1.单一职责原则：一个类只负责一项职责。

2.里氏替换原则：所有引用基类的地方必须能透明地使用其子类的对象。

3.依赖倒置原则：高层模块不应该依赖底层模块，二者都应该依赖其抽象。抽象不应该依赖细节。细节应该依赖抽象。其核心就是**面向接口编程** 。在JAVA中的表现就是：

* 模块间的依赖通过抽象发生，实现类之间不发生直接的依赖关系，其依赖关系是通过接口或抽象类产生的。
* 抽象类或接口不依赖于实现类。
* 实现类依赖接口或抽象类。

4.接口隔离原则：一个类对另一个类的依赖关系应该建立在最小的接口上。

5.迪米特法则：一个对象应该对其他对象保持最少的了解。

6.开闭原则：一个类、模块和函数应该对扩展开放，对修改关闭。

[回到目录](#index)

<h2 id="单例模式">单例模式</h2> 

核心作用:保证一个类只有一个实例，并且提供一个访问该实例的全局访问点。

单例模式的优点：

* 由于单例模式只产生一个实例，减少了系统性能开销，当一个对象的产生需要比较多的资源时，如读取配置、产生其他依赖对象时，则可以使用在应用启动时直接产生一个单例对象，然后永久驻留内存的方式来解决。
* 单例模式可以在系统设置全局的访问点，优化环境共享资源访问，例如可以设计一个单例类，负责所有数据表的映射处理。

常见的五种单例模式实现方式：

* **饿汉式**（线程安全，调用效率高，但是不能延时加载）
* **懒汉式**  （线程安全，调用效率不高，但是可以延时加载）
* 静态内部类式（线程安全，调用效率高，可以延时加载）
* 枚举单例（线程安全，调用效率高，不能延时加载）
* 双重检测锁式（不建议使用）

1.饿汉式(单例对象立即加载)：

```java
public class Singleton1{
  private static Singleton1 instance = new Singleton1();
  //构造器私有化
  private Singleton1(){
  }
  public static Singleton1 getInstance(){
    return instance;
  }
}
```

2.懒汉式(单例对象延迟加载)：

```java
public class Singleton2{
  private static Singleton2 instance;
  private Singleton(){
  }
  //由于可能会有多个线程调用，所以要加synchronized
  public static synchronized Singleton getInstance(){
    if(instance == null){
      instance = new Singleton();
    }
    return instance;
  }
}
```

3.静态内部类式（兼备了并发高效调用和延迟加载的优势）：

线程安全：JVM进行类加载的过程天然就是线程安全的。

懒加载：第一次去初始化Singlton3类的时候，并不会立即初始化SingletonInstance类，只有在真正调用`getInstance()` 时，才去加载和初始化SingletonInstance。

调用效率：高。

```java
public class Singleton3{
  
  //在静态内部类里面进行
  private static class SingletonInstance{
    private static final Singleton3 instance = new Singleton3();
  }
  public static Singleton3 getInstance(){
    return SingletonInstance.instance;
  }
  private Singleton3(){
  }
}
```

4.枚举单例：

枚举可以避免反射和反序列化的漏洞，调用效率也高，但是不能延时加载。

```java
public enum Singleton4{
  //这个枚举元素，本身就是单例对象。
  INSTANCE;
  //可以使用各种自定义的操作方法
  public void operation(){  
  }
  public int doSomething(){
  }
}
```

5.双重检测锁(由于编译器优化原因和JVM底层内部模型原因，偶尔会出问题)：

```java
public class Singleton5{
  private static Singleton5 instance = null;
  private Singleton5(){
  }
  public static Singleton5 getInstance(){
    //如果instance为null，则进行同步。
    if(instance == null){
      Singleton5 s;
      synchronized(Singleton5.class){
        s = instance;
        if(s == null){
          synchronized(Singleton5.class){
            if(s == null){
              s = new Singleton5();
            }
          } 
          instance = s;
        }
      }
    }
    return instance;
  }
}
```

[回到目录](#index)

<h2 id="工厂方法模式">工厂方法模式</h2> 

接口：Car

```java
public interface Car(){
  void run();
}
```

Car的实现类：

```java
public class Audio implements Car{
  public void run(){
    System.out.println("奥迪在跑");
  }
}
public class Byd implements Car{
  public void run(){
    System.out.println("比亚迪在跑");
  }
}
```

接口：CarFactory

```java
public interface CarFactory{
  Car createCar();
}
```

CarFactory的实现类：

```java
public class AudiFactory implements CarFactory{
  public Car createCar(){
    return new Audi();
  }
}
```

```java
public class BydFactory implements CarFactory{
  public Car createCar(){
    return new Byd();
  }
}
```

在使用的时候就可以：

```java
Car car1 = new AudiFactory().createCar();
Car car2 = new BydFactory().createCar();
```

工厂方法模式优点：良好的封装性，结构清晰。可扩展性好。

[回到目录](#index)

<h2 id="抽象工厂模式">抽象工厂模式</h2>

抽象工厂模式：为创建一组相关或相互依赖的对象提供一个接口，而无需指定他们的具体类。抽象工厂模式工厂方法模式的优点。

```java
public interface Car{
  
}
```

```java
public interface Engine{
  void run();
  void start();
}

public class LuxuryEngine implements Engine{
  public void run(){
    System.out.println("转的快");
  }
  public void start(){
    System.out.println("启动快，可自动启停");
  }
}
public class LowEngine implements Engine{
  public void run(){
    System.out.println("转的慢");
  }
  public void start(){
    System.out.println("启动慢，不可自动启停");
  }
}
```

```java
public interface Seat{
  void massaeg();
}

public class LuxurySeat implements Seat{
  public void massage(){
    System.out.println("可以自动按摩");
  }
}

public class LowSeat implements Seat{
  public void massage(){
    System.out.println("不可按摩");
  }
}
```

```java
public interface Tyre{
  void revolve();
}

public class LuxuryTyre implements Tyre{
  public void revolve(){
    System.out.println("磨损慢")
  }
}
public class LowTyre implements Tyre{
  public void revolve(){
    System.out.println("磨损快")
  }
}
```

```java
public interface CarFactory{
  Engine createEngine();
  Seat createSeat();
  Tyre createTyre();
}
```

```java
public class LuxuryCarFactory implements CarFactory{
  public Engine createEngine(){
    return new LuxuryEngine();
  }
  public Seat createSeat(){
    return new LuxurySeat();
  }
  public Tyre createTyre(){
    return new LuxuryTyre();
  }
}
public class LowCarFactory implements CarFactory{
  public Engine createEngine(){
    return new LowEngine();
  }
  public Seat createSeat(){
    return new LowSeat();
  }
  public Tyre createTyre(){
    return new LowTyre();
  }
}
```

几种工厂模式比较：

* 简单工厂模式：虽然某种程度不符合设计原则，但实际使用最多。
* 工厂方法模式：不修改已有类的前提下，通过增加新的工厂类实现扩展。
* 抽象工厂模式：不可以增加产品，可以增加产品族。

[回到目录](#index)