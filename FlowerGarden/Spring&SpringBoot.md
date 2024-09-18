## 什么是Spring框架

我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合

## todo: Spring包含的模块有哪些

![](Attachments/Images/Pasted%20image%2020240315210852.png)

**Core Container**

Spring 框架的核心模块，也可以说是基础模块，主要提供 IoC 依赖注入功能的支持。Spring 其他所有的功能基本都需要依赖于该模块

- **spring-core**：Spring 框架基本的核心工具类。
- **spring-beans**：提供对 bean 的创建、配置和管理等功能的支持。
- **spring-context**：提供对国际化、事件传播、资源加载等功能的支持。
- **spring-expression**：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用

AOP

## Spring,Spring MVC,Spring Boot之间什么关系

Spring 包含了多个功能模块，其中最重要的是 Spring-Core（主要提供 IoC 依赖注入功能的支持） 模块， Spring 中的其他模块（比如 Spring MVC）的功能实现基本都需要依赖于该模块

Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码

使用 Spring 进行开发各种配置过于麻烦比如开启某些 Spring 特性时，需要用 XML 或 Java 进行显式配置。于是，Spring Boot 诞生了

Spring 旨在简化 J2EE 企业应用程序开发。Spring Boot 旨在简化 Spring 开发（减少配置文件，开箱即用！）

# IOC
## 谈谈你对IOC的了解

**IoC（Inversion of Control:控制反转）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。

**为什么叫控制反转？**
- **控制**：指的是对象创建（实例化、管理）的权力
- **反转**：控制权交给外部环境（Spring 框架、IoC 容器）

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来

在 Spring 中， IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象

## IOC解决了什么问题

IoC 的思想就是两方之间不互相依赖，由第三方容器来管理相关资源。这样有什么好处呢？

1. 对象之间的耦合度或者说依赖程度降低；
2. 资源变的容易管理；比如你用 Spring 容器提供的话很容易就可以实现一个单例

## IOC和DI有区别吗

IoC（Inverse of Control:控制反转）是一种设计思想或者说是某种模式。这个设计思想就是 **将原本在程序中手动创建对象的控制权交给第三方比如 IoC 容器。** 对于我们常用的 Spring 框架来说， IoC 容器实际上就是个 Map（key，value）,Map 中存放的是各种对象。不过，IoC 在其他语言中也有应用，并非 Spring 特有。

IoC 最常见以及最合理的实现方式叫做依赖注入（Dependency Injection，简称 DI）

# AOP
## 谈谈你对AOP的了解

**AOP(Aspect-Oriented Programming:面向切面编程)** 能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理

也可以使用 **AspectJ**

## AOP为什么叫面向切面编程

AOP 之所以叫面向切面编程，是因为它的核心思想就是将横切关注点从核心业务逻辑中分离出来，形成一个个的**切面（Aspect）**

## todo: AOP 关键术语

- **横切关注点（cross-cutting concerns）** ：多个类或对象中的公共行为（如日志记录、事务管理、权限控制、接口限流、接口幂等等）。
- **切面（Aspect）**：对横切关注点进行封装的类，一个切面是一个类。切面可以定义多个通知，用来实现具体的功能。
- **连接点（JoinPoint）**：连接点是方法调用或者方法执行时的某个特定时刻（如方法调用、异常抛出等）。
- **通知（Advice）**：通知就是切面在某个连接点要执行的操作。通知有五种类型，分别是前置通知（Before）、后置通知（After）、返回通知（AfterReturning）、异常通知（AfterThrowing）和环绕通知（Around）。前四种通知都是在目标方法的前后执行，而环绕通知可以控制目标方法的执行过程。
- **切点（Pointcut）**：一个切点是一个表达式，它用来匹配哪些连接点需要被切面所增强。切点可以通过注解、正则表达式、逻辑运算等方式来定义。比如 `execution(* com.xyz.service..*(..))`匹配 `com.xyz.service` 包及其子包下的类或接口。
- **织入（Weaving）**：织入是将切面和目标对象连接起来的过程，也就是将通知应用到切点匹配的连接点上。常见的织入时机有两种，分别是编译期织入（AspectJ）和运行期织入（AspectJ）。

## AOP解决了什么问题

OOP 不能很好地处理一些分散在多个类或对象中的公共行为（如日志记录、事务管理、权限控制、接口限流、接口幂等等），这些行为通常被称为 **横切关注点（cross-cutting concerns）** 。如果我们在每个类或对象中都重复实现这些行为，那么会导致代码的冗余、复杂和难以维护。

AOP 可以将横切关注点（如日志记录、事务管理、权限控制、接口限流、接口幂等等）从 **核心业务逻辑（core concerns，核心关注点）** 中分离出来，实现关注点的分离。

## AOP的应用场景有哪些

- 日志记录：自定义日志记录注解，利用 AOP，一行代码即可实现日志记录。
- 性能统计：利用 AOP 在目标方法的执行前后统计方法的执行时间，方便优化和分析。
- 事务管理：`@Transactional` 注解可以让 Spring 为我们进行事务管理比如回滚异常操作，免去了重复的事务管理逻辑。`@Transactional`注解就是基于 AOP 实现的。
- 权限控制：利用 AOP 在目标方法执行前判断用户是否具备所需要的权限，如果具备，就执行目标方法，否则就不执行。例如，SpringSecurity 利用`@PreAuthorize` 注解一行代码即可自定义权限校验。
- 接口限流：利用 AOP 在目标方法执行前通过具体的限流算法和实现对请求进行限流处理。
- 缓存管理：利用 AOP 在目标方法执行前后进行缓存的读取和更新。

## Spring AOP和AspectJ AOP有什么区别

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Spring AOP 已经集成了 AspectJ , Spring AOP更简单, AspectJ更强大更快

## AspectJ定义的通知类型有哪些

- **Before**（前置通知）：目标对象的方法调用之前触发
- **After** （后置通知）：目标对象的方法调用之后触发
- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发
- **AfterThrowing**（异常通知）：目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。
- **Around** （环绕通知）：编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方法

## 多个切面的执行顺序如何控制

1、通常使用`@Order` 注解直接定义切面顺序
2、实现`Ordered` 接口重写 `getOrder` 方法。

# Bean
## 什么是Spring Bean

Bean 代指的就是那些被 IoC 容器所管理的对象
我们需要告诉 IoC 容器帮助我们管理哪些对象，这个是通过配置元数据来定义的。配置元数据可以是 XML 文件、注解或者 Java 配置类

## 将一个类声明为Bean的注解有哪些

- `@Component`：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- `@Controller` : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 `Service` 层返回数据给前端页面。

## @Component和@Bean的区别是什么

- `@Component` 注解作用于类，而`@Bean`注解作用于方法。
- `@Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。
- `@Bean` 注解比 `@Component` 注解的自定义性更强，而且很多地方我们只能通过 `@Bean` 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现

## 注入Bean的注解有哪些

Spring 内置的 `@Autowired` 以及 JDK 内置的 `@Resource` 和 `@Inject` 都可以用于注入 Bean

![](Attachments/Images/Pasted%20image%2020240315212821.png)

## @Autowired和@Resource的区别是什么

`Autowired` 属于 Spring 内置的注解，默认的注入方式为`byType`（根据类型进行匹配），也就是说会优先根据接口类型去匹配并注入 Bean （接口的实现类）
**当一个接口存在多个实现类的话**，`byType`这种方式就无法正确注入对象了,这种情况下，注入方式会变为 `byName`（根据名称进行匹配）,使用的是变量名去寻找 , 可以通过@Qualifier显式指定名称而不是依赖变量的名称

`@Resource`属于 JDK 提供的注解，默认注入方式为 `byName`。如果无法通过名称匹配到对应的 Bean 的话，注入方式会变为`byType`
`@Resource` 有两个比较重要且日常开发常用的属性：`name`（名称）、`type`（类型）
如果仅指定 `name` 属性则注入方式为`byName`，如果仅指定`type`属性则注入方式为`byType`，如果同时指定`name` 和`type`属性（不建议这么做）则注入方式为`byType`+`byName`

总结 : 
- `@Autowired` 是 Spring 提供的注解，`@Resource` 是 JDK 提供的注解。
- `Autowired` 默认的注入方式为`byType`（根据类型进行匹配），`@Resource`默认注入方式为 `byName`（根据名称进行匹配）。
- 当一个接口存在多个实现类的情况下，`@Autowired` 和`@Resource`都需要通过名称才能正确匹配到对应的 Bean。`Autowired` 可以通过 `@Qualifier` 注解来显式指定名称，`@Resource`可以通过 `name` 属性来显式指定名称。
- `@Autowired` 支持在构造函数、方法、字段和参数上使用。`@Resource` 主要用于字段和方法上的注入，不支持在构造函数或参数上使用。

## Bean的作用域有哪些

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 `getBean()` 两次，得到的是不同的 Bean 实例。
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。
- **application/global-session** （仅 Web 应用可用）：每个 Web 应用在启动时创建一个 Bean（应用 Bean），该 bean 仅在当前应用启动时间内有效。
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

通过`@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)`配置bean的作用域

##  Bean是线程安全的吗

Spring 框架中的 Bean 是否线程安全，取决于其作用域和状态。这里以最常用的两种作用域 prototype 和 singleton 为例介绍。几乎所有场景的 Bean 作用域都是使用默认的 singleton ，重点关注 singleton 作用域即可

**prototype 作用域下**，每次获取都会创建一个新的 bean 实例，不存在资源竞争问题，所以不存在线程安全问题。
**singleton 作用域下**，IoC 容器中只有唯一的 bean 实例，可能会存在资源竞争问题（取决于 Bean 是否有状态）。如果这个 bean 是有状态的话，那就存在线程安全问题（有状态 Bean 是指包含可变的成员变量的对象）。

不过，大部分 Bean 实际都是无状态（没有定义可变的成员变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的

对于有状态单例 Bean 的线程安全问题，常见的有两种解决办法：

1. 在 Bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

## todo Bean的生命周期

- Bean 容器找到配置文件中 Spring Bean 的定义。
- Bean 容器利用 Java Reflection API 创建一个 Bean 的实例。
- 如果涉及到一些属性值 利用 `set()`方法设置一些属性值。
- 如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入 Bean 的名字。
- 如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 如果 Bean 实现了 `BeanFactoryAware` 接口，调用 `setBeanFactory()`方法，传入 `BeanFactory`对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果 Bean 实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。
# Spring MVC
## todo : 说说你对Spring MVC的理解

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码

Spring MVC是是属于Spring Framework生态里面的一个模块，它是在Servlet基础上构建并且使用MVC模式设计的一个Web框架

## Spring MVC的核心组件有哪些

- **`DispatcherServlet`**：**核心的中央处理器**，负责接收请求、分发，并给予客户端响应。
- **`HandlerMapping`**：**处理器映射器**，根据 URL 去匹配查找能处理的 `Handler` ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
- **`HandlerAdapter`**：**处理器适配器**，根据 `HandlerMapping` 找到的 `Handler` ，适配执行对应的 `Handler`；
- **`Handler`**：**请求处理器**，处理实际请求的处理器。
- **`ViewResolver`**：**视图解析器**，根据 `Handler` 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 `DispatcherServlet` 响应客户端

## SpringMVC工作原理

![](Attachments/Images/Pasted%20image%2020240315224707.png)

1. 客户端（浏览器）发送请求， `DispatcherServlet`拦截请求。
2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping` 。`HandlerMapping` 根据 URL 去匹配查找能处理的 `Handler`（也就是我们平常说的 `Controller` 控制器） ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
3. `DispatcherServlet` 调用 `HandlerAdapter`适配器执行 `Handler` 。
4. `Handler` 完成对用户请求的处理后，会返回一个 `ModelAndView` 对象给`DispatcherServlet`，`ModelAndView` 顾名思义，包含了数据模型以及相应的视图的信息。`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
5. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
6. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
7. 把 `View` 返回给请求者（浏览器）

## 统一异常处理怎么做

具体会使用到 `@ControllerAdvice` + `@ExceptionHandler`

这种异常处理方式下，会给所有或者指定的 `Controller` 织入异常处理的逻辑（AOP），当 `Controller` 中的方法抛出异常的时候，由被`@ExceptionHandler` 注解修饰的方法进行处理

`ExceptionHandlerMethodResolver` 中 `getMappedMethod` 方法决定了异常具体被哪个被 `@ExceptionHandler` 注解修饰的方法处理异常
`getMappedMethod()`会首先找到可以匹配处理异常的所有方法信息，然后对其进行从小到大的排序，最后取最小的那一个匹配的方法(即匹配度最高的那个)

# 事务
## 什么是事务

**事务是逻辑上的一组操作，要么都执行，要么都不执行**

## 事务的特性(ACID)了解吗

atomic \[əˈtɒmɪk]
atomicity \[ˌætəˈmɪsəti]

- **原子性**（`Atomicity`）：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
- **一致性**（`Consistency`）：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；
- **隔离性**（`Isolation`）：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
- **持久性**（`Durability`）：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

这里要额外补充一点：**只有保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。也就是说 A、I、D 是手段，C 是目的！**


## Spring管理事务的方式有几种?

- **编程式事务**：在代码中硬编码(在分布式系统中推荐使用) : 通过 `TransactionTemplate`或者 `TransactionManager` 手动管理事务，事务范围过大会出现事务未提交导致超时，因此事务要比锁的粒度更小。
- **声明式事务**：在 XML 配置文件中配置或者直接基于注解（单体应用或者简单业务系统推荐使用） : 实际是通过 AOP 实现（基于`@Transactional` 的全注解方式使用最多）

## PlatformTransactionManager,TransactionDefinition,TransactionStatus

Spring 框架中，事务管理相关最重要的 3 个接口如下：

- **`PlatformTransactionManager`**：（平台）事务管理器，Spring 事务策略的核心。
- **`TransactionDefinition`**：事务定义信息(事务隔离级别、传播行为、超时、只读、回滚规则)。
- **`TransactionStatus`**：事务运行状态。

**PlatformTransactionManager**

**Spring 并不直接管理事务，而是提供了多种事务管理器** 。Spring 事务管理器的接口是：**`PlatformTransactionManager`**

通过这个接口，Spring 为各个平台如：JDBC(`DataSourceTransactionManager`)、Hibernate(`HibernateTransactionManager`)、JPA(`JpaTransactionManager`)等都提供了对应的事务管理器，但是具体的实现就是各个平台自己的事情了

```java
public interface PlatformTransactionManager {
    //获得事务
    TransactionStatus getTransaction(@Nullable TransactionDefinition var1) throws TransactionException;
    //提交事务
    void commit(TransactionStatus var1) throws TransactionException;
    //回滚事务
    void rollback(TransactionStatus var1) throws TransactionException;
}
```

**TransactionDefinition** : 事务属性

事务管理器接口 **`PlatformTransactionManager`** 通过 **`getTransaction(TransactionDefinition definition)`** 方法来得到一个事务，这个方法里面的参数是 **`TransactionDefinition`** 类 ，这个类就定义了一些基本的事务属性。

**什么是事务属性呢？** 事务属性可以理解成事务的一些基本配置，描述了事务策略如何应用到方法上。

事务属性包含了 5 个方面：

- 隔离级别
- 传播行为
- 回滚规则
- 是否只读
- 事务超时

```java
public interface TransactionDefinition {
    int PROPAGATION_REQUIRED = 0;
    int PROPAGATION_SUPPORTS = 1;
    int PROPAGATION_MANDATORY = 2;
    int PROPAGATION_REQUIRES_NEW = 3;
    int PROPAGATION_NOT_SUPPORTED = 4;
    int PROPAGATION_NEVER = 5;
    int PROPAGATION_NESTED = 6;
    int ISOLATION_DEFAULT = -1;
    int ISOLATION_READ_UNCOMMITTED = 1;
    int ISOLATION_READ_COMMITTED = 2;
    int ISOLATION_REPEATABLE_READ = 4;
    int ISOLATION_SERIALIZABLE = 8;
    int TIMEOUT_DEFAULT = -1;
    // 返回事务的传播行为，默认值为 REQUIRED。
    int getPropagationBehavior();
    //返回事务的隔离级别，默认值是 DEFAULT
    int getIsolationLevel();
    // 返回事务的超时时间，默认值为-1。如果超过该时间限制但事务还没有完成，则自动回滚事务。
    int getTimeout();
    // 返回是否为只读事务，默认值为 false
    boolean isReadOnly();

    @Nullable
    String getName();
}
```

**TransactionStatus** : 事务状态
`TransactionStatus`接口用来记录事务的状态 该接口定义了一组方法,用来获取或判断事务的相应状态信息。

`PlatformTransactionManager.getTransaction(…)`方法返回一个 `TransactionStatus` 对象。

```java
public interface TransactionStatus{
    boolean isNewTransaction(); // 是否是新的事务
    boolean hasSavepoint(); // 是否有恢复点
    void setRollbackOnly();  // 设置为只回滚
    boolean isRollbackOnly(); // 是否为只回滚
    boolean isCompleted; // 是否已完成
}
```


## TransactionDefinition中的事务传播属性

![](Attachments/Images/Pasted%20image%2020240330203711.png)

**事务传播行为是为了解决业务层方法之间互相调用的事务问题**

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中运行，也可能开启一个新事务，并在自己的事务中运行

正确的事务传播行为可能的值如下: (propagation: 传播)

**1.`TransactionDefinition.PROPAGATION_REQUIRED`** 

使用的最多的一个事务传播行为，我们平时经常使用的`@Transactional`注解默认使用就是这个事务传播行为。如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

**`2.TransactionDefinition.PROPAGATION_REQUIRES_NEW`**

创建一个新的事务，如果当前存在事务，则把当前事务挂起。也就是说不管外部方法是否开启事务，`Propagation.REQUIRES_NEW`修饰的内部方法会新开启自己的事务，且开启的事务相互独立，互不干扰。

**3.`TransactionDefinition.PROPAGATION_NESTED`** (nested : 嵌套的)

如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于`TransactionDefinition.PROPAGATION_REQUIRED`。
嵌套事务只会执行部分回滚 , 内部报错只回滚内部 , 外部报错全部回滚

**4.`TransactionDefinition.PROPAGATION_MANDATORY `** (mandatory :强制性)

如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

这个使用的很少。

若是错误的配置以下 3 种事务传播行为，事务将不会发生回滚：

- **`TransactionDefinition.PROPAGATION_SUPPORTS`**: 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- **`TransactionDefinition.PROPAGATION_NOT_SUPPORTED`**: 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- **`TransactionDefinition.PROPAGATION_NEVER`**: 以非事务方式运行，如果当前存在事务，则抛出异常。

## TransactionDefinition中的隔离级别

- **`TransactionDefinition.ISOLATION_DEFAULT`** :使用后端数据库默认的隔离级别，MySQL 默认采用的 `REPEATABLE_READ` 隔离级别 Oracle 默认采用的 `READ_COMMITTED` 隔离级别.
- **`TransactionDefinition.ISOLATION_READ_UNCOMMITTED`** :最低的隔离级别，使用这个隔离级别很少，因为它允许读取尚未提交的数据变更，**可能会导致脏读、幻读或不可重复读**
- **`TransactionDefinition.ISOLATION_READ_COMMITTED`** : 允许读取并发事务已经提交的数据，**可以阻止脏读，但是幻读或不可重复读仍有可能发生**
- **`TransactionDefinition.ISOLATION_REPEATABLE_READ`** : 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，**可以阻止脏读和不可重复读，但幻读仍有可能发生。**
- **`TransactionDefinition.ISOLATION_SERIALIZABLE`** : 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，**该级别可以防止脏读、不可重复读以及幻读**。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

## TransactionDefinition的其他属性

事务超时属性timeout,事务只读属性readOnly,事务回滚规则rollbackFor

## @Transactional(rollbackFor = Exception.class)注解了解吗

当 `@Transactional` 注解作用于类上时，该类的所有 public 方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义

`@Transactional` 注解默认回滚策略是只有在遇到`RuntimeException`(运行时异常) 或者 `Error` 时才会回滚事务，而不会回滚 `Checked Exception`（抛出的受检查异常）。这是因为 Spring 认为`RuntimeException`和 Error 是不可预期的错误，而受检异常是可预期的错误，可以通过业务逻辑来处理

可以使用 `@Transactional` 注解的 `rollbackFor` 和 `noRollbackFor` 属性来指定哪些异常需要回滚，哪些异常不需要回滚

## @Transactional事务注解原理

`@Transactional` 的工作机制是基于 AOP 实现的，AOP 又是使用动态代理实现的。如果目标对象实现了接口，默认情况下会采用 JDK 的动态代理，如果目标对象没有实现了接口,会使用 CGLIB 动态代理。

# Spring Boot

## 为什么要有Springboot

Springboot旨在简化Spring开发

## Springboot有哪些优点

1. 开发容易,不需要编写大量样板代码,xml配置等
2. 可以很容易的与Spring生态系统集成,如Spring JDBC,ORM,Security
3. 提供嵌入式HTTP服务器,如Tomcat和Jetty,可以轻松的开发和测试web程序应用
4. 提供了多种插件,可以使用内置工具(Maven和Gradle)开发和测试Springboot应用程序

## 什么是Spring Boot Starter

**Spring Boot Starter是一系列依赖关系的集合**

如果没有它, Spring MVC,Tomcat和Jackson这样的库需要手动一个一个添加,但是现在添加spring-boot-starter-web即可

## Springboot支持内嵌哪些Servlet容器

![](Attachments/Images/Pasted%20image%2020240316001354.png)

## 如何在Springboot程序中使用Jetty而不是Tomcat

![](Attachments/Images/Pasted%20image%2020240316001455.png)

## 介绍一下@SpringBootApplication注解

@SpringBootApplication可以看做是@Configuration,@EnableAutoConfiguration,@ComponentScan注解的集合

- @EnableAutoConfiguration : 启用Springboot的自动配置机制
- @ComponentScan : 扫描被@Component注解的bean,默认扫描该类所在包下面所有的类
- @Configuration : 允许在上下文中注册额外的bean或者导入其他配置类

## Springboot的自动配置是怎么实现的

@EnableAutoConfiguration里面通过@Import导入了AutoConfigurationImportSelector类,这个类实现了ImportSelector接口,里面有selectImports接口

```java
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {

}

public interface DeferredImportSelector extends ImportSelector {

}

public interface ImportSelector {
    String[] selectImports(AnnotationMetadata var1);
}
```

`AutoConfigurationImportSelector` 类实现了 `ImportSelector`接口，也就实现了这个接口中的 `selectImports`方法，该方法主要用于**获取所有符合条件的类的全限定类名，这些类需要被加载到 IoC 容器中**。

**LoadSpringFactories（）方法**

在selectImports里面会调用到getAutoConfigurationEntry( )方法

**第 1 步**:

判断自动装配开关是否打开。默认`spring.boot.enableautoconfiguration=true`，可在 `application.properties` 或 `application.yml` 中设置

**第 2 步**：

用于获取`EnableAutoConfiguration`注解中的 `exclude` 和 `excludeName`

**第 3 步**

获取需要自动装配的所有配置类，读取`META-INF/spring.factories`

spring-boot/spring-boot-project/spring-boot-autoconfigure/src/main/resources/META-INF/spring.factories

**第 4 步**：

筛选满足@Conditional注解的配置类

## 什么是yaml,yaml的优势在哪里

yaml是一种人类可读的数据序列化语言
yaml配置的方式更加直观清晰,简洁明了,有层次感,但是不支持@PropertySource注解导入自定义的yaml配置

## Springboot读取配置文件的方法有哪些

1. 通过@Value("${property}")
2. 通过@ConfigurationProperties读取并与@Component注册Bean绑定
3. 通过@ConfigurationProperties和@EnableConfigurationProperties配合注册
4. 通过@PropertySource读取指定的properties文件
5. Environment

## todo: SpringBoot加载配置文件的优先级

## Springboot如何监控系统运行状态

可以使用Spring Boot Actuator
通过get方法访问/health接口,就可以获取应用程序的健康指标

## Spring Boot如何做参数校验

**JSR(Java Specification Requests）** 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解
校验的时候我们实际用的是 **Hibernate Validator** 框架,这个框架是JSR参数校验的具体实现
需要引入spring-boot-starter-validation依赖

**以下是一些常用的字段验证的注解:**
- `@NotEmpty` 被注释的字符串的不能为 null 也不能为空
- `@NotBlank` 被注释的字符串非 null，并且必须包含一个非空白字符
- `@Null` 被注释的元素必须为 null
- `@NotNull` 被注释的元素必须不为 null
- `@AssertTrue` 被注释的元素必须为 true
- `@AssertFalse` 被注释的元素必须为 false
- `@Pattern(regex=,flag=)`被注释的元素必须符合指定的正则表达式
- `@Email` 被注释的元素必须是 Email 格式。
- `@Min(value)`被注释的元素必须是一个数字，其值必须大于等于指定的最小值
- `@Max(value)`被注释的元素必须是一个数字，其值必须小于等于指定的最大值
- `@DecimalMin(value)`被注释的元素必须是一个数字，其值必须大于等于指定的最小值
- `@DecimalMax(value)` 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
- `@Size(max=, min=)`被注释的元素的大小必须在指定的范围内
- `@Digits(integer, fraction)`被注释的元素必须是一个数字，其值必须在可接受的范围内
- `@Past`被注释的元素必须是一个过去的日期
- `@Future` 被注释的元素必须是一个将来的日期

## @NotEmpty对比@NotBlank

`@NotEmpty`不能为 null，且长度必须大于0，一般用在**集合类或数组**上。换句话说 ，被标注的字段必须至少包含一个字符、元素或项
`@NotBlank`只适用于字符串类型的字段，不能为 null，且调用 trim() 后，长度必须大于0，既：必须有实际字符

在需要验证的参数上添加@Valid注解,如果验证失败,会抛出MethodArgumentNotValidException

## @Validated和@Valid功能上的区别：

注解位置: 

- `@Validated`：用在类型、方法和方法参数上。但不能用于成员属性（field）
- `@Valid`：可以用在方法、构造函数、方法参数和成员属性（field）上

分组校验 :

- `@Validated`：提供分组功能，可以在参数验证时，根据不同的分组采用不同的验证机制
- `@Valid`：没有分组功能

嵌套校验 :

默认都不会做嵌套校验,但是可以在属性上加上@Valid,不能加@Validated


## 如何做分组校验

1. 定义分组接口
2. 定义需要检验的参数bean,在各种注解中指明groups属性
3. @Validated()中指定分组

```java
public interface IGroupA { }   public interface IGroupB { }

//--------------------------------------
//不指定分组,都会进行检查
@NotBlank(message = "用户名不能为空") 
private String name;
//只在分组为IGroupB的情况下进行验证 
@Min(value = 18, message = "年龄不能小于18岁", groups = {IGroupB.class}) private Integer age;
//-------------------------------------

//检查分组为GroupA的情况
public String addStu(@Validated({IGroupA.class}) @RequestBody StudentBean studentBean)

```

结果就是只会对name进行检查

## 什么是组序列?怎么定义组序列?

默认情况下 不同级别的约束验证是无序的，但是在一些情况下，顺序验证却是很重要。

一个组可以定义为其他组的序列，使用它进行验证的时候必须符合该序列规定的顺序。在使用组序列验证的时候，如果序列前边的组验证失败，则后面的组将不再给予验证。

使用@GroupSequence定义组序列

```java

//定义组序列
@GroupSequence({Default.class, IGroupA.class, IGroupB.class}) public interface IGroup { }

//需要检验的Bean
@NotBlank(message = "用户名不能为空") 
private String name; 
@Min(value = 18, message = "年龄不能小于18岁", groups = IGroupA.class) private Integer age; 
@Pattern(regexp = "^((13[0-9])|(14[5,7,9])|(15([0-3]|[5-9]))|(166)|(17[0,1,3,5,6,7,8])|(18[0-9])|(19[8|9]))\\d{8}$", message = "手机号格式错误") 
private String phoneNum; 
@Email(message = "邮箱格式错误") 
private String email;
@MyConstraint(groups = IGroupB.class) 
private String className;

//使用组序列检验
public String addStu(@Validated({IGroup.class}) @RequestBody StudentBean studentBean)
```

如果age出错，那么对组序列在IGroupA后的IGroupB不进行校验，即例子中的className不进行校验


## Springboot如何实现定时任务

在类上配置@Scheduled注解
启动类上配置@EnableScheduling注解

## BeanFactory和FactoryBean的区别

BeanFactory : **这个其实是所有Spring Bean的容器根接口，给Spring 的容器定义一套规范，给IOC容器提供了一套完整的规范，比如我们常用到的getBean方法等**

FactoryBean : 该类是SpringIOC容器是创建Bean的一种形式

有些人就要问了，我直接使用Spring默认方式创建Bean不香么，为啥还要用FactoryBean做啥，在某些情况下，对于实例Bean对象比较复杂的情况下，使用传统方式创建bean会比较复杂，例如（使用xml配置），这样就出现了FactoryBean接口，可以让用户通过实现该接口来自定义该Bean接口的实例化过程。即包装一层，将复杂的初始化过程包装，让调用者无需关系具体实现细节。

 BeanFactory和FactoryBean其实没有什么比较性的，只是两者的名称特别接近，所以有时候会拿出来比较一番，BeanFactory是提供了OC容器最基本的形式，给具体的IOC容器的实现提供了规范，FactoryBean可以说为IOC容器中Bean的实现提供了更加灵活的方式，FactoryBean在IOC容器的基础上给Bean的实现加上了一个简单工厂模式和装饰模式，我们可以在getObject()方法中灵活配置


## 过滤器和拦截器的区别

什么是过滤器

过滤器Filter基于Servlet实现，过滤器的主要应用场景是对字符编码、跨域等问题进行过滤。Servlet的工作原理是拦截配置好的客户端请求，然后对Request和Response进行处理。Filter过滤器随着web应用的启动而启动，只初始化一次。

Filter的使用比较简单，继承Filter 接口，实现对应的init、doFilter以及destroy方法即可。

---

什么是拦截器

拦截器是SpringMVC中实现的一种基于Java反射（动态代理）机制的方法增强工具，拦截器的实现是**继承HandlerInterceptor 接口**，并实现接口的preHandle、postHandle和afterCompletion方法。

1、**preHandle**：请求方法前置拦截，该方法会在Controller处理之前进行调用，Spring中可以有多个Interceptor，这些拦截器会按照设定的Order顺序调用，当有一个拦截器在preHandle中返回false的时候，请求就会终止。

2、**postHandle**：preHandle返回结果为true时，在Controller方法执行之后，视图渲染之前被调用

3、**afterCompletion**：在preHandle返回ture，并且整个请求结束之后，执行该方法。

---

区别

**相同点**：

1、拦截器与过滤器都是体现了AOP的思想，对方法实现增强，都可以拦截请求方法。

2、拦截器和过滤器都可以通过Order注解设定执行顺序

**不同点**：

1、**过滤器属于Servlet级别，拦截器属于Spring级别** Filter是在javax.servlet包中定义的，要依赖于网络容器，因此只能在web项目中使用。

Interceptor是SpringMVC中实现的，归根揭底拦截器是一个Spring组件，由Spring容器进行管理。

2、**过滤器和拦截器的执行顺序不同**：

![](Attachments/Images/Pasted%20image%2020240410103913.png)

首先当一个请求进入Servlet之前，过滤器的doFilter方法进行过滤，

进入Servlet容器之后，执行Controller方法之前，拦截器的preHandle方法进行拦截，

执行Controller方法之后，视图渲染之前，拦截器的postHandle方法进行拦截，

请求结束之后，执行拦截器的postHandle方法。

3、过滤器基于**函数回调**方式实现，拦截器基于**Java反射机制**实现

---

使用场景

实际开发中，拦截器的应用场景会比过滤器要更多，下面是拦截器和过滤器的主要应用场景

**拦截器的应用场景**：权限控制，日志打印，参数校验

**过滤器的应用场景**：跨域问题解决，编码转换

拦截器在用户权限校验场景下使用会比较多，由于一般是前后端分离项目，过滤器的使用场景就会少很多。

