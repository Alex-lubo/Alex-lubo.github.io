---
title: STL对象在dll间传递时的注意事项
date: 2018-02-21 22:12:08
tags: C++
---

工作中有一个需求，在vs2005中编译的exe调用vs2013编译的dll，其中需要把一些list的数据内容传递到dll中的函数中。发现编译没有问题，dll的引用也没有问题。
但是执行时发现不对。仔细看，发现exe中list里的内容是ok的，但是在dll中发现list的大小和内容都发生了变化，size从129变成了很大的一个数值，内容的数据结构
基本乱掉了。
一番google之后发现，STL对象在dll间传递时会出现异常。要微软的解释是：

> 大部分C++标准库里提供的类直接或间接地使用了静态变量。由于这些类是通过模板扩展而来的，因此每个可执行映像（通常是.dll或.exe文件）就会存在一份只属于自
> 己的、给定类的静态数据成员。当一个需要访问这些静态成员的类方法执行时，它使用的是“这个方法的代码当前所在的那份可执行映像”里的静态成员变量。由于两份可
> 执行映像各自的静态数据成员并未同步，这个行为就可能导致访问违例，或者数据看起来似乎丢失或被破坏了。

意思就是说： exe和dll中分别会存自己的静态变量。而这些静态变量内存的分配（构造和析构）过程由于exe和dll的编译器不同和运行时库的不一致从而导致内存
布局不一致。

如果非要能够直接传递stl对象，那么需要两个条件：
1. 相同的编译器和编译设置。
2. 完全一样的运行时库。

很显然我们的需求不能满足这两个条件，因此只能另想办法解决。

既然不能出传递stl对象，传递自己自定义的数据类型和数据结构是可以的。思路就是：定义数据结构，然后分别传递到dll， 然后dll内部对数据进行stl的封装，然后
stl的方式去操作。实践表明是可以的。

函数的定义为：

    void AddReportVul(VulShowInfo& vulInfo);`

其中VulShowInfo是自己定义的一个结构体。最终的格式为：
 ```
 struct VulShowInfo
	{
		DWORD         dwVulUUID;
		const WCHAR*  strVulName;
		unsigned long sizeVulName;
		const WCHAR*  strDescription;
		unsigned long sizeDescription;
		SYSTEMTIME    UTCPublishTime;
		ULONG         ulKbSize;
		BOOL          bIsThirdLeak;
		DWORD		  dwVulOUID;
		const WCHAR*  strThirdPatchInfo;
		unsigned long sizeThirdPatchInfo;
	};
```
  
其中本来传`"const char*"`， 在传递之前进行宽窄字符串的转换（UnicodeToUTF8），然后将string转const char*（str.c_str()）。然而发现传到dll中还是有问题，
对应的string的错误提示是	`"Invalid character in string."`
然后字符串的内容也丢失。后来采用wchar_t来传，在dll这边再进行UnicodeToUTF8，问题得到解决。
