ServeletIntialzer的理解
---

# 1. SpringInitializer
1. 先来看代码:
```java
public class ServletInitializer extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(KesheApplication.class);
    }

}
```

## 1.1. 首先了解WebApplicationInitializer
1. 现在JavaConfig配置方式在逐步取代xml配置方式。而WebApplicationInitializer可以看做是Web.xml的替代，它是一个接口。通过实现WebApplicationInitializer，在其中可以添加servlet，listener等，在加载Web项目的时候会加载这个接口实现类，从而起到web.xml相同的作用。下面就看一下这个接口的详细内容。
```java
public interface WebApplicationInitializer {
    void onStartup(ServletContext var1) throws ServletException;
}
```
2. 同时在这个包下面还有另一个类
```java

@HandlesTypes(WebApplicationInitializer.class)
public class SpringServletContainerInitializer implements ServletContainerInitializer {
	@Override
	public void onStartup(@Nullable Set<Class<?>> webAppInitializerClasses, ServletContext servletContext)
			throws ServletException {
		List<WebApplicationInitializer> initializers = new LinkedList<>();
		if (webAppInitializerClasses != null) {
			for (Class<?> waiClass : webAppInitializerClasses) {
				// Be defensive: Some servlet containers provide us with invalid classes,
				// no matter what @HandlesTypes says...
                // 反射
				if (!waiClass.isInterface() && !Modifier.isAbstract(waiClass.getModifiers()) &&
						WebApplicationInitializer.class.isAssignableFrom(waiClass)) {
					try {
						initializers.add((WebApplicationInitializer)
								ReflectionUtils.accessibleConstructor(waiClass).newInstance());
					}
					catch (Throwable ex) {
						throw new ServletException("Failed to instantiate WebApplicationInitializer class", ex);
					}
				}
			}
		}
		if (initializers.isEmpty()) {
			servletContext.log("No Spring WebApplicationInitializer types detected on classpath");
			return;
		}
		servletContext.log(initializers.size() + " Spring WebApplicationInitializers detected on classpath");
		AnnotationAwareOrderComparator.sort(initializers);
		for (WebApplicationInitializer initializer : initializers) {
			initializer.onStartup(servletContext);
		}
	}
}
```
3. 简单理解一下:先判断webAppInitializerClasses这个Set是否为空。如果不为空的话，找到这个set中不是接口，不是抽象类，并且是WebApplicationInitializer接口实现类的类，将它们保存到list中。当这个list为空的时候，抛出异常。不为空的话就按照一定的顺序排序，并将它们按照一定的顺序实例化。调用其onStartup方法执行。到这里，就可以解释WebApplicationInitializer实现类的工作过程了。
4. 但是，在web项目运行的时候SpringServletContainerInitializer这个类又是怎样被调用的呢。
```java

package javax.servlet;
import java.util.Set;

public interface ServletContainerInitializer {
    void onStartup(Set<Class<?>> var1, ServletContext var2) throws ServletException;
}
```
5. 这个接口是javax.servlet下的
    + 官方解释：为了支持和不使用web.xml。提供了ServletContainerInitializer。通过SPI机制，在启动web容器时，自动到jar包下找到META-INF/services下以ServletContainerInitializer的全路径名称命名的文件，它的内容为ServletContainerInitializer实现类的全路径，将它们实例化。既然这样的话，那么SpringServletContainerInitializer作为ServletContainerInitializer的实现类，它的jar包下也应该有相应的文件。

## 1.2. 自己实例化相同的功能
1. 定义接口WebParameter，它相当于WebApplicarionInitializer.
```java
public interface WebParameter {
    void loadOnstarp(ServletContext servletContext);
}
```
2. 定义Servlet:
```java
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("TestSetvlet");
    }
}
```
3. 定义MyWebParameter作为WebParameter的实现类，将Servlet添加到上下文，并设置好映射。
```java
public class MyWebParameter implements WebParameter {
    public void loadOnstarp(ServletContext servletContet) {
        ServletRegistration.Dynamic testSetvelt=servletContext.addServlet("test","com.test.servlet.MyServlet");
        testSetvelt.setLoadOnStartup(1);
        testSetvelt.addMapping("/test");
    }
}
```
4. 
```java
@HandlesTypes({WebParameter.class})
public class WebConfig implements ServletContainerInitializer {
    public void onStartup(Set<Class<?>> set, ServletContext servletContext) throws ServletException {
        Iterator var4;
        if (set!=null){
            var4=set.iterator();
            while(var4.hasNext()){
                Class<?> clazz= (Class<?>) var4.next();
                if (!clazz.isInterface()&& !Modifier.isAbstract(clazz.getModifiers())&&WebParameter.class.isAssignableFrom(clazz)){
                    try {
                        ((WebParameter) clazz.newInstance()).loadOnstarp(servletContext);
                    }catch (Exception e){
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```
5. 根据SPI机制，定义一个META-INF/services文件夹，并在其下定义相关文件名称，并将WebConfig的类全名称填入其中。
6. 之后使用maven将这个包导入进去即可

# 2. @HandlesTypes
1. 简单来说，这个注解表示:`customServletContainerInitializer`可以处理的类，在onStartup方法中通过`set<Clas<?>> c`获取到。
2. Spring框架在3.1中已经实现了这个接口，实现类为:`SprinbgServletContainerInitializer`，加载的接口为`WebApplicationInitializer`

# 3. 对SpringBootServletInitializer的理解

## 3.1. 不同的容器
1. 使用嵌入式的Servlet容器：
    1. 优点: 简单,便携
    2. 缺点: 默认不支持jsp,优化定制比较复杂
2. 使用外置Servlet容器的步骤:
    1. 必须创建war项目,需要建好web项目的目录结构
    2. 嵌入式Tomcat依赖scope指定provided
    3. 编写SpringBootServletInitializer类子类,并重写configure方法
    4. 启动服务器
```java
public class ServletInitializer extends SpringBootServletInitializer {  
    @Override  
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {  
        return application.sources(SpringBoot04WebJspApplication.class);  
    }  
} 
```

## 3.2. servlet 3.0+ 规则
1. 服务器启动(web应用启动),会创建当前web应用里面所有jar包里面的ServletContainerlnitializer实例
2. ServletContainerInitializer的实现放在jar包的META-INF/services文件夹下
3. 还可以使用@HandlesTypes注解，在应用启动的时候加载指定的类。

## 3.3. 外部Tomcat启动的流程与原理
1. 启动Tomcat
2. 根据上述描述的Servlet3.0+规则，可以在Spring的web模块里面找到有个文件名为javax.servlet.ServletContainerInitializer的文件，而文件的内容为org.springframework.web.SpringServletContainerInitializer，用于加载SpringServletContainerInitializer类
3. 看看SpringServletContainerInitializer定义
```java
@HandlesTypes(WebApplicationInitializer.class)
public class SpringServletContainerInitializer implements ServletContainerInitializer {

    @Override
    public void onStartup(Set<Class<?>> webAppInitializerClasses, ServletContext servletContext)
            throws ServletException {
        List<WebApplicationInitializer> initializers = new LinkedList<WebApplicationInitializer>();
        if (webAppInitializerClasses != null) {
            for (Class<?> waiClass : webAppInitializerClasses) {
                // Be defensive: Some servlet containers provide us with invalid classes,
                // no matter what @HandlesTypes says...
                if (!waiClass.isInterface() && !Modifier.isAbstract(waiClass.getModifiers()) &&
                        WebApplicationInitializer.class.isAssignableFrom(waiClass)) {
                    try {
　　　　　　　　          //为所有的WebApplicationInitializer类型创建实例,并加入集合中
                        initializers.add((WebApplicationInitializer) waiClass.newInstance());
                    }
                    catch (Throwable ex) {
                        throw new ServletException("Failed to instantiate WebApplicationInitializer class", ex);
                    }
                }
            }
        }
        if (initializers.isEmpty()) {
            servletContext.log("No Spring WebApplicationInitializer types detected on classpath");
            return;
        }
        servletContext.log(initializers.size() + " Spring WebApplicationInitializers detected on classpath");
        AnnotationAwareOrderComparator.sort(initializers);
　　　  //调用每一个WebApplicationInitializer实例的onstartup方法
        for (WebApplicationInitializer initializer : initializers) {
            initializer.onStartup(servletContext);
        }
    }
}
```

# 4. 深入了解SpringBootServletInitializer
```java
public class ServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(DemoWarApplication.class);
    }
}
```
1. SpringBootServletInitializer就是一个org.springframework.web.context.WebApplicationContext，容器启动时会调用其onStartup(ServletContext servletContext)方法，接下来我们就来看一下这个方法:
```java
public void onStartup(ServletContext servletContext) throws ServletException {
        this.logger = LogFactory.getLog(this.getClass());
        final WebApplicationContext rootAppContext = this.createRootApplicationContext(servletContext);
        if(rootAppContext != null) {
            servletContext.addListener(new ContextLoaderListener(rootAppContext) {
                public void contextInitialized(ServletContextEvent event) {
                }
            });
        } else {
            this.logger.debug("No ContextLoaderListener registered, as createRootApplicationContext() did not return an application context");
        }
    }
```
2. createRootApplicationContext(servletContext)
```java
protected WebApplicationContext createRootApplicationContext(ServletContext servletContext) {

        //创建SpringApplicationBuilder，并用其生产出SpringApplication对象

        SpringApplicationBuilder builder = this.createSpringApplicationBuilder();

        builder.main(this.getClass());

        ApplicationContext parent = this.getExistingRootWebApplicationContext(servletContext);
        if(parent != null) {
            this.logger.info("Root context already created (using as parent).");
            servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, (Object)null);
            builder.initializers(new ApplicationContextInitializer[]{new ParentContextApplicationContextInitializer(parent)});
        }

        //初始化并封装SpringApplicationBuilder对象，为SpringApplication对象增加ApplicationContextInitializer和ApplicationListener做准备
        builder.initializers(new ApplicationContextInitializer[]{new ServletContextApplicationContextInitializer(servletContext)});
        builder.listeners(new ApplicationListener[]{new ServletContextApplicationListener(servletContext)});
        //指定创建的ApplicationContext类型
        builder.contextClass(AnnotationConfigEmbeddedWebApplicationContext.class);
 
        //传递入口类，并构建SpringApplication对象
        //可以通过configure()方法对SpringBootServletInitializer进行扩展
        builder = this.configure(builder);
        SpringApplication application = builder.build();
        if(application.getSources().isEmpty() && AnnotationUtils.findAnnotation(this.getClass(), Configuration.class) != null) {
            application.getSources().add(this.getClass());
        }
        Assert.state(!application.getSources().isEmpty(), "No SpringApplication sources have been defined. Either override the configure method or add an @Configuration annotation");
        if(this.registerErrorPageFilter) {
            application.getSources().add(ErrorPageFilter.class);
        }
        //最后调用SpringApplication的run方法
        return this.run(application);
    }
```
3. 总体上来讲，这个过程就是通过SpringApplicationBuilder来构建一个SpringApplication对象，并且调用相应的run方法来启动。

# 5. 扩展SpringBootServletInitializer
```java
public class ServletInitializer extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        application.initializers(MyApplicationContextInitializer1,MyApplicationContextInitializer2);
        application.listeners(MyApplicationListener1,MyApplicationListener2)
        return application.sources(DemoWarApplication.class);
    }
}
```
1. 与扩展SpringApplication类似，ApplicationContextInitializer和ApplicationListener可以基于SpringApplicationBuilder提供的public方法进行扩展

# 6. 参考
1. <a href = "https://blog.csdn.net/qq_28289405/article/details/81279742">Spring boot中的 ServletInitializer</a>