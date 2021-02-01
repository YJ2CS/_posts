---
title: spring-in-action-4-第三部分
siteurl: spring-in-action-4-第三部分
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-spring-in-action-4-第三部分
date: '2021-01-29T10:33:06+08:00'
date updated: '2021-01-29T10:52:53+08:00'

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

## 3、第三章：高级装配

本章内容：

- Spring profile
- 条件化的 bean 声明
- 自动装配与歧义性
- bean 的作用域
- Spring 表达式语言

3.1、环境与 profile

开发软件的时候，一个很大的挑战就是将应用程序从一个环境迁移到另外一个环境。数据库配置、加密算法以及外部系统的集成是跨环境部署时会发生变化的典型例子

3.1.1、配置 profile bean

Spring 为环境相关的 bean 所提供的解决方案其实与构建时的方案没有太大差别。在这个过程中需要根据环境决定该创建哪个 bean 和不创建哪个 bean

不过 spring 并不是构建的时候做这个决策，而是运行时。这样的结果就是同一个部署单元（可能会是 WAR 文件）能够适用于所有的环境，没有必要重新构建

在 Java 配置中，可以使用 `@Profile` 注解指定某个 bean 属于哪一个 profile：

```java
@Configuration
@Profile("dev")
public class DevelopmentProfileConfig {
  @Bean(destroyMethod="shutdown")
  public DataSource dataSource() {
    return new EmbeddedDatabaseBuiler()
         .setType(EmbeddedDatabaseType.H2)
         .addScript("classpath:schema.sql")
         .addScript("classpath:test-data.sql")
         .build(); 
  }  
}
```

上面是在类级别上使用 @Profile 注解。从 Spring3.2 开始，也可以在方法级别上使用 @Profile 注解

在 XML 中配置 profile

可以通过 `<beans`> 元素的 profile 属性，在 XML 中配置 profile bean

```xml
//dev profile的bean
<beans profile="dev">
    <jdbc:embedded-database>
        <jdbc:script location="classpath:shema.sql" />
        <jdbc:script location="classpath:test-data.sql" />
    </jdbc:embedded-database>
</beans>
//qa profile的bean
<beans profile="qa">
    <bean 
    class= .../>
</beans>
```

#### 3.1.2、激活 profile

Spring 在确定哪个 profile 处于激活状态时，需要依赖两个独立的属性：spring.profiles.active 和 spring.profiles.default

如果设置了 spring.profiles.active 属性的话，它的值就会用来确定哪个 profile 是激活的。如果没有，Spring 将会查找 spring.profiles.default 的值

如果两者均没有设置，那就没有激活的 profile，因此只会创建那些没有定义在 profile 中的 bean

有多种方式来设置这两个属性：

- 作为 DispatcherServlet 的初始化参数
- 作为 web 应用的上下文参数
- 作为 JNDI 条目
- 作为环境变量
- 作为 JVM 的系统属性
- 在集成测试类上，使用 @ActiveProfiles 注解设置

```xml
//在web应用的web.xml文件中设置默认的profile
//为上下文设置默认的profile
<context-param>
    <param-name>spring.profiles.default</param-name>
    <param-value>dev</param-value>
</context-param>

//为Servlet设置默认的profile
<servlet>
  <servlet-name>appServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>spring.profiles.default</param-name>
    <param-value>dev</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>
```

Spring 提供了 @ActiveProfiles 注解，可以用它来指定运行测试时要激活哪个 profile。在集成测试时，通常想要激活的是开发环境的 profile

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes={PersistenceTestConfig.class})
@ActiveProfiles("dev")
public class PersistenceTest {
}
```

3.2、条件化的 bean

**@Condition 注解，它可以用到带有 @Bean 注解的方法上。**

**如果给定的条件计算结果为 true，就会创建这个 bean，否则这个 bean 就会被忽略**

```java
@Bean
@Conditional(MagicExistsCondition.class)
public MagicBean magicBean() {
  return new MagicBean();  
}
```

可以看到，@Conditional 中给定了一个 Class，它指明了条件——也就是 MagicExistsCondition

@Conditional 将会通过 Condition 接口进行条件对比：

```java
public interface Condition {
  boolean matches(ConditionContext ctxt, AnnotatedTypeMetadata metadata);  
}
```

设置给 @Conditional 的类可以是任意实现了 Condition 接口的类型。这个接口实现起来简单直接，只需提供 matches() 方法的实现即可

```java
public class MagicExistsCondition implements Condition {
  public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    Environment env = context.getEnvironment();
    return env.containsProperty("magic");
  }  
}
```

ConditionContext 是一个接口，大致如下：

```java
public interface ConditionContext {
  BeanDefinitionRegistry getRegistry();
  ConfigurableListableBeanFactory getBeanFactory();
  Environment getEnvironment();
  ResourceLoader getResourceLoader();
  ClassLoader getClassLoader();      
}
```

通过 ConditionContext，我们可以做到如下几点：

- 借助 getRegistry() 返回的 BeanDefinitionRegistry 检查 bean 定义
- 借助 getBeanFactory() 返回的 ConfigurableListableBeanFactory 检查 bean 是否存在，甚至探查 bean 的属性
- 借助 getEnvironment() 返回的 Environment 检查环境变量是否存在以及它的值是什么
- 读取并探查 getResourceLoader() 返回的 ResourceLoader 所加载的资源
- 借助 getClassLoader() 返回的 ClassLoader 加载并检查类是否存在

AnnotatedTypeMetadata 则能够让我们检查带有 @Bean 注解的方法上还有什么其他注解，它也是一个接口：

```java
public interface AnnotatedTypeMetadata {
  boolean isAnnotated(String annotationType);
  Map<String, Object> getAnnotationAttributes(String annotationType);
  Map<String, Object> getAnnotationAttributes(String annotationType,boolean classValuesAsString);
  MultiValueMap<String, Object> getAllAnnotationAttributes(String annotationType);
  MultiValueMap<String, Object> getAllAnnotationAttributes(String annotationType, boolean classValuesAsString);  
}
```

借助 isAnnotated() 方法，能够判断带有 @Bean 注解的方法是不是还有其他特定的注解。

借助其他的那些方法，能够检查 @Bean 注解的方法上其他注解的属性

Spring4 开始，对 @Profile 注解进行了重构，使其基于 @Conditional 和 Condition 实现：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@Documented
@Conditional(ProfileCondition.class)
public @interface Profile {
  String[] value();    
}
```

```java
//ProfileCondition检查某个bean profile是否可用
class ProfileCondition implements Condition {
  public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    if (context.getEnvironment() != null) {
      MultiValueMap<String, Object> attrs = metadata.getAllAnnotationAttributes(Profile.class.getName());
      if (attrs != null) {
        for (Object value : atrrs.get("value")) {
          if (context.getEnvironment().acceptsProfiles(((String[])value))) {
            return true;
          }
        }
        return false;
      }
    }
    return true;    
  }
}
```

ProfileCondition 通过 AnnotatedTypeMetadata 得到了用于 @Profile 注解的所有属性。借助该信息，它会明确检查 value 属性，该属性包含了 bean 的 profile 名称

然后它根据通过 ConditionContext 得到的 Environment 来检查 [借助 acceptsProfiles() 方法]该 profile 是否处于激活状态

3.3、处理自动装配的歧义性

假如我们使用 @Autowired 注解标注了 setDessert() 方法

```java
@Autowired
public void setDessert(Dessert dessert) {
  this.dessert = dessert;  
}
```

本例中，Dessert 是一个接口，且有三个类实现了这个接口，分别为 Cake、Cookies 和 IceCream：

```java
@Component
public class Cake implements Dessert { ... }

@Component
public class Cookies implements Dessert { ... }

@Component
public class IceCream implements Dessert { ... }
```

Spring 没有办法识别应该自动装载哪一个，就会抛出 NoUniqueBeanDefinitionException

发生歧义性的时候，可以将可选 bean 中的某一个设置为首先（primary），或使用限定符（qualifier）来帮助 Spring 将可选的 bean 的范围缩小到只有一个 bean

3.3.1、标示首选的 bean——@Primary 注解：它只能标示一个优先的可选方案

```java
@Component
@Primary
public class IceCream implements Dessert {...}
```

如果通过 Java 配置显式声明：

```java
@Bean
@Primary
public Dessert iceCream() {
  return new IceCream();  
}
```

如果使用 XML 配置 bean：

```xml
<bean primary="true" />
```

3.3.2、限定自动装配的 bean

Spring 的限定符能够在所有可选的 bean 上进行缩小范围的操作，最终能够达到只有一个 bean 满足所规定的限制条件

```java
//确保要将IceCream注入到setDessert()之中：
@Autowired
@Qualifier("iceCream")
public void setDessert(Dessert dessert) {
  this.dessert = dessert;  
}
```

为 @Qulifier 注解所设置的参数就是想要注入的 bean 的 ID——**基于 bean ID 作为限定符问题：与要注入的 bean 的名称是紧耦合的**

创建自定义的限定符——可以为 bean 设置自己的限定符，而不是依赖于将 bean ID 作为限定符

所要做的就是在 bean 声明上添加 @Qualifier 注解。例如，它可以与 @Component 组合使用：

```java
@Component
@Qualifier("cold")
public class IceCream implements Desserts {...}
```

这种情况下，cold 限定符分配给了 IceCreambean，因为它没有耦合类名，因此可以随意重构 IceCream 的类名。

在注入的地方，只要引用 cold 限定符就可以了：

```java
@Autowired
@Qualifier("cold")
public void setDessert(Dessert dessert) {
  this.dessert = dessert;  
}
```

当使用自定义的 @Qualifier 值时，最佳实践是为 bean 选择特征性或描述性术语

3.4、bean 的作用域——**@Scope 注解**

默认情况，Spring 应用上下文所有 bean 都是单例 (singleton) 的形式创建的

Spring 定义了多种作用域：

- 单例（singleton）：在整个应用中，只创建 bean 的一个实例
- 原型（prototype）：每次注入或者通过 Spring 应用上下文获取的时候，都会创建一个新的 bean 实例
- 会话（session）：在 web 应用中，为每个会话创建一个 bean 实例
- 请求（request）：在 web 应用中，为每个请求创建一个 bean 实例

```java
//也可以使用@Scope("prototype")设置原型作用域，当不那么安全
@Component //如果使用组件扫描来发现和什么bean
//@Bean：在java配置中将Notepad声明为原型bean
@Scope(ConfigurabaleBeanFactory.SCOPE_PROTOTYPE)
public class Notepad {...}
```

 同样，如果使用 XML 配置 bean 的话，可以使用 `<bean>` 元素的 scope 属性来设置作用域：

```xml
<bean scope="prototype" />
```

3.4.1、使用会话和请求作用域

在 web 应用中，如果能够实例化在会话和请求范围内共享的 bean，那将是非常有价值的事情。

如，在电子商务应用中，可能会有一个 bean 代表用户的购物车，如果购物车是单例的话，那么将导致所有的用户都会向同一个购物车中添加商品

另一方面，如果购物车是原型作用域的，那么应用中某一个地方往购物车中添加商品，在应用的另外一个地方可能就不可用了

就购物车 bean 来说，会话作用域是最为合适的：在当前会话相关操作中，这个 bean 实际上相当于单例的

```java
@Component
@Scope(value=WebApplicationContext.SCOPE_SESSION, proxyMode=ScopedProxyMode.INTERFACES)
public ShoppingCart cart() {...}
```

proxyMode=ScopedProxyMode.INTERFACES 这个属性解决了将会话或请求作用域的 bean 注入到单例中所遇到的问题

假如我们要将 ShoppingCart bean 注入到单例 StoreSercice bean 的 Setter 方法中：

```java
@Component
public class StoreService {
  @Autowired
  public void setShoppingCart(ShoppingCart shoppingCart) {
    this.shoppingCart = shoppingCart;    
  }    
}
```

因为 StoreService 是一个单例的 bean，会在 Spring 应用上下文加载的时候创建。当它创建的时候，Spring 会试图将 ShoppingCart bean 注入到 setShoppingCart() 方法中

但是后者是会话作用域的，此时并不存在。

另外，系统中将会有多个 ShoppingCart 实例：每个用户一个；我们希望当 StoreService 处理购物车功能时，它所使用的 ShoppingCart 实例恰好是当前会话所对应的那一个

Spring 并不会将实际的 ShoppingCart bean 注入到 StoreService 中，Spring 会注入一个到 ShoppingCart bean 的代理。

这个代理会暴露与 ShoppingCart 相同的方法，所以 StoreService 会认为它就是一个购物车。

但是当 StoreService 调用 ShoppingCart 的方法时，代理会对其进行懒解析并将调用委托给会话作用域内真正的 ShoppingCart bean

ScopedProxyMode.INTERFACES，表明这个代理要实现 ShoppingCart 接口，并将调用委托给实现 bean

**如果 ShoppingCart 是接口而不是类的话，这是可以的（也是最理想的代理模式）**，如果它是一个具体的类的话，Spring 就没有办法创建基于接口的代理了。

此时必须使用 CGLib 来生成基于类的代理，要将 proxyMode 设置为：`ScopedProxyMode.TARGET_CLASS`，表明要以生成目标类扩展的方式创建代理

![](https://img2018.cnblogs.com/blog/286343/201908/286343-20190803140734987-2042752098.png)

3.4.2、在 XML 中声明作用域代理

```xml
<bean scope="session">
    <aop:scoped-proxy />
</bean>
```

`<aop:scoped-proxy>` 告诉 Spring 为 bean 创建一个作用域代理。默认，它会使用 CGLib 创建目标类的代理，可以要求它生成基于接口的代理：

```xml
<aop:scoped-proxy proxy-target-class="false" />
```

3.5、运行时值注入

当讨论依赖注入的时候，通常讨论的是将一个 bean 引用注入到另一个 bean 的属性或构造器参数中。它通常来讲是指的是将一个对象与另一个对象进行关联

但 bean 装配的另外一个方面指的是将一个值注入到 bean 的属性或者构造器参数中

为避免硬编码，想让值在运行时再确定，为了实现这些功能，Spring 提供了两种在运行时求值的方式：

- 属性占位符（Property placeholder）
- Spring 表达式语言（SpEL）

3.5.1、注入外部的值

处理外部值的最简单方式就是声明属性源并通过 Spring 的 Environment 来检索属性：

```java
//使用@PropertySource注解和Environment
@Configuration
@PropertySource("classpath:/com/soundsystem/app.properties") //声明属性源
public class ExpressiveConfig {
  @Autowired
  Environment env;
  @Bean
  public BlankDisc disc() {
    return new BlanDisc(env.getProperty("disc.title"), env.getProperty("disc.artist")); //检索属性值
  }        
}
```

@PropertySource 引用了类路径中一个名为 app.properties 的文件：

```properties
disc.title=Sgt. Peppers Lonely Hearts Club Band
disc.artist=The Beatles
```

这个属性文件会加载到 Spring 的 Environment 中。稍后可从这里检索属性。同时，在 disc() 方法中，会创建一个新的 BlankDisc，它的构造器参数是从属性文件中获取的，

而这是通过调用 getProperty() 实现的，它有四个重载的方法：

```java
String getProperty(String key)
String getProperty(String key, String defaultValue)
T getProperty(String key, Class<T> type)
T getProperty(String key, Class<T> type, T defaultValue)
```

如果想检查一下某个属性是否存在的话，调用 containsProperty() 方法

```java
boolean titleExists = env.containsProperty("disc.title");
```

如果想将属性解析为类的话，可以使用 getPropertyAsClass() 方法：

```java
Class<CompactDisc> cdClass = env.getPropertyAsClass("disc.class", CompactDisc.class);
```

Environment 还提供了一些方法来检查哪些 profile 处于激活状态：

```java
String[] getActiveProfiles() //返回激活profile名称的数组
String[] getDefaultProfiles() //返回默认profile名称的数组
boolean acceptsProfiles(String... profiles) //如果environment支持给定profile的话，就返回true
```

解析属性占位符

在 Spring 装配中，占位符的形式为使用 ${...}  包装的属性名称：

```xml
<bean c:_title="${disc.title}" c:_artist="${disc.artist}" />
```

如果我们依赖于组件扫描和自动装配来创建和初始化应用组件的话，可以使用 @Value 注解：

```java
public BlankDisc(@Value("${disc.title}" String title, @Value("${disc.artist}") String artist) {
  this.title = title;
  this.artist = artist;    
}
```

为了使用占位符，要配置 PropertySourcesPlaceholderConfigurer。

如下的 @Bean 方法在 Java 中配置了 PropertySourcesPlaceholderConfigurer：

```java
@Bean
public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() {
  return new PropertySourcesPlaceholderConfigurer();  
}
```

如果使用 XML 配置的话，Spring context 命名空间中的 `<context:propertyplaceholder>` 元素将会为你生成 `PropertySourcesPlaceholderConfigurer bean`

3.5.2、使用 Spring 表达式语言进行装配：强大简洁

- 使用 bean 的 ID 来引用 bean
- 调用方法和访问对象的属性
- 对值进行算术、关系和逻辑运算
- 正则表达式匹配
- 集合操作

**SpEL 表达式要放到 “`#{...}`” 之中**

```java
//通过systemProperties对象引用系统属性：
#{systemProperties['disc.title']}
```

注意：不要让你的表达式太智能，否则测试越只要。建议尽可能让表达式保持简洁
