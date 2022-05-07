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