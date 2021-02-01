---
title: spring-in-action-4-第四部分
siteurl: spring-in-action-4-第四部分
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-spring-in-action-4-第四部分
date: '2021-01-29T10:34:02+08:00'
date updated: '2021-01-29T10:55:56+08:00'

---

> Not every day is going to offer us a chance to save somebody's life, but every day offers us an opportunity to affect one.
> — <cite>Mark Bezos</cite>

> 无意中搜索到了前辈的这本笔记，它包含了四个部分。初步阅读后感觉受益颇多，暂且转载过来，以作备份。原链接：[Spring 实战 第4版 读书笔记 - cateatmycode - 博客园 ](https://www.cnblogs.com/mumue/p/11217784.html)

> 注：
> 由于spring实战第四版已经基本停止印刷，新手想购买这本书只能去淘二手了。
>
> 初学者阅读本书应当会遇到一些困难，可以尝试预先学习《spring 源码深度解析》-郝佳 编著 的前几章，这会帮助理解
>
> 事实上，spring实战的第四版和第五版应该都是绕不过的堪。两书侧重点不同，有机会都应当阅读，见原文：[Java读书笔记 - cateatmycode - 博客园](https://www.cnblogs.com/mumue/p/11217595.html)
>
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/mumue/p/11217784.html)

### 4、第四章：面向切面的 Spring

本章内容：

- 面向切面编程的基本原理
- 通过 POJO 创建切面
- 使用 @AspectJ 注解
- 为 AspectJ 切面注入依赖

在软件开发中，散布于应用中多处的功能被称为横切关注点（cross-cutting concern），这些横切关注点从概念上是与应用的业务逻辑相分离的（但是往往会直接嵌入到应用的业务逻辑中

切面能帮助我们模块化横切关注点。简单的说，横切关注点可以被描述为影响应用多处的功能。例如：安全

如果要重用通用功能的话，最常见的面向对象技术是继承（inheritance）或委托（delegation）。

但是如果在整个应用中都使用相同的基类，继承往往会导致一个脆弱的对象体系

而使用委托可能需要对委托对象进行复杂的调用

使用切面编程时，我们仍然在一个地方定义通用功能，但是可以通过声明的方式定义这个功能要以何种方式在何处应用，而无需修改受影响的类

好处：

首先，现在每个关注点都集中于一个地方，而不是分散到多处代码中；

其次，服务模块更简洁，因为它们只包含主要关注点（或核心功能）的代码，而次要关注点的代码被转移到切面中了

4.1.1、定义 AOP 术语

常用术语：通知（advice）、切点（pointcut）和连接点（join point）

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190803151137943-1817676726.png)

通知（advice）：切面的工作被称为通知。通知定义了切面是什么以及何时使用。Spring 切面可以应用 5 种类型的通知：

- 前置通知（Before）：在目标方法被调用之前调用通知功能
- 后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么
- 返回通知（After-returning）：在目标方法成功执行之后调用通知
- 异常通知（After-throwing）：在目标方法抛出异常后调用通知
- 环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为

 连接点（Join point）：是在应用执行过程种能够插入切面的一个点

切点（Pointcut）：如果说通知定义了切面的 “什么” 和“何时”的话，那么切点就定义了“何处”

切面（Aspect）：切面是通知和切点的结合。通知和切点共同定义了切面的全部内容——它是什么，在何时和何处完成其功能

引入（Introduction）：引入允许我们向现有的类添加新方法或属性

织入（Weaving）：织入是把切面应用到目标对象并创建新的代理对象的过程，切面在指定的连接点被织入到目标对象中：

- 编译期：切面在目标类编译时被织入。这种方式需要特殊的编译器。AspectJ 的织入编译器就是以这种方式织入切面的
- 类加载期：切面在目标类加载到 JVM 时被织入。这种方式需要特殊的类加载器（ClassLoader），它可以在目标类被引入应用之前增强该目标类的字节码，如 AspectJ5 的加载时织入
- 运行期：切面在应用运行的某个时刻被织入。织入切面时，AOP 容器会为目标对象动态地创建一个代理对象。Spring AOP 就是以这种方式织入切面的

4.1.2、Spring 对 AOP 的支持

- 基于代理的经典 Spring AOP
- 纯 POJO 切面
- @AspectJ 注解驱动的切面
- 注入式 AspectJ 切面（适用于 Spring 各版本）

现在的 Spring 引入了简单的声明式 AOP 和基于注解的 AOP 之后，Spring 经典的 AOP 就 显得过于笨重和复杂

借助 Spring 的 aop 命名空间，我们可以将纯 POJO 转换为切面，但是需要 XML 配置

Spring 借鉴了 AspectJ 的切面，以提供注解驱动的 AOP。本质上，它依然是 Spring 基于代理的 AOP，好处在于不使用 xml 来完成

如果你的 AOP 需求超过了简单的方法调用（如构造器或属性拦截），那就需要考虑使用 AspectJ 来实现切面

Spring 通知是 Java 编写的——Spring 所创建的通知都是用标准的 Java 类编写的

Spring 在运行时通知对象——通过在代理类中包裹切面，Spring 在运行期把切面织入到 Spring 管理的 bean 中

如下图，代理类封装了目标类，并拦截被通知方法的调用，再把调用转发给真正的目标 bean。当代理拦截到方法调用时，在调用目标 bean 方法之前，会执行切面逻辑

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190803202515519-1475526938.png)

因为 Spring 基于动态代理，所以 Spring 只支持方法连接点，这与其他的 AOP 框架是不同的，如 AspectJ

Spring 缺少对字段连接点的支持，无法让我们创建细粒度的通知，如拦截对象字段的修改，而且它不支持构造器连接点，我们就无法在 bean 创建时应用通知

4.2、通过切点来选择连接点

```java
//为阐述Spring中的切面，需要有个主题来定义切面的切点。为此，我们定义了一个Performance接口：
package concert;
public interface Performance {
  public void perform();  
}
```

假设我们要编写 Performance 的 perform 方法触发通知：

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190805114523439-183637072.png)

假如需要配置的切点仅匹配 concert 包，可以使用 within() 指示器来限制匹配：

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190805114853730-493388460.png)

在 Spring 的 XML 配置里面描述切点时，使用 `and`，`or`，`not` 来代替 `&&`，`||`，`!`

4.2.2、在切点中选择 bean

```java
//限定bean的ID为Woodstock
execution(* concert.Performance.perform()) and bean('woodstock')
```

4.3、使用注解创建切面——使用注解来创建切面是 AspectJ

5 引入的关键特性

```java
//Audience类：观看演出的观众的切面
@Aspect
public class Audience {
  //通过@Pointcut注解声明频繁使用的切点表达式
  @Pointcut("execution(** concert.Perforance.perform(...))")
  public void performance() {}
  //表演之前：未使用Pointcut
  @Before("execution(** concert.Perforance.perform(...))")
  public void silenceCellPhones() {
    System.out.println("Silencing cell phones");
  }
  //表演之后
  @AfterReturnint("performance() ")
  public void applause() {
    System.out.println("CLAP CLAP CLAP");
  }
  //表演失败之后
  @AfterThrowing("performance() ")
  public void demandRefund() {
    System.out.println("Demanding a refund");
  }
}
```

需要注意，Audience 类依然是一个 POJO，像其他 Java 类一样，可以装配为 Spring 中的 bean：

```java
@Bean
public Audience audience() {
  return new Audience();  
}
```

**如果就此止步的话，Audience 只会是 Spring 容器中的一个 bean，即使使用了 AspectJ 注解，也不会被视为切面，这些注解不会解析，也不会创建将其转换为切面的代理**

如果使用 JavaConfig 的话，只需

```java
@Configuration
@EnableAspectJAutoProxy //启用AspectJ 自动代理
@ComponentScan
public bean ConcertConfig {
  @Bean
  public Audience audience() { //声明Audience bean
    return new Audience();
  }
}
```

如果使用 XML 来装配 bean 的话，使用 aop 命名空间中的 `<aop:aspectj-autoproxy>` 元素：

```xml
<context:component-scan base-package="concert" />
<aop:aspectj-autoproxy /> //启用AspectJ 自动代理
<bean /> //声明Audience bean
```

Spring 的 AspectJ 自动代理仅仅使用 @AspectJ 作为创建切面的指导，切面依然是基于代理的。本质上，它依然是 Spring 基于代理的切面

4.3.2、创建环绕通知

```java
@Aspect
public class Audience {
  @Pointcut("execution(** concert.Performance.perform(...))")
  public void performance() {}
  //环绕通知方法
  @Around("performance()")
  public void watchPerformance(ProceedingJoinPoint jp) {
    try { 
      System.out.println("Silencing cell phones");
      System.out.println("Taking seats");
      jp.proceed();
      System.out.println("CLAP CLAP CLAP!");
    } catch (Throwable e) {
      System.out.println("Demanding a refund");
    }
  }  
}
```

接受 ProceedingJoinPoint 作为参数，这个对象是必须有的，要在通知中通过它来调用被通知的方法。

当将控制权交给被通知的方法时，它需要调用 ProceeddingJoinPoint 的 proceed() 方法。如果不调用此方法，你的通知实际上会阻塞对被调用方法的调用

4.3.3、处理通知中的参数

```java
@Aspect
public class TrackCounter {
  private Map<Integer, Integer> trackCounts = new HashMap<Integer, Integer>();
  @Pointcut("execution(* soundsystem.CompactDisc.playTrack(int))" + "&& args(trackNumber)") //通知play-Track()方法
  public void trackPlayed(int trackNumber) {}
  @Before("trackPlayed(trackNumber)") //在播放前，为该磁道计数
  public void countTrack(int trackNumber) {
    int currentCount = getPlayCount(trackNumber);
    trackCounts.put(trackNumber, currentCount + 1);    
  }  
  public int getPlayCount(int trackNumber) {
    return trackCounts.containsKey(trackNumber) ? trackCounts.get(trackNumber) : 0;    
  }  
}
```

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190805161649615-49901468.png)

现在我们可以在 Spring 配置中将 BlankDisc 和 TrackCounter 定义为 bean，并启用 AspectJ 自动代理：

```java
//配置TrackCount记录每个磁道播放的次数
@Configuration
@EnableAspectJAutoProxy //启用AspectJ自动代理
public class TrackCounterConfig {
  @Bean
  public CompactDisc sgtPeppers () { //CompactDisc bean
    BlankDisc cd = new BlankDisc();
    cd.setTitle("Sgt. Pepper's Lonely Hearts Club Band");
    cd.setArtist("The Beatles");
    List<String> tracks = new ArrayList<String>();
    tracks.add("Sgt. Pepper's Lonely Hearts Club Band");
    tracks.add("With a Little Help from My Friends");
    tracks.add("Fixing a Hole");    
    cd.setTracks(tracks);
    return cd;    
  }  
  @Bean
  public TrackCounter trackCounter() { //TrackCounter bean
    retrun new TrackCounter();
  }
}
```

最后，测试 TrackCounter 切面

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=TrackCounterConfig.class)
public class TrackCounterTest {
  @Rule
  public final StandardOutputStreamLog log = new StandardOutputStreamLog();
  @Autowired
  private CompactDisc cd;
  @Autowired
  private TrackCounter counter;
  @Test
  public void testTrackCounter() {
    cd.playTrack(1); //播放一些磁道
    cd.playTrack(2);
    cd.playTrack(3);
    cd.playTrack(3);
    //断言期望的数量
    assertEquals(1, counter.getPlayCount(1));
    assertEquals(1,counter.getPlayCount(2));
    assertEquals(4,counter.getPlayCount(3));
    assertEquals(0,counter.getPlayCount(4));
  }
}
```

目前为止，所使用的切面中，所包装的都是被通知对象的已有方法。下面看一下如何通过编写切面，为被通知的对象引入全新的功能

4.3.4、通过注解引入新功能

动态语言，可以不用直接修改对象或类的定义就能够为对象或类增加新的方法。但 Java 不是动态语言，类编译完成了，就很难为该类添加新的功能了

切面可以为 Spring bean 添加新方法：

为示例中的所有的 Performance 实现引入下面的 Encoreable

```java
package concert;
public interface Encoreable {
  void performEncore();  
}
```

借助 AOP 的引入功能，我们可以不必在设计上妥协或者侵入性地改变现有的实现，为了实现该功能，我们要创建一个新的切面：

```java
package concert;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.DeclareParents;
@Aspect
public class EncoreableIntroducer {
  @DeclareParents(value="concert.Performance+", defaultImpl=DefaultEncoreable.class)
  public static Encoreable encoreable;  
}
```

可以看到，EncoreableIntroducer 是一个切面，但是和我们之前创建的切面不同，它并没有提供前置、后置或环绕通知，而是通过 @DeclareParents 注解，将 Encoreable 接口引入到 Performance bean 中

@DeclareParents 注解由三部分组成：

- value 属性指定了哪种类型的 bean 要引入该接口。标记符后面的加号表示是 Performance 的所有子类型，而不是 Performance 本身
- defaultImpl 属性指定了为引入功能提供实现的类。这里我们指定 DefaultEncoreable 提供实现
- @DeclareParents 注解所标注的静态属性指明了要引入了接口。这里我们所引入的是 Encoreable 接口

和其他的切面一样，我们需要在 Spring 应用中将 EncoreableIntroducer 声明为一个 bean：

```xml
<bean />
```

Spring 的自动代理机制将会获取到它的声明，当 Spring 发现一个 bean 使用了 @Aspect 注解时，Spring 就会创建一个代理，然后将调用委托给被代理的 bean 或被引入的实现，这取决于调用的方法属于被代理的 bean 还是属于被引入的接口

在 Spring 中，注解和自动代理提供了一种很便利的方式来创建切面。但是，你必须能够为通知类添加注解，为了做到这一点，必须要有源码。

4.4、在 XML 中声明切面

基于注解的配置要优于基于 Java 的配置，基于 Java 的配置要优于基于 XML 的配置。

但如果你需要声明切面，但是又不能为通知类添加注解的时候，那么就必须专向 XML 配置了
