**Python GUI**
1. 真正的用户交互程序的制作。以wxPython为例
2. GUI:图形用户界面
3. wxPython运行快速，支持跨平台开发。
4. 官网上可以查到不同GUI组件之间的关系
<!-- TOC -->

- [1. 第一个简单的wxPython程序](#1-第一个简单的wxpython程序)
- [2. GUI框架](#2-gui框架)
- [3. 组件](#3-组件)
  - [3.1. 组件容器(Containers)](#31-组件容器containers)
  - [3.2. 动态组件(Dynamic Widgets)](#32-动态组件dynamic-widgets)
    - [3.2.1. 按钮](#321-按钮)
    - [3.2.2. 菜单(Menu及其组件)](#322-菜单menu及其组件)
    - [3.2.3. 列表(ListCtrl)](#323-列表listctrl)
    - [3.2.4. 单选框(RadioBox)和复选框(CheckBox)](#324-单选框radiobox和复选框checkbox)
  - [3.3. 静态组件(Static Widgets)](#33-静态组件static-widgets)
    - [3.3.1. 静态文本(StaticText)和文本框(TextCtrl)](#331-静态文本statictext和文本框textctrl)
  - [3.4. 其他组件](#34-其他组件)
- [4. 事件处理机制(Event Handling)](#4-事件处理机制event-handling)
- [5. 布局管理](#5-布局管理)
  - [5.1. 使用sizer的步骤](#51-使用sizer的步骤)
  - [5.2. wx.BoxSizer](#52-wxboxsizer)
  - [5.3. wx.FlexGridSizer](#53-wxflexgridsizer)
  - [5.4. wx.GridSizer](#54-wxgridsizer)
  - [5.5. wx.GridBagSizer](#55-wxgridbagsizer)
  - [5.6. wx.StaticBoxSizer](#56-wxstaticboxsizer)
- [6. 其他GUI库](#6-其他gui库)
  - [6.1. PyQt，更适用于专业人士](#61-pyqt更适用于专业人士)
  - [6.2. Tkinter](#62-tkinter)
  - [6.3. PyGTK](#63-pygtk)

<!-- /TOC -->
# 1. 第一个简单的wxPython程序
```python
import wx
app = wx.App()# 创建应用对象
frame = wx.Frame(None,title = 'Hello.world!')# 创建frame对象，框架(容器)
frame.Show(True)
app.MainLoop()# 进入事件循环，必须写
```
面向对象版
```python
import wx
class MyApp(wx.App):
    def OnInit(self):# OnInit方法会在最开始创建对象的时候被调用
        frame = wx.Frame(None,title = 'Hello.world!')
        frame.Show(True)
        return True
app = Myapp()
app.MainLoop()
```

# 2. GUI框架

```python
import wx
class Frame1(wx.Frame):
    def __init__(self,superior):
        wx.Frame.__init__(self,parent = superior,title = 'Example',pos = (100,200),size = (350,200))
        panel = wx.Panel(self)
        text1 = wx.TextCtrl(panel,value = "Hello,world!",size = (350,200))
if __name__ == '__main__':
    app = wx.App()
    frame = Frame1(None)
    frame.Show()
    app.MainLoop()
```

# 3. 组件

## 3.1. 组件容器(Containers)
1. 用于容纳其他组件
2. 例如:wx.Panel

## 3.2. 动态组件(Dynamic Widgets)
1. 可以被用户编辑
2. 例如:wx.Button、wx.TextCtrl、wx.ListBox等

### 3.2.1. 按钮
1. 功能:接受用户的点击事件，触发相应的操作。
2. 常用按钮:
    1. wx.Button:文本按钮
    2. wx.BitmapButton:位图按钮
    3. wx.ToggleButton:开关按钮
    4. 绑定处理按钮点击的事件
3. 属性
    1. label:按钮文字
    2. size:按钮大小
    3. pos:
    4. Font:
4. 方法
    1. SetLabel:设置按钮文字
    2. SetDefault
    3. Enable:设置为可操作状态

### 3.2.2. 菜单(Menu及其组件)
1. 菜单:
    1. 菜单栏
    2. 菜单
    3. 菜单项命令
2. wxPython用于创建菜单的类
    1. wx.MenuBar
    2. wx.Menu
    3. wx.MenuItem
3. 菜单事件
    1. wx.EVT_MENU
    2. 使用Bind对事件进行绑定`Bind(wx.EVT_MENU,处理方法)`

### 3.2.3. 列表(ListCtrl)
1. 列表用于显示多个条目并且可供用户选择
2. 列表能够以下面四种不同模式建造:
    1. wx.LC_ICON(图标)
    2. wx.LC_SMALL_ICON(小图标)
    3. wx.LC_LIST(列表)
    4. wx.LC_REPORT(报告)

### 3.2.4. 单选框(RadioBox)和复选框(CheckBox)
1. 复选框用于从一组可选项中，同时选中多个选项
2. 单选框用于从一组互斥的选项中，选取其一

## 3.3. 静态组件(Static Widgets)
1. 显示信息用，不能被用户编辑
2. 例如:wx.StaticBitmap、wx.StaticText、wx.StaticLine等

### 3.3.1. 静态文本(StaticText)和文本框(TextCtrl)
1. 文本框用于接受用户在框内输入的信息，或显示由程序提供的信息。
2. 静态文本框(标签)`wx.text1.AppendText(str)`
3. 文本框(单行，多行，富文本框):`wx.TextCtrl(panel,value = ,size = ,style = )`

## 3.4. 其他组件
1. 例如:wx.ToolBar,wx.MenuBar,wx.StatusBar

# 4. 事件处理机制(Event Handling)
1. 事件响应方法

```python
import wx
class Frame1(wx.Frame):
    def __init__(self,superior):
        self.panel.Bind(wx.EVT_LEFT_UP,self.OnClick)# 将鼠标左键拾起事件(EVT_LEFT_UP)绑定到派生的子类的OnClick方法上
    def OnClick(self,event):
        posm = event.GetPosition()# 获得鼠标单击位置
        wx.StaticTex(parent = self.panel,label = "hello, World!",pos = (posm.s,posm.y))
```

# 5. 布局管理
1. 绝对定位:每个窗口部件被创建时可以显式地指定它的位置和大小
    + 定位不灵活、调整大小困难、受设备、操作系统甚至字体影响
2. 灵活布局的解决方案:sizer
    + 每个sizer都有自己的定位策略
    + 开发者只需要选择合适策略的sizer将窗口组件放入，并且指定好需求即可
    + frame内置一个布局
3. sizer不是一个容器或是一个窗口部件，而只是一个屏幕布局的算法
    + sizer允许嵌套
    + 常用的Sizer

## 5.1. 使用sizer的步骤
1. 创建自动调用尺寸的容器，例如panel
2. 创建sizer
3. 创建子窗口(窗体部件)
4. 使用sizer的Add()方法将每个子窗口添加给sizer
5. 调用容器的SetSizer(sizer)方法，或是SetSizerAndFit(sizer)方法

## 5.2. wx.BoxSizer
1. 在一条线上布局窗口组件，可以水平或者垂直
2. 嵌套sizer可以使布局更加复杂
3. 创建:`wx.BoxSizer(wx.VERTICAL)`垂直方向
## 5.3. wx.FlexGridSizer
1. 每个网格未必一样大
2. 每一个行的高度由每一行最高的决定，每一列的宽度由每一列最宽的决定

## 5.4. wx.GridSizer
1. 宽度取决于最宽的组件，高度取决于最高的组件
2. 所有的网格的大小相同。

## 5.5. wx.GridBagSizer

## 5.6. wx.StaticBoxSizer

# 6. 其他GUI库
1. Tkinter的高效
2. PyQt的庞大

## 6.1. PyQt，更适用于专业人士
1. 可以代替Python内置的Tkinter
2. 具有强大的Qt(图形用户界面应用程序开发框架)
3. 提供了GPL和商业协议两种授权方式，可以免费用于自由软件的开发
4. 例子见PPT
5. 优点:
    1. 文档比其他GUI库丰富
    2. 与Qt、C++开发经验互通等
6. 缺点:
    1. 要注意避免内存泄露
    2. 运行时庞大
    3. 需要学习一些C++知识

## 6.2. Tkinter
1. 绑定了Python的Tk GUI工具集，通过内嵌在Python解释器内部的Tcl解释器实现
2. Tkinter的调用转化成Tcl命令，然后交给Tcl解释器进行解释，实现Python的GUI界面
3. 特点:比较简单、但是性能不太好，执行速度慢
4. 例子参见PPT

## 6.3. PyGTK
1. 是一套GTK+GUI库的Python封装
2. 为创建桌面程序提供了一套综合的图形元素和其他使用的编程工具。
3. 基于LGPL协议的免费软件
4. 例子见PPT
5. 特点
    1. 底层GTK+提供了各式的可视元素和功能
    2. 缺点:在windows平台表现不太好
