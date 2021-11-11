# IOC 体系

## Resource 体系


**Resource 的实现原因**
> 在学 Java SE 的时候，我们学习了一个标准类 `java.net.URL`，该类在 Java SE 中的定位为统一资源定位器（Uniform Resource Locator），
> 但是我们知道它的实现基本只限于网络形式发布的资源的查找和定位。然而，实际上资源的定义比较广泛，除了网络形式的资源，
> 还有以二进制形式存在的、以文件形式存在的、以字节流形式存在的等等。而且它可以存在于任何场所，比如网络、文件系统、应用程序中。
> 所以 `java.net.URL` 的局限性迫使 Spring 必须实现自己的资源加载策略，该资源加载策略需要满足如下要求：

- 职能划分清楚。资源的定义和资源的加载应该要有一个清晰的界限；
- 统一的抽象。统一的资源定义和资源加载策略。资源加载后要返回统一的抽象给客户端，客户端要对资源进行怎样的处理，应该由抽象资源接口来界定。

> `org.springframework.core.io.Resource`，对资源的抽象。它的每一个实现类都代表了一种资源的访问策略，
> 如 ClassPathResource、RLResource、FileSystemResource 等

### 统一资源：Resource

> `org.springframework.core.io.Resource` 为 Spring 框架所有资源的抽象和访问接口，
> 它继承 `org.springframework.core.io.InputStreamSource`接口。作为所有资源的统一抽象，Resource 定义了一些通用的方法，
> 由子类 `AbstractResource` 提供统一的默认实现。定义如下：

**Resource 根据资源的不同类型提供不同的具体实**

- FileSystemResource ：对 java.io.File 类型资源的封装，只要是跟 File 打交道的，基本上与 FileSystemResource 也可以打交道。支持文件和 URL 的形式，实现 WritableResource 接口，且从 Spring Framework 5.0 开始，FileSystemResource 使用 NIO2 API进行读/写交互。
- ByteArrayResource ：对字节数组提供的数据的封装。如果通过 InputStream 形式访问该类型的资源，该实现会根据字节数组的数据构造一个相应的 ByteArrayInputStream。
- UrlResource ：对 java.net.URL类型资源的封装。内部委派 URL 进行具体的资源操作。
- ClassPathResource ：class path 类型资源的实现。使用给定的 ClassLoader 或者给定的 Class 来加载资源。
- InputStreamResource ：将给定的 InputStream 作为一种资源的 Resource 的实现类。

### ResourceLoader 体系

[参考](http://svip.iocoder.cn/Spring/IoC-load-Resource/)

> 有了资源，就应该有资源加载，Spring 利用 `org.springframework.core.io.ResourceLoader` 来进行统一资源加载

> Spring 将资源的定义和资源的加载区分开了，Resource 定义了统一的资源，那资源的加载则由 ResourceLoader 来统一定义。

> `org.springframework.core.io.ResourceLoader` 为 Spring 资源加载的统一抽象，
> 具体的资源加载则由相应的实现类来完成，所以我们可以将 ResourceLoader 称作为统一资源定位器
> ResourceLoader，定义资源加载器，主要应用于根据给定的资源文件地址，返回对应的 Resource


## BeanFactory 体系

> org.springframework.beans.factory.BeanFactory，是一个非常纯粹的 bean 容器，
> 它是 IoC 必备的数据结构，其中 BeanDefinition 是它的基本结构。
> BeanFactory 内部维护着一个BeanDefinition map ，并可根据 BeanDefinition 的描述进行 bean 的创建和管理
> BeanFactory 有三个直接子类 ListableBeanFactory、HierarchicalBeanFactory 和 AutowireCapableBeanFactory 。
> DefaultListableBeanFactory 为最终默认实现，它实现了所有接口

## BeanDefinition 体系

> `org.springframework.beans.factory.config.BeanDefinition` ，用来描述 Spring 中的 Bean 对象

## BeanDefinitionReader 体系

> `org.springframework.beans.factory.support.BeanDefinitionReader` 的作用是读取 Spring 的配置文件的内容，
> 并将其转换成 Ioc 容器内部的数据结构 ：BeanDefinition

## ApplicationContext 体系

> `org.springframework.context.ApplicationContext` ，这个就是大名鼎鼎的 Spring 容器，
> 它叫做应用上下文，与我们应用息息相关。它继承 BeanFactory ，所以它是 BeanFactory 的扩展升级版，
>  ApplicationContext 的结构就决定了它与 BeanFactory 的不同，其主要区别有：

- 继承 org.springframework.context.MessageSource 接口，提供国际化的标准访问策略。
- 继承 org.springframework.context.ApplicationEventPublisher 接口，提供强大的事件机制。
- 扩展 ResourceLoader ，可以用来加载多种 Resource ，可以灵活访问不同的资源。
- 对 Web 应用的支持。