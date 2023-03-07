# Golang Notes
<h2 align="left">目录</h2>
[toc]

## 简介

Go 语言被设计成一门应用于搭载 Web 服务器，存储集群或类似用途的巨型中央服务器的系统编程语言。对于高性能分布式系统领域而言，Go 语言无疑比大多数其它语言有着更高的开发效率。它提供了海量并行的支持，这对于游戏服务端的开发而言是再好不过了。具有简洁、快速、安全、并行、有趣、开源、内存管理、数组安全、编译迅速等优势。新增Goroutine, channel, defer等；舍弃枚举，泛型等。

## Go基础

### 变量
1. 单变量
    ```go
    //1. 指定变量类型，声明后若不赋值，使用默认值。
    var v_name v_type
    v_name = value
    // or
    var v_name v_type = value
    //2. 根据值自行判定变量类型。
    var v_name = value
    //3. 省略var, 注意 :=左侧的变量不应该是已经声明过的，否则会导致编译错误; := 需要赋值。
    c := 10
    ```

2. 多变量

    ```go
    var a, b, c int
    a, b, c = 5, 7, "abc"
    a, b = b, a // 交换两个变量 a, b的值

    a, b, c := 5, 7, "abc"
    ```
3. 匿名变量

4. 指针变量
    ```go
    var ptr_name *v_type = &v //v的类型是v_type
    var ptr_name = &v
    ptr_name := &v
    ```
    > 类似C++中nullptr表示空指针， nil代表golang中的空指针；类似C/C++, *ptr表示取地址内容，解引用
5. 数组变量


6. 结构体变量


> <font color=Red>Golang 抛弃了枚举变量</font> 

### 常量

### 函数

### 流程控制

## 切片(Slice)
## 范围(Range)
## Map(集合)
## 接口(Interface)

## 反射
## 错误与异常处理

## 并发