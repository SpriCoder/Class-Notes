IDEA 使用经验
---

# 1. 不能适用java jdk
1. 将src等源文件夹右键，选择mark as:对应类型

# 2. IDEA 自动生成类注释和方法注释

## 2.1. 生成类注释
1. 打开file中的Preferences(mac)/settings(windows)
2. 找到Editor -> File and Code Templates 中具体想要自动生成的模板。
3. 具体变量，参考右下角的Desciption

## 2.2. 生成方法注释
1. 打开file中的Preferences/settings
2. 找到Editor -> Live Templates 中具体想要自动生成的模板。
3. 可以编辑快捷键和模板。

# 3. Java 扩展 jar 包的方案
1. 可以直接下载到jar包然后配置到IDEA中去
2. 我们还可以通过配置到maven管理的项目的pom.xml文件去中。

# 4. lombok和IDEA自带的generate操作
1. lombok的使用应该是慎重的(比较强的入侵性)
2. 我们可以右键，选择generate来自动生成对应的set和get方法

# 5. IDEA统计代码数量
1. 在Plugin中的Statistic扩展包可以用来统计代码的行数

# 6. 代码的全局搜索
1. 在这个类中进行搜索:Edit -> find(Ctrl + F)
2. 在全部代码文件中搜索:Edit -> Find in Path(Ctrl + Shift + F)

# 7. Structure侧边栏的使用
1. 左侧m表示方法，f表示属性占位符
   1. **红色**关闭的锁表示private
   2. **圆圈**表示默认不带任何修饰符
   3. **一把钥匙**表示protected
   4. **绿色**打开的锁表示public
2. 展示实现的接口类和继承的抽象类(I)
3. 圆圈p表示会显示所有的get/set方法
4. 圆圈f表示是否显示属性字段
5. 三个小矩形表示继承的方法
6. 两个同心圆:展示出匿名内部类
7. 等等，详见参考2

# 8. Favorites的使用
1. 在代码里面进行favorites然后会被注册记录下来

# 9. TODO注册
1. 使用//TODO进行注册
2. 竖栏的使用见参考三

# 10. debug 面板的使用
1. debug面板打开之后，我们可以进行进一步的定制化。 
   1. spend：断点级别，ALL表示对程序而言，Thread表示到线程级别。
   2. Condition：断点进入的条件
   3. Log to console:在流程走到断点时打印一些信
       1. "Breakpoint hit" message
       2. Stacktrace会打印堆栈信息
   4. Evaluate and log:打印指定参数的值
   5. Remove once hit:断点走一次就放掉
   6. Disable until breakpoint is hit:只有进入了指定断点后才能进入此断点
2. 多线程debug:在debugger里面选择不同的线程切换即可
3. 详见参考三

# 11. Spring的查看
1. 在Spring中可以查看bean(从对应包入手)，可以查看具体的应用和使用情况
2. 在下边栏中找到spring，然后可以查看使用情况
3. MVC的情况和API可以通过里面的MVC找到

# 12. Settings迁移
1. 换电脑的时候，我们只要先Export Settings，然后在新的电脑里面Import Settings即可

# 13. 分屏功能
1. x选中想要分屏的文件右键找到Split Vertically实现左右分屏
2. Split Horizontaly是上下分屏

# 14. 查看方法调用情况
1. 在Structure中选择中方法，然后我们可以从Navigate菜单栏中选择Call Hierarchy

# 15. 自己的扩展插件
1. Alipay DevTools
2. Java Methods References Diagram
3. Lombok
4. PlantUNL integeration
5. SequenceDiagram
6. soapUI Plugin
7. static
8. Websocket Client

## 自动生成
1. 在类里面 Alt + Insert 来生成getter、setter、consturctor等
2. 在方法上 Alt + Insert 来生成Test方法

# 16. 参考
1. <a href = "https://blog.csdn.net/qq_35246620/article/details/54705071">详解IDEA 的 添加jar包</a>
2. <a href = "https://blog.csdn.net/lz710117239/article/details/104175842">java开发工具（1）你真的会用IDEA么？（上）</a>
3. <a href = "https://blog.csdn.net/lz710117239/article/details/104224149">java开发工具（2）你真的会用IDEA么？（中）</a>