<!-- TOC -->

- [1. java学习笔记之GUI](#1-java学习笔记之gui)
  - [1.1. GUI综述](#11-gui综述)
  - [1.2. GUI的关键部分以及GUI的创建](#12-gui的关键部分以及gui的创建)
    - [1.2.1. GUI的创建](#121-gui的创建)
  - [所有组件的根父类](#所有组件的根父类)
  - [1.3. GUI发展过程](#13-gui发展过程)
  - [1.4. 事件和布局](#14-事件和布局)
  - [1.5. 事件](#15-事件)
  - [1.6. 使用内部类](#16-使用内部类)
  - [1.7. layout部分](#17-layout部分)
    - [1.7.1. 向图形上添加东西的方法](#171-向图形上添加东西的方法)
    - [布局管理器](#布局管理器)
  - [1.8. 简单界面组件](#18-简单界面组件)
    - [1.8.1. Button](#181-button)
    - [1.8.2. Button的监听](#182-button的监听)
    - [1.8.3. 文本输入](#183-文本输入)
    - [1.8.4. 选择组件](#184-选择组件)
    - [1.8.5. 菜单](#185-菜单)
    - [1.8.6. 对话框](#186-对话框)
- [2. MVC Design Pattern](#2-mvc-design-pattern)
  - [2.1. 设计模式](#21-设计模式)

<!-- /TOC -->
# 1. java学习笔记之GUI

## 1.1. GUI综述
1. GUI：Graphical user interface
2. WIMP:window,icon,menu,pointer
3. WYSIWYG:What-You-See-Is-What-You-Get

## 1.2. GUI的关键部分以及GUI的创建
1. 组件Component
2. 布局Layout
3. 事件Event

### 1.2.1. GUI的创建
```java
JFrame frame = new JFrame();//创建frame框架
JButton button = new JButton(str);//创建一个str的按钮
frame.getContentPane().add(button);//获得当前框架主体，添加button
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//这一行程序会在window关闭时把程序结束
frame.setSize(300,300);//设置frame大小
frame.setVisible(true);//设置frame可见
```

## 所有组件的根父类
1. Component

## 1.3. GUI发展过程
1. AWT
2. Swing
3. SWT
4. JavaFx

## 1.4. 事件和布局
1. 首先有一个窗口
2. 组件

组件名称|组件内容
--|--
`JFrame`|窗口
`Button`|按钮
`Image`|图片
`Color`|颜色元素

3. 具体行为

相应行为|意义
--|--
`frame.getContentPane().add(Button)`|布局按钮
`setVisible(true)`|设置为可见
`setSize(300,300)`|设置控件大小
`new ImageIcon("name.jpg").getImage()`|获取一个图像的布局
`fillOval(,,,)`|绘制一个椭圆
`new Color(,,)`|定义一个正常的颜色

4. 监听者和被监听者
    + 实现过程：
        1. 继承为一个监听器:`implements ActionListener`
        2. 指定一个按钮:`.addActionLiastener(this)`
        3. 定义被按动的行为:`actionOerformed()`
## 1.5. 事件
1. 用指定的方法制作特定的方法
```java
public static MidiEvent makeEvent(int comd,int chan,int one,int two,int tick){
    MidiEvent a = null;
    try{
        ShortMessage a = new ShortMessage();
        a.setMessage(comd,chan,one,two);
        event = new MidiEvent(a,tick);
    }catch(Exception e){

    }
    return event;
}
```
2. 使用队列来完成保存记录

## 1.6. 使用内部类
1. 外部类方面的相应状态的变更可以修改内部类的属性
2. 使用内部类来完成动画的制作

## 1.7. layout部分
1. 基本布局被分为五部分：north、west、center、east、north.
    1. 添加布局：`frame.getContentPane().add(BorderLayout.CENTER,button)`
    2. 注意整个布局
2. layout manager:
    1. 用于自动化完成控件的布局
    2. BorderLayout:按照五部分分每个布局
    3. FlowLayout:从左向右布局，期望非文字的控件
    4. BoxLayout:类似FlowLayout布局，但是可以强制设置转换类型
    5. 具体使用见PPT
    6. 其他布局管理器：
        1. 网格布局
        2. 网格组布局
    7. 订制布局管理器
3. 各种布局须知：
    1. 当Frame的大小改变Frame中的按钮的位置会随之改变的布局是：FlowLayout
    2. 将容器划分为固定的网格进行布局的布局管理器是：GridLayout
    3. 每个区域只能用那一个组件的布局管理器：
        1. BorderLayout
        2. GridLayout
        3. CardLayout
    4. 组件大小随容器大小变化：
        1. BorderLayout
        2. GridLayout

### 1.7.1. 向图形上添加东西的方法
1. 在frame上放置widgt
2. 在widgt上绘制2D图形
    1. 创建自己的绘图组件：
        + 创建JPanel的子类
        + 重写public void paintComponet(Graphics g){//TO-DO}
        + `Image image = new ImageIcon(name).getImage();`：获得一个JPEG显示
        + `g.drawImage(image,3,4,this)`：画出这个图，左边距离panel左侧3像素，上侧距离顶部4像素
    2. g参数所引用的对象实际上是个Graphics2D的实例
        1. 但是想要引用Graphics2D对象，需要使用强制转化
            + `Graphics2D g2d = new (Graphics2D) g;`
        2. setPaint():可以设置画笔
3. 在widgt上绘制JPEG图

### 布局管理器
1. 嵌套布局
2. BorderLayout
    1. 标准的五大布局
    2. 每个区域只能放置一个组件，很难保证控件保持原有的大小
    3. 中间区域只能有剩下部分的像素
    4. 不好进行直接像素设置大小
3. FlowLayout
    1. 面板默认的布局管理器
    2. 从左向右布局方式
4. BoxLayout
    1. 使用默认大小，按照加入方式进行添加
    2. 从上向下添加
5. 使用布局管理器来在Frame窗体中布置一个按钮，此按钮大小不手Frame框体大小变化影响
## 1.8. 简单界面组件

### 1.8.1. Button
1. 考Button:参见53页PPT Testfield.label
    1. 是否继承
    2. 是够被监听
    3. 是否被布局
    4. 是够实现了actionperformed方法
2. 调用repaint方法由系统帮你调用，未必什么时候调用
    1. 重绘过程是异步的
    2. 如果多次调用repaint()，那么只会调用最后一次
3. 实现被按动效果：如果被按动，那么重新绘制这个东西

### 1.8.2. Button的监听
1. 来源：java.awt.event
2. 相应的监听事件：具体地请查API
    1. ActionListener:actionPerformed(ActionEvent ev)
    2. ItemListener:itemStateChanged(ItemEvent ev)
    3. KeyListener:
        + keyPressed(KeyEvent ev)
        + keyReleased(KeyEvent ev)
        + keyTyped(KeyEvent ev)
3. 取得ActionEvent：
    1. 实现ActionListener这个接口
        + `implements ActionListener`
    2. 向按钮进行注册(告诉它你要监听事件)
        + `button.addActionListener(this)`
    3. 定义事件的处理方法
4. 多个监听：内部类来实现
5. 所有的监听接口中定义的方法，访问权限都是public，返回值是void的
```java
public void actionPerformed(ActionEvent event){
    button.setText(str);//TO-DO
}
```

### 1.8.3. 文本输入
1. 文本：JTextField
    1. 允许编辑单行文本的文本组件
    2. 构造函数：
        + `JTextField field = new JTextField(20);`
        + `JTextField field = new JTextField("Your name")`
        + `JTextField field = new JTexField(String text, int columns)`构造使用要显示的指定文本初始化的新文本字段，宽度足够容纳指定列数。
    3. 使用的方法：
        1. `.getText()`：获得文本
        2. `.setText(str)`：设置文本
            + 如果是""，那么就是清空
        3. `.addActionListener(this)`：添加监视器
        4. `.selectAll()`：选择全部文本
        5. `.requestFocus()`：将GUI焦点拉回文本段落来使得用户完成输入
2. 文本域：JTextArea
    1. 构造函数：`JTextArea text = new JTextArea(10,20)//行高和字宽`
    2. 其他方法：
        1. `.setText(str)`
        2. `.append(str)`
        3. `.selectAll()`
        4. `.requestFocus()`
        5. 设置垂直的滚动条：
```java
JScrollPane scroller = new JScrollPane(text);
text.setLineWrap(true);//开启自动换行
scroller.setVerticalScrollBarPolicy(ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS);
scroller.setHorizontalScrollBarPolicy(ScrollPaneConstants.HORIZONTAL_SCROLLBAR_NEVER);//设置了垂直滚动条
panel.add(scroller);
```
3. 标签和标签组件:JLable
4. 密码域
5. 滚动窗格

### 1.8.4. 选择组件
1. 复选框
2. 单选按钮
3. 边框
4. 组合框
5. 滑块
6. JCheckBox：
    1. 构造方法
        + `JCheckBox check = new JCheckBox(str)`
    2. 其他方法：
        + 添加监听Item的事件
            + `check.addItemListener(this)`
        + 处理事件(判别是否被选中)
            + `.isSelected()`
            + 实现`public void itemStateChanged(ItemEvent ev){}`
        + 用程序要设置选择或者不选择:
            + `check.setSelected(true);`
            + `check.setSelected(false);`

### 1.8.5. 菜单
JList
1. 构造函数
```java
int[] result = {1,2,3};//任意类型的数组即可
list = new JList(listEntries);
```
2. 具体使用：
    1. 显示垂直滚动条：同JCheckBox
    2. 设置显示行数：`list.setVisibleRowCount(number)`
    3. 限制单选:`.setSelectionMode(ListSelectionModel.SINGLE_SELECTION)`
    4. 对选择事件做注册：`.addListSelectedListener(this)`
    5. 处理选择事件
```java
public void valueChanged(ListSelectedEvent lse){
    if(!lse.getValueIsAdjusting()){
        String selection = (String) list.getSelectedValue();
        System.out.println(selection);
    }
}
```
### 1.8.6. 对话框

# 2. MVC Design Pattern
1. MVC之间的交互
2. MVC模式
    1. 模型：存储内容
    2. 视图：显示内容
    3. 控制器：处理用户输入
3. controller模式:不同被点击形式等

## 2.1. 设计模式
1. 容器和组件是Composite模式
2. 带滚动条的面板是Decorator模式
3. 布局管理器是Strategy模式
