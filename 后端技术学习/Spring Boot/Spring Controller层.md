Spring Controller层
---

<!-- TOC -->

- [1. Controller层](#1-controller层)
  - [1.1. RestController](#11-restcontroller)
  - [1.2. RequestMapping](#12-requestmapping)
  - [1.3. RequestParam](#13-requestparam)
  - [1.4. PathVariable](#14-pathvariable)
  - [1.5. RequestBody](#15-requestbody)
  - [1.6. GetMapping](#16-getmapping)
- [2. @RestController和@Controller的区别](#2-restcontroller和controller的区别)
  - [2.1. @Controller](#21-controller)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. Controller层

## 1.1. RestController
- RestController是REST风格API的控制器
```java
@RestController
@RequestMapping("/story")
public class StoryController {  
    @Autowired 
    private StoryService storyService;
}
```
1. @Controller对应表现层的Bean。这里用RestController，默认类中的方法都会以**json**的格式返回给前端。
2. 使用@Controller注解标识StoryController之后，就表示要把StoryController交给Spring容器管理，在Spring容器中会存在一个名字为“storyController”的bean。
3. 如果@Controller不指定名称，则默认的bean名字为这个类的类名首字母小写，如果指定名称【@Controller(value=“StoryController”)】或者【@Controller(“StoryController”)】，则使用value作为bean的名字。

## 1.2. RequestMapping
```java
@RestController
@RequestMapping("/story")
public class StoryController {
    @Autowired
    private StoryService storyService;
}
```
1. @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
2. @RequestMapping注解常用属性有：
    + value指定请求的地址
    + method指定请求类型， GET、POST、PUT、DELETE等
    + consumes指定请求的接收内容类型，如application/json, text/html
    + produces指定返回内容类型。
    + params指定request中必须包含某些参数值，才让该方法处理请求。
    + headers指定request中必须包含某些指定的header值，才让该方法处理请求。

## 1.3. RequestParam
```java
@RestController
@RequestMapping(“/story”)
public class StoryController {
    @Autowired
    private StoryService storyService;
    /*** 交换两个story*/
    @PostMapping(“/exchange”)
    public ResultVO<Story> exchangeById(@RequestParam int src_id,@RequestParam int tar_id){…}
}
```
1. @RequestParam从request里面取参数值。
使用http://IP:port/story/exchange?src_id=num1&tar_id=num2访问，将num1和num2传递赋值给src_id和tar_id
2. Eg.@PostMapping(“/url”) 等同于@RequestMapping(value = "/url",method = RequestMethod.POST)

## 1.4. PathVariable
```java
/**根据task获取对应的所有story*/
@GetMapping("/list/{taskId}")
public ResultVO<List<Story>> getByTask(@PathVariable int taskId){…}
```
1. @PathVariable映射url片段到java方法的参数。
2. Eg.使用http://IP:port/story/list/num访问，num将作为参数传递给taskId
3. Eg.@GetMapping(“/url”) 等同于
@RequestMapping(value = "/url",method = RequestMethod.GET)
4. @PathVariable 支持三种参数
    + name 指定绑定参数的名称，要跟URL上面的一样
    + required 指定这个参数是不是必须的
    + value 跟name一样的作用，是name属性的一个别名
    + @RequestParam支持四种参数，加上了defaultValue，表示如果本次请求没有携带这个参数，或者参数为空，那么就会启用默认值

## 1.5. RequestBody
```java
/**新增story*/
@PostMapping("/create")
public ResultVO<Story> createStory(@RequestBody StoryVO storyInput){…}
```
1. @RequestBody映射请求体到java方法的参数，一般不用于get请求。
2. 前端访问需要传递JSON字符串，字符串定义了StoryVO的属性值。
3. ResponseBody注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式，紫萼如到response对象输出指定格式的数据。
4. 前端使用ajax访问方式如下示例
```js
var data = {
    “taskId”: $(“#taskId”).val(),
    “name”: $(“#name”).val(),
    “storyPoint”: $(“#storyPoint”).val(),
    “priority”: $(“#priority”).val(),
    “description”: 
$(“#description”).val(),
    “posId”: $(“#posId”).val(),
    “acceptance”:
$(“#acceptance”).val(),
    “releaseId”: $(“#iteration”).val(),
}
$.ajax({
    type: “POST”,
    url: “/story/create”,
    dataType:“json”,
    contentType : ‘application/json’,
    data:JSON.stringify(data),
    success: function (data) {…}
})
```

## 1.6. GetMapping
```java
@GetMapping("/storyMapping")
public ModelAndView getMapping(@RequestParam int proId){
    ModelAndView modelAndView = new ModelAndView("storymap");
    modelAndView.addObject("proId",proId);
    return modelAndView;
}
```
1. controller除了下发操作到数据库，还用于跳转界面，返回值为ModelAndView类型。
2. ModelAndView的构造参数storymap是指要跳转的目标页面storymap.html的路径。
3. ModelAndView可以携带参数，这个方法传递了proId给目标页面，在storymap.html文件中接收方式如下：
```html
<script th:inline="javascript">
var proId = [[${proId}]]
</script>
```

# 2. @RestController和@Controller的区别
>官方文档:@RestController is a stereotype annotation that combines @ResponseBody and @Controller.
1. @RestController注解相当于@ResponseBody ＋ @Controller合在一起的作用。

## 2.1. @Controller
1. 这个注解在:org.springframework.stereotype包下，其中被@RequestMapping标记的方法被分发处理器扫描识别，将不同的请求分发得到对应接口。
2. 查找方式主要是通过包扫描
3. 在这种模式下，需要使用@ResponseBody注解

# 3. 参考
1. <a href = "https://blog.csdn.net/lusa1314/article/details/85992253?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task">spring注解：@RestController、@Controller、@ResponseBody</a>