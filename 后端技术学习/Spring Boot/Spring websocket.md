java websocket
---
1. 对于互联网中的直播以及录播等方式，我们经常使用websocket来保证持久化的连接。Websocket是从HTML 5 开始提供的一种在单个TCP连接上进行全双工通讯的协议。
2. 本文主要记录了部分的关于websocket配置和使用中出现的问题。

<!-- TOC -->

- [1. 配置Websocket](#1-配置websocket)
- [2. 通过HTTP请求WebSocket的原理](#2-通过http请求websocket的原理)
- [3. websocket技术上的实践](#3-websocket技术上的实践)
	- [3.1. @ServerEndpoint创建WebSocket端点](#31-serverendpoint创建websocket端点)
	- [3.2. @ServerEndpoint 说明路径解析的方式](#32-serverendpoint-说明路径解析的方式)
	- [3.3. 简单实例](#33-简单实例)
	- [3.4. 其他注解](#34-其他注解)
	- [3.5. 将spring boot框架和websocket进行合并](#35-将spring-boot框架和websocket进行合并)
	- [3.6. springboot后台实现](#36-springboot后台实现)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. 配置Websocket
1. 使用pom.xml来完成Websocket的配置。
```xml
<dependency>
    <groupId>javax.websocket</groupId>
    <artifactId>javax.websocket-api</artifactId>
    <version>1.1</version>
    <scope>provided</scope>
</dependency>
```
2. 可以通过下载jar文件，然后我们配置到IDEA中去。

# 2. 通过HTTP请求WebSocket的原理
1. 原理:客户端B通过websocket向服务器端S发起连接，服务器保存这个websocket连接。当服务器A向服务器发起请求时，服务器S接收HTTP请求并通过websocket向客户端B发起请求。

# 3. websocket技术上的实践

## 3.1. @ServerEndpoint创建WebSocket端点
1. 如果在使用内嵌容器的Spring Boot应用中使用@ServerEndpoint，那么我们需要单独的声明一个Bean容器来完成注入。
```java
@Bean
public ServerEndpointExporter serverEndpointExporter() {
    return new ServerEndpointExporter();
}
```
2. 而部署到一个单独的Servlet容器时，该角色将一个servlet容器初始化方法执行，此时Bean也就不需要的。

## 3.2. @ServerEndpoint 说明路径解析的方式
1. @ServerEndpoint注解是一个类层次的注解，它的功能主要是将目前的类定义成一个websocket服务器端,注解的值将被用于监听用户连接的终端访问URL地址,客户端可以通过这个URL来连接到WebSocket服务器端
2. 同时其括号内的路径即为发起路径的方式

## 3.3. 简单实例
```java
import java.io.IOException;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;
/**
* @ServerEndpoint 注解是一个类层次的注解，它的功能主要是将目前的类定义成一个websocket服务器端,注解的值将被用于监听用户连接的终端访问URL地址,客户端可以通过这个URL来连接到WebSocket服务器端
*/
@ServerEndpoint("/WebSocketTest")
public class WebSocketTest {
	private Session session;
	@OnOpen//打开连接执行
	public void onOpw(Session session) {
		this.session=session;
		System.out.println("打开了连接");
	}
	@OnMessage//收到消息执行,框架的核心方法
	public void onMessage(String message,Session session) {
		System.out.println(message);
	    try {
		    sendMessage(message);
	    } catch (IOException e) {
		    // TODO Auto-generated catch block
		    e.printStackTrace();
	    }	
	}
	@OnClose//关闭连接执行
	public void onClose(Session session) {
		System.out.println("关闭连接");
	}
	@OnError//连接错误的时候执行
	public void onError(Throwable error,Session session) {
		System.out.println("错误的时候执行");
		error.printStackTrace();
	}
    /*
    websocket  session发送文本消息有两个方法：getAsyncRemote()和
    getBasicRemote()  getAsyncRemote()和getBasicRemote()是异步与同步的区别，
    大部分情况下，推荐使用getAsyncRemote()。
    */
	public void sendMessage(String message) throws IOException{
	    this.session.getAsyncRemote().sendText(message);
    }
}
```

## 3.4. 其他注解
1. 对于websocket，tomcat对于websocket的高并发的支持并不是很好，所以我们需要在类中设置对应的最大的对话量，与此同时我们也应该尝试使用netty等框架用来支撑实际的框架模型。
2. 以下为一个简单的使用tomcat实现的，同时我们也应当了解到关于map的多线程同步安全性。
```java
@ServerEndpoint("/acgist.video/{uid}")
public class AcgistVideo {
	// 最大通话数量
	private static final int MAX_COUNT = 20;
	private static final long MAX_TIME_OUT = 2 * 60 * 1000;
	// 用户和用户的对话映射
	private static final Map<String, String> user_user = Collections.synchronizedMap(new HashMap<String, String>()); 
	// 用户和websocket的session映射
	private static final Map<String, Session> sessions = Collections.synchronizedMap(new HashMap<String, Session>());
	
	/**
	 * 打开websocket
	 * @param session websocket的session
	 * @param uid 打开用户的UID
	 */
	@OnOpen
	public void onOpen(Session session, @PathParam("uid")String uid) {
		session.setMaxIdleTimeout(MAX_TIME_OUT);
		sessions.put(uid, session);
	}

	/**
	 * websocket关闭
	 * @param session 关闭的session
	 * @param uid 关闭的用户标识
	 */
	@OnClose
	public void onClose(Session session, @PathParam("uid")String uid) {
		remove(session, uid);
	}

	/**
	 * 收到消息
	 * @param message 消息内容
	 * @param session 发送消息的session
	 * @param uid
	 */
	@OnMessage
	public void onMessage(String message, Session session, @PathParam("uid")String uid) {
		try {
			if(uid != null && user_user.get(uid) != null && AcgistVideo.sessions.get(user_user.get(uid)) != null) {
				Session osession = sessions.get(user_user.get(uid)); // 被呼叫的session
				if(osession.isOpen())
					osession.getAsyncRemote().sendText(new String(message.getBytes("utf-8")));
				else
					this.nowaiting(osession);
			} else {
				this.nowaiting(session);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 没有人在等待
	 * @param session 发送消息的session
	 */
	private void nowaiting(Session session) {
		session.getAsyncRemote().sendText("{\"type\" : \"nowaiting\"}");
	}
	
	/**
	 * 是否可以继续创建通话房间
	 * @return 可以：true；不可以false；
	 */
	public static boolean canCreate() {
		return sessions.size() <= MAX_COUNT;
	}
	
	/**
	 * 判断是否可以加入
	 * @param oid 被申请对话的ID
	 * @return 如果能加入返回：true；否则返回false；
	 */
	public static boolean canJoin(String oid) {
		return !(oid != null && user_user.containsKey(oid) && user_user.get(oid) != null);
	}
	
	/**
	 * 添加视频对象
	 * @param uid 申请对话的ID
	 * @param oid 被申请对话的ID
	 * @return 是否是创建者：如果没有申请对话ID为创建者，否则为为加入者。创建者返回：true；加入者返回：false；
	 */
	public static boolean addUser(String uid, String oid) {
		if(oid != null && !oid.isEmpty()) {
			AcgistVideo.user_user.put(uid, oid);
			AcgistVideo.user_user.put(oid, uid);
			
			return false;
		} else {
			AcgistVideo.user_user.put(uid, null);
			
			return true;
		}
	}
	
	/**
	 * 移除聊天用户
	 * @param session 移除的session
	 * @param uid 移除的UID
	 */
	private static void remove(Session session, String uid) {
		String oid = user_user.get(uid);
		
		if(oid != null) user_user.put(oid, null); // 设置对方无人聊天
		
		sessions.remove(uid); // 异常session
		user_user.remove(uid); // 移除自己
		
		try {
			if(session != null && session.isOpen()) session.close(); // 关闭session
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```

## 3.5. 将spring boot框架和websocket进行合并
1. 步骤一:springboot底层帮我们自动配置了websocket，引入maven依赖。
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```
2. 步骤二:如果你采用springboot内置容器启动项目的，则配置一个Bean。如果是采用外部的容器，则可以不需要配置。
```java
@Component
public class WebSocketConfig {
    /**
     * ServerEndpointExporter 作用
     *
     * 这个Bean会自动注册使用@ServerEndpoint注解声明的websocket endpoint
     *
     * @return
     */
    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }
}
```
3. 
步骤三:编写服务器端核心代码，保证连接的建立和正常使用。
```java
@Component
@ServerEndpoint(value = "/websocket/{ip}")
public class MyWebSocket {
 
	private static final Logger log = LoggerFactory.getLogger(MyWebSocket.class);
 
	// 静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
	private static int onlineCount = 0;
 
	// concurrent包的线程安全Map，用来存放每个客户端对应的MyWebSocket对象。
	private static ConcurrentHashMap<String, MyWebSocket> webSocketMap = new ConcurrentHashMap<String, MyWebSocket>();
 
	// 与某个客户端的连接会话，需要通过它来给客户端发送数据
	private Session session;
 
	private String ip; // 客户端ip
	public static final String ACTION_PRINT_ORDER = "printOrder";
	public static final String ACTION_SHOW_PRINT_EXPORT = "showPrintExport";
 
	/**
	 * 连接建立成功调用的方法
	 */
	@OnOpen
	public void onOpen(Session session, @PathParam("ip") String ip) {
		this.session = session;
		this.ip = ip;
		webSocketMap.put(ip, this);
		addOnlineCount(); // 在线数加1
//		System.out.println("有新连接加入！当前在线人数为" + getOnlineCount());
		log.info("有新连接加入,ip:{}！当前在线人数为:{}", ip, getOnlineCount());
		ExportService es = BeanUtils.getBean(ExportService.class);
		List<String> list = es.listExportCodesByPrintIp(ip);
		ResponseData<String> rd = new ResponseData<String>();
		rd.setAction(MyWebSocket.ACTION_SHOW_PRINT_EXPORT);
		rd.setList(list);
		sendObject(rd);
	}
 
	/**
	 * 连接关闭调用的方法
	 */
	@OnClose
	public void onClose(@PathParam("ip") String ip) {
		webSocketMap.remove(ip); // 从set中删除
		// Map<String, String> map = session.getPathParameters();
		// webSocketMap.remove(map.get("ip"));
		subOnlineCount(); // 在线数减1
		// System.out.println("有一连接关闭！当前在线人数为" + getOnlineCount());
		log.info("websocket关闭，IP：{},当前在线人数为:{}", ip, getOnlineCount());
	}
 
	/**
	 * 收到客户端消息后调用的方法
	 *
	 * @param message
	 *            客户端发送过来的消息
	 */
	@OnMessage
	public void onMessage(String message, Session session) {
//		System.out.println("来自客户端的消息:" + message);
		log.debug("websocket来自客户端的消息:{}", message);
		OrderService os = BeanUtils.getBean(OrderService.class);
		OrderVo ov = os.getOrderDetailByOrderNo(message);
//		System.out.println(ov);
		ResponseData<OrderVo> rd = new ResponseData<OrderVo>();
		ArrayList<OrderVo> list = new ArrayList<OrderVo>();
		list.add(ov);
		rd.setAction(MyWebSocket.ACTION_PRINT_ORDER);
		rd.setList(list);
		sendObject(rd);
//		log.info("推送打印信息完成，单号：{}", ov.getOrderNo());
	}
 
	/**
	 * 发生错误时调用
	 */
	@OnError
	public void onError(Session session, Throwable error) {
//		System.out.println("发生错误");
		log.error("webSocket发生错误！IP：{}", ip);
		error.printStackTrace();
	}
 
	/**
	 * 像当前客户端发送消息
	 * 
	 * @param message
	 *            字符串消息
	 * @throws IOException
	 */
	public void sendMessage(String message) {
		try {
			this.session.getBasicRemote().sendText(message);
			// this.session.getAsyncRemote().sendText(message);
		} catch (IOException e) {
			e.printStackTrace();
			log.error("发送数据错误，ip:{},msg：{}", ip, message);
		}
	}
 
	/**
	 * 向当前客户端发送对象
	 * 
	 * @param obj
	 *            所发送对象
	 * @throws IOException
	 */
	public void sendObject(Object obj) {
		ObjectMapper mapper = new ObjectMapper();
		mapper.setSerializationInclusion(Include.NON_NULL);
		String s = null;
		try {
			s = mapper.writeValueAsString(obj);
		} catch (JsonProcessingException e) {
			e.printStackTrace();
			log.error("转json错误！{}", obj);
		}
		this.sendMessage(s);
	}
 
	/**
	 * 群发自定义消息
	 */
	public static void sendInfo(String message) {
		for (Entry<String, MyWebSocket> entry : webSocketMap.entrySet()) {
			MyWebSocket value = entry.getValue();
			value.sendMessage(message);
		}
	}
 
	public static synchronized int getOnlineCount() {
		return onlineCount;
	}
 
	public static synchronized void addOnlineCount() {
		MyWebSocket.onlineCount++;
	}
 
	public static synchronized void subOnlineCount() {
		MyWebSocket.onlineCount--;
	}
 
	public static ConcurrentHashMap<String, MyWebSocket> getWebSocketMap() {
		return webSocketMap;
	}
 
}
```

## 3.6. springboot后台实现
1. 导入后台连接websocket的客户端依赖
```xml
<!--websocket作为客户端-->
<dependency>
    <groupId>org.java-websocket</groupId>
    <artifactId>Java-WebSocket</artifactId>
    <version>1.3.5</version>
</dependency>
```
2. 把客户端需要配置到springboot容器中去，以方便程序调用
```java

package com.example.socket.config;
import lombok.extern.slf4j.Slf4j;
import org.java_websocket.client.WebSocketClient;
import org.java_websocket.drafts.Draft_6455;
import org.java_websocket.handshake.ServerHandshake;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;
import java.net.URI;

/**
 * @Description: 配置websocket后台客户端
 */
@Slf4j
@Component
public class WebSocketConfig {
    @Bean
    public WebSocketClient webSocketClient() {
        try {
            WebSocketClient webSocketClient = new WebSocketClient(new URI("ws://localhost:8085/websocket/test"),new Draft_6455()) {
                @Override
                public void onOpen(ServerHandshake handshakedata) {
                    log.info("[websocket] 连接成功");
                }
                @Override
                public void onMessage(String message) {
                    log.info("[websocket] 收到消息={}",message);
                }
                @Override
                public void onClose(int code, String reason, boolean remote) {
                    log.info("[websocket] 退出连接");
                }

                @Override
                public void onError(Exception ex) {
                    log.info("[websocket] 连接错误={}",ex.getMessage());
                }
            };
            webSocketClient.connect();
            return webSocketClient;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```
3. 使用后台客户端发送消息
```java
package com.example.socket.code;

/**
 * @Description: websocket 接口
 */
public interface WebSocketService {
    /**
     * 群发
     * @param message
     */
     void groupSending(String message);
    /**
     * 指定发送
     * @param name
     * @param message
     */
     void appointSending(String name,String message);
}
```

4. 实现接口
```java

package com.example.socket.code;
import org.java_websocket.client.WebSocketClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

/**
 * @Description: websocket接口实现类
 */
@Component
public class SocketClient implements WebSocketService{

    @Autowired
    private WebSocketClient webSocketClient;
    @Override
    public void groupSending(String message) {
        // 这里我加了6666-- 是因为我在index.html页面中，要拆分用户编号和消息的标识，只是一个例子而已
        // 在index.html会随机生成用户编号，这里相当于模拟页面发送消息
        // 实际这样写就行了 webSocketClient.send(message)
        webSocketClient.send(message+"---6666");
    }
    @Override
    public void appointSending(String name, String message) {
        // 这里指定发送的规则由服务端决定参数格式
        webSocketClient.send("TOUSER"+name+";"+message);
    }
}
```

5. 实现最终的controller
```java
package com.example.socket.chat;

import com.example.socket.code.ScoketClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Description: 测试后台websocket客户端
 */

@RestController
@RequestMapping("/websocket")
public class IndexController {

    @Autowired
    private SocketClient webScoketClient;

    @GetMapping("/sendMessage")
    public String sendMessage(String message){
        webScoketClient.groupSending(message);
        return message;
    }
}
```

# 4. 参考
1. <a href = "https://blog.csdn.net/weixin_38111957/article/details/86352677">springboot+websocket实现</a>
2. <a href = "https://blog.csdn.net/fang_qiming/article/details/79419809">使用websocket建立长连接</a>
3. <a href = "https://blog.csdn.net/weixin_39793752/article/details/80949128">websocket @ServerEndpoint(value = "/websocket/{ip}")详解</a>