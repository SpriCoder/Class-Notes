java的文件与流
---

1. 标准输入输出流

<!-- TOC -->

- [1. File类](#1-file类)
	- [1.1. 相关的方法](#11-相关的方法)
- [2. FileInputStream类](#2-fileinputstream类)
- [3. InputStreamReader](#3-inputstreamreader)
	- [3.1. 一些问题](#31-一些问题)
	- [3.2. 详细参考](#32-详细参考)
- [4. BufferedReader](#4-bufferedreader)
- [5. 基于字节流的类](#5-基于字节流的类)
	- [5.1. InputStream类](#51-inputstream类)
	- [5.2. OutputStream类](#52-outputstream类)
		- [5.2.1. write()方法实例解析](#521-write方法实例解析)
- [6. 基于字符流的类](#6-基于字符流的类)
	- [6.1. Reader类](#61-reader类)
	- [6.2. Writer类](#62-writer类)
- [7. RandomAccessFile 专门处理文件的类](#7-randomaccessfile-专门处理文件的类)
- [8. 文件相对路径的设置](#8-文件相对路径的设置)
	- [8.1. /和.的对应路径](#81-和的对应路径)
	- [8.2. 三种文件读取方式](#82-三种文件读取方式)
- [9. 参考](#9-参考)

<!-- /TOC -->

# 1. File类
1. 实例化:`File file = new File(file_path)`

## 1.1. 相关的方法
方法|返回值|功能
--|--|--
file.exists()|boolean|判断是否存在这个文件路径的文件
file.createNewFile()|boolean|用来创建创建对应的文件(如果有重新创建)

# 2. FileInputStream类
1. 构造方法：
    + 参数：
        1. File
        2. String

# 3. InputStreamReader
1. InputStreamReader类是从字节流到字符流的桥接器:它使用指定的字符集读取字节并将其解码为字符集。
2. 它使用的字符集可以通过名称指定，也可以明确指定，或者可以接受平台的默认字符集。
3. 继承关系:`public class InputStreamReader extends Reader {}`

## 3.1. 一些问题
1. 字节流到字符流的桥梁怎么理解?
    1. 计算机存储的单位是字节
    2. 字节流读取是单字节读取，但是不同字符集解码成字符需要不通过个数
    3. 需要一个流把字节流读取的字节进行缓冲而后在通过字符集解码成字符返回，因而形式上看是字符流
    4. InputStreamReader流就是起这个作用，实现从字节流到字符流的转换
2. 使用指定的字符集并将它们解码为字符怎么理解?
    + 字节本质是8个二进制位，且不同的字符集对同一字节解码后的字符结果是不同的，因此在读取字符时务必要指定合适的字符集，否则读取的内容会产生乱码
    
## 3.2. 详细参考
<a href = "https://blog.csdn.net/ai_bao_zi/article/details/81133476">JAVA基础知识之InputStreamReader流</a>

# 4. BufferedReader
```java
StringBuilder sb = new StringBuilder();
File filename = new File(path);
InputStreamReader reader = new InputStreamReader(new FileInputStream(filename));
BufferedReader br = new BufferedReader(reader);
String line;
line = br.readLine();
```
    
# 5. 基于字节流的类

## 5.1. InputStream类
```java
/**
 * 字节流文件读写,按照一个字节一个字节的读取，将读取到的字节写入新的文件
 */
public class FileRwByByte {
	public void readAndWrite(String filename, String target) throws IOException {
		// 定义源文件
		File file = new File(filename);
		InputStream fis = new FileInputStream(file);
		// 获取文件名
		String fileName = file.getName();
		// 定义写文件路径
		String aimPath = target;
		OutputStream fos = new FileOutputStream(aimPath);

		// 单字节读取:定义字节，接收读取到的源文件字节内容
		int by;
		while ((by = fis.read()) != -1) {
			fos.write(by);
		}

        // 定义字节数组，接收读取到的源文件字节内容,按照数组进行读取
        byte[] bytes = new byte[1024];
        while (fis.read(bytes) != -1) {
            fos.write(bytes);
        }

		fos.flush();
		fis.close();
		fos.close();
        
	}
}
```

## 5.2. OutputStream类

### 5.2.1. write()方法实例解析
1. <a href = "https://blog.csdn.net/dodod2012/article/details/80581162">java.io.OutputStream类的中write方法的解析</a>
2. 标准方法声明:`public void write(byte[] b, int off, int len)`
    + b是数据
    + off是在数据偏移的开始
    + len是指写入的字节数
3. 无返回值
4. 抛出的异常:`IOException`
5. 方法简述:方法从指定的字节数组开始到当前输出流关闭写入len字节。一般的合约write(b, off, len)，一些在数组b中的字节写入，以便输出流;元素b[off]是写入的第一个字节和b[off+len-1]是写的这个操作的最后一个字节。OutputStream的write方法调用的每个被写出其中一个字节参数的write方法。子类被鼓励重写此方法，并提供了一个更有效的实现。如果b为null，一个NullPointerException异常抛出。如果断为负，或len为负，或者off + len位置大于数组b的长度，则抛出IndexOutOfBoundsException。

# 6. 基于字符流的类

## 6.1. Reader类
1. 有按照单字节读写和读取字节数组

```java
/**
 * 字符流文件读写
 */
public class FileReaderDemo {
	public void readAndWrite(String filename,String target) throws IOException {
		// 定义源文件
		File file = new File(filename);
		Reader reader = new FileReader(file);
		// 获取文件名
		String fileName = file.getName();
		// 定义写文件路径
		String aimPath = target;
		Writer writer = new FileWriter(aimPath);
		// 定义单个字符，接收读取到的源文件字符内容
		int ch;
		while ((ch = reader.read()) != -1) {
			writer.write(ch);
        }
        // 定义字符数组，接收读取到的源文件字符内容
        char[] chars = new char[1024];
        while (reader.read(chars) != -1) {
            writer.write(chars);
        }
		writer.flush();
		writer.close();
		reader.close();
    }
}
```

## 6.2. Writer类

# 7. RandomAccessFile 专门处理文件的类
```java
reader = new RandomAccessFile(disk_device, "r");
// 本作业只有128M磁盘，不会超出int表示范围
// ps: java的char是两个字节，但是write()方法写的是字节，因此会丢掉char的高8-bits，读的时候需要用readByte()
// pss: 读磁盘会很慢，请尽可能减少read函数调用
reader.skipBytes(Integer.parseInt(new Transformer().binaryToInt("0" + eip)));
```
1. `reader.close()`:关闭这个文件处理

# 8. 文件相对路径的设置

## 8.1. /和.的对应路径
```java
File file = new File("/");
System.out.println("/ 代表的绝对路径为：" + file.getAbsolutePath());//是对应磁盘根目录的位置。

File file1 = new File(".");
System.out.println(". 代表的绝对路径为" + file1.getAbsolutePath());//是当前java项目的根目录
```

## 8.2. 三种文件读取方式
1. 通过new的方式打开文件
```java
File file = new File("src/test1.txt");

File file = new File("src/test/test2.txt");

File file = new File("test3.txt");
```
2. 通过类的相对路径：这里是不放置在项目根目录底下的情况
```java
File file = new File(ResourceRead.class.getResource("/test1.txt").getFile());

File file = new File(ResourceRead.class.getResource("/test/test2.txt").getFile());
```
3. 使用线程的类加载器：和第二种相似
```java
File file = new File(Thread.currentThread().getContextClassLoader().getResource("test1.txt").getFile());

File file = new File(Thread.currentThread().getContextClassLoader().getResource("test/test2.txt").getFile());
```

# 9. 参考
1. <a href = "https://blog.csdn.net/dimudan2015/article/details/81910690">专门处理文件的类</a>
2. <a href = "https://blog.csdn.net/qq_41856633/article/details/89715920">java相对路径的书写</a>
3. <a href = "https://blog.csdn.net/magi1201/article/details/84959949">Java字节流与字符流读写文件</a>