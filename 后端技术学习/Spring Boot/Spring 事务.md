Spring 事务
---
1. 数据库事务是企业应用中最为重要的内容之一。
2. 事务往往会涉及到注释@Transactional

<!-- TOC -->

- [1. Spring的数据管理器的设计](#1-spring的数据管理器的设计)
	- [1.1. 数据管理器分类](#11-数据管理器分类)
- [2. 通过标签来完成Spring的事务回滚](#2-通过标签来完成spring的事务回滚)
	- [2.1. 根据异常进行回滚](#21-根据异常进行回滚)
	- [2.2. 回滚部分异常](#22-回滚部分异常)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. Spring的数据管理器的设计
1. 在Spring中数据库事务是通过`PlatformTransactionManager`进行管理的，而能够支持事务的是`org.springframework.transaction.support.TransactionTemplate`，它是Spring所提供的事务管理器的模板。

```java
//事务管理器
private PlatformTransactionManager transactionManager;
......
@Override
public <T> T execute(TransactionCallback<T> action) throws TransactionException{
	// 使用自定义的事务管理器
	if(this.transactionManager instanceof CallbackPreferringPlatformTransactionManager){
		return ((CallbackPreferringPlatformTransactionManager)this.transactionManager).execute(this,action);
	}else{//系统默认管理器
		//获取事务状态
		TransactionStatus status = this.transactionManager.getTransaction(this);
		T result;
		try{
			//回调接口方法
			result = action.doInTransaction(status);
		}catch(RuntimeException ex){
			//Transactional code threw application exception -> rollback
			//回滚异常方法
			rollbackOnException(status, ex);
			//抛出异常
			throw ex;
		}catch(Error ex){
			//Transactional code threw error -> rollback
			//回滚异常方法
			rollbackOnException(status, err);
			//抛出错误
			throw err;
		}catch(Throwable ex){
			//Transactional code threw unexpected exception -> rollback
			//回滚异常方法
			rollbackOnException(status, ex);
			//抛出无法捕获异常
			throw new UndeclaredThrowableException(ex, "TransactionCallback threw undeclared checked exception");
		}
		//提交事务
		this.transactionManager.commit(status);
		//返回结果
		return result;
	}
}
```

1. 事务的创建、提交和回滚是通过`PlatformTransactionManager`接口来完成。
2. 当事务产生异常时会回滚事务，在默认的实现中所有的异常都会回滚，可以通过配置去修改在某些异常发生时回滚或者不会滚事务。
3. 当无异常时，会提交事务。
4. 多种事务管理器如下

## 1.1. 数据管理器分类
![](img/transaction/1.png)

1. 可以看到多个数据库管理器，并且支持JTA事务，常用的是`DataSourceTransactionManager`，集成抽象事务管理器`AbstractPlatformTransactionManager`，而`AbstractPlatformTransactionManager`又实现了`PlatformTransactionManager`。这样Spring就可以如同源码中那样使用`PlatformTransactionManager`接口的方法，创建、提交或者回滚事务了。
2. `PlatformTransactionManager`接口的源码如下：

```java
public interface PlatformTransactionManager{
    //获取事务状态
    TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;
    // 提交事务
    void commit(TransactionStatus status) throws TransactionException;
    //回滚事务
    void rollback(TransactionStatus status) throws TransactionException;
}
```

# 2. 通过标签来完成Spring的事务回滚
1. 将所有的对于数据库的操作都放置到一个被@Transactional标记的Service方法中可以全部回滚
```java
@Transactional
public boolean UserRegister(ESysUser entity) {
	try {
		//插入ESysUser
		eSysUserDao.insert(entity);
	} catch (Exception e) {
		// handle exception 启动回滚
		TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
		log.error(e.getMessage(), e);
		return false;
	}
}
```
## 2.1. 根据异常进行回滚
>`@Transactional(rollbackFor = { Exception.class})`
1. 没有捕获异常，则会抛出异常，则将doDbStuff1()中的数据库操作进行回滚。

```java
@Transactional(rollbackFor = { Exception.class })    
public void test() throws Exception {    
     doDbStuff1();    
     doDbStuff2();//假如这个操作数据库的方法会抛出异常，方法doDbStuff1()对数据库的操作会回滚。    
}  
```

2. 如果在程序中自已捕获异常未往外抛，如下代码事务不会回滚
```java
@Transactional(rollbackFor = { Exception.class })    
public void test() {    
     try {    
        doDbStuff1();    
        doDbStuff2();//假如这个操作数据库的方法会抛出异常，现在方法doDbStuff1()对数据库的操作  不会回滚。    
     } catch (Exception e) {    
           e.printStackTrace();       
     }    
}
```

3. 既要捕获异常，又要回滚
```java
@Transactional(rollbackFor = { Exception.class })    
public void test() {    
	try {    
		doDbStuff1();
		doDbStuff2();
	} catch (Exception e) {
		e.printStackTrace();
		TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();//就是这一句了，加上之后，如果doDbStuff2()抛了异常,
		return;  
  }    
}
```

## 2.2. 回滚部分异常
1. 使用`Object savePoint = TransactionAspectSupport.currentTransactionStatus().createSavepoint();`设置回滚点。
2. 使用`TransactionAspectSupport.currentTransactionStatus().rollbackToSavepoint(savePoint);`回滚到savePoint。
```java
@Transactional(rollbackFor = Exception.class)
@Override
public void saveEntity() throws Exception{
    Object savePoint = null;
    try {
        userDao.saveUser();
        //设置回滚点
        savePoint = TransactionAspectSupport.currentTransactionStatus().createSavepoint();
        studentDao.saveStudent(); //执行成功
        int a = 10/0; //这里因为除数0会报异常,进入catch块
    }catch (Exception e){
        System.out.println("异常了=====" + e);
        //手工回滚异常
        TransactionAspectSupport.currentTransactionStatus().rollbackToSavepoint(savePoint);
    }
}
```

# 3. 参考
1. <a href = "https://blog.csdn.net/ARPOSPF/article/details/105321290?depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-2&utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-2">【Java EE】深入Spring数据库事务管理</a>
2. <a href = "https://blog.csdn.net/qq_41401062/article/details/102662906">spring事务回滚：当同时需要在多个数据表插入数据时，一个出错如何实现全部回滚</a>
3. <a href = "https://blog.csdn.net/qq_31997407/article/details/77835851">Spring中抛出异常时，既要要返回错误信息，还要做事务回滚</a>
4. <a href = "https://www.cnblogs.com/myitnews/p/12364455.html">SpringBoot事务简单操作及手动回滚</a>