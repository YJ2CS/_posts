---
title: spring-in-action-4-第二部分
siteurl: spring-in-action-4-第二部分
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-spring-in-action-4-第二部分
date: '2021-01-29T10:31:29+08:00'
date updated: '2021-01-29T10:48:48+08:00'

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

### 2、第二章：装配 Bean

- 声明 bean
- 构造器注入和 Setter 方法注入
- 装配 bean
- 控制 bean 的创建和销毁

在 Spring 中，对象无需自己查找或创建与其相关联的其他对象。容器负责把需要相互协作的对象引用赋予给各个对象

创建应用对象之间协作关系的行为称为装配（wiring），这也是依赖注入（DI）的本质

> 什么是 bean：<https://blog.csdn.net/weixin_42594736/article/details/81149379> 
>
> <https://www.cnblogs.com/cainiaotuzi/p/7994650.html>

配置 Spring 容器最常见的三种方法：

2.1、 Spring 配置的可选方案——**尽可能的使用自动装配机制，显式配置越少越好**

- 在 XML 中进行显式配置
- 在 Java 中进行显式配置
- 隐式的 bean 发现机制和自动装配

2.2、Spring 从两个角度来实现自动化装配 bean：

- 组件扫描（component scanning）：Spring 会自动发现应用上下文中所创建的 bean
- 自动装配（autowiring）：Spring 自动满足 bean 之间的依赖

CD 为我们阐述 DI 如何运行提供了一个很好的样例——你不将 CD 插入（注入）到 CD 播放器中，那么播放器其实是没有太大用处的。CD 播放器依赖于 CD 才能完成它的使命

```java
//CompactDisc接口在Java中定义了CD的概念
package soundsystem;
public interface CompactDisc {
  void play();  
}
```

CompactDisc 作为接口，它定义了 CD 播放器对一盘 CD 所能进行的操作，**它将 CD 播放器的任意实现与 CD 本身的耦合降低到了最小的程度**

我们还需要一个 CompactDisc 的实现

```java
//带有@Component注解的CompactDisc实现类SgtPeppers
package soundsystem;
import org.springframework.stereotype.Component;
@Component
public class SgtPeppers implements CompactDisc {
  private String title = "Sgt.Pepper's Lonely Hearts Club Band";
  private String artist = "The Beatles";

  public void play() {
    System.out.println("Playing " + title + " by " + artist);
  }      
}
```

 SgtPeppers 类上使用了 **@Component 注解，这个注解表明该类会作为组件类，并告知 Spring 要为这个类创建 bean。**

**组件扫描默认是不启用的**。还需要显式配置一下 Spring，从而命令它去寻找带有 @Component 注解的类，并为其创建 bean

```java
//@Component注解启用了组件扫描
package soundsystem;
import org.springframework.context.annotation.componentScan;
import org.springframework.context.annotation.Con;
@Configuration
@ComponentScan
public class CDPlayerConfig {
}
```

CDPlayerConfig 类并没有显式声明任何 bean，只不过它使用了 @ComponentScan 注解，这个注解能够在 Spring 中启用组件扫描

如果没有其他配置的话，`@ComponentScan` 默认会扫描与配置类相同的包。

因为 `CDPlayerConfig` 类位于 `soundsystem` 包中，因此 Spring 将会扫描这个包以及这个包下的所有子包，查找带有 `@Component` 注解的类

这样，就能发现 `CompactDisc`，并且会在 Spring 中自动为其创建一个 bean

也可以用 XML 来启用组件扫描，可以使用 Spring context 命名空间的 `<context:component-scan>` 元素

```xml
<context:component-scan base-package="soundsystem" />
```

下面，我们创建一个简单的 JUnit 测试，它会创建 Spring 上下文，并判断 `CompactDisc` 是不是真的创建出来了

```java
package soundsystem;
import static org.junit.Assert.*;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
//使用SpringJUnit4ClassRunner以便在测试开始的时候自动创建Spring的应用上下文
//注解ContextConfiguration会告诉它需要在CDPlayerConfig中加载配置
//因为CDPlayerConfig类中包含了@ComponentScan，因此最终的应用上下文应该包含CompactDiscbean
//为了证明这一点，在测试代码中有一个CompactDisc类型的属性，并且这个属性带有@Autowired注解，以便于将CompactDiscbean注入到测试代码中
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=CDPlayerConfig.class)
public class CDPlayerTest {
  @Autowired
  private CompactDisc cd;
  @Test
  public void cdShouldNotBeNull() {
    assertNotNull(cd);    
  }  
}
```

只添加一行 `@ComponentScan` 注解就能自动创建无数个 bean

#### 2.2.2、为组件扫描的 bean 命名

Spring 应用上下文中所有的 `bean` 都会给定一个 ID。默认会根据类名为其指定一个 ID，如上面例子中 `SgtPeppersbean` 的 ID 为 `sgtPeppers`，

**也就是将类名的第一个字母变小写**

```java
//设定bean的ID
@Component("lonelyHeartsClub")
//也可以用@Named注解来为bean设置ID
@Named("lonelyHeartsClub")
public class SgtPeppers implements CompactDisc{...}
```

#### 2.2.3、设置组件扫描的基础包

**默认会以配置类所在的包作为基础包 (`basepackage`) 来扫描组件**。如果想扫描不同的包，或者扫描多个包，又该如何呢？

```java
//指明包的名称
@Configuration
@ComponentScan("soundsystem")
public class CDPlayerConfig() {}
```

如果想更清晰地表明你所设置的是基础包，可以通过 `basePackages` 属性进行配置：

```java
@Configuration
@ComponentScan(basePackages="soundsystem")
public class CDPlayerConfig() {}
```

多个基础包：basePackages={"soundsystem", "video"}\
上面所设置的基础包是以 String 类型表示的，这种方法是类型不安全（not type-safe）的

```java
//另外一种方法，将其指定为包种所包含的类或接口：
@Configuration
@ComponentScan(basePackageClasses={CDPlayer.class, DVDPlayer.class })
public class CDPlayerConfig {}
```

如果所有的对象都是独立的，彼此之间没有任何依赖，就像 SgtPeppersbean 这样，那你所需要的可能就是组件扫描而已

但是很多对象会依赖其他的对象才能完成任务，这就需要一种方法能够将组件扫描得到的 bean 和它们的依赖装配在一起——Spring 的自动装配

#### 2.2.4、通过为 bean 添加注解实现自动装配

**自动装配就是让 Spring 自动满足 bean 依赖的一种方法**，在满足依赖的过程种，会在 Spring 应用上下文种寻找匹配某个 bean 需求的其他 bean

为了声明要进行自动装配，借助 Spring 的 @Autowired 注解

@Autowired 注解不仅能用在构造器上，还能用在属性的 Setter 方法上：

```java
@Autowired
public void setCompactDisc(CompactDisc cd) {
  this.cd = cd;  
}
```

在 Spring 初始化 bean 之后，它会尽可能得去满足 bean 的依赖，在上面的例子中，依赖是通过带有 @Autowired 注解的方法进行声明

假如有且只有一个 bean 匹配依赖需求的话，那这个 bean 将会被装配进来

如果没有匹配的 bean，那么在应用上下文创建的时候，Spring 会抛出一个异常，为避免这个异常，可以：@Autowired(required=false)

当 required 设置为 false 时，Spring 会尝试执行自动装配，但如果没有匹配的 bean 的话，Spring 将会让这个 bean 处于未装配状态

如果你的代码没有进行 null 检查的话，这个处于未装配状态的属性有可能会出现 NullPointerException

如果有多个 bean 满足依赖关系的话，Spring 会抛出一个异常，表明没有明确指定要选择哪个 bean 进行自动装配

@Autowired 还可以替换为 @Inject——不推荐

2.3、通过 Java 代码装配 bean

比如你想要将第三方库中的组件装配到你的应用中，这种情况下，是没有办法在它的类上添加 @Component 和 @Autowired 注解的，因此就不能使用自动化装配的方案了

要采用显式装配的方式。两种方案：Java 和 XML。

进行显示配置时，JavaConfig 是更好的方案。因为它更强大、类型安全且对重构友好

JavaConfig 是配置代码，意味着它不应该包含任何业务逻辑，JavaConfig 也不应该侵入到业务逻辑代码中，**通常会将 JavaConfig 放到单独的包中**

2.3.1、创建配置类

**创建 JavaConfig 类的关键在于为其添加 @Configuration 注解，表明这个类是一个配置类，该类应该包含在 Spring 应用上下文中如何创建 bean 的细节**

到此为止，我们都是依赖组件扫描来发现 Spring 应该创建的 bean，下面将关注于显示配置

2.3.2、声明简单的 bean

要在 JavaConfig 中声明 bean，需要编写一个方法，这个方法会创建所需类型的实例，然后给这个方法添加 @Bean 注解，如下面的代码声明了 CompactDisc bean：

```java
@Bean
public CompactDisc sgtPepper() {
  return new SgtPeppers();  
}
```

 @Bean 注解会告诉 Spring 这个方法将会返回一个对象，该对象要注册为 Spring 应用上下文中的 bean

2.3.3、借助 JavaConfig 实现注入

在 JavaConfig 中装配 bean 的最简单的方式就是引用创建 bean 的方法，如下就是一种声明 CDPlayer 的可行方案：

```java
@Bean
public CDPlayer cdPlayer() {
  return new CDPlayer(sgtPeppers());  
}
```

在这里并没使用默认的构造器构造实例，而是调用了需要传入 CompactDisc 对象的构造器来创建 CDPlayer 实例

看起来，CompactDisc 是通过调用 sgtPeppers() 得到的，但情况并非完全如此。

因为 sgtPeppers() 方法添加了 @Bean 注解，Spring 将会拦截所有对它的调用，并确保直接返回该方法所创建的 bean，而不是每次都对其进行实际的调用

通过调用方法来引用 bean 的方式有点令人困惑。还有一种理解起来更简单的方式：

```java
@Bean
public CDPlayer cdPlayer(CompactDisc compactDisc) {
  return new CDPlayer(compactDisc)  
}
```

2.4、通过 XML 装配 bean——Spring 现在有了强大的自动化配置和基于 Java 的配置，XML 不应该在是你的第一选择了

2.4.1、创建 XML 配置规范

在使用 XML 为 Spring 装配 bean 之前，你需要创建一个新的配置规范。

在使用 JavaConfig 的时候，这意味着要创建一个带有 @Configuration 注解的类，而在 XML 配置中，意味着要创建一个以 `<beans>` 元素为根的 XML 文件

2.4.2、声明一个简单的 `<bean>`：`<bean >` 元素类似于 JavaConfig 中的 @Bean 注解

```xml
//创建这个bean的类通过class属性类指定的，并且要使用全限定的类名
//建议设置id，不要自动生成
<bean />
```

2.4.3、借助构造器注入初始化 bean

```xml
//通过ID引用SgtPeppers
<bean>
    <constructor-arg ref="compactDisc" />
</bean>
```

当 Spring 遇到这个 `<bean>` 元素时，会创建一个 CDPlayer 实例。`<constructor-arg >` 元素会告诉 Spring 要将一个 ID 为 compactDisc 的 bean 引用传递到 CDPlayer 的构造器中

当构造器参数的类型是 java.util.List 时，使用 `<list>` 元素。set 也一样

```xml
<bean>
    <constructor-arg value="The Beatles" />
    <constructor-arg>
        <list>
          <ref bean="sgtPeppers />
          <ref bean="whiteAlbum />      
        </list>
    </constructor-arg>
</bean>
```

2.4.4、使用 XML 实现属性注入

**通用规则：对强依赖使用构造器注入，对可选性依赖使用属性注入**

**`<property>` 元素**为属性的 Setter 方法所提供的功能与 `<constructor-arg>` 元素为构造器所提供的功能是一样的

```xml
<bean>
    <property  />
</bean>
```

借助 `<property>` 元素的 value 属性注入属性

```xml
<bean>
    <property  />
    <property >
        <list>
            <value>Getting Better</value>
            <value>Fixing a Hole</value>
        </list>
    </property>
</bean>
```

### 2.5、导入和混合配置

#### 2.5.1、在 JavaConfig 中引用 XML 配置

我们假设 CDPlayerConfig 已经很笨重，要将其拆分，方案之一就是将 BlankDisc 从 CDPlayerConfig 拆分出来，定义到它自己的 CDConfig 类中：

```java
package soundsystem;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class CDConfig {
  @Bean 
  public CompactDisc compactDisc() {
    return new SgtPeppers();    
  }    
}
```

然后将两个类在 CDPlayerConfig 中使用 @Import 注解导入 CDConfig：

```java
package soundsystem;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Import;

@Configuration
@Import(CDConfig.class)
public class CDPlayerConfig {
  @Bean
  public CDPlayer cdPlayer(CompactDisc compactDisc) {
    return new CDPlayer(compactDisc);    
  }
}
```

**更好的办法是：创建一个更高级别的 SoundSystemConfig，在这个类中使用 @Import 将两个配置类组合在一起：**

```java
package soundsystem;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@Import({CDPlayerConfig.class, CDConfig.class})
public class SoundSystemConfig { 
}
```

如果有一个配置类配置在了 XML 中，则使用 @ImportResource 注解

```java
@Configuration
@Import(CDPlayerConfig.class)
@ImportResource("classpath:cd-config.xml")
public class SoundSystemConfig {
}
```

2.5.2、在 XML 配置中引用 JavaConfig

**在 JavaConfig 配置中，使用 @Import 和 @ImportResource 来拆分 JavaConfig 类**\
**在 XML 中，可以使用 import 元素来拆分 XML 配置**

```xml
<beans  ....>
  <bean />
  <import resource="cdplayer-config.xml" />
</beans>
```

**不管使用 JavaConfig 还是使用 XML 进行装配，通常都会创建一个根配置，这个配置会将两个或更多的装配类 / 或 XML 文件组合起来**

**也会在根配置中启用组件扫描 (通过 ` < context:component-scan>  `或 `@ComponentScan`)**

## **小结：**

Spring 框架的核心是 Spring 容器。容器负责管理应用中组件的生命周期，它会创建这些组件并保证它们的依赖能够得到满足，这样的话，组件才能完成预定的任务

Spring 中装配 bean 的三种主要方式：自动化配置、基于 java 的显式配置以及基于 XML 的显示配置。这些技术描述了 Spring 应用中的组件以及这些组件之间的关系

尽可能使用自动化装配，避免显式装配所带来的维护成本。基于 Java 的配置，它比基于 XML 的配置更加强大、类型安全并且易于重构
