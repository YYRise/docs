---
title: "atomic.Value"
date: 2021-09-18T09:50:07+08:00
draft: false
---

[Go 语言标准库中 atomic.Value 的前世今生](https://blog.betacat.io/post/golang-atomic-value-exploration/)
## 1. 写入（`Store`）操作​

![img](/images/Go/atomic-value-store.drawio.png)

## 2. 存入map或slice时注意并发

无论存入map或slice（及其指针），下面代码会发生并发panic：
```go
func mapValue(){
	var atomicMap  atomic.Value
	m := map[string]string{
		"a":"v_a",
	}
	atomicMap.Store(m)	

	go func() {
		for{
			loadM := atomicMap.Load().(map[string]string)
			v := loadM["a"]
			if v != "v_a"{
				fmt.Println(v)
			}
		}
	}()
	
	go func() {
		for{
			loadM := atomicMap.Load().(map[string]string)
			loadM["a"] = fmt.Sprintf("%d", rand.Intn(100))
		}
	}()
}
```
> `Store` 一个全新`map`或`slice`则无并发问题