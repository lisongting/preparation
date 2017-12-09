# 23种设计模式

<h3 id="index">目录</h3>

[六大设计原则](#六大设计原则)

1.创建型

* [单例模式](#单例模式)

* [工厂方法模式](#工厂方法模式)

* [抽象工厂模式](#抽象工厂模式)

* [建造者模式](#建造者模式)

* [原型模式](#原型模式)

  ​



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

1.简单工厂模式

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

工厂类

```java
public class CarFactory{
  public static Car getCar(String name){
    if(name.equals("byd")){
      return new Byd();
    }else if(name.equals("audi0")){
      return new Audi();
    }
  }
}

```



2.工厂方法模式

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

工厂方法模式优点：良好的封装性，结构清晰。可扩展性好。缺点：要写很多类。

一般情况下使用简单工厂模式就可以了。



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
* 抽象工厂模式：不可以增加产品，可以增加产品族(轮胎、座椅、发动机.....)。

[回到目录](#index)

<h2 id="建造者模式">建造者模式</h2>
工厂类模式提供的是创建单个类的模式，而建造者模式则是将各种产品集中起来进行管理，用来创建复合对象，所谓复合对象就是指某个类具有不同的属性。建造者模式(Builder)适用于：用于某个对象的构建过程比较复杂的情况。

```java
public class Bread{
    private String brand;
    public Bread(){}
    public Bread(String brand){
        this.brand = brand;
    }
    public void setBrand(String s){
        brand = s;
    }
    public String getBrand(){
        return brand;
    }
}
public class Cream{
    private String flavor ;
    public Cream(){}
    public Cream(String f){
        flavor = f;
    }
    public void setFlavor(String s){
        flavor = s;
    }
    public String getFlavor(){
        return flavor;
    }
}

public class Fruit{
    private String name;
    public Fruit(){}
    public Fruit(String name){
        this.name = name;
    }
    public void setName(String name){
        this.name = name;
    }
}

public class Cake{
    private Bread bread;
    private Cream cream;
    private Fruit fruit;
    public Cake(){}
    public void setBread(Bread b){
        bread = b;
    }
    public void setCream(Cream c){
        cream = c;
    }
    public void setFruit(Fruit f){
        fruit = f;
    }
    public Bread getBread(){
        return bread;
    }
    public Cream getCream(){
        return cream;
    }
    public Fruit getFruit(){
        return fruit;
    }
}
```

接口：
```java
public interface Builder{
    Bread buildBread();
    Fruit buildFruit();
    Cream buildCream();
}

//用于组装对象
public interface Director{
    Cake createCake();
}
```
接口实现类
```java
//这个CakeBuilder就可以自定义，进行各种不同的实现，制作出来的蛋糕因此口味也不同
public class CakeBuilder implements Builder{
    public Bread buildBread(){
        return new Bread("xxx-brand");
    }
    public Fruit buildFruit(){
        return new Fruit("strawberry");
    }
    public Cream buildCream(){
        return new Cream("normal-sweet");
    }
}

public class CakeDirector implements Director{
    private Builder builder;
    public CakeDirector(Builder b){
        builder = b;
    }
    Cake createCake(){
        Cake cake = new Cake();
        cake.setBread(builder.createBread());
        cake.setFruit(builder.createFruit());
        cake.setCream(builder.createCream());
        return cake;
    }
}
```
使用
```java
CakeDirector director = new CakeDirector(new CakeBuilder());
Cake c = director.createCake();
```

以下这种是另一种我觉得不错的建造者模式：
(仿照OKHttp中的建造者模式)
```java
public class Request {
    private String url;
    private String method;

    public Request(){}    
    public String getUrl(){
        return url;
    }
    public void  setUrl(String s){
        this.url = s;
    }
    public String getMethod(){
        return method;
    }
    public void setMethod(String method){
        this.method = method;
    }

    public static class Builder(){
        private Request request = new Request();
        private static final String DEFAULT_URL = "http://localhost/resource";
        private static final String DEFAULT_MEHTOD = "GET"
        public Builder(){}
        public Builder setUrl(String url){
            request.setUrl(url);
            return this;
        }
        public  Builder setMethod(String method){
            request.setMethod(method);
            return this;
        }
        public Request build(){
            if(request.getMethod()==null){
                request.setMethod(DEFAULT_MEHTOD);
            }
            if(request.getUrl()==null){
                request.setUrl(DEFAULT_URL);
            }
            return request;
        }

    }
}
```


[回到目录](#index)

<h2 id="原型模式">原型模式</h2>
原型模式(prototype)也叫克隆模式、拷贝模式。
原型模式的适用场景：当通过new产生一个对象时，需要非常繁琐的数据准备或访问权限，则可以使用原型模式。
实际就是java中的clone操作，以某个对象为原型，复制出新的对象。
```java
public class Sheep implements Cloneable{
    private String name;
    private Date birthday ;
    public Sheep(){}
    public Sheep(String name,Date date){
        this.name = name;
        this.date = date;
    }
    protected Object  clone() throws CloneNotSupportedException{
        Object objct = super.clone();
        return object;
    }
    public void setName(String name){
        this.name = name;
    }
    public void setBirthday(Date date){
        this.birthday  = date;
    }
    public String getName(){
        return name;
    }
    public int getDate(){
        return birthday;
    }
}
```

以上的这种是属于浅克隆，当出现下面这样的代码时就会有问题:
```java
Date d = new Date(111111111111L);
Sheep s1 = new Sheep("dorly",d);
Sheep s2 = s1.clone();
d.setTime(2222222222222L);
```
这里修改了d之后，s1中的birthday 和s2中的birthday 都被修改了。因为s1和s2指向的是同一个date对象。
因此，使用原型模式要使用深克隆：
```java
public class Sheep implements Cloneable{
    private String name;
    private Date birthday ;
    public Sheep(){}
    public Sheep(String name,Date date){
        this.name = name;
        this.date = date;
    }
    protected Object  clone() throws CloneNotSupportedException{
        Object objct = super.clone();
        //深克隆，将其内部属性进行拷贝
        Sheep s = (Sheep) object;
        s.birthday = this.birthday.clone();
        return object;
    }
    public void setName(String name){
        this.name = name;
    }
    public void setBirthday(Date date){
        this.birthday  = date;
    }
    public String getName(){
        return name;
    }
    public int getDate(){
        return birthday;
    }
}
```

[回到目录](#index)

<h2 id="适配器模式">适配器模式</h2>
