---
title: "Interface"
date: 2021-09-22T19:33:26+08:00
draft: false
---

## 1. iface结构

![img](/images/Go/interface-iface.png)

## 2. 相关结构体源码

```go
type eface struct { // 16 字节
    _type *_type
    data  unsafe.Pointer
}

type iface struct { // 16 字节
	tab  *itab				// 接口表指针，指向类型信息
	data unsafe.Pointer 	// 数据指针，指向具体的数据
}

type itab struct { // 32 字节
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
	size       uintptr  // 类型占用的内存空间，为内存空间的分配提供信息
	ptrdata    uintptr  //	
	hash       uint32   // 类型的 hash 值，能够快速确定类型是否相等	
	tflag      tflag    // 类型的 flag，和反射相关	
	align      uint8    // 内存对齐相关
	fieldalign uint8    //	
	kind       uint8    // 类型的编号，有bool, slice, struct 等等等等
	alg        *typeAlg //	
	equal func(unsafe.Pointer, unsafe.Pointer) bool // 用于判断当前类型的多个对象是否相等，该字段是为了减少 Go 语言二进制包大小，从 typeAlg 结构体中迁移过来的
	gcdata     *byte    // gc 相关
	str        nameOff  //
	ptrToThis  typeOff  //
}
```

## 3. 不能将其他类型的slice赋值给 `[]interface{}`

原文：[InterfaceSlice](https://github.com/golang/go/wiki/InterfaceSlice)

[demo](https://github.com/YYRise/go-wheel/tree/master/interface)

- 原因1： `[]interface{}` 类型并不是 `interface{}`，而是元素类型是 `interface{}` 的切片。
- 原因2： `interface{}` 有两个字段（类型和数据或指针）， 因此，长度为N的 `[]interface{}` 占用 `N*2` 字长的数据块。 而同样长度的 `[]MyType` 占用 `N*sizeof(MyType)` 字长的数据块。

