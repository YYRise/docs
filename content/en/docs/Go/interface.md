---
title: "Interface"
date: 2021-09-22T19:33:26+08:00
draft: false
---

## 1. iface结构

![img](/images/Go/interface-iface.png)

```go
type iface struct {
	tab  *itab				// 接口表指针，指向类型信息
	data unsafe.Pointer 	// 数据指针，指向具体的数据
}

type itab struct {
	inter  *interfacetype // 接口的类型 // 8字节
	_type  *_type         // 实体的类型，包括内存对齐方式，大小等 // 8字节
	link   *itab          // 8字节
	hash   uint32         // copy of _type.hash. Used for type switches. // 4字节
	bad    bool           // type does not implement interface // 1字节
	inhash bool           // has this itab been added to hash? // 1字节
	unused [2]byte        // 2字节

	/*
		// 这里存储的是第一个方法的函数指针，如果有更多的方法，则在它之后的内存空间里继续存储
		// 从汇编角度来看，通过增加地址就能获取到这些函数指针
		// 这些方法是按照函数名称的字典序进行排列的
	*/
	fun [1]uintptr // 8字节
}

type interfacetype struct {
	typ     _type     // Go 语言中各种数据类型的结构体
	pkgpath name      // 定义接口的包名
	mhdr    []imethod // 接口所定义的函数列表
}

type _type struct {
	size       uintptr  // 类型大小
	ptrdata    uintptr  //	
	hash       uint32   // 类型的 hash 值	
	tflag      tflag    // 类型的 flag，和反射相关	
	align      uint8    // 内存对齐相关
	fieldalign uint8    //	
	kind       uint8    // 类型的编号，有bool, slice, struct 等等等等
	alg        *typeAlg //	
	gcdata     *byte    // gc 相关
	str        nameOff  //
	ptrToThis  typeOff  //
}
```