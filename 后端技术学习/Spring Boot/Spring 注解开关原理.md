Spring 注解开关原理
---

# 1. 如果完成自动化配置
1. 定义一个Annotation，让使用了这个Annotaion的应用程序自动化地注入一些类或者做一些底层的事情。
2. 我们使用`@Import`注解配合一个配置类来完成上面的事情。
3. 最简单的注解:`EnableContentService`，使用这个注解的程序会自动注入ContentService这个Bean
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(ContentConfiguration.class)
public @interface EnableContentService {}

public interface ContentService {
    void doSomething();
}

public class SimpleContentService implements ContentService {
    @Override
    public void doSomething() {
        System.out.println("do some simple things");
    }
}
```
4. 之后在应用程序入口使用`@EnableContentService`注解，Spring Boot使用了`ImportSelector`来完成

# 2. ImportSelector的使用
1. 修改`@EnableContentService`注解，添加属性policy，并且import一个Selector

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(ContentImportSelector.class)
public @interface EnableContentService {
    String policy() default "simple";
}
```

2. 这个`ContentImportSelector`根据`EnableContentService`注解里的`policy`加载不同的`bean`：也就是policy如果是core，则会加载`CoreContentService`，否则会加载`SimpleContentService`
```java
public class ContentImportSelector implements ImportSelector {

    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        Class<?> annotationType = EnableContentService.class;
        AnnotationAttributes attributes = AnnotationAttributes.fromMap(importingClassMetadata.getAnnotationAttributes(
                annotationType.getName(), false));
        String policy = attributes.getString("policy");
        if ("core".equals(policy)) {
            return new String[] { CoreContentConfiguration.class.getName() };
        } else {
            return new String[] { SimpleContentConfiguration.class.getName() };
        }
    }

}
```

## 2.1. CoreContentService
```java
public class CoreContentService implements ContentService {
    @Override
    public void doSomething() {
        System.out.println("do some import things");
    }
}
```

## 2.2. CoreContentConfiguration
```java
public class CoreContentConfiguration {
    @Bean
    public ContentService contentService() {
        return new CoreContentService();
    }
}
```

# 3. ImportSelector在Spring Boot中使用
1. SpringBoot里的`ImportSelector`是通过SpringBoot提供的`@EnableAutoConfiguration`这个注解里完成的。
2. 这个`@EnableAutoConfiguration`注解可以显式地调用，否则它会在`@SpringBootApplication`注解中隐式地被调用。
3. `@EnableAutoConfiguration`注解中使用了`EnableAutoConfigurationImportSelector`作为`ImportSelector`。下面这段代码就是`EnableAutoConfigurationImportSelector`中进行选择的具体代码：
```java
@Override
public String[] selectImports(AnnotationMetadata metadata) {
    try {
        AnnotationAttributes attributes = getAttributes(metadata);
        List<String> configurations = getCandidateConfigurations(metadata,
                attributes);
        configurations = removeDuplicates(configurations); // 删除重复的配置
        Set<String> exclusions = getExclusions(metadata, attributes); // 去掉需要exclude的配置
        configurations.removeAll(exclusions);
        configurations = sort(configurations); // 排序
        recordWithConditionEvaluationReport(configurations, exclusions);
        return configurations.toArray(new String[configurations.size()]);
    }
    catch (IOException ex) {
        throw new IllegalStateException(ex);
    }
}
```

4. 其中`getCandidateConfigurations`方法将获取配置类：
```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata,
        AnnotationAttributes attributes) {
    return SpringFactoriesLoader.loadFactoryNames(
            getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader());
}
```
5. `SpringFactoriesLoader.loadFactoryNames`方法会根据`FACTORIES_RESOURCE_LOCATION`这个静态变量从所有的jar包中读取`META-INF/spring.factories`文件信息：
```java
public static List<String> loadFactoryNames(Class<?> factoryClass, ClassLoader classLoader) {
    String factoryClassName = factoryClass.getName();
    try {
        Enumeration<URL> urls = (classLoader != null ? classLoader.getResources(FACTORIES_RESOURCE_LOCATION) :
                ClassLoader.getSystemResources(FACTORIES_RESOURCE_LOCATION));
        List<String> result = new ArrayList<String>();
        while (urls.hasMoreElements()) {
            URL url = urls.nextElement();
            Properties properties = PropertiesLoaderUtils.loadProperties(new UrlResource(url));
            String factoryClassNames = properties.getProperty(factoryClassName); // 只会过滤出key为factoryClassNames的值
            result.addAll(Arrays.asList(StringUtils.commaDelimitedListToStringArray(factoryClassNames)));
        }
        return result;
    }
    catch (IOException ex) {
        throw new IllegalArgumentException("Unable to load [" + factoryClass.getName() +
                "] factories from location [" + FACTORIES_RESOURCE_LOCATION + "]", ex);
    }
}
```
6. `getCandidateConfigurations`方法中的`getSpringFactoriesLoaderFactoryClass`方法返回的是`EnableAutoConfiguration.class`，所以会过滤出key为`org.springframework.boot.autoconfigure.EnableAutoConfiguration`的值。
7. 之后根据文件内容进行配置初始化

# 4. 参考
1. <a href = "http://fangjian0423.github.io/2016/11/13/springboot-enable-annotation/">SpringBoot自动化配置的注解开关原理</a>