java_加密
---
<!-- TOC -->

- [1. 为什么要进行加密?](#1-为什么要进行加密)
- [2. 普遍使用的三种加密方式](#2-普遍使用的三种加密方式)
- [3. 加密例子](#3-加密例子)
- [4. 后端中已经使用的方法](#4-后端中已经使用的方法)
- [5. MessageDigest简介](#5-messagedigest简介)
	- [5.1. MessageDigest对象](#51-messagedigest对象)
	- [5.2. 使用方法](#52-使用方法)
		- [5.2.1. 创建MessageDigest对象](#521-创建messagedigest对象)
		- [5.2.2. MessageDigest传送要计算的数据](#522-messagedigest传送要计算的数据)
		- [5.2.3. 计算信息摘要](#523-计算信息摘要)
- [6. 参考](#6-参考)

<!-- /TOC -->
# 1. 为什么要进行加密?
1. 在网站中，为了保护网站会员的用户名和密码等隐私信息，所以我们在用户注册时就直接进行MD5方式或其他方式进行加密，即使是数据库管理员也不能查看该会员的密码等信息，在数据库中查看密码效果如：8e830882f03b2cb84d1a657f346dd41a效果。
2. 因为MD5算法是不可逆的，所以被很多网站广泛使用.

# 2. 普遍使用的三种加密方式
1. 使用位运算符，将加密后的数据转换成16进制
2. 使用格式化方式，将加密后的数据转换成16进制（推荐）
3. 使用算法，将加密后的数据转换成16进制

# 3. 加密例子
```java

import java.security.MessageDigest;

import java.security.NoSuchAlgorithmException;
/**
 * 使用Java自带的MessageDigest类
 * @author stormbroken
 */
public class EncryptionUtil {
	/**
	 * @param source 需要加密的字符串
	 * @param hashType 加密类型 （MD5 和 SHA）
	 * @return
	 */
	public static String getHash(String source, String hashType) {
		// 用来将字节转换成 16 进制表示的字符
		char hexDigits[] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
		try {
			MessageDigest md = MessageDigest.getInstance(hashType);
			// 通过使用 update 方法处理数据,使指定的 byte数组更新摘要
			md.update(source.getBytes());
			// 获得密文完成哈希计算,产生128 位的长整数
			byte[] encryptStr = md.digest();
			// 每个字节用 16 进制表示的话，使用两个字符
			char str[] = new char[16 * 2];
			// 表示转换结果中对应的字符位置
			int k = 0;
			// 从第一个字节开始，对每一个字节,转换成 16 进制字符的转换
			for (int i = 0; i < 16; i++) {
				// 取第 i 个字节
				byte byte0 = encryptStr[i];
				// 取字节中高 4 位的数字转换, >>> 为逻辑右移，将符号位一起右移
				str[k++] = hexDigits[byte0 >>> 4 & 0xf];
				// 取字节中低 4 位的数字转换
				str[k++] = hexDigits[byte0 & 0xf]; 
			}
			// 换后的结果转换为字符串
			return new String(str); 
		} catch (NoSuchAlgorithmException e) {
			e.printStackTrace();
		}
		return null;
	}
	/** @param source 需要加密的字符串
	 * @param hashType  加密类型 （MD5 和 SHA）
	 * @return
	 */
	public static String getHash2(String source, String hashType) {
		StringBuilder sb = new StringBuilder();
		MessageDigest md5;
		try {
			md5 = MessageDigest.getInstance(hashType);
			md5.update(source.getBytes());
			for (byte b : md5.digest()) {
				sb.append(String.format("%02X", b)); // 10进制转16进制，X 表示以十六进制形式输出，02 表示不足两位前面补0输出
			}
			return sb.toString();
		} catch (NoSuchAlgorithmException e) {
			e.printStackTrace();
		}
		return null;
	}
	/** @param source 需要加密的字符串
	 * @param hashType  加密类型 （MD5 和 SHA）
	 * @return
	 */
	public static String getHash3(String source, String hashType) {
		// 用来将字节转换成 16 进制表示的字符
		char hexDigits[] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
		StringBuilder sb = new StringBuilder();
		MessageDigest md5;
		try {
			md5 = MessageDigest.getInstance(hashType);
			md5.update(source.getBytes());
			byte[] encryptStr = md5.digest();
			for (int i = 0; i < encryptStr.length; i++) {
				int iRet = encryptStr[i];
				if (iRet < 0) {
					iRet += 256;
				}
				int iD1 = iRet / 16;
				int iD2 = iRet % 16;
				sb.append(hexDigits[iD1] + "" + hexDigits[iD2]);
			}
			return sb.toString();
		} catch (NoSuchAlgorithmException e) {
			e.printStackTrace();
		}
		return null;
	}
}
```

# 4. 后端中已经使用的方法
```java
public static String encrypt(String word) throws NoSuchAlgorithmException {
  //用于加密密码
  MessageDigest md = MessageDigest.getInstance("MD5");
  byte[] input = word.getBytes();
  byte[] output = md.digest(input);
  return Base64.encodeBase64String(output);
}
```

# 5. MessageDigest简介
1. java.security.MessageDigest类用于为应用程序提供信息摘要算法的功能。
	+ 目标是用于生成散列码。
	+ 信息摘要是安全的**单向哈希**函数。

## 5.1. MessageDigest对象
1. MessageDigest通过其getInstance系列静态函数来进行实例化和初始化。
2. MessageDigest对象通过`update`方法处理数据，任何时刻都可以使用`reset`来重置摘要，通过`digest`方法之一来完成哈希计算并返回结果。
3. 对于给定数量的更新数据，`digest`方法只能被调用一次。`digest`方法被调用后，`MessageDigest`对象被重新设置成初识状态。
	+ 并且不是单实例的

## 5.2. 使用方法

### 5.2.1. 创建MessageDigest对象
1. `public static MessageDigest getInstance(String algorithm)`
2. 算法名是忽略大小写的

### 5.2.2. MessageDigest传送要计算的数据
1. 使用update方法来向已经初始化的MessageDigest对象提供传送要计算的数据。
```java
public void update(byte input);
public void update(byte[] input);
public void update(byte[] input, int offset, int len);
```

### 5.2.3. 计算信息摘要
`byte s[]=m.digest( );`

# 6. 参考
1. <a href = "https://blog.csdn.net/xiaokui_wingfly/article/details/38045871#">digest</a>
2. <a href = "https://blog.csdn.net/xiao__jia__jia/article/details/94477019">MessageDigest简介</a>