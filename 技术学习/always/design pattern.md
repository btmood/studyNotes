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



### 单例模式【创建型】

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

### 工厂模式【创建型】

什么时候该用工厂模式？相对于直接 new 来创建对象，用工厂模式来创建究竟有什么好处呢？

工厂模式分为三种更加细分的类型：简单工厂、工厂方法和抽象工厂

#### 简单工厂（Simple Factory）

本来是通过new来创建对象的，但是可能原本模块比较复杂，这个时候把创建对象的代码抽出来放到一个单独的类里，通过这个类创建对象就是简单工厂

#### 工厂方法（Factory Method）

每一个对象都单独写一个工厂类来创建对象，需要增加对象那就增加工厂类就可以了，但是这样也会导致原模块变得复杂，因为要判断出不同的工厂类，所以可以组合简单工厂和工厂方法。创建出一个产生工厂类对象的工厂类



**什么时候简单工厂？什么时候工厂方法模式？**

当对象的创建逻辑比较复杂，不只是简单的 new 一下就可以，而是要组合其他类对象，做各种初始化操作的时候，我们推荐使用工厂方法模式，将复杂的创建逻辑拆分到多个工厂类中，让每个工厂类都不至于过于复杂。而使用简单工厂模式，将所有的创建逻辑都放到一个工厂类中，会导致这个工厂类变得很复杂。


在某些场景下，如果**对象不可复用（不能用map来避免if判断，感觉就是算法中常用的哈希法）**，那工厂类每次都要返回不同的对象。如果我们使用简单工厂模式来实现，就只能选择第一种包含 if 分支逻辑的实现方式。如果我们还想避免烦人的 if-else 分支逻辑，这个时候，我们就推荐使用工厂方法模式。

#### 抽象工厂

工厂方法模式需要针对每一个对象创建一个工厂类，如果有非常多的业务类，就会造成类爆炸。

抽象工厂就是来解决这样的问题的，我们可以让一个工厂负责创建多个不同类型的对象，而不是只创建一种 parser 对象。这样就可以有效地减少工厂类的个数

#### DI容器与工厂模式

spring DI容器的原理

既然我们上面说过每个类都需要hard code一个工厂类，那么spring的工厂类代码不是要爆炸了，其实不是这样，spring使用了**配置文件**加**反射**的方式解决了这个问题。

如下解析beans.xml，并根据class来反射创建对象就好了

```java
配置文件beans.xml
<beans>
   <bean id="rateLimiter" class="com.xzg.RateLimiter">
      <constructor-arg ref="redisCounter"/>
   </bean>
 
   <bean id="redisCounter" class="com.xzg.redisCounter" scope="singleton" lazy-init="true">
     <constructor-arg type="String" value="127.0.0.1">
     <constructor-arg type="int" value=1234>
   </bean>
</bean>
```

### 建造者模式【创建型】

直接说建造者模式可能很生僻，但是`.build()`这样的调用应该能经常看到，只要调用了这个方法那就是使用的建造者模式

我们在创建一个对象的时候构造函数可能有非常多的参数，这些参数也可能有不少依赖关系需要去判断，如果都写在构造函数里，构造函数就会变得很长。

也可以使用set函数设置值，但是依赖关系就无法判断了，以及暴露了set接口。

那么我们就能使用建造者模式来解决这个问题

**与工厂模式的区别**

实际上，工厂模式是用来创建不同但是相关类型的对象（继承同一父类或者接口的一组子类），由给定的参数来决定创建哪种类型的对象。建造者模式是用来创建一种类型的复杂对象，通过设置不同的可选参数，“定制化”地创建不同的对象。

### 原型模式【创建型】

如果对象的创建成本比较大，而同一个类的不同对象之间差别不大（大部分字段都相同），在这种情况下，我们可以利用对已有对象（原型）进行复制（或者叫拷贝）的方式来创建新对象，以达到节省创建时间的目的。这种基于原型来创建对象的方式就叫作原型设计模式（Prototype Design Pattern），简称原型模式。

**何谓创建成本大？**

创建对象申请内存，变量赋值其实完全可以忽略，但是如果创建对象需要用到IO、网络、RPC等成本就大了，这种情况下，我们就可以利用原型模式，从其他已有对象中直接拷贝得到，而不用每次在创建新对象的时候，都重复执行这些耗时的操作。

**复制对象产生新对象？复制的是值开始指针？**

 |

\\|/

**深拷贝和浅拷贝**

文章[https://blog.csdn.net/baiye_xing/article/details/71788741]

浅拷贝：对一个对象进行拷贝时，这个对象对应的类里的成员变量。

对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行值拷贝，也就是将该属性值复制一份给新的对象。因为是两份不同的数据，所以对其中一个对象的该成员变量值进行修改，不会影响另一个对象拷贝得到的数据
对于数据类型是引用数据类型的成员变量(也就是子对象，或者数组啥的)，也就是只是将该成员变量的引用值（引用拷贝【并发引用传递，Java本质还是值传递】）复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成员变量值。
深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。
也就是说浅拷贝对于子对象只是拷贝了引用值，并没有真正的拷贝整个对象。
深拷贝实现思路：

对于每个子对象都实现Cloneable 接口，并重写clone方法。最后在最顶层的类的重写的 clone 方法中调用所有子对象的 clone 方法即可实现深拷贝。【简单的说就是：每一层的每个子对象都进行浅拷贝=深拷贝】

利用序列化。【先对对象进行序列化，紧接着马上反序列化出 】


### 代理模式【结构型】

**什么是？**

代理模式（Proxy Design Pattern）的原理和代码实现都不难掌握。它在不改变原始类（或叫被代理类）代码的情况下，通过引入代理类来给原始类附加功能【**装饰器是增强功能，代理是附加新的功能**】。我们通过一个简单的例子来解释一下这段话。

**代理模式的形式**

代理模式有不同的形式, 主要有三种 **静态代理**、**动态代理** (JDK 代理、接口代理)和 **Cglib** **代理** (可以在内存 动态的创建对象，而不需要实现接口， 他是属于动态代理的范畴)

#### 静态代理

代理类和被代理类实现同样接口，通过接口调用相同方法，代理类中可以增强功能

```java
public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建目标对象(被代理对象)
		TeacherDao teacherDao = new TeacherDao();
		
		//创建代理对象, 同时将被代理对象传递给代理对象
		TeacherDaoProxy teacherDaoProxy = new TeacherDaoProxy(teacherDao);
		
		//通过代理对象，调用到被代理对象的方法
		//即：执行的是代理对象的方法，代理对象再去调用目标对象的方法 
		teacherDaoProxy.teach();
	}
```

#### 动态代理

又称jdk代理、接口代理

被代理类需要实现接口，代理类不需要

代理类会通过java.lang.reflect.Proxy创建出一个代理对象，通过`Proxy.newProxyInstance()`方法增强功能

```java
//给目标对象 生成一个代理对象
   public Object getProxyInstance() {
      
      /* 说明
         public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
                                          
            1. ClassLoader loader ： 指定当前目标对象使用的类加载器, 获取加载器的方法固定
            2. Class<?>[] interfaces: 目标对象实现的接口类型，使用泛型方法确认类型
            3. InvocationHandler h : 事情处理，执行目标对象的方法时，会触发事情处理器方法, 
             会把当前执行的目标对象方法作为参数传入
       */
      return Proxy.newProxyInstance(target.getClass().getClassLoader(), 
            target.getClass().getInterfaces(), 
            new InvocationHandler() {
               
               @Override
               public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                  // TODO Auto-generated method stub
                  System.out.println("JDK代理开始~~");
                  //反射机制调用目标对象的方法
                  Object returnVal = method.invoke(target, args);
                  System.out.println("JDK代理提交");
                  return returnVal;
               }
            }); 
   }
```

#### cglib代理

静态代理和动态代理都要求被代理类实现接口，但有时候被代理类就是没有实现接口，这个时候就可以用cglib代理了。

Cglib代理也叫作**子类代理**，它是在内存中构建一个子类对象从而实现对目标对象功能扩展, 有些书也将Cglib代理归属到动态代理。

- 目标对象需要实现接口，用 JDK 代理

- 目标对象不需要实现接口，用 Cglib 代理

Cglib 包的底层是通过使用字节码处理框架 ASM 来转换字节码并生成新的类

需要引入 cglib 的 jar 文件，在内存中动态构建子类，注意代理的类不能为 final，否则报错

`java.lang.IllegalArgumentException`，目标对象的方法如果为 final/static,那么就不会被拦截,即不会执行目标对象额外的业务方法.

**应用场景**

1. 业务系统的非功能性需求开发

   监控、统计、鉴权、限流、事务、幂等、日志

2. 代理模式在RPC、缓存中的应用

   只要使用到spring aop的就是代理模式

### 桥接模式【结构型】

Bridge 模式基于类的最小设计原则，通过使用封装、聚合及继承等行为让不同的类承担不同的职责。它的主要特点是把抽象(Abstraction)与行为实现(Implementation)分离开来，从而可以保持各部分的独立性以及应对他们的功能扩展

将抽象和实现解耦，让它们可以独立变化。

还有另外一种理解方式：“一个类存在两个（或多个）独立变化的维度，我们通过组合的方式，让这两个（或多个）维度可以独立进行扩展。”通过组合关系来替代继承关系，避免继承层次的指数级爆炸。这种理解方式非常类似于，我们之前讲过的“组合优于继承”设计原则，



桥接模式替代多层继承方案，可以减少子类的个数，降低系统的管理和维护成本

组合优于继承：我认为桥接模式就是通过组合来提到继承避免继承层数过多

### 装饰器模式【结构型】

### 适配器模式【结构型】

适配器模式(Adapter Pattern)将某个类的接口转换成客户端期望的另一个接口表示，**主的目的是兼容性**，让原本因接口不匹配不能一起工作的两个类可以协同工作。其别名为包装器(Wrapper) 。适配器模式属于结构型模式。主要分为三类：**类适配器模式、对象适配器模式、接口适配器模**式 

对于这个模式，有一个经常被拿来解释它的例子，就是 USB 转接头充当适配器，把两种不兼容的接口，通过转接变得可以一起工作。

类适配与对象适配的区别：继承和组合

```java
// 类适配器: 基于继承
public interface ITarget {
    void f1();
    void f2();
    void fc();
}

public class Adaptee {

    public void fa() { 
        //... 
    }

    public void fb() {
        //... 
    }

    public void fc(){
        //... 
    }

}

public class Adaptor extends Adaptee implements ITarget {

    public void f1() {
        super.fa();
    }

    public void f2() {
        //...重新实现f2()...
    }

// 这里fc()不需要实现，直接继承自Adaptee，这是跟对象适配器最大的不同点
}

// 对象适配器：基于组合
public interface ITarget {
    void f1();
    void f2();
    void fc();
}
public class Adaptee {

    public void fa() {
        //... 
    }

    public void fb() {
        //... 
    }

    public void fc(){
        //... 
    }

}

public class Adaptor implements ITarget {
    private Adaptee adaptee;

    public Adaptor(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void f1() {
        adaptee.fa(); //委托给Adaptee
    }

    public void f2() {
        //...重新实现f2()...
    }

    public void fc() {
        adaptee.fc();
    }
}
```

**类适配和对象适配如何选择？**

- 如果 Adaptee 接口并不多，那两种实现方式都可以。
- 如果 Adaptee 接口很多，而且 Adaptee 和 ITarget 接口定义大部分都相同，那我们推荐使用类适配器，因为 Adaptor 复用父类 Adaptee 的接口，比起对象适配器的实现方式，Adaptor 的代码量要少一些。
- 如果 Adaptee 接口很多，而且 Adaptee 和 ITarget 接口定义大部分都不相同，那我们推荐使用对象适配器，因为组合结构相对于继承更加灵活

**适配器模式的应用场景**

一般来说，适配器模式可以看作一种“补偿模式”，用来补救设计上的缺陷。应用这种模式算是“无奈之举”。如果在设计初期，我们就能协调规避接口不兼容的问题，那这种模式就没有应用的机会了。

- 封装有缺陷的接口设计

- 统一多个类的接口设计

- 替换依赖的外部系统

- 兼容老版本接口
- 适配不同格式的数据

### 小结

- 代理模式：在不改变原始类接口的条件下，为原始类定义一个代理类，主要目的是控制访问，而非加强功能，这是它跟装饰器模式最大的不同。
- 桥接模式：目的是将接口部分和实现部分分离，从而让它们可以较为容易、也相对独立地加以改变。
- 装饰者模式：在不改变原始类接口的情况下，对原始类功能进行增强，并且支持多个装饰器的嵌套使用。
- 适配器模式：是一种事后的补救策略。适配器提供跟原始类不同的接口，而代理模式、装饰器模式提供的都是跟原始类相同的接口。

### 门面模式【结构型】

门面模式，也叫外观模式

**解决的问题**

开发接口的时，如何确定接口的颗粒度？

- 如果颗粒度太大，一个接口返回 n 多数据，要做 n 多事情，就会导致接口不够通用、可复用性不好。接口不可复用，那针对不同的调用者的业务需求，我们就需要开发不同的接口来满足，这就会导致系统的接口无限膨胀。
- 如果接口的粒度过小，在接口的使用者开发一个业务功能时，就会导致需要调用 n 多细粒度的接口才能完成。调用者肯定会抱怨接口不好用。

**定义**

门面模式为子系统提供一组统一的接口，定义一组高层接口让子系统更易用。

**应用场景**

这里所说的接口并非是java中的interface，而是被其它客户端调用的方法或接口

- 用易用性问题

  比如说子系统调用太过复杂，有很多方法要顺序调用，那么就可以运用门面模式创建一个高层类，把这些调用封装起来，让客户端只去调用高层类的方法

- 解决性能问题

  比如app一个页面要调用三个接口，会造成卡顿，那么可以运用门面模式创建高层类将这三个接口的功能封装成一个接口，那么此时app就只需要调用一个接口了

- 解决分布式事务问题

  比如在分布式系统中某个功能需要调用两个接口，但是这两个接口都设计到事务，如果使用分布式事务的话会很麻烦，所以可以使用门面模式改成一个接口，让两个功能都放到一个事务中进行。

### 组合模式【结构型】

组合模式跟我们之前讲的面向对象设计中的“组合关系（通过组合来组装两个类）”，完全是两码事。这里讲的“组合模式”，主要是用来处理树形结构数据。

正因为其应用场景的特殊性，数据必须能表示成树形结构，这也导致了这种模式在实际的项目开发中并不那么常用。但是，一旦数据满足树形结构，应用这种模式就能发挥很大的作用，能让代码变得非常简洁。

**定义**

将一组对象组织（Compose）成树形结构，以表示一种“部分 - 整体”的层次结构。组合让客户端可以统一单个对象和组合对象的处理逻辑。

***TODO***

### 享元模式【结构型】

所谓“享元”，顾名思义就是被共享的单元。享元模式的意图是复用对象，节省内存，前提是享元对象是不可变对象。

定义中的“不可变对象”指的是，一旦通过构造函数初始化完成之后，它的状态（对象的成员变量或者属性）就不会再被修改了。所以，不可变对象不能暴露任何 set() 等修改内部状态的方法。之所以要求享元是不可变对象，那是因为它会被多处代码共享使用，避免一处代码对享元进行了修改，影响到其他使用它的代码。

**代码结构**

主要是通过工厂模式，在工厂类中，通过一个 Map 来缓存已经创建过的享元对象，来达到复用的目的。

**和单例模式的区别**

1. 在单例模式中，一个类只能创建一个对象，而在享元模式中，一个类可以创建多个对象，每个对象被多处代码引用共享。实际上，享元模式有点类似于之前讲到的单例的变体：多例。
2. 我们前面也多次提到，区别两种设计模式，不能光看代码实现，而是要看设计意图，也就是要解决的问题。尽管从代码实现上来看，享元模式和多例有很多相似之处，但从设计意图上来看，它们是完全不同的。应用享元模式是为了对象复用，节省内存，而应用多例模式是为了限制对象的个数。

**跟缓存的区别**

在享元模式的实现中，我们通过工厂类来“缓存”已经创建好的对象。这里的“缓存”实际上是“存储”的意思，跟我们平时所说的“数据库缓存”“CPU 缓存”“MemCache 缓存”是两回事。我们平时所讲的缓存，主要是为了提高访问效率，而非复用。

**跟对象池的区别**

1. 对象池、连接池（比如数据库连接池）、线程池等也是为了复用，那它们跟享元模式有什么区别呢？
2. 你可能对连接池、线程池比较熟悉，对对象池比较陌生，所以，这里我简单解释一下对象池。像 C++ 这样的编程语言，内存的管理是由程序员负责的。为了避免频繁地进行对象创建和释放导致内存碎片，我们可以预先申请一片连续的内存空间，也就是这里说的对象池。每次创建对象时，我们从对象池中直接取出一个空闲对象来使用，对象使用完成之后，再放回到对象池中以供后续复用，而非直接释放掉。
3. 虽然对象池、连接池、线程池、享元模式都是为了复用，但是，如果我们再细致地抠一抠“复用”这个字眼的话，对象池、连接池、线程池等池化技术中的“复用”和享元模式中的“复用”实际上是不同的概念。
4. 池化技术中的“复用”可以理解为“重复使用”，主要目的是节省时间（比如从数据库池中取一个连接，不需要重新创建）。在任意时刻，每一个对象、连接、线程，并不会被多处使用，而是被一个使用者独占，当使用完成之后，放回到池中，再由其他使用者重复利用。享元模式中的“复用”可以理解为“共享使用”，在整个生命周期中，都是被所有使用者共享的，**主要目的是节省空间**。

**享元模式在Integer中的应用**

这其实是一个老生常谈的问题

Integer对象的值在 -128 到 127 之间，会从 IntegerCache 类中直接返回，否则才调用 new 方法创建。

而IntegerCache使用的就是享元模式，它就相当于是一个工厂类

实际上，JDK 也提供了方法来让我们可以自定义缓存的最大值，有下面两种方式。如果你通过分析应用的 JVM 内存占用情况，发现 -128 到 255 之间的数据占用的内存比较多，你就可以用如下方式，将缓存的最大值从 127 调整到 255。不过，这里注意一下，JDK 并没有提供设置最小值的方法。

```java
//方法一：
-Djava.lang.Integer.IntegerCache.high=255
//方法二：
-XX:AutoBoxCacheMax=255
```

实际上，除了 Integer 类型之外，其他包装器类型，比如 Long、Short、Byte 等，也都利用了享元模式来缓存 -128 到 127 之间的数据。比如，Long 类型对应的 LongCache 享元工厂类及 valueOf() 函数代码如下所示：

对于下面这样三种创建整型对象的方式，我们优先使用后两种。

```java
Integer a = new Integer(123);

Integer a = 123;

Integer a = Integer.valueOf(123);
```



**享元模式在String中的应用**

String 类利用享元模式来复用相同的字符串常量。JVM 会专门开辟一块存储区来存储字符串常量，这块存储区叫作“字符串常量池”。

不过，String 类的享元模式的设计，跟 Integer 类稍微有些不同。Integer 类中要共享的对象，是在类加载的时候，就集中一次性创建好的。但是，对于字符串来说，我们没法事先知道要共享哪些字符串常量，所以没办法事先创建好，只能在某个字符串常量第一次被用到的时候，存储到常量池中，当之后再用到的时候，直接引用常量池中已经存在的即可，就不需要再重新创建了。

### 观察者模式【行为型】

创建型设计模式主要解决“对象的创建”问题，结构型设计模式主要解决“类或对象的组合或组装”问题，那行为型设计模式主要解决的就是“类或对象之间的交互”问题。

行为型模式包括：观察者模式、模板模式、策略模式、职责链模式、状态模式、迭代器模式、访问者模式、备忘录模式、命令模式、解释器模式、中介模式

**1. 定义**

**观察者模式**也被称为**发布订阅模式**

在对象之间定义一个一对多的依赖，当一个对象状态改变的时候，所有依赖的对象都会自动收到通知。

一般情况下，被依赖的对象叫作**被观察者**（Observable），依赖的对象叫作**观察者**（Observer）。不过，在实际的项目开发中，这两种对象的称呼是比较灵活的，有各种不同的叫法，比如：Subject-Observer、Publisher-Subscriber、Producer-Consumer、EventEmitter-EventListener、Dispatcher-Listener。不管怎么称呼，只要应用场景符合刚刚给出的定义，都可以看作观察者模式。2. 

**2. 观察者模式的模板代码**

```java
import java.util.ArrayList;
import java.util.List;

public interface Subject {
  void registerObserver(Observer observer);

  void removeObserver(Observer observer);

  void notifyObservers(Message message);
}

public interface Observer {
  void update(Message message);
}

public class ConcreteSubject implements Subject {
  private List<Observer> observers = new ArrayList<Observer>();

  @Override
  public void registerObserver(Observer observer) {
    observers.add(observer);
  }

  @Override
  public void removeObserver(Observer observer) {
    observers.remove(observer);
  }

  @Override
  public void notifyObservers(Message message) {
    for (Observer observer : observers) {
      observer.update(message);
    }
  }
}

public class ConcreteObserverOne implements Observer {

  @Override
  public void update(Message message) {
    // TODO: 获取消息通知，执行自己的逻辑...
    System.out.println("ConcreteObserverOne is notified.");
  }
}

public class ConcreteObserverTwo implements Observer {

  @Override
  public void update(Message message) {
    // TODO: 获取消息通知，执行自己的逻辑...
    System.out.println("ConcreteObserverTwo is notified.");
  }
}

public class Demo {

  public static void main(String[] args) {
    ConcreteSubject subject = new ConcreteSubject();
    subject.registerObserver(new ConcreteObserverOne());
    subject.registerObserver(new ConcreteObserverTwo());
    subject.notifyObservers(new Message());
  }
}
```

**3. 一些理解**

观察者模式分为观察者和被观察者，被观察者只有一个，观察者有多个。具体到代码中被观察者需要通知所有的观察者执行某个任务，如果把观察者全部写在被观察者里面的某个方法里，耦合会非常严重。

这里就定义一个被观察者集合`List<Observe>`，放到被观察者类中，创建被观察者时，把所有观察者放到集合中，被观察者要通知他们时，只需要遍历集合就可以了。

**设计模式要干的事情就是解耦。创建型模式是将创建和使用代码解耦，结构型模式是将不同功能代码解耦，行为型模式是将不同的行为代码解耦，具体到观察者模式，它是将观察者和被观察者代码解耦。**借助设计模式，我们利用更好的代码结构，将一大坨代码拆分成职责更单一的小类，让其满足开闭原则、高内聚松耦合等特性，以此来控制和应对代码的复杂性，提高代码的可扩展性。

**应用**

1. 观察者模式的应用场景非常广泛，小到代码层面的解耦，大到架构层面的系统解耦，再或者一些产品的设计思路，都有这种模式的影子，比如，邮件订阅、RSS Feeds，本质上都是观察者模式。
2. 不同的应用场景和需求下，这个模式也有截然不同的实现方式，开篇的时候我们也提到，有同步阻塞的实现方式，也有异步非阻塞的实现方式；有进程内的实现方式，也有跨进程的实现方式。
3. 之前讲到的实现方式，从刚刚的分类方式上来看，它是一种同步阻塞的实现方式。观察者和被观察者代码在同一个线程内执行，被观察者一直阻塞，直到所有的观察者代码都执行完成之后，才执行后续的代码。对照上面讲到的用户注册的例子，register() 函数依次调用执行每个观察者的 handleRegSuccess() 函数，等到都执行完成之后，才会返回结果给客户端。
4. 如果注册接口是一个调用比较频繁的接口，对性能非常敏感，希望接口的响应时间尽可能短，那我们可以将同步阻塞的实现方式改为异步非阻塞的实现方式，以此来减少响应时间。具体来讲，当 userService.register() 函数执行完成之后，我们启动一个新的线程来执行观察者的 handleRegSuccess() 函数，这样 userController.register() 函数就不需要等到所有的 handleRegSuccess() 函数都执行完成之后才返回结果给客户端。userController.register() 函数从执行 3 个 SQL 语句才返回，减少到只需要执行 1 个 SQL 语句就返回，响应时间粗略来讲减少为原来的 1/3。
5. 那如何实现一个异步非阻塞的观察者模式呢？简单一点的做法是，在每个 handleRegSuccess() 函数中，创建一个新的线程执行代码。不过，我们还有更加优雅的实现方式，那就是基于 EventBus 来实现。今天，我们就不展开讲解了。在下一讲中，我会用一节的时间，借鉴 Google Guava EventBus 框架的设计思想，手把手带你开发一个支持异步非阻塞的 EventBus 框架。它可以复用在任何需要异步非阻塞观察者模式的应用场景中。
6. 刚刚讲到的两个场景，不管是同步阻塞实现方式还是异步非阻塞实现方式，都是进程内的实现方式。如果用户注册成功之后，我们需要发送用户信息给大数据征信系统，而大数据征信系统是一个独立的系统，跟它之间的交互是跨不同进程的，那如何实现一个跨进程的观察者模式呢？
7. 如果大数据征信系统提供了发送用户注册信息的 RPC 接口，我们仍然可以沿用之前的实现思路，在 handleRegSuccess() 函数中调用 RPC 接口来发送数据。但是，我们还有更加优雅、更加常用的一种实现方式，那就是基于消息队列（Message Queue，比如 ActiveMQ）来实现。
8. 当然，这种实现方式也有弊端，那就是需要引入一个新的系统（消息队列），增加了维护成本。不过，它的好处也非常明显。在原来的实现方式中，观察者需要注册到被观察者中，被观察者需要依次遍历观察者来发送消息。而基于消息队列的实现方式，被观察者和观察者解耦更加彻底，两部分的耦合更小。被观察者完全不感知观察者，同理，观察者也完全不感知被观察者。被观察者只管发送消息到消息队列，观察者只管从消息队列中读取消息来执行相应的逻辑。

### 模板模式【行为型】

模板方法模式在一个方法中定义一个算法骨架，并将某些步骤推迟到子类中实现。模板方法模式可以让子类在不改变算法整体结构的情况下，重新定义算法中的某些步骤。

这里的“算法”，我们可以理解为广义上的“业务逻辑”，并不特指数据结构和算法中的“算法”。这里的算法骨架就是“模板”，包含算法骨架的方法就是“模板方法”，这也是模板方法模式名字的由来

**代码示例**

```java
public abstract class AbstractClass {
    public final void templateMethod() {
        //...
        method1();
        //...
        method2();
        //...
    }

    protected abstract void method1();
    protected abstract void method2();
}

public class ConcreteClass1 extends AbstractClass {

    @Override
    protected void method1() {
        //...
    }

    @Override
    protected void method2() {
        //...
    }
}

public class ConcreteClass2 extends AbstractClass {

    @Override
    protected void method1() {
        //...
    }

    @Override
    protected void method2() {
        //...
    }
}

AbstractClass demo = ConcreteClass1();
demo.templateMethod();
```

看了代码瞬间明白，就是父类先定义一些抽象方法（强迫子类重写），父类中直接使用这些抽象方法实现业务逻辑。然后子类继承父类重写抽象方法，再调用父类的业务方法就实现了新的功能。相当于是创建了一个模板，由子类去填充。

**应用场景**

1. 复用

   模板模式把一个算法中不变的流程抽象到父类的模板方法 templateMethod() 中，将可变的部分 method1()、method2() 留给子类 ContreteClass1 和 ContreteClass2 来实现。所有的子类都可以复用父类中模板方法定义的流程代码。

**Java InputStream**

Java IO 类库中，有很多类的设计用到了模板模式，比如 InputStream、OutputStream、Reader、Writer。我们拿 InputStream 来举例说明一下。

我把 InputStream 部分相关代码贴在了下面。在代码中，read() 函数是一个模板方法，定义了读取数据的整个流程，并且暴露了一个可以由子类来定制的抽象方法。不过这个方法也被命名为了 read()，只是参数跟模板方法不同。

```java
import java.io.IOException;

public abstract class InputStream implements Closeable {
    //...省略其他代码...

    public int read(byte b[], int off, int len) throws IOException {
        if (b == null) {
            throw new NullPointerException();
        } else if (off < 0 || len < 0 || len > b.length - off) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return 0;
        }
        int c = read();
        if (c == -1) {
            return -1;
        }
        b[off] = (byte) c;
        int i = 1;
        try {
            for (; i < len; i++) {
                c = read();
                if (c == -1) {
                    break;
                }
                b[off + i] = (byte) c;
            }
        } catch (IOException ee) {
        }
        return i;

    }
    public abstract int read() throws IOException;
}

public class ByteArrayInputStream extends InputStream {
    //...省略其他代码...

    @Override
    public synchronized int read() {
        return (pos < count) ? (buf[pos++] & 0xff) : -1;
    }
}
```

**Java AbstractList**

在 Java AbstractList 类中，addAll() 函数可以看作模板方法，add() 是子类需要重写的方法，尽管没有声明为 abstract 的，但函数实现直接抛出了 UnsupportedOperationException 异常。前提是，如果子类不重写是不能使用的。

```java
public boolean addAll(int index,Collection<?extends E> c){
        rangeCheckForAdd(index);
        boolean modified=false;

        for(E e:c){
            add(index++,e);
            modified=true;
        }
        return modified;
}

public void add(int index,E element){
        throw new UnsupportedOperationException();
}
```

2. 扩展

这里所说的扩展，并不是指代码的扩展性，而是指框架的扩展性，有点类似我们之前讲到的控制反转。基于这个作用，模板模式常用在框架的开发中，让框架用户可以在不修改框架源码的情况下，定制化框架的功能。



Java Servlet



3. 回调

   对于回调这一概念的理解

```java
public interface ICallback {
  void methodToCallback();
}

public class BClass {

  public void process(ICallback callback) {
    // ...
    callback.methodToCallback();
    // ...
  }
}

public class AClass {

  public static void main(String[] args) {
    BClass b = new BClass();
    b.process(
        new ICallback() { // 回调对象
          @Override
          public void methodToCallback() {
            System.out.println("Call back me.");
          }
        });
  }
}
```

感觉非常像函数式编程

同步回调——模板模式

异步回调——观察者模式

JdbcTemplate

setClickListener(）

addShutdownHook()

**模板模式vs回调**

1. 回调的原理、实现和应用到此就都讲完了。接下来，我们从应用场景和代码实现两个角度，来对比一下模板模式和回调。
2. 从应用场景上来看，同步回调跟模板模式几乎一致。它们都是在一个大的算法骨架中，自由替换其中的某个步骤，起到代码复用和扩展的目的。而异步回调跟模板模式有较大差别，更像是观察者模式。
3. 从代码实现上来看，回调和模板模式完全不同。回调基于组合关系来实现，把一个对象传递给另一个对象，是一种对象之间的关系；模板模式基于继承关系来实现，子类重写父类的抽象方法，是一种类之间的关系。
4. 前面我们也讲到，组合优于继承。实际上，这里也不例外。在代码实现上，回调相对于模板模式会更加灵活，主要体现在下面几点。

- 像 Java 这种只支持单继承的语言，基于模板模式编写的子类，已经继承了一个父类，不再具有继承的能力。
- 回调可以使用匿名类来创建回调对象，可以不用事先定义类；而模板模式针对不同的实现都要定义不同的子类。
- 如果某个类中定义了多个模板方法，每个方法都有对应的抽象方法，那即便我们只用到其中的一个模板方法，子类也必须实现所有的抽象方法。而回调就更加灵活，我们只需要往用到的模板方法中注入回调对象即可。

### 策略模式【行为型】

策略模式最常用的场景是避免冗长的if-else、switch。不过，它的作用还不止如此。它也可以像模板模式那样，提供框架的扩展点等等。

**定义**

官方：定义一族算法类，将每个算法分别封装起来，让它们可以互相替换。策略模式可以使算法的变化独立于使用它们的客户端（这里的客户端代指使用算法的代码）。

我们知道，工厂模式是解耦对象的创建和使用，观察者模式是解耦观察者和被观察者。策略模式跟两者类似，也能起到解耦的作用，不过，它解耦的是策略的定义、创建、使用这三部分。接下来，我就详细讲讲一个完整的策略模式应该包含的这三个部分。

**策略模式的使用**

- 定义

  定义策略模式其实十分简单，就是接口——实现的方式，每种策略都是一个实现类

  ```java
  public interface Strategy {
    void algorithmInterface();
  }
  
  public class ConcreteStrategyA implements Strategy {
    @Override
    public void algorithmInterface() {
      // 具体的算法...
    }
  }
  
  public class ConcreteStrategyB implements Strategy {
    @Override
    public void algorithmInterface() {
      // 具体的算法...
    }
  ```

- 创建

  既然有这么多策略需要创建，那么显而易见工厂模式可以使用。

  一般来说策略类是无状态的，所以对象可复用，就能使用下面的方式，如果不能复用，就要使用另一种工厂模式了

  ```java
  public class StrategyFactory {
    private static final Map<String, Strategy> strategies = new HashMap<>();
  
    static {
      strategies.put("A", new ConcreteStrategyA());
      strategies.put("B", new ConcreteStrategyB());
    }
  
    public static Strategy getStrategy(String type) {
      if (type == null || type.isEmpty()) {
        throw new IllegalArgumentException("type should not be empty.");
      }
      return strategies.get(type);
    }
  }
  ```

- 使用

  客户端如何选用策略呢，最常用的是运行时动态选择策略，根据用户输入、配置文件等等选择策略

  当然另一个种方式“非运行时动态确定”，实际上退化成了多态的特性

**策略模式如何去除if-else**

实际上没什么神秘的，秘诀就是***查表（哈希）***

算法里经常会用到的哈希法，把所有可能性都放到哈希里，然后通过key去取值不就行了

**开闭原则？**

上面创建策略的代码种，如果新增策略我们肯定要对哈希进行修改，这样不就违背了开闭原则？有另一种思路可以解决这个问题：

- 创建出策略类，给该类加上注解
- 启动时动态扫描所有注解，将策略放到一起，这样就不必去维护工厂类中的哈希了

### 职责链模式【行为型】

在前面，我们学习了模板模式、策略模式，今天，我们来学习职责链模式。这三种模式具有相同的作用：复用和扩展，在实际的项目开发中比较常用，特别是框架开发中，我们可以利用它们来提供框架的扩展点，能够让框架的使用者在不修改框架源码的情况下，基于扩展点定制化框架的功能。

**定义**

官方：将请求的发送和接收解耦，让多个接收对象都有机会处理这个请求。将这些接收对象串成一条链，并沿着这条链传递这个请求，直到链上的某个接收对象能够处理它为止。

在职责链模式中，多个处理器（也就是刚刚定义中说的“接收对象”）依次处理同一个请求。一个请求先经过 A 处理器处理，然后再把请求传递给 B 处理器，B 处理器处理完后再传递给 C 处理器，以此类推，形成一个链条。链条上的每个处理器各自承担各自的处理职责，所以叫作职责链模式。

### 状态模式【行为型】

状态模式一般用来实现状态机，而状态机常用在游戏、工作流引擎等系统开发中。不过，状态机的实现方式有多种，除了状态模式，比较常用的还有分支逻辑法和查表法。

**有限状态机**

状态机有 3 个组成部分：状态（State）、事件（Event）、动作（Action）。其中，事件也称为转移条件（Transition Condition）。事件触发状态的转移及动作的执行。不过，动作不是必须的，也可能只转移状态，不执行任何动作。

事件会导致状态改变以及动作的发生

### 迭代器模式【行为型】

迭代器模式（Iterator Design Pattern），也叫作游标模式（Cursor Design Pattern）。

这里说的“集合对象”也可以叫“容器”“聚合对象”，实际上就是包含一组对象的对象，比如数组、链表、树、图、跳表。迭代器模式将集合对象的遍历操作从集合类中拆分出来，放到迭代器类中，让两者的职责更加单一

迭代器是用来遍历容器的，所以，一个完整的迭代器模式一般会涉及**容器和容器迭代器**部分两部分内容。为了达到基于接口而非实现编程的目的，容器又包含容器接口、容器实现类，迭代器又包含迭代器接口、迭代器实现类。