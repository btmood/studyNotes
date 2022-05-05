## 设计原则

### 单一职责原则（Single Responsibility Principle）

一个类或者模块只负责完成一个职责（或者功能）

但是评价一个类的职责是否足够单一，我们并没有一个非常明确的、可以量化的标准。需要根据具体的场景去判断。

e.g. userInfo数据怎样算是单一？

类的职责也不是一定越单一越好，实际上，不管是应用设计原则还是设计模式，最终的目的还是提高代码的可读性、可扩展性、复用性、可维护性等。我们在考虑应用某一个设计原则是否合理的时候，也可以以此作为最终的考量标准。

e.g. 序列化和反序列化是否应该拆分成两个类

### 开闭原则（Open Closed Principle）

软件实体（模块、类、方法等）应该“对扩展开放、对修改关闭”。

添加一个新的功能应该是，在已有代码基础上扩展代码（新增模块、类、方法等），而非修改已有代码（修改模块、类、方法等）。

**修改代码就意味着违背开闭原则吗？**

同样一个代码改动，在粗代码粒度下，被认定为“修改”，在细代码粒度下，又可以被认定为“扩展”。比如，改动一，添加属性和方法相当于修改类，在类这个层面，这个代码改动可以被认定为“修改”；但这个代码改动并没有修改已有的属性和方法，在方法（及其属性）这一层面，它又可以被认定为“扩展”。

添加一个新功能，不可能任何模块、类、方法的代码都不“修改”，我们要做的是尽量让修改操作更集中、更少、更上层，尽量让最核心、最复杂的那部分逻辑代码满足开闭原则。

**扩展代码的方法论**

多态、依赖注入、基于接口而非实现编程，以及大部分的设计模式（比如，装饰、策略、模板、职责链、状态等）

**如何运用开闭原则**

最重要的是对业务有全面的认识，能够预留扩展点

对于一些比较确定的、短期内可能就会扩展

对于远期不确定的就不用留扩展点了，到时候再去重构代码

**扩展性和可读性的矛盾**

有些情况下，代码的扩展性会跟可读性相冲突。

在某些场景下，代码的扩展性很重要，我们就可以适当地牺牲一些代码的可读性；在另一些场景下，代码的可读性更加重要，那我们就适当地牺牲一些代码的可扩展性。

### 里式替换原则（Liskov Substitution Principle）

子类对象（object of subtype/derived class）能够替换程序（program）中父类对象（object of base/parent class）出现的任何地方，并且保证原来程序的逻辑行为（behavior）不变及正确性不被破坏。

说起来其实很简单，就是子类重写父类方法以后，并不会改变父类方法的功能，直接用子类方法替换父类方法也能实现同样的功能，不会报错

其实里氏替换就是对多态加了一些限制，多态能让子类作为参数传入父类型的参数中，里氏替换要求子类方法和父类同名方法必须功能一直，只能在方法上做扩展，而不能改变功能

**违背里氏替换？**

1.子类违背父类声明要实现的功能

2.子类违背父类对输入、输出、异常的约定

3.子类违背父类注释中所罗列的任何特殊说明

### 接口隔离原则（Interface Segregation Principle）

客户端不应该被强迫依赖它不需要的接口。其中的“客户端”，可以理解为接口的调用者或者使用者。客户端不应该依赖它不需要的接口，即**一个类对另一个类的依赖应该建立在最小的接口**上

**什么是接口？**

- 一组 API 接口集合

比如一个user类有增删改查操作，但是用户端只有查，那么就不能让用户端继承增删改的api，要单独继承

可以把增删改查拆分成两个接口，因为接口是可以多实现的

- 单个 API 接口或函数

某个count()方法包含了各种各样的计算，但是调用只需要用到其中一部分计算，那么就要将这个方法拆分开来，说起来似乎有点像单一职责原则。

但是可能调用方需要用到count()方法中所有功能，那么这个方法就符合接口隔离原则了

- OOP 中的接口概念

目的就是为了设计功能单一的接口，可以通过接口能多实现的特性来完成多功能的导入。也能避免一个类实现了一个庞大的接口，导致导入了很多不必要的功能

### 依赖反转原则（DIP）

**什么是控制反转IOC**

控制反转并不是一种具体的实现技巧，而是一个比较笼统的设计思想，一般用来指导框架层面的设计。

这里的“控制”指的是对程序执行流程的控制，而“反转”指的是在没有使用框架之前，程序员自己控制整个程序的执行。在使用框架之后，整个程序的执行流程可以通过框架来控制。流程的控制权从程序员“反转”到了框架。

**什么是依赖注入DI**

依赖注入跟控制反转恰恰相反，它是一种具体的编码技巧。

不通过 new() 的方式在类内部创建依赖类对象，而是将依赖的类对象在外部创建好之后，通过构造函数、函数参数等方式传递（或注入）给类使用。

**什么是依赖注入框架DI Framework**

通过依赖注入框架提供的扩展点，简单配置一下所有需要创建的类对象、类与类之间的依赖关系，就可以实现由框架来自动创建对象、管理对象的生命周期、依赖注入等原本需要程序员来做的事情。

**什么是依赖反转原则**

高层模块（high-level modules）不要依赖低层模块（low-level）。高层模块和低层模块应该通过抽象（abstractions）来互相依赖。除此之外，抽象（abstractions）不要依赖具体实现细节（details），具体实现细节（details）依赖抽象（abstractions）。


所谓高层模块和低层模块的划分，简单来说就是，在调用链上，调用者属于高层，被调用者属于低层。在平时的业务代码开发中，高层模块依赖底层模块是没有任何问题的。实际上，这条原则主要还是用来指导框架层面的设计，跟前面讲到的控制反转类似。

Tomcat 是运行 Java Web 应用程序的容器。我们编写的 Web 应用程序代码只需要部署在 Tomcat 容器下，便可以被 Tomcat 容器调用执行。按照之前的划分原则，Tomcat 就是高层模块，我们编写的 Web 应用程序代码就是低层模块。Tomcat 和应用程序代码之间并没有直接的依赖关系，两者都依赖同一个“抽象”，也就是 Servlet 规范。Servlet 规范不依赖具体的 Tomcat 容器和应用程序的实现细节，而 Tomcat 容器和应用程序依赖 Servlet 规范。

### 迪米特法则（Law of Demeter）

每个模块（unit）只应该了解那些与它关系密切的模块（units: only units “closely” related to the current unit）的有限知识（knowledge）。或者说，每个模块只和自己的朋友“说话”（talk），不和陌生人“说话”（talk）。


不该有直接依赖关系的类之间，不要有依赖；有依赖关系的类之间，尽量只依赖必要的接口（也就是定义中的“有限知识”）

主要用来实现**高内聚、低耦合**

## 设计模式

23种设计模式

- 创建型

常用的有：单例模式、工厂方法模式、抽象工厂模式、建造者模式

不常用的有：原型模式

- 结构型

常用的 有：代理模式、桥接模式、装饰着模式、适配器模式

不常用的有：门面模式、组合模式、享元模式

- 行为型

常用的有：观察者模式、模板模式、策略模式、职责链模式、迭代器模式、状态模式

不常用的有：访问者模式、备忘录模式、命令模式、解释器模式、中介模式



### 单例模式

一个类只允许创建一个对象（或者实例），那这个类就是一个单例类，这种设计模式就叫作单例设计模式，简称单例模式。

**实现单例的注意点**

- 构造函数需要是 private 访问权限的，这样才能避免外部通过 new 创建实例；
- 考虑对象创建时的线程安全问题；
- 考虑是否支持延迟加载；
- 考虑 getInstance() 性能是否高（是否加锁）。

#### 饿汉式（静态变量）

在类加载的时候，instance 静态实例就已经创建并初始化好了，所以，instance 实例的创建过程是线程安全的。不过，这样的实现方式不支持延迟加载（在真正用到 IdGenerator 的时候，再创建实例

```java
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static final IdGenerator instance = new IdGenerator();
  private IdGenerator() {}
  public static IdGenerator getInstance() {
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

#### 饿汉式（静态代码块）

本质上和静态变量是一样的，都是提前加载

```java
public class SingletonTest02 {

  public static void main(String[] args) {
    // 测试
    Singleton instance = Singleton.getInstance();
    Singleton instance2 = Singleton.getInstance();
    System.out.println(instance == instance2); // true
    System.out.println("instance.hashCode=" + instance.hashCode());
    System.out.println("instance2.hashCode=" + instance2.hashCode());
  }
}

// 饿汉式(静态变量)

class Singleton {

  // 1. 构造器私有化
  private Singleton() {}

  // 2.本类内部创建对象实例
  private static Singleton instance;

  static { // 在静态代码块中，创建单例对象
    instance = new Singleton();
  }

  // 3. 提供一个公有的静态方法，返回实例对象
  public static Singleton getInstance() {
    return instance;
  }
}
```

#### 懒汉式（线程安全，同步方法）

懒汉式相对于饿汉式的优势是支持延迟加载。

```java
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static IdGenerator instance;
  private IdGenerator() {}
  public static synchronized IdGenerator getInstance() {
    if (instance == null) {
      instance = new IdGenerator();
    }
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

不过懒汉式的缺点也很明显，我们给 getInstance() 这个方法加了一把大锁（synchronzed），导致这个函数的并发度很低。而这个函数是在单例使用期间，一直会被调用。如果这个单例类偶尔会被用到，那这种实现方式还可以接受。但是，如果频繁地用到，那频繁加锁、释放锁及并发度低等问题，会导致性能瓶颈，这种实现方式就不可取了。

#### 懒汉式（线程安全，同步代码块）

```java
class Singleton {
  private static Singleton instance;

  private Singleton() {}

  public static Singleton getInstance() {
    if (instance == null) {
      synchronized (Singleton.class) {
        instance = new Singleton();
      }
    }
    return instance;
  }
}
```

#### 懒汉式（线程不安全）

实际开发并不推荐用线程不安全的方式

```java
class Singleton {
  private static Singleton instance;

  private Singleton() {}

  // 提供一个静态的公有方法，当使用到该方法时，才去创建 instance
  // 即懒汉式
  public static Singleton getInstance() {
    if (instance == null) {
      instance = new Singleton();
    }
    return instance;
  }
```

#### 双重检测

饿汉式不支持延迟加载，懒汉式有性能问题，不支持高并发。那我们再来看一种既支持延迟加载、又支持高并发的单例实现方式，也就是双重检测实现方式。在这种实现方式中，只要 instance 被创建之后，即便再调用 getInstance() 函数也不会再进入到加锁逻辑中了。所以，这种实现方式解决了懒汉式并发度低的问题。具体的代码实现如下所示：

```java
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static IdGenerator instance;
  private IdGenerator() {}
  public static IdGenerator getInstance() {
    if (instance == null) {
      synchronized(IdGenerator.class) { // 此处为类级别的锁
        if (instance == null) {
          instance = new IdGenerator();
        }
      }
    }
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

这种实现方式有些问题。因为指令重排序，可能会导致 IdGenerator 对象被 new 出来，并且赋值给 instance 之后，还没来得及初始化（执行构造函数中的代码逻辑），就被另一个线程使用了。要解决这个问题，我们需要给 instance 成员变量加上 volatile 关键字，禁止指令重排序才行。

据说，只有很低版本的 Java 才会有这个问题。我们现在用的高版本的 Java 已经在 JDK 内部实现中解决了这个问题（解决的方法很简单，只要把对象 new 操作和初始化操作设计为原子操作，就自然能禁止重排序）。

#### 静态内部类

我们再来看一种比双重检测更加简单的实现方法，那就是利用 Java 的静态内部类。它有点类似饿汉式，但又能做到了延迟加载。

```java
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private IdGenerator() {}

  private static class SingletonHolder{
    private static final IdGenerator instance = new IdGenerator();
  }
  
  public static IdGenerator getInstance() {
    return SingletonHolder.instance;
  }
 
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

SingletonHolder 是一个静态内部类，当外部类 IdGenerator 被加载的时候，并不会创建 SingletonHolder 实例对象。只有当调用 getInstance() 方法时，SingletonHolder 才会被加载，这个时候才会创建 instance。instance 的唯一性、创建过程的线程安全性，都由 JVM 来保证。所以，这种实现方法既保证了线程安全，又能做到延迟加载。

#### 枚举

式通过 Java 枚举类型本身的特性，保证了实例创建的线程安全性和实例的唯一性。

```java
public enum IdGenerator {
  INSTANCE;
  private AtomicLong id = new AtomicLong(0);
 
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

#### 单例的缺点

我们在项目中使用单例，都是用它来表示一些全局唯一类，比如配置信息类、连接池类、ID 生成器类。单例模式书写简洁、使用方便，在代码中，我们不需要创建对象，直接通过类似 IdGenerator.getInstance().getId() 这样的方法来调用就可以了。但是，这种使用方法有点类似硬编码（hard code），会带来诸多问题。

1. 使用单例意味着放弃了面向对象的特点，比如多态、继承

2. 单例会隐藏类之间的依赖关系，降低可读性

3. 有参构造的单例如何实现？

#### 单例的作用范围？

单例模式是进程内唯一的，也就是说单例也是在线程内和线程间唯一的

**这里可能和jvm有关**

#### 集群的单例模式？

我们需要把这个单例对象序列化并存储到外部共享存储区（比如文件）。进程在使用这个单例对象的时候，需要先从外部共享存储区中将它读取到内存，并反序列化成对象，然后再使用，使用完成之后还需要再存储回外部共享存储区。

为了保证任何时刻，在进程间都只有一份对象存在，一个进程在获取到对象之后，需要对对象加锁，避免其他进程再将其获取。在进程使用完这个对象之后，还需要显式地将对象从内存中删除，并且释放对对象的加锁。按照这个思路，

### 工厂模式

工厂模式分为三种更加细分的类型：简单工厂、工厂方法和抽象工厂