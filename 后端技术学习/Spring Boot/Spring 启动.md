Spring boot 项目启动
---

<!-- TOC -->

- [1. 全过程](#1-全过程)
- [2. 参考](#2-参考)

<!-- /TOC -->

# 1. 全过程
>`SpringApplication`的`run`方法的实现是我们启动SpringApplication的入口，该方法的主要流程大体可以归纳如下：
1. 如果我们使用的是`SpringApplication`的静态run方法，那么，这个方法里面首先要创建一个`SpringApplication`对象实例，然后调用这个创建好的`SpringApplication`的实例方法。在`SpringApplication`实例初始化的时候，它会提前做几件事情： 
   1. 根据`classpath`里面是否存在某个特征类（`org.springframework.web.context.ConfigurableWebApplicationContext`）来决定是否应该创建一个为Web应用使用的`ApplicationContext`类型。
   2. 使用`SpringFactoriesLoader`在应用的classpath中查找并加载所有可用的`ApplicationContextInitializer`。
   3. 使用`SpringFactoriesLoader`在应用的classpath中查找并加载所有可用的`ApplicationListener`。
   4. 推断并设置main方法的定义类。
2. `SpringApplication`实例初始化完成并且完成设置后，就开始执行run方法的逻辑了，方法执行伊始，首先遍历执行所有通过`SpringFactoriesLoader`可以查找到并加载的`SpringApplicationRunListener`。调用它们的`started()`方法，告诉这些`SpringApplicationRunListener`，即将开始执行。
3. 创建并配置当前Spring Boot应用将要使用的`Environment`（包括配置要使用的`PropertySource`以及`Profile`）。
4. 遍历调用所有`SpringApplicationRunListener`的`environmentPrepared()`的方法，通知他们已经准备好了。
5. 如果`SpringApplication`的`showBanner`属性被设置为true，则打印banner。
6. 根据用户是否明确设置了`applicationContextClass`类型以及初始化阶段的推断结果，决定该为当前SpringBoot应用创建什么类型的`ApplicationContext`并创建完成，然后根据条件决定是否添加`ShutdownHook`，决定是否使用自定义的`BeanNameGenerator`，决定是否使用自定义的`ResourceLoader`，当然，最重要的，将之前准备好的Environment设置给创建好的`ApplicationContext`使用。
7. `ApplicationContext`创建好之后，`SpringApplication`会再次借助`Spring-FactoriesLoader`，查找并加载`classpath`中所有可用的`ApplicationContext-Initializer`，然后遍历调用这些`ApplicationContextInitializer`的`initialize（applicationContext）`方法来对已经创建好的`ApplicationContext`进行进一步的处理。
8. 遍历调用所有`SpringApplicationRunListener`的`contextPrepared()`方法。
9. 最核心的一步，将之前通过`@EnableAutoConfiguration`获取的所有配置以及其他形式的IoC容器配置**加载**到已经准备完毕的`ApplicationContext`。
10. 遍历调用所有`SpringApplicationRunListener`的`contextLoaded()`方法。
11. 调用`ApplicationContext`的`refresh()`方法，完成IoC容器可用的最后一道工序。
12. 查找当前`ApplicationContext`中是否注册有`CommandLineRunner`，如果有，则遍历执行它们。
13. 正常情况下，遍历执行`SpringApplicationRunListener的finished()`方法、（如果整个过程出现异常，则依然调用所有`SpringApplicationRunListener`的`finished()`方法，只不过这种情况下会将异常信息一并传入处理）

# 2. 参考
1. <a href = "https://pppppkun.github.io/2019/10/25/SpringBoot入门之启动原理解析/"></a>