### 什么是Spring beans?

 beans 是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。
 
 ### 一个 Spring Bean 定义 包含什么？
 
 一个Spring Bean 的定义包含容器必知的所有配置元数据，包括
 - 如何创建一个bean，
 - 它的生命周期详情
 - 及它的依赖。

### 如何给Spring 容器提供配置元数据?

- XML配置文件。

- 基于注解的配置。

- 基于java的配置。


### 解释Spring支持的几种bean的作用域。

Spring框架支持以下五种bean的作用域：

-   **singleton :** bean在每个Spring ioc 容器中只有一个实例。
-   **prototype**：一个bean的定义可以有多个实例。
-   **request**：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext。
-   **session**：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext。
-   **global-session**：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

### 什么是bean装配

装配，或bean 装配是指在Spring 容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。

### 什么是bean的自动装配？

Spring 容器能够自动装配相互合作的bean，这意味着容器不需要\<constructor-arg>和\<property>配置，能通过Bean工厂自动处理bean之间的协作。


### Bean 工厂和 Application contexts  有什么区别

**BeanFactory：**

是Spring里面最底层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能；

**ApplicationContext：**

应用上下文，继承BeanFactory接口，它是Spring的一个更高级的容器，提供了更多的有用的功能；

1) 国际化（MessageSource）

2) 访问资源，如URL和文件（ResourceLoader）

3) 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层  

4) 消息发送、响应机制（ApplicationEventPublisher）

5) AOP（拦截器）

**两者装载bean的区别**

**BeanFactory：**

BeanFactory在启动的时候不会去实例化Bean，中有从容器中拿Bean的时候才会去实例化；

**ApplicationContext：**

ApplicationContext在启动的时候就把所有的Bean全部实例化了。它还可以为Bean配置lazy-init=true来让Bean延迟实例化；

**我们该用BeanFactory还是ApplicationContent**

延迟实例化的优点：（**BeanFactory**）

应用启动的时候占用资源很少；对资源要求较高的应用，比较有优势； 

不延迟实例化的优点： （**ApplicationContext**）

1\. 所有的Bean在启动的时候都加载，系统运行的速度快； 

2\. 在启动的时候所有的Bean都加载了，我们就能在系统启动的时候，尽早的发现系统中的配置问题 

3\. 建议web应用，在启动的时候就把所有的Bean都加载了。（把费时的操作放到系统启动中完成）


## Spring Bean的生命周期

简单来说，Spring Bean的生命周期只有四个阶段：实例化 Instantiation --> 属性赋值 Populate  --> 初始化 Initialization  --> 销毁 Destruction

（1）实例化Bean：

对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。

对于ApplicationContext容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean。

（2）设置对象属性（依赖注入）：实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring根据BeanDefinition中的信息 以及 通过BeanWrapper提供的设置属性的接口完成属性设置与依赖注入。

（3）处理Aware接口：Spring会检测该对象是否实现了xxxAware接口，通过Aware类型的接口，可以让我们拿到Spring容器的一些资源：
- 如果这个Bean实现了BeanNameAware接口，会调用它实现的setBeanName(String beanId)方法，传入Bean的名字；
- 如果这个Bean实现了BeanClassLoaderAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。
- 如果这个Bean实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()方法，传递的是Spring工厂自身。
- 如果这个Bean实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文；


（4）BeanPostProcessor前置处理：如果想对Bean进行一些自定义的前置处理，那么可以让Bean实现了BeanPostProcessor接口，那将会调用postProcessBeforeInitialization(Object obj, String s)方法。

（5）InitializingBean：如果Bean实现了InitializingBean接口，执行afeterPropertiesSet()方法。

（6）init-method：如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。

（7）BeanPostProcessor后置处理：如果这个Bean实现了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法；由于这个方法是在Bean初始化结束时调用的，所以可以被应用于内存或缓存技术；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个Bean了。

（8）DisposableBean：当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法；

（9）destroy-method：最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。


**Spring框架中的Bean是线程安全的么？如果线程不安全，那么如何处理**

1. 对于prototype作用域的Bean，每次都创建一个新对象，也就是线程之间不存在Bean共享，因此不会有线程安全问题。
2. 对于singleton作用域的Bean，所有的线程都共享一个单例实例的Bean，因此是存在线程安全问题的。但是如果单例Bean是一个无状态Bean，也就是线程中的操作不会对Bean的成员执行查询以外的操作，那么这个单例Bean是线程安全的。比如Controller类、Service类和Dao等，这些Bean大多是无状态的，只关注于方法本身。

>- 有状态Bean(Stateful Bean) ：就是有实例变量的对象，可以保存数据，是非线程安全的。
>
>- 无状态Bean(Stateless Bean)：就是没有实例变量的对象，不能保存数据，是不变类，是线程安全的。

对于有状态的bean（比如Model和View），就需要自行保证线程安全，最浅显的解决办法就是将有状态的bean的作用域由“singleton”改为“prototype”。

也可以采用ThreadLocal解决线程安全问题，为每个线程提供一个独立的变量副本，不同线程只操作自己线程的副本变量。


