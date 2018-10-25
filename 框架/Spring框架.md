## Spring框架

## 依赖注入

### 无依赖注入

![1532244930765](E:\mydairy\框架\无依赖注入.png)

缺点：强耦合，实现功能单一，测试较难

### 实现方式一  构造器注入

![1532245052847](E:\mydairy\框架\构造器注入.png)

要点是BraveKnight没有与任何特定的Quest实现发生耦
合。对它来说，被要求挑战的探险任务只要实现了Quest接口，那么
具体是哪种类型的探险就无关紧要了。这就是DI所带来的最大收益
——松耦合。功能丰富

### 实现方式二 XML配置方式

![1532245632324](E:\mydairy\框架\XML配置类.png)

通过XML将SlayDragonQuest注入到BraveKnight中

![1532245665128](E:\mydairy\框架\XML配置注入.png)

XML 注入使用方法

![1532246192614](E:\mydairy\框架\XML注入使用过程.png)

通过ClassPathXmlApplicationContext 获取相应配置文件

通过context.getBean 实例化对应的类，然后调用，具体实例化过程不可见，对用户透明，只有配置文件知道实际调用过程。

###实现方式三 通过注解方式注入

![1532245937954](E:\mydairy\框架\注解注入.png)

## 应用切面AOP

DI能够让相互协作的软件组件保持松散耦合，而面向切面编程允许你把遍布应用各处的功能分离出来形成可用的组件

这些组件还经常承担着额外的职责。诸如日志、事务管理和安全这样的系统服务经常融入到自身具有核心业务逻辑的组件中去，这些系统服务通常被称为横切关注点，因为它们会跨越系统的多个组件。

### 通过XML声明AOP

![1532247422888](E:\mydairy\框架\XML使用AOP.png)

## Bean

Spring容器负责创建对象，装配他们，配置他们并且管理他们的整个生命周期，从生存到死亡（从new到funaleze()）

Spring自带了多种类型的应用上下文。下面罗列的几个是你最有可能
遇到的。
AnnotationConfigApplicationContext：从一个或多个
基于Java的配置类中加载Spring应用上下文。
AnnotationConfigWebApplicationContext：从一个或
多个基于Java的配置类中加载Spring Web应用上下文。
ClassPathXmlApplicationContext：从类路径下的一个或
多个XML配置文件中加载上下文定义，把应用上下文的定义文件
作为类资源。
FileSystemXmlapplicationcontext：从文件系统下的一
个或多个XML配置文件中加载上下文定义。
XmlWebApplicationContext：从Web应用下的一个或多个
XML配置文件中加载上下文定义。

## Bean生命周期

下图展示了Bean装载到Spring应用上下文的一个典型的生命周期过程

![1532248290456](E:\mydairy\框架\Bean生命周期.png)

1．Spring对bean进行实例化；
2．Spring将值和bean的引用注入到bean对应的属性中；
3．如果bean实现了BeanNameAware接口，Spring将bean的ID传递给
setBean-Name()方法；
4．如果bean实现了BeanFactoryAware接口，Spring将调
用setBeanFactory()方法，将BeanFactory容器实例传入；
5．如果bean实现了ApplicationContextAware接口，Spring将调
用setApplicationContext()方法，将bean所在的应用上下文的
引用传入进来；
6．如果bean实现了BeanPostProcessor接口，Spring将调用它们
的post-ProcessBeforeInitialization()方法；
7．如果bean实现了InitializingBean接口，Spring将调用它们的
after-PropertiesSet()方法。类似地，如果bean使用init-
method声明了初始化方法，该方法也会被调用；
8．如果bean实现了BeanPostProcessor接口，Spring将调用它们
的post-ProcessAfterInitialization()方法；
9．此时，bean已经准备就绪，可以被应用程序使用了，它们将一直
驻留在应用上下文中，直到该应用上下文被销毁；
10．如果bean实现了DisposableBean接口，Spring将调用它的
destroy()接口方法。同样，如果bean使用destroy-method声明
了销毁方法，该方法也会被调用。

## SpringBean 的载入和解析

概要的bean的加载过程:从XML读取Bean的信息，对xml里面的配置信息读取加载解析，保存起来放在beanDefinition，完成bean信息载入解析存储，最后根据bean的id进行注入。

稍微具体一点的过程:通过XmlBeanFactory来创建BeanFactory对象（静态方法，通过，反射机制来创建默认BeanFactory对象），然后调用loadBeandefinition通过resource（resources是对IO即输入流的封装）来读取Xml文件，再把xml转换DOM对象，然后注册进去BeanDefinition,在注册过程中，会根据bean规则对dom对象解析得到bean的定义，解析出来的内容存储在BeanDefinitionholder中，BeanDefinitionholder是对BeanDefinition(定义了bean 包含作用域，属性，方法，依赖等)的封装。然后取得BeanDefinition并放到BeanDefinitionMap保存，等待注入，到此为止完成了对bean的载入，解析和暂存起来。

总的来说

Spring中bean的加载过程

1.获取配置文件资源

2.对获取的xml资源进行一定的处理检验

3.处理包装资源

4.解析处理包装过后的资源

5.加载提取bean并注册(添加到beanDefinitionMap中)

 

接下来是的注入的过程。

通过bean的name获得beanDefinition

## AOP

DI有助于应用对象间解耦，而AOP可以实现横切关注点与他们所影响的对象之间的解耦。

### 什么是面向切面编程

![1532328120625](E:\mydairy\jvm虚拟机\切面.png)

在使用面向切面编程时，我们仍然在一个地方定义通用功能，但是可以通过声明的方式定义这个功能要以何种方式在何处应用，而无需修改受影响的类。横切关注点可以被模块化为特殊的类，这些类被称为切面（aspect）。这样做有两个好处：首先，现在每个关注点都集中于一个地方，而不是分散到多处代码中；其次，服务模
块更简洁，因为它们只包含主要关注点（或核心功能）的代码，而次要关注点的代码被转移到切面中了

描述切面的常用术语有通知（advice）、切点（pointcut）和连接点（join point）

![1532328399503](E:\mydairy\jvm虚拟机\切面术语.png)

在AOP中切面的工作被称为通知（advice）

Spring切面可以应用5种类型的通知：

前置通知（Before）：在目标方法被调用之前调用通知功能；
后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么；
返回通知（After-returning）：在目标方法成功执行之后调用通知；
异常通知（After-throwing）：在目标方法抛出异常后调用通知；
环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。

Advice(通知)，定义了在连接点做什么，为切面增强提供织入接口。

连接点（Join Point）

连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

切点（PointCut）

如果说通知定义了切面的“什么”和“何时”的话，那么切点就定义了“何处”。切点的定义会匹配通知所要织入的一个或多个连接点。

PointCut 切点，决定Advice作用于那些连接点，也就是说PointCut定义了需要增强的方法的集合

切面（Aspect）

切面是通知和切点的结合。通知和切点共同定义了切面的全部内容——它是什么，在何时和何处完成其功能。

## Spring 提供了4种类型的AOP支持

- 基于代理的经典Spring AOP支持

- 纯POJO切面
- @AspectJ注解驱动的切面
- 注入式AspectJ切面

前三种都是Spring AOP实现的变体，Spring AOP构建在动态代理基础
之上，因此，Spring对AOP的支持局限于方法拦截。
