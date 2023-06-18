# Distribution System Notes

<h2 align="left">目录</h3>
[toc]

MIT 6.824 分布式系统

## Introduction
MIT6.824 课程以分布式基础理论：容错、备份、一致性为脉络，以精选的工业级系统论文为主线，再填充上翔实的阅读材料和精到的课程实验，贯通学术理论和工业实践，实在是一门不可多得的分布式系统佳课。此外在学这门课程的同时也加上自己的理解与总结。

### 课程背景
构建分布式系统的原因：

1. Parallelism，资源并行（提高效率）。
2. Fault tolerance，容错。
3. Physical，系统内在的物理分散。
4. Security，不可信对端（区块链）。

分布式系统面临的挑战：

1. Concurrency，系统构件很多，并行繁杂，交互复杂。
2. Partial failure，存在部分失败，而不是像单机一样要么正常运行，要么完全宕机。
3. Performance，精巧设计才能获取与机器数量线性相关的性能。


### 课程内容
本课程旨在学习支撑应用的基础设施**抽象**（abstraction），包括

1. Storage，存储，一个很直接并常用的抽象；如何构建多副本、容错、高性能分布式存储系统。
2. Communication，通信，如何可靠的通信。
3. Computation，现代的大规模计算，如 MapReduce

最终理想是提供能够屏蔽分布式细节的、类似于单机的通用接口，同时能兼具**容错**和**性能**。

对于上述抽象，我们有哪些实现呢？

1. RPC：像在本机调用一样跨节点通信
2. Concurrency，Threads：并发载体
3. Concurrency，Lock：并发控制。

性能，分片，容错，共识算法的关系如图所示：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/summary.jpg)

#### Performance 性能
scalability，可扩展性
- 可以线性的集结计算机资源：使用两倍的机器获取两倍的吞吐。
- 意味着遇到瓶颈你只需要花少量的钱买机器，而不用付很多的工资找程序员重构。
- 但这个特点很难实现。通常你将一个组件扩展后，瓶颈就转移到了另一个组件，全组件的无限扩展很难。

#### Fault Tolerance 容错
单机虽好，作为上千台机器组成的集群来说，故障却是常态。比如说：

- 主机宕机
- 网络抖动
- 交换机故障

Availability 可用性  
Recoverbility 可恢复性，无干预 、不影响正确性的可恢复

手段：  
- NV storage：持久化  
- Replication：多副本

##### 共识算法(consensus algorithm)

说到容错，就不得不提**共识算法**。共识算法<u>解决的是对某个提案（proposal）大家达成一致意见的过程</u>。提案的含义在分布式系统中十分宽泛，如多个事件发生的顺序、某个键对应的值、谁是领导……等等。可以认为任何可以达成一致的信息都是一个提案。对于分布式系统来讲，各个节点通常都是相同的确定性状态机模型（又称为状态机复制问题，state-machine replication），从相同初始状态开始接收相同顺序的指令，则可以保证相同的结果状态。因此，系统中多个节点最关键的是对多个事件的顺序进行共识，即排序。

根据解决的是非拜占庭的普通错误情况还是拜占庭错误情况，共识算法可以分为Crash Fault Tolerance（CFT）类算法和Byzantine Fault Tolerance（BFT）类算法。
- CFT类共识算法：仅能够容忍宕机、网络延迟/断开等良性错误的共识算法。包括Paxos、Raft及其变种等。
- BFT类共识算法：除了能够容忍上述错误，还能够容忍任意类型的恶意攻击的共识算法

**共识算法的三个主要特性**
1. 共识算法可以保证在任何**非拜占庭**情况下的正确性。
    - 通常来说，共识算法可以解决网络延迟、网络分区、丢包、重复发送、乱序问题，无法解决拜占庭问题（如存储不可靠、消息错误）。
2. 共识算法可以保证在**大多数机器**正常的情况下集群的高可用性，而少部分的机器缓慢不影响整个集群的性能。
3. **不依赖外部时间**来保证日志的一致性。

共识算法一般以库的形式出现，后面会介绍的共识算法包括：Paxos, Raft, ZAB, Quorum, Chain replication

**参考：**
- [分布式系统的一致性与共识性](https://zhuanlan.zhihu.com/p/35596768)
- [共识算法是什么？](https://zhuanlan.zhihu.com/p/478615386)

#### Consistency 一致性

分布式系统产生不一致的因素：
1. 缓存
2. 多副本

不同程度的一致性：
1. 强一致性：每个客户端每次都能读到（自己 or 他人）之前所写数据。在多副本系统实现强一致性代价十分高昂，需要进行大量的通信。简单说两种方法：
    - 每次更改同时写到所有副本
    - 每次读取都去读所有副本，使用具有最新时间戳的数据。
2. 弱一致性，为了性能，工业级系统通常选择弱一致性。

##### 共识性与一致性的区别
**一致性**往往指分布式系统中多个副本**对外呈现的数据的状态**(如 replicated state machine)。如前面提到的顺序一致性、线性一致性，描述了多个节点对数据状态的维护能力。

**共识性**则描述了分布式系统中多个节点之间，彼此对某个状态达成一致结果的过程。

因此，**一致性描述的是结果状态(各副本状态)**，**共识则是一种手段**。**达成某种共识并不意味着就保障了一致性（这里的一致性指强一致性）。只能说共识机制，能够实现某种程度上的一致性。**(consensus algorithm 使得各服务器的 replicated state machine-- operation log 保持一致，是实现一致性的手段；consensus algorithm 也保证了服务器集群的高可用，只有有超过一半的机器正常，机器就能正常运行)

实践中，要保障系统满足不同程度的一致性，核心过程往往需要通过共识算法来达成。


## CAP And Base
### CAP Theorem
CAP 原则指出，一个分布式存储系统提供的服务，不可能同时满足以下三点：
1. **(强)一致性**（_Consistency_）：对于不同节点的请求，要么给出包含最新的修改响应、要么给出一个出错响应（等同于所有节点访问同一份最新的数据副本）。**顺序一致性要求所有机器的数据都是最新的**。
2. **可用性**（_Available_）：对于每个请求都会给出一个非错响应，但是不保证响应数据的时效性。
3. **分区容错性**（_Partition tolerance_）：当系统中节点间出现**网络分区**时，系统仍然能够正常响应请求。

**在分布式系统中，失败必然的，分区容错（P）是一定需要的，因此设计系统时需要在可用性（A）和一致性（C）间进行权衡**。
- **CP**(优先保证一致性): 网络分区使得被请求节点不能够保证数据全局最新时，返回一个出错信息给用户；或者什么也不返回，直到客户端超时。
- **AP**(优先保证可用性): 网络分区使得有些节点不可达，导致系统不能够及时同步数据。尽管被请求节点试图返回其可见范围内的最新数据，但仍不能保证该数据是全局最新的，即牺牲一致性保证可用性。

### Base Theorem
由于不能同时满足 CAP，所以出现 BASE 理论
1. BA: Basically Available， 表示基本可用，表示允许一定程度的不可用，比如由于系统故障，请求时间变长，或者由于系统故障导致部分非核心功能不可用，都是允许的
2. S：Soft state，表示分布式系统可以处于一种中间状态，比如数据正在同步
3. E: Eventually consistent， 表示最终一致性，即不要求分布式系统数据实时达到一致性，允许在经过一段时间后再达到一致，在达到一致过程中，系统也是可用的。

## MapReduce
MapReduce 是谷歌 2004 年（Google 内部是从 03 年写出第一个版本）发表的论文里提出的一个概念。MapReduce是一个用于大规模数据（大于1TB）处理的分布式计算模型、编程模型，它最初是由Google设计并实现的，在Google提出时，给它的定义是：Map/Reduce是一个编程模型（programming model），是一个用于处理和生成大规模数据集（processing and generating large data sets）的相关的实现。

MapReduce的数据处理模型非常简单：map函数和reduce函数的输入和输出都遵循<key,value>键值对的格式，简单的用符号表示就是：
```
map (k1,v1)             -→ list(k2,v2)
reduce (k2,list(v2))    -→ list(v2)
```

拿由谷歌这篇论文提出，后来成为大数据处理界的 _hello world_ 级别示例程序 _Word Count_ （对一堆文档中的单词计数）来说，_map_ 和 _reduce_ 的实现长这样：

```
map(String key, String value):
  // key: document name
  // value: document contents
  for each word w in value:
    EmitIntermediate(w, "1");   // 将文件中所有单词构成集合 W，W中的元素名称可以重复，每个元素的值为1

reduce(String key, Iterator values):
  // key: a word
  // values: a list of counts
  int result = 0;
  for each v in values:
    result += ParseInt(v); // 合并集合 W 中名称相同的元素，并将他们相同名称的元素值累加
  Emit(AsString(result));
```

### MapReduce 原理
MapReduce 论文如下：
[MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/map_reduce.jpg)

MapReduce是 Master/Slave 架构。

MapReduce库将输入的大文件被分成很多块(M个)，每个块16M-64M，分配给不同的机器，每个机器执行由Master管理的Map task(M个)和Reduce task(R个)，这里每个机器可以执行多个Map task或Reduce task。如果Master失败，终止 Map task 的计算。每个 Map task 产生的中间文件，根据键值计算哈希值(hash(key) mod R)，被分成 R份。每个 Reduce task 的输入是 M个 Map task 产生的中间文件对应该 Reduce task ID 的分片集合，即每个 Reduce task 有 M个输入，每个 Reduce task合并这 M 个输入并排序，最后产生一个输出。

如果 Map task 失败，重新计算，因为map产生的临时文件在局部机器的硬盘上，变得不可读取；如果 Reduce task 失败，不需要重新执行，因为虽然Reduce task虽然产生临时文件，但最后输出的临时文件是在全局文件系统(GFS)上的。此外 R个 Reduce task输出的文件不需要合并，通常作为另一个MapReduce的输入。

### MapReduce 实验
MIT6.824 MapReduce Lab(Spring 2022) 实现代码见：https://github.com/CJuncheng/MIT6.824-LAB，``Lab1_MapReduce``分支。主要参考：https://github.com/s09g/mapreduce-go(Spring 2020)。

<h4 align="left">MapReduce数据结构设计</h4>

1 . Task 结构体定义

Task 对应四个状态：
```go
/** state value
 * 0 : map
 * 1 : reduce
 * 2 : wait
 * 3 : exit, nothing to do
 */
const (
	Map TaskState = iota
	Reduce
	Wait
	Exit
)
```
Worker Task 分为 Map task 和 Reduce task, 这两个阶段的结构体可以复用，具体定义如下：
```go
type Task struct {
	State          TaskState
	InFileName     string   // map task 要读取的文件名
	TaskID         int      // map task id 对应文件 id (M个)；reduce task id(R个)
	NReduce        int      // reduce task 的数量
	LocalFileNames []string // 对于 map task，是一个 map task 产生的 R个文件名集合; 对
	/* 对于 reduce task，是多个Map task 产生的中间文件对应该 Reduce task ID 的分片集合，
            该reduce task 将 M 个文件片合并，排序，输出一个文件*/
	OutFileName string // 每个reduce task 的输出文件名
}
```


2 . Coordinator (就是 Master)结构体定义
```go
type Coordinator struct {
	// Your definitions here.
	TaskQue       chan *Task               //等待执行的task
	TaskMeta      map[int]*CoordinatorTask // Coordinator为每个task 维护一个状态信息， int 代表 task ID
	State         TaskState                // Coordinator 阶段 对应的task任务状态
	NReduce       int
	InFiles       []string   // M个文件对应 M 个 Map task
	Intermediates [][]string // [i][j]: i 表示 reduce id([0, R)), j 代表 M个 map task 的索引([0, M))。Intermediates[i] 表示 reduce task i 对应的 M个输入文件片集合
}
```


<h4 align="left">MapReduce执行过程的实现</h4>

**MakeCoordinator阶段**

1 . 启动 MakeCoordinator

```go
func MakeCoordinator(files []string, nReduce int) *Coordinator {
  c := Coordinator{
    TaskQue:       make(chan *Task, max(nReduce, len(files))),
    TaskMeta:      make(map[int]*CoordinatorTask),
    State:         Map,
    NReduce:       nReduce,
    InFiles:       files,
    Intermediates: make([][]string, nReduce),
    }
  c.createMapTask() // 为每个文件创建一个  map task
  c.server()

  // 启动一个goroutine 检查超时的任务
  go c.checkTimeOut()
  return &c
}

func (c *Coordinator) createMapTask() {
  // 根据输入文件，每个文件是一个 map task (split)
  for idx, filename := range c.InFiles {
    task := Task{
      State:      Map,
      InFileName: filename,
      TaskID:     idx, // 为每个文件 split 分配一个 task
      NReduce:    c.NReduce,
    }
    c.TaskQue <- &task
    c.TaskMeta[idx] = &CoordinatorTask{
      TaskStatus:    Idle,
      TaskReference: &task,
    }
  }
}

// start a thread that listens for RPCs from worker.go
func (c *Coordinator) server() {
  rpc.Register(c)
  rpc.HandleHTTP()
  //l, e := net.Listen("tcp", ":1234")
  sockname := coordinatorSock()
  os.Remove(sockname)
  l, e := net.Listen("unix", sockname)
  if e != nil {
    log.Fatal("listen error:", e)
  }
  go http.Serve(l, nil)
}
```
  - 为每个文件 split 分配一个 map task(M)，并将这M个文件放入队列
  - 开启一个线程监听来自 worker 的 RPC请求
  - 开启一个协程进行超时检查

**Worker阶段**

2 . 启动一个 worker(死循环)
```go
func Worker(mapf func(string, string) []KeyValue,
  reducef func(string, []string) string) {

  // Your worker implementation here.
  for {
    task := callTask() // 调用（请求）任务（RPC调用）
    switch task.State {
    case Map:
      mapTask(mapf, task)
      break
    case Reduce:
      reduceTask(reducef, task)
      break
    case Wait:
      time.Sleep(time.Duration(time.Second * 10))
      break
    case Exit:
      fmt.Printf("All of tasks have been completed, nothing to do\n")
      return
    default:
      fmt.Printf("Invalid state code, try again!\n")
    }
  }
}
```   

3 . worker RPC调用
```go
// worker.go
func callTask() *Task {

  args := ExampleArgs{}
  reply := Task{}
  ok := call("Coordinator.AssignTask", &args, &reply)
  return &reply
  }
  func call(rpcname string, args interface{}, reply interface{}) bool {
  sockname := coordinatorSock()
  c, err := rpc.DialHTTP("unix", sockname)
  if err != nil {
    log.Fatal("dialing:", err)
  }
  defer c.Close()

  err = c.Call(rpcname, args, reply)
  if err == nil {
    return true
  }

  fmt.Println(err)
  return false
}

// coordinator.go

// coordinator等待worker调用
func (c *Coordinator) AssignTask(args *ExampleArgs, reply *Task) error {
  mu.Lock()
  defer mu.Unlock()
  if len(c.TaskQue) > 0 {
    *reply = *<-c.TaskQue
    // 记录 task 的启动时间
    c.TaskMeta[reply.TaskID].TaskStatus = InProcess
    c.TaskMeta[reply.TaskID].StartTime = time.Now()
  } else if c.State == Exit {
    *reply = Task{State: Exit}
  } else {
    *reply = Task{State: Wait}
  }
  return nil
}
```
- 通过rpc请求调用，从MakeCoordinator队列取出一个 Task，返回 Task 信息；根据Task状态，执行不同的任务

4 . 进入 mapTask 阶段
```go
func mapTask(mapf func(string, string) []KeyValue, task *Task) {
   intermediates := []KeyValue{}

   filename := task.InFileName
   file, err := os.Open(filename)
   if err != nil {
     log.Fatalf("cannot open %v", filename)
   }
   content, err := ioutil.ReadAll(file)
   if err != nil {
     log.Fatalf("cannot read %v", filename)
   }
   file.Close()

   kva := mapf(filename, string(content))
   intermediates = append(intermediates, kva...)

   nReduce := task.NReduce

   var mapBuff map[int]ByKey
   mapBuff = make(map[int]ByKey, nReduce)

   // 切片， R份
   for _, kv := range intermediates {
     idx := ihash(kv.Key) % nReduce
     mapBuff[idx] = append(mapBuff[idx], kv)
   }

   var localFileNames []string // 每个 Map Task 产生的 R 个中间文件名称集合

   for i := 0; i < task.NReduce; i++ {
     localFileName := writeToLocalFile(task.TaskID, i, mapBuff[i])
     localFileNames = append(localFileNames, localFileName)
   }

   task.LocalFileNames = localFileNames
   taskCompleted(task)
}
```
- 所有 map 任务结束后才创建 reduce 任务
  
5 . 进入 reduceTask 阶段
```go
func reduceTask(reducef func(string, []string) string, task *Task) {
  //fmt.Printf("This is reduce task\n")
  intermediate := readFromLocalFile(task.LocalFileNames) // 合并 reduce task 的输入文件片集合
  sort.Sort(ByKey(intermediate))

  currentDir, _ := os.Getwd()
  //currentDir += "/out"
  tmpFile, err := ioutil.TempFile(currentDir, "mr-tmp-r-*")
  if err != nil {
    log.Fatal("Fail to creat temp file", err)
  }

  i := 0
  for i < len(intermediate) {
    j := i + 1
    for j < len(intermediate) && intermediate[j].Key == intermediate[i].Key {
      j++
    }
    values := []string{}
    for k := i; k < j; k++ {
      values = append(values, intermediate[k].Value)
    }
    output := reducef(intermediate[i].Key, values)

    // this is the correct format for each line of Reduce output.
    fmt.Fprintf(tmpFile, "%v %v\n", intermediate[i].Key, output)
    i = j
  }
  tmpFile.Close()

  oname := fmt.Sprintf("mr-out-%d", task.TaskID)
  os.Rename(tmpFile.Name(), oname)
  task.OutFileName = oname
  taskCompleted(task)
}
```

**参考：**
- [MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)
- [MapReduce —— 历久而弥新](https://www.qtmuniao.com/2019/04/30/map-reduce/)
- [MapReduce简介与实例](https://www.cnblogs.com/gzshan/p/11159033.html)


## RPC and Threads
### RPC

RPC 微服务框架

[分布式计算](https://zh.wikipedia.org/wiki/%E5%88%86%E5%B8%83%E5%BC%8F%E8%AE%A1%E7%AE%97 "分布式计算")中，**远程过程调用**（英语：**R**emote **P**rocedure **C**all，**RPC**）是一个计算机通信[协议](https://zh.wikipedia.org/wiki/%E7%B6%B2%E7%B5%A1%E5%82%B3%E8%BC%B8%E5%8D%94%E8%AD%B0 "网络传输协议")。该协议允许运行于一台计算机的[程序](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F "程序")调用另一个[地址空间](https://zh.wikipedia.org/wiki/%E5%9C%B0%E5%9D%80%E7%A9%BA%E9%97%B4 "地址空间")（通常为一个开放网络的一台计算机）的[子程序](https://zh.wikipedia.org/wiki/%E5%AD%90%E7%A8%8B%E5%BA%8F "子程序")，而程序员就像调用本地程序一样，无需额外地为这个交互作用编程（无需关注细节）。RPC是一种服务器-客户端（Client/Server）模式，经典实现是一个通过**发送请求-接受回应**进行信息交互的系统。

如果涉及的软件采用[面向对象编程](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B "面向对象编程")，那么远程过程调用亦可称作**远程调用**或**远程方法调用**，例：[Java RMI](https://zh.wikipedia.org/wiki/Java_RMI "Java RMI")。

RPC是一种[进程间通信](https://zh.wikipedia.org/wiki/%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1 "进程间通信")的模式，程序分布在不同的[地址空间](https://zh.wikipedia.org/wiki/%E5%9C%B0%E5%9D%80%E7%A9%BA%E9%97%B4 "地址空间")里。如果在同一主机里，RPC可以通过不同的虚拟地址空间（即便使用相同的物理地址）进行通讯，而在不同的主机间，则通过不同的物理地址进行交互。许多技术（通常是不兼容）都是基于这种概念而实现的。

#### RPC 基本原理
RPC提供调用控制，消息编解码(序列化和反序列化)，通信传输。有的调用涉及远程接口的代理(proxy)实现。

<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/rpc.png" width = "70%" height = "70%">
</div>

**The following steps take place during a RPC**: 

1. A client invokes a **client stub procedure**, passing parameters in the usual way. The client stub resides within the client’s own address space. 
2. The client stub **marshalls(pack)** the parameters into a message. Marshalling includes converting the representation of the parameters into a standard format, and copying each parameter into the message. 
3. The client stub passes the message to the transport layer, which sends it to the remote server machine. 
4. On the server, the transport layer passes the message to a server stub, which **demarshalls(unpack)** the parameters and calls the desired server routine using the regular procedure call mechanism. 
5. When the server procedure completes, it returns to the server stub **(e.g., via a normal procedure call return)**, which marshalls the return values into a message. The server stub then hands the message to the transport layer. 
6. The transport layer sends the result message back to the client transport layer, which hands the message back to the client stub. 
7. The client stub demarshalls the return parameters and execution returns to the caller.

**RPC的目标就是把 stub 和 RPC Runtime 步骤都封装起来，让用户对这些细节透明**:
1. **Stub(存根)**: 
The function of the stub is to **provide transparency to the programmer-written application code**. 

   - **On the client side**, the stub handles the interface between the client’s local procedure call and the run-time system, **proxy**, **marshalling and unmarshalling data**(序列化/反序列化), **invoking the RPC run-time protocol**, and if requested, carrying out some of the **binding steps**. 
   - **On the server side**, the stub provides a similar interface between the run-time system and the local manager procedures that are executed by the server.
  
1. **RPC Runtime**: 
RPC run-time system is a library of routines and a set of services that handle the network communications that underlie the RPC mechanism. In the course of an RPC call, client-side and server-side run-time systems’ code handle **binding, establish communications over an appropriate protocol, pass call data between the client and server, and handle communications errors.**


下面看看涉及到的具体内容（从客户端到服务端单向过程分析）
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/rpc2_.png)

- 调用方式：同步调用 or 异步调用 
- IDL(Interface Define Language): Stub 暴露给用户的是调用接口，业务逻辑代码
  - 接口：函数集合，供用户调用
  - 数据类型
- 动态代理：屏蔽远程接口调用的细节
- 序列化和反序列化: 序列化就是将对象转换成二进制数据的过程，而反序列就是反过来将二进制转换为对象的过程。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/rpc_serialize.png)

- Runtinme: 实现远程调用的工具包(library)。
  - 传输协议：选择什么样的二进制传输协议 

#### RPC运行模式

- **静态模式**： 预编译接口， 生成存根代码stub，与应用代码编译。C++,JAVA, OBJc ...
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/rpc_runmodel.png)
- **动态模式** ： 依赖语言特性实现动态调用（反射、内省） Python, Java,Php, GO...
  - 相当于上图只保留 ``IDL`` 和 ``User Code``

#### GO-RPC
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/rpc_go.png" width = "50%" height = "50%">
</div>

- GOB 只能go->go（主要讲）
- JSON 可以 GO->JAVA

**GOB**
头文件：encoding/gob
binary values exchanged between an Encoder (transmitter) and a Decoder (receiver) 

**HTTP-RPC**
net/rpc/Client
net/rpc/Server



**参考：**
- [远程过程调用](https://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)
- [Remote Procedure Call (RPC) in Operating System](https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/)


### Threads
**Q&A**
1. _不使用线程还能如何处理并发？_
**基于事件驱动的异步编程**。但是多线程模型更容易理解一些，毕竟每个线程内执行顺序和你的代码顺序是大体一致的。
2. _Go 是否知道锁和资源（一些共享的变量）间的映射_？
Go 并不知道，它仅仅就是等待锁、获取锁、释放锁。需要程序员在脑中、逻辑上来自己维护。
3. _Go 会锁上一个 Object 的所有变量还是部分_？
和上个问题一样，Go 不知道任何锁与变量之间的关系。Lock 本身的源语很简单，goroutine0 调用 mu.Lock 时，没有其他 goroutine 持有锁，则 goroutine0 获取锁；如果其他 goroutine 持有锁，则一直等待直到其释放锁；当然，在某些语言，如 Java 里，会将对象或者实例等与锁绑定，以指明锁的作用域。
4. _Lock 应该是某个对象的私有变量_？
如果可以的话，最好这样做。但如果由跨对象的加锁需求，就需要拿出来了，但要注意避免死锁。

**线程协调（Coordination）**
1. channels：go 中比较推荐的方式，分阻塞和带缓冲。
2. sync.Cond：信号机制。
3. waitGroup：阻塞直到一组 goroutine 执行完毕，后面还会提到。

## GFS, HDFS, Ceph and Azure 

### GFS
分布式文件系统设计是困难的：
1. 性能（High Performance）–> 分片（sharding）
    分布式系统，自然想利用大量的机器提供成比例的性能，于是通常将数据分散到不同的机器上，以并行读取。我们称之为：分片（Sharding）。但分片一多，故障率就上来了。  
2. 故障（Faults）—> 容错（tolerance） 
    故障多了，就需要进行自动容错。最简单直接、通常也最有效的容错方法就是：备份（Replication，或译为冗余、副本）。如果副本是可修改的，就需要定期同步，这就引出了一致性的问题。
3. 副本（Replication）—> 一致性（Consistency）
    当然，通过精心的设计，可以维持系统的一致性，但这就意味着你需要损失性能。
4. 一致性（Consistency）—> 低性能（Low Performance）

文件系统的演变过程如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/gfs_fs_change.png)

GFS的特点：
1. 体量大，速度快（Big，Fast）：海量数据的快速存取
2. 全球部署（Global）：不同 site 的数据访问和共享
3. 分片（Sharding）：多客户端并发访问，增大吞吐
4. 自动恢复（Auto recovery）：机器太多，自动化运维

#### GFS整体架构
GFS中有三种节点：GFS client，GFS master，GFS chunkserver。
- GFS client：维持专用**接口**，与应用交互。
- GFS master：维持**元数据**，统一管理chunk位置与租约。
- GFS chunkserver：**存储数据**。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/gfs_architecture.png)

#### GFS的存储设计

考虑到文件可能非常大，并且大小不均。GFS没有选择直接以文件为单位进行存储，而是把文件分为一个个**chunk**来存储。GFS把每个chunk设为**64MB**。

相对而言，64MB这个值是偏大的。为什么GFS会这么设计呢?

优点：
1. 使用GFS的系统需要存储的**文件都偏大**（几GB），所以较大的chunk可以有效**减少系统内部的寻址和交互次数**；
2. 大的chunk意味着client可能**在一个chunk上执行多次操作**，这可以**复用TCP连接，节省网络开销**
3. 更大的chunk可以**减少chunk数量**，从而节省元数据存储开销，相当于**节省了系统内最珍贵的内存资源**，这对GFS来讲是非常关键的。

缺点：
1. 更大的chunk意味着多个线程同时操作一个chunk的可能性增加，容易产生**热点问题**，对这个问题，GFS在一致性设计方面做出了对于性能的妥协。

A1. 文件怎样分散存储在多台服务器上？
**<font color=Red>分割存储</font>**
B1. 怎样支持大文件（几个GB）存储？
**<font color=Red>采用了更大的chunk，以及配套的一致性策略</font>**

chunk在chunkserver中的分布如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/chunk_distribution.jpg)
每个文件有三个副本，文件对应的三个chunk是随机分布的，chunkserver 只关注 chunk,不关注 chunk属于哪个文件。
无论多大的文件，在底层的存储层面都可以拆分成一样的chunk。通过chunk的调度保证 chunkserver 的负载均衡。实现存储与计算的解耦

#### GFS的master设计

管理元数据的节点设计成一个**单点**还是设计成**多节点**(分布式)？比较如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/master_number.jpg)
GF最终选择了**单master节点**的方案，用来管理三类元数据:

1. 所有文件和chunk的**namespace**【持久化】(文件名和chunk编号)
2. **文件到chunk的映射**【持久化】
3. **每个chunk的位置** 【不持久化】: 
    - 为什么不做持久化？因为master重启时候，可以从各个chunkserver处收集chunk的位置信息，master并不经常重启，这样性能更高。
> 通过 operation log and checkpoint 将前两项持久化到磁盘上
> 每个chunk 对应版本号、primary与否、lease(租约)等信息

由此，我们就可以简单推断出GFS读取一个文件的过程了：<u>文件名→获取文件对应的所有chunk名→获取所有chunk的位置→依次到对应的chunkserver中读取chunk</u>

**GFS采用了一系列措施来确保 master不会成为整个系统的瓶颈**：
1. GFS**把控制流和数据流分离**。GFS所有的数据流都不经过master，而是直接由client和chunkserver交互；只有控制流才会经过master
2. GFS的client会**缓存**master中的元数据，在大部分情况下，都无需访问master；
3. 为了避免master的**内存**成为系统的瓶颈，GFS采用了一些手段来节省master的内存，包括**增大chunk的大小以节省chunk的数量**、对元数据进行定制化的压缩等。
    - 一个64MB的chunk，它的元数据小于64B（64字节），**存储1PB的数据也只有3GB的元数据**


A1. 怎样实现自动扩缩容？
**<font color=Red>在master单点上增减、调整chunk的元数据即可</font>**
A2. 怎样知道一个文件存储在哪台机器上？
**<font color=Red>根据master中文件到chunk再到chunk位置的映射来定位到具体的chunkserver</font>**

#### GFS的高可用设计
高可用问题现在比较通用的做法是**共识算法**，比如Paxos和Raft。GFS诞生的时候，共识算法不像现在这么成熟，所以GFS借鉴了主备的思想，为系统的**元数据**(master高可用)和**文件数据**都单独设计了高可用方案。

**master的高可用设计**
master的三类元数据中，**namespace和文件与chunk的对应关系**需要持久化存储，也需要保证高可用。GFS在正在使用的master称为 **primary master**。在primary master之外，GFS还维持一个 **shadow master** 作为备份。Master在正常运行时，对元数据做的所有修改操作，都要先**记录日志**（write ahead log, WAL），再真正去修改内存中的元数据。primary master会实时向shadow master同步WAL，只有shadow master同步日志完成，元数据修改操作才算成功。
- 生成新增元数据的日志并写入本地磁盘 →
- 把WAL传输给shadow master →
- 得到反馈后再正式修改primary master的内存。

GFS的机器总数可能非常的多，从而个别机器宕机的发生会十分频繁。面对这种小问题, GFS如何切换设备？
- 如果master宕机，会通过Google的**Chubby**(本质上是共识算法)来识别并切换到shadowmaster，这个切换是**秒级**的
- master的高可用机制就和**MySQL的主备机制**非常像。

**chunk的高可用设计**
每个chunk都有**三个副本**, master维持chunk的副本信息。

GFS保证chunk高可用性的思路：
- 在GFS中，对一个chunk的每次写入，必须确保在**三个副本中的写入都完成**，才视为写入完成
- 一个chunk的所有副本都会具有完整的数据
- 如果一个chunkserver宕机，它上面的所有chunk都有另外两个副本依旧可以保存这个chunk的数据
- 如果这个宕机的副本在一段时间之后还没有恢复，那么**master**就可以在另一个chunkserver重建一个副本，从而始终把chunk的副本数目维持在**3个**（可以设置）。
- GFS维持每个chunk的**校验和**，读取时可以通过校验和进行数据的校验。如果校验和不匹配，chunkserver会反馈给master处理，master会选择其他副本进行读取，并重建此chunk副本。
- 为了减少对master的压力，GFS采用了一种**租约（Lease）机制**，把文件的读写权限下放给某一个chunk副本。Master可以把租约授权给某个**chunk副本**，我们把这个chunk副本称为**primary**，在租约生效的一段时间内，对这个chunk的写操作直接由这个副本负责，租约的有效期一般为60秒。**租约的主备只决定控制流走向，不影响数据流**。

Chunk副本的放置也是一个关键问题，GFS中有三种情况需要master发起创建chunk副本：**新chunk创建、chunk副本复制（re-replication）和负载均衡（rebalancing）**
- chunk副本复制：如一个副本所在的chunkserver宕机，导致chunk副本数小于预期值（一般为3）后，新增一个chunk副本
- 负载均衡：发生在master定期对chunkserver的监测，如果发现某个chunkserver的负载过高，就会执行负载均衡操作，把chunk副本搬到另外的chunkserver上。当然，这里的“搬迁”操作，实际上就是<u>新建chunk和删除原chunk的操作</u>。

这三个副本操作中，master对副本位置的选择策略是相同的，要遵循以下三点：
- 新副本所在的chunkserver的**资源利用率较低**；
- 新副本所在的chunkserver**最近**创建的chunk副本不多。这里是为了防止某个chunkserver瞬间增加大量副本，成为**热点**；
- chunk的**其他副本不能在同一机架**。这里是为了保证机架或机房级别的高可用。

A3. 怎样保证服务器在故障时文件不损坏不丢失？
**<font color=Red>master的WAL和主备、chunk的多副本</font>**
B2. 超多机器的情况下，怎样实现自动监控、容错与恢复？
**<font color=Red>master的主备切换由chubby负责，chunk的租约、副本位置与数量由master负责</font>**


#### GFS的读写流程
GFS系统的**读写流程**要保证**一致性机制**
- **读取**： **快速**，为了极致的性能，可以读到落后的版本，但一定不能是错误的
- **写入**：进一步分为两种：**改写（overwrite）和追加（append）**
  - **改写**: 要求**正确**，<u>通常不用在意性能。在意性能的改写可以转为追加</u>。 
  - **追加**: 要求**快速**，<u>为了极致的性能，可以允许一定的异常，但追加的数据一定不能丢失</u>(**原子操作**)。


**GFS的写入流程**

当写入，修改内存同时在磁盘上记操作日志（operation log）+ 快照（CheckPoint）。

GFS的写入要在三个副本都完成写入后才能返回写入结果。GFS的写入采用了两个在现在看来都非常高端的技术：
- **流水线技术**：<u>client会把文件数据发往离自己最近的一个副本</u>，无论它是否是主（是否持有租约）。这个副本在接收到数据后，就立刻向其他副本转发（一边接收，一边转发）。这样就控制了数据的流向，节省了网络传输代价
  - 与流水线技术对应的是普通的**主备同步**，数据是从Client到主，再从主到备这样单向流动。 
- **数据流与控制流分离技术**: 意味着GFS对一致性的保证可以不受数据同步的干扰。<u>数据流传输使用流水线技术(chunkserver不分主次)， 控制流由持有租约的chunkserver自己决定，来单独负责写入的一致性保证</u>。从而达到性能和一致性的均衡。
  
GFS的写入流程具体如下：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/write_control_and_data_flow.png" width = "65%" height = "65%">
</div>

1. 
2. Client向Master询问要写入chunk的租约在哪个chunkserver上(**Primary Replica**)，以及其他副本(**Secondary Replicas**)的位置（通常Client中直接就有缓存）
3. Client将数据推送到所有的副本上，这一步就会用到流水线技术，也是写入过程中唯一的数据流操作。
4. 确认所有副本都收到了数据之后，Client发送正式写入的请求到Primary Replica。Primary Replica接收到这个请求后，会对这个Chunk上所有的操作排序，然后按照顺序执行写入。**Primary Replica唯一确定写入顺序，保证副本一致性。**
5. Primary Replica把Chunk写入的顺序同步给SecondaryReplica(此时，Primary Replica上写入已经成功了)。
6. 所有的Secondary Replica返回Primary Replica写入完成。
7. Primary Replica返回写入结果给Client。
    - 所有副本都写入成功：Client确认写入完成
    - 部分Secondary Replica写入**失败**（没有响应）：Client认为写入失败，并从第3步开始重新执行。
> 如果一个写入操作涉及到多个Chunk，Client会把它们分为**多个写入**来执行。

GFS的**改写**和**追加**
- **改写**: 改写可以完全适配上面描述的写入的步骤，重复执行改写也不会产生副本之间的不一致。改写涉及到多个chunk，如果保证强一致性代价很大
- **追加**：GFS推荐使用追加的方式写入文件


**GFS的读取流程**

1. client 收到读取一个文件的请求后，查看自身缓存中有没有文件相关的元数据。如果没有，则请求master(或 show master)获取元数据的信息并缓存
2. client计算文件偏移量对应的chunk
3. 然后**clien向离自身最近的chunkserver发送读请求**。如果发现这个chunkserver没有自己所需的chunk，说明缓存失效，再请求master获取新的元数据
4. client 读取时会进行chunk校验和的确认。如果校验和验证不通过，选择其他副本进行读取
5. client 返回读取的结果

B3. 怎样支持快速的顺序读和追加写？
**<font color=Red>总体上GFS是三写一读的模式。写入采用了流水线技术和数据流与控制流分离技术保证性能；追加对一致性的保证更简单，也更加高效，所以写入多采用追加的形式。读取则所有副本都可读，在就近读取的情况下性能非常高。</font>**

#### GFS的一致性模型
读写流程的时候，已经明确了维持副本间一致的基本方法。但在实际应用中，因为有多个Client，我们的写入往往是并发执行的，这会带来副本间不一致的风险。

GFS把文件数据的一致性大体上分为三个层次：inconsistent，consistent，defined。
- **consistent**：**一致的**。表示文件无论从哪个副本读取，读到的结果都一样。
- **defined**：已定义的。defined就包含了consistent, 文件发生了修改操作后，读取是一致的，且Client可以看到mutation实际进行了什么操作写了什么数据。
- **inconsistent**：**不一致的**
  
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/gfs_mutation.jpg" width = "75%" height = "75%">
</div>

- 无论是 write 还是 record append 只要 **写入失败**， 对应的都是**inconsistent**
- **串行改写成功：defined**。因为所有副本都完成改写后才能返回成功，并且重复执行改写也不会产生副本间不一致，所以串行改写成功数据是defined。
- **并发改写成功：consistent but undefined**。对于单个改写操作而言，成功就意味着副本间是一致的。但并发改写操作可能会涉及多个chunk，不同chunk对改写的执行顺序不一定相同，而这有可能造成应用读取不到预期的结果。
- **追加写成功：defined interspersed with inconsistent**（已定义但有可能存在副本间不一致）。这里允许一定异常（**弱一致性**）

GFS为了实现追加（主要操作）的一致性特性，对追加做了一些额外的限制：
- **单次append的大小不超过64MB**。
- 如果文件最后一个chunk的大小不足以提供此次追加所需空间，则把此空间**padding**填满，然后新增chunk进行append。
- 每次append都会限制在一个chunk上，从而可以保证追加操作的**原子性**，在并发执行时也可以保证Client读取符合最新追加的结果。
- **重复追加**的问题相对来讲很好解决。

A4. 使用多副本的话，怎样保证副本之间的一致？
**<font color=Red>GFS通过以下四种方式保证一致性</font>**
1. 对一个chunk所有副本的写入顺序都是一致的。这是由控制流和数据流分离技术实现的，控制流都是由primary发出，而副本的写入顺序也是由primary到secondary。
2. 使用**chunk版本号**来检测chunk副本是否出现过宕机。失效的副本不会再进行写入操作，master不会再记录这个副本的信息（等Client刷缓存时同步），GC程序会自动回收这些副本。
3. master会定期检查chunk副本的checksum来确认其是否正确。
4. GFS推荐应用更多地使用追加来达到更高的一致性。


#### GFS的快照机制和垃圾回收机制

**GFS的快照(Snapshot)机制**
需要快照的场景：保存文件的一个瞬时状态为另一个文件，用于备份或回滚。
快照的机制：**COW**（Copy On Write，写时复制）
1. Master回收对应chunk的租约，停止对应chunk的所有写入【停止文件fileA的写入】
2. 拷贝一份文件的元数据并命名为快照文件（快照文件的元数据仍指向原文件的chunk）【拷贝文件fileA的元数据，生成fileA_backup元数据】
3. 增加文件所有chunk的引用计数【本来fileA的所有chunk引用计数都为1，只有fileA引用，现在变为2，fileA和fileA_backup都引用】
4. Master正常授权租约，允许对chunk进行写入【开启文件fileA的写入】
5. 下次对文件进行修改时，发现其chunk的引用计数大于1，修改时会先拷贝一个新chunk，再在新chunk上写入。【fileA的三个chunk A B C，chunk C有写入，那么写入时拷贝chunk C为$C^{'}$，并写入$C^{'}$。此时fileA指向A B $C^{'}$，fileA_backup指向A B C】

**GFS的垃圾回收(GC)机制**
需要GC的场景：
- 客户端**直接删除文件**
- 因**丢失修改操作**而失效的副本
- 因**checksum校验失败**而失效的副本

GC的机制：
- ①不立刻清除chunk的物理存储，而是修改文件的元数据，把文件名改为一个包含删除时间戳的、隐藏的名字。Master会定期对namespace元数据进行扫描，当发现文件删除超过3天（可配置），就会把这个元数据删除掉。对应的chunk会走②③一样的流程删除。
- ②③Master定期扫描各Chunkserver汇报的chunk集合，当发现没有对应文件元数据的chunk（不被任何文件包含的chunk）时，Chunkserver就可以把这些chunk真正删除掉了。

**参考：**
- https://www.bilibili.com/video/BV1fT411c7y6/?spm_id_from=333.337.search-card.all.click


### Ceph
Ceph是美国加州大学Santa Cruz分校的Sage Weil 专为博士论文设计的分布式存储系统，其设计目标是**高性能、高可用性及高可扩展性**。成果与2004年发表，并随后贡献给开源社区。Ceph是目前应用最广泛的分布式存储系统，已得到众多厂商的支持。许多**超融合系统**的分布式存储都是基于Ceph深度定制的。

**Ceph的主要特点：**

- 高性能  
    a. 摒弃了传统的集中式存储元数据寻址的方案，采用**CRUSH算法**，数据分布均衡，并行度高。  
    b.考虑了容灾域的隔离，能够实现各类负载的副本放置规则，例如跨机房、机架感知等。  
    c. 能够支持上千个存储节点的规模，支持TB到PB级的数据。
- 高可用性  
    a. 副本数可以灵活控制。  
    b. 支持故障域分隔，在数据一致性方面，Ceph实现跨集群**强一致性**，可以获得传统集中式存储的使用体验。
    c. 多种故障场景自动进行修复自愈。  
    d. 没有单点故障，自动管理。
- 高可扩展性  
    a. **去中心化**（但对技术要求高）。  
    b. 扩展灵活（但会导致整个存储系统性能下降）。  
    c. 随着节点增加而线性增长。
- 特性丰富  
    a. 支持三种存储接口：块存储、文件存储、对象存储。  
    b. 支持自定义接口，支持多种语言驱动。


#### Ceph 架构设计与组件

**Ceph 架构设计**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/ceph_architecture.png)

- **RGW(RADOSGW)**: 全称 RADOS gateway, 是 Ceph 对外提供的**对象存储**服务接口，接口与 S3 和 Swift 兼容。
- **RBD**: 全称 RADOS block device，是 Ceph 对外提供的**块设备存储**服务接口。将ceph提供的空间，模拟成一个个独立的设备，当ceph环境部署完后，服务端就准备好rbg接口。
- **CephFS**： CephFS 全称 Ceph File System，是 Ceph 对外提供的**文件存储**服务。
- **Librados**：Librados 是 Rados 提供库，因为 RADOS 是协议很难直接访问，因此上层的 RBD、RGW 和 CephFS 都是通过 librados 访问的，目前提供 PHP、Ruby、Java、Python、C和C++支持。
- **RADOS**：RADOS 全称 Reliable Autonomic Distributed Object Store，是 Ceph **集群**的精华，用户实现数据分配、Failover 等集群操作。Ceph提供了一个基于RADOS的可无限扩展的Ceph存储集群( Ceph Storage Cluster)。
  
三种存储接口介绍：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/ceph_architecture2.png)

**Ceph 组件(守护进程，Daemons)**

一个 **<font color=Red>Ceph 存储集群(Ceph Storage Cluster)</font>**至少需要一个 **Ceph Monitor, Ceph Manager 和 Ceph OSDs**(OBJECT STORAGE DAEMON 对象存储守护进程)。此外**如果运行 Ceph 文件系统的客户端还需要配置 MDSs**(Ceph元数据服务器)
- **Monitors**: Ceph Monitor (守护进程ceph-mon) **负责监控整个集群，维护集群的健康状态，维护展示集群状态的各种图表，如OSD map, Monitor Map, PG map 以及 Crush map**。监视器还负责管理守护进程和客户端之间的身份验证。通常至少需要三个监视器才能实现冗余和高可用性。基于 paxos 协议实现节点间的信息同步。
- **OSDs**：Ceph OSD (Object Storage Daemon, 对象存储守护进程ceph-osd) **存储数据，处理数据复制、恢复、重新平衡**，并通过检查其他 Ceph OSD 守护进程的**心跳**来向 Ceph 监视器和管理器提供一些监控信息。通常至少需要 3 个 Ceph OSD 来实现冗余和高可用性。**一般情况下一块硬盘对应一个OSD**。
- **Managers**：Ceph 管理器 (守护进程ceph-mgr) **负责跟踪运行时指标和 Ceph 集群的当前状态，包括存储利用率、当前性能指标和系统负载**。Ceph 管理器守护进程还托管基于 Python 的模块来管理和公开 Ceph 集群信息，包括基于 Web 的 Ceph 仪表板和 REST API。高可用性通常至少需要两个管理器。基于 raft 协议实现节点间的信息同步。(<font color=Red>ceph-mgr是从monitr分离出的一项功能</font>)
- **MDSs**：Ceph 元数据服务器（MDS[Metadata Server]、ceph-mds）**代表 Ceph 文件系统存储元数据以及管理目录结构**。 Ceph 元数据服务器允许 POSIX (为应用程序提供的接口标准) 文件系统用户执行基本命令（如 Is、 find 等)，而不会给 Ceph 存储集群带来巨大负担。

#### Ceph 资源划分与写入流程

Ceph采用 crush 算法，在大规模集群下，实现数据的快速、准确存放，同时能够在硬件故障或扩展硬件设备时，做到尽可能小的数据迁移，其原理如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/ceph_pg.png)
> 这里虚线方框内的三个OSDs代表一个节点，每个节点对应一个IP地址。

1. 当用户要将数据存储到Ceph集群时，数据先被分割成多个object，(每个object一个object id，大小可设置，默认是4MB），object是Ceph存储的最小存储单元，包含**元数据和原始数据**。object id获取方式如下：
    - ino (File 的元数据，File 的唯一 id)
    - ono (File 切分产生的某个 object 的序号)
    - oid(object id: ino + ono)
2. Object 是 RADOS 需要的对象。由于object的数量很多，为了有效减少了Object到OSD的索引表、降低元数据的复杂度，使得写入和读取更加灵活，引入了**PG(Placement Group )：PG用来管理object，一个pg可以包含多个object**。Ceph 指定一个**静态 hash 函数**计算 oid 的值，将 oid 映射成一个近似均匀分布的伪随机值，然后和 mask 按位相与，得到 pgid，映射到某个PG中。
    - hash(oid) & mask-> pgid
    - mask = PG 总数 m (m 为 2 的整数幂)-1
    > PG 全称 Placement Grouops，是一个逻辑的概念，一个 PG 包含多个 OSD。引入 PG 这一层其实是为了更好的分配数据和定位数据。
3.  Pg再通过**CRUSH算法**计算，映射到osd中。如果是三副本的，则每个pg都会映射到三个osd，保证了数据的冗余。 
    - CRUSH(pgid)->(osd1,osd2,osd3) 。

Ceph 写入流程概括如下：
1. 数据通过负载均衡获得节点动态IP地址；
2. 通过块、文件、对象协议将文件传输到节点上；
3. 数据被分割成4M对象并取得对象ID；
4. 对象ID通过HASH算法被分配到不同的PG；
5. 不同的PG通过CRUSH算法被分配到不同的OSD

#### Ceph 网络模型
Ceph生产环境一般分为两个阶段
1. 公共网络：用于用户的数据通信
2. 集群网络：用于集群内部的管理通信
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/ceph_network.png)


####  Ceph 通信框架

**网络通信框架三种不同的实现方式：**
- Simple 线程模式
    - **特点**：每一个网络链接，都会创建两个线程，一个用于接收，一个用于发送。
    - **缺点**：大量的链接会产生大量的线程，会消耗 CPU 资源，影响性能。
- Async 事件的 I/O 多路复用模式
    - **特点**：这种是目前网络通信中广泛采用的方式。k 版默认已经使用 Asnyc 了。
- XIO 方式使用了开源的网络通信库 accelio 来实现
    - **特点**：这种方式需要依赖第三方的库 accelio 稳定性，目前处于试验阶段。

**Ceph 通信框架设计模式**
订阅发布模式又名**观察者模式**，它意图是 “定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新”。

**Ceph 通信框架流程图**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/ceph_communication.png)

**步骤：**
- Accepter 监听 peer 的请求，调用 SimpleMessenger::add\_accept\_pipe () 创建新的 Pipe 到 SimpleMessenger::pipes 来处理该请求。
- Pipe 用于消息的读取和发送。该类主要有两个组件，Pipe::Reader，Pipe::Writer 用来处理消息读取和发送。
- **Messenger 作为消息的发布者，各个 Dispatcher 子类作为消息的订阅者**，Messenger 收到消息之后， 通过 Pipe 读取消息，然后转给 Dispatcher 处理。
- Dispatcher 是订阅者的基类，具体的订阅后端继承该类，初始化的时候通过 Messenger::add\_dispatcher\_tail/head 注册到 Messenger::dispatchers. 收到消息后，通知该类处理。
- DispatchQueue 该类用来缓存收到的消息，然后唤醒 DispatchQueue::dispatch\_thread 线程找到后端的 Dispatch 处理消息。

**参考：**
- [分布式存储 Ceph 的演进经验 · SOSP '19](https://draveness.me/papers-ceph/)
- [Ceph分布式存储工作原理及部署介绍](https://www.cnblogs.com/kevingrace/p/8387999.html)
- [Ceph 介绍及原理架构分享](https://www.cszhi.com/2018/09/19/Ceph%E4%BB%8B%E7%BB%8D%E5%8F%8A%E5%8E%9F%E7%90%86%E6%9E%B6%E6%9E%84%E5%88%86%E4%BA%AB/)


## Primary-Backup Replication

主备副本之间应该没有相关的故障，如不要购买相同品牌的电脑。Replication 是否值得取决于你想要多少 replica，你想要花多少钱以及遭受故障后会对你产生多少影响。

VM FT Paper中两种方法Replication：
1. **State transfer**：备份状态，状态是primary在内存的状态，CPU，Memory, and I/O devices。
    - 状态体积大，要求带宽多
    - 状态迁移代价大
2. **Replicated state machines**：发送给backup(备机)那些需要知道的来自外界的输入或外部事件
    - 更有吸引力，需求带宽少
    - 但是实现复杂
    - 论文中是在单核机器上。但是对于多核机器，交错执行的指令顺序不确定，在多核机器上该方法表现不好。State transfer处理多核的性能更强大

构建 replicated state machines 需要考虑的问题:
- 复制什么状态 --> 全部状态，底层状态，寄存器等。优点是可以支持很多应用程序，缺点是没有那么高效。<u>VMWARE使用这种复制方法</u>。
- 考虑主备(P/B)一致性程度（弱 or 强）
- 异常处理(Anomalies)
- 创建新的备份

**VM FT 实现了一种经典的单核 CPU 双机热备容错方案, 主备服务器的状态应该相近，当 primary 错误，能够立即从 backup 恢复。从机器级别进行复制，可以使得任意软件都具有容错性，错误对于 client 层面隐藏**。VM FT 只解决 Fail-stop faults, 服务因故障而停止（可以被检测），如硬件错误导致 Fail-stop。

Fault-Tolerant Virtual Machines 的原理图如下：


<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/vm-ft.png" width = "60%" height = "60%">
</div>

VMM 启动 Windows 或者 Linux 实例(包含操作系统和应用)。假定P/B精确复制。
Primary VM 接受的所有输入通过 **Logging channel** 发送给 Backup VM。
VMware FT 使用 deterministic replay 产生 log entries 记录 Primary VM 的执行过程。Primary VM 通过 Logging channel 将 log entries 发送给 Backup VM。这里有一个 **Output Rule**: ``the primary VM may not send
an output to the external world, until the backup VM has received and acknowledged the log entry associated with the operation producing the output``.

也存在一些问题，即 state 可能不是确定的， non-determinism clients，不确定来自多方面：
- inputs -- packets = data + interrupt
- 指令不确定性，如随机指令，在不同机器上产生不同结果
- 多核并行，指令交错执行的顺序 

> 机器是多核的， 但是VMM对外显示的是单核的  

P/B VMM 在相同指令处执行中断，不能让B先于P执行指令，保证一致性。通过设置 VM’s CPU limit 来保证 P 和 B 的差距不会太大，太大会导致性能损失。

发生心跳检测错误或者 logging traffic 长时间停止，说明有错误产生。如果 P 失败， B继续执行 log entries 的操作，直到结束，然后切换为 P状态。在共享存储执行 **test-and-set** 原子操作，保证发生错误时，P/B只有一个存活（比如如，PB无法相互通信, 但能和Client通信）。**共享存储**使得主备得以通信，只不过通过信号量而非 socket。

- **Disk IOs**: NIC 使用DMA直接将数据搬到内存，给VMM中断， VMM 记录哪个指令将P挂起。Bounce buff 和内存大小相同，由 disk operation 管理。
- **Network IO**: TCP 连接复用提高了通信效率。

## Paxos
**Paxos算法**是[莱斯利·兰伯特](https://zh.wikipedia.org/wiki/%E8%8E%B1%E6%96%AF%E5%88%A9%C2%B7%E5%85%B0%E4%BC%AF%E7%89%B9 "莱斯利·兰伯特")（英语：Leslie Lamport，[LaTeX](https://zh.wikipedia.org/wiki/LaTeX "LaTeX")中的“La”）于1990年提出的**一种基于消息传递且具有高度容错特性的共识（consensus）算法**。需要注意的是，Paxos常被误称为“一致性算法”。**但是“一致性（consistency）”和“共识（consensus）”并不是同一个概念**。Paxos是一个共识（consensus）算法。Paxos算法有三种类型：Basic Paxos, Multi Paxos 和 Fast Paxos。共识算法为了保证高可用。

### Basic Paxos

**角色介绍**：
1. Client: 系统外部角色，请求发起者。像民众。
2. Propser: 接受 Client 请求，Propser 有多个； 向 Acceptor 提出提案，提案信息包括提案编号和提议的 value。
3. Accpetor: 收到提案后可以接受（accept）提案，并应答 Propser。若提案获得多数派（majority）的 acceptors 的接受，则称该提案被批准（chosen）
4. learners：只能获得被批准（chosen）的决议（value）。

**决议的提出与批准**：
通过一个决议分为两个阶段：

1. prepare阶段：
    1. proposer选择一个提案编号n并将prepare请求发送给acceptors中的一个多数派；
    2. acceptor收到prepare消息后，如果提案的编号大于它已经回复的所有prepare消息(回复消息表示接受accept)，则acceptor将自己上次接受的提案回复给proposer，并承诺不再回复小于n的提案，接收提案；否则拒绝。
2. 批准阶段：
    1. 当一个proposer收到了多数acceptors对prepare的回复后，就进入批准阶段。它要向回复prepare请求的acceptors发送accept请求，包括编号n和根据P2c决定的value（如果根据P2c没有已经接受的value，那么它可以自由决定value）。
    2. 在不违背自己向其他proposer的承诺的前提下，acceptor收到accept请求后即批准这个请求。

这个过程在任何时候中断都可以保证正确性。例如如果一个proposer发现已经有其他proposers提出了编号更高的提案，则有必要中断这个过程。因此为了优化，在上述prepare过程中，如果一个acceptor发现存在一个更高编号的提案，则需要通知proposer，提醒其中断这次提案。

Basic Paxos的实例如下：
```
Client   Proposer      Acceptor     Learner
   |         |          |  |  |       |  | --- First Request ---
   X-------->|          |  |  |       |  |  Request
   |         X--------->|->|->|       |  |  Prepare(N)
   |         |<---------X--X--X       |  |  Promise(N,I,{Va,Vb,Vc})
   |         X--------->|->|->|       |  |  Accept!(N,I,V)
   |         |<---------X--X--X------>|->|  Accepted(N,I,V)
   |<---------------------------------X--X  Response
   |         |          |  |  |       |  |
```
式中V = (Va, Vb, Vc) 中最新的一个。

Basic Paxos 的问题：**难实现、效率低(2轮RPC)、活锁**

### Multi Paxos
Basic Paxos 中存在多个 Proposer，prepare 阶段 就是决定使用哪个 Proposer 的提案。Multi Paxos 引入 Leader 概念，即只有一个 Proposer, 所有请求都有经过 Leader。因此，可以省略第一阶段，Multi Paxos 实例如下：

```
Client   Proposer       Acceptor     Learner
   |         |          |  |  |       |  |  --- Following Requests ---
   X-------->|          |  |  |       |  |  Request
   |         X--------->|->|->|       |  |  Accept!(N,I+1,W)
   |         |<---------X--X--X------>|->|  Accepted(N,I+1,W)
   |<---------------------------------X--X  Response
   |         |          |  |  |       |  |
```

**应用**：
谷歌公司（Google公司）在其分布式锁服务中应用了Multi-Paxos算法。谷歌公司（Google公司）在其分布式数据库spanner中使用Multi-Paxos作为分布式共识保证的基础组件。Apache ZooKeeper 使用一个类Multi-Paxos的共识算法作为底层存储协同的机制。

**参考：**
- [Paxos算法](https://zh.wikipedia.org/wiki/Paxos%E7%AE%97%E6%B3%95)

## GO, Threads and Raft

### 内存模型

用线程有两个作用：

1. 提高性能，利用多核。
2. 更优雅的构造代码。

在本课程实验中，我们并不要求使用线程以极尽性能，只要求程序的正确性。

对锁的使用也一样，不要求细粒度的加锁以提升性能，可以大范围加锁以简化代码。

[内存模型](https://golang.org/ref/mem)中主要提到了多线程执行时，代码运行的顺序性。主要结论是，单个线程内的可以保证执行效果和代码语句顺序一致，但是跨线程间，如果没有做显式同步（通过锁或者 channel），那么语句执行的先后是没有办法保证的。内存模型大致就讲这么多，本节课的重点在于探讨实验中可能会用到的一些典型代码模式。

### goroutine 和并发
goroutine 是并发执行的，可以通过匿名函数和非匿名函数创建
```go
func sub() { 
	time.Sleep(15 * time.Millisecond)
	fmt.Println("sub goroutine execution started")
}
func main() {
	fmt.Println("main goroutine execution started")
	go sub() // 不使用匿名函数开启一个协程
	go func() { // 使用匿名函数开启一个线程
		//time.Sleep(15 * time.Millisecond) t2
		fmt.Println("sub goroutine execution started")
	}()
	time.Sleep(10 * time.Millisecond) // t1
	fmt.Println("main goroutine execution stopped")
}
```
当**主协程**运行的时候，<u>Go 调度器在主协程执行完之前并不会将控制权移交给子协程</u>。不幸的是，一旦主协程执行完毕，整个程序会立即终止，调度器再也没有时间去运行子协程了。**通过设置定时时间，调度器就将控制权转移给了子协程**。如果子协程的定时时间比主协程时间久，则子协程也不会运行主体部分。

### 同步原语
下面介绍四种GO语言的同步原语

#### sync.WaitGroup 等待组

```go
func sendRPC(i int) {
	fmt.Println(i)
}
func main() {
	var wg sync.WaitGroup
	for i := 0; i < 5; i++ {
		wg.Add(1) // 开始时调用Add方法表示有个任务开始执行
		go func(x int) {
			sendRPC(x)
			wg.Done() // 调用Done方法，表示任务已经执行完成了。
		}(i)
	}
	wg.Wait()  // 调用Wait()方法，等待所有协程完成任务
}
```

#### sync.Mutex 锁

主要有以下两种情况需要使用锁：

1. 保证多线程间共享变量的可见性。
2. 保证一个代码块的原子性（不会与其他 goroutine 中的语句交替执行）。

当然这两种情况往往是一种情况。由于 go 内存模型的不可捉摸性（fancy），最好通过加锁来保护所有对共享变量的访问，否则写出的多线程代码很可能会出现一些难以发现和调试的问题。
```go
func main() {
  counter := 0
  var mu sync.Mutex
  for i := 0; i < 1000; i++ {
    go func() {
      mu.Lock()
      defer mu.Unlock()
      counter = counter + 1
    }()
  }

  time.Sleep(1 * time.Second) // 这里用 WaitGroup 更为稳妥，等待1s只能保证大概率正确。
  mu.Lock()
  println(counter)
  mu.Unlock()
}
```

下面看一个 Alice 和 Bob 相互借钱，并试图维持总钱数不变的例子。我们各用一个线程来分别表示 Alice 借给 Bob 钱和 Bob 借给 Alice 钱的过程。

```go
func main() {
	alice := 10000
	bob := 10000
	var mu sync.Mutex

	total := alice + bob

	go func() {
		for i := 0; i < 1000; i++ {
			mu.Lock()
			alice -= 1
			mu.Unlock()
			mu.Lock()
			bob += 1
			mu.Unlock()
		}
	}()
	go func() {
		for i := 0; i < 1000; i++ {
			mu.Lock()
			bob -= 1
			mu.Unlock()
			mu.Lock()
			alice += 1
			mu.Unlock()
		}
	}()

	start := time.Now()
	for time.Since(start) < 1*time.Second {
		mu.Lock()
		if alice+bob != total {
			fmt.Printf("observed violation, alice = %v, bob = %v, sum = %v\n", alice, bob, alice+bob)
		}
		mu.Unlock()
	}
}
```
上述代码会打印出 violation 么？答案是会的。因为观察线程可能在某人借出钱，但是另外一个人没有收到钱的时候打印出 violation。可以从以下两个角度来理解这个问题：

1. **原子性**。出借和借钱应该是一个原子性操作，因此需要使用锁整个包裹起来。否则在中间某个时刻观察，就会产生不一致：钱被借出了，但是还没有收到，即在 “控制”。
2. **不变性**（invariant）。锁可以守护不变性，即当获取锁进入临界区后，可能会破坏不变性（借钱，此时钱在 “空中”，此时观察会凭空少了一块钱），但是释放锁前恢复（收钱，从 “空中” 放到另一个人的账户里，两人账户和维持不变）即可。

因此需要将交易过程改为如下：
```go
go func() {
		for i := 0; i < 1000; i++ {
			mu.Lock()
			alice -= 1
			mu.Unlock()
			mu.Lock()
			bob += 1
			mu.Unlock()
	}
}() // 另一个程序也修改
```
#### sync.Cond：条件变量(信号机制)

在 raft 里，会有一个场景，Candidate 向所有 Followers 要票，然后根据收集到的票数决定是否成为 Leader。一般实现会有**忙等待**(一致循环，直到条件满足)，并且在循环过程中不断加锁、释放锁会造成极大的 CPU 占用。一种很简单的性能提升方法，在忙等待循环中加一个 ``time.Sleep(50 * time.Millisecond)``，就能极大的减少 CPU 的占用。

另一种更高效的方式，是使用 `condition`，<u>当不满足条件时，调用``Wait()``，将线程加入等待队列，释放锁。于是另一个线程就可以通过 `Lock` 获取锁，并且在出临界区前，通过调用 `Broadcast` 来唤醒等待队列里的所有线程，让他们争抢锁</u>。

> 需要注意的是，`Condition` 的使用模式要求<u>`cond.Broadcast` 和 `cond.Wait` 都必须在对应的锁所守护的临界区间</u>内，并且调用 `cond.Broadcast` 后要及时释放锁，否则会引起其他线程的对一个未释放的锁的争抢。

```go
func main() {
	rand.Seed(time.Now().UnixNano())

	count := 0
	finished := 0
	var mu sync.Mutex
	cond := sync.NewCond(&mu)

	for i := 0; i < 10; i++ {
		go func() {
			vote := requestVote()
			mu.Lock()
			defer mu.Unlock()
			if vote {
				count++
			}
			finished++
			cond.Broadcast()
		}()
	}

	mu.Lock()
	for count < 5 && finished != 10 {
		cond.Wait()
	}
	if count >= 5 {
		println("received 5+ votes!")
	} else {
		println("lost")
	}
	mu.Unlock()
}

func requestVote() bool {
	time.Sleep(time.Duration(rand.Intn(100)) * time.Millisecond)
	return rand.Int() % 2 == 0
}
```
> WaitGroup 和 条件变量都适用生产者消费者问题

#### channel 通道
带缓冲区（unbuffered channel）的 channel，可以设定缓冲区的大小，当作队列来使用。

不带缓冲（unbuffered channel）的 channel 通常被用作多线程间进行同步的一种控制手段。**只有两端同时有发送者和接收者，channel 才不会被阻塞**(即为同步)

使用 channel 进行同步，可以发挥类似 WaitGroup 的作用：
```go
func main() {
	done := make(chan bool)
	for i := 0; i < 5; i++ {
		go func(x int) {
			sendRPC(x)
			done <- true
		}(i)
	}
	for i := 0; i < 5; i++ {
		<-done
	}
}

func sendRPC(i int) {
	println(i)
}
```

### Raft 中常见的两种 bug

- 死锁(相互等待) -->
  - RPC调用期间不持有锁
- RPC 回应过期 -->
  - 在 Candidate 收回选票结果时，需要判断自己是否仍然在原来的 term 内，以及是否仍然是 Candidate。否则可能选出两个 leader。
    ```go
    if rf.state != Candidate || rf.currentTerm != term {
      return
    }
    ```
    不过，其实只检查 currentTerm 就足够了，但是只检查 state 是不够的。

## Fault Tolerance: Raft
Paxos算法难以理解，实现困难。Raft(Reliable, Replicated, Redundant, And Fault-Tolerant)一种用于替代Paxos的共识算法, Raft 比 Paxos 更易于理解，也更易于实现。**Raft 在程序中以库的形式存在**。

Raft 共识算法相较于其他共识算法的新特点：
1. **Strong leader**
2. **Leader election**
3. **Membership changes**

Raft主要做了两方面的事情：
1. **问题分解**：把共识算法分为三个子问题，分别是**领导者选举（leader election）、日志备份（log replication）、安全性（safety）**
2. **状态简化**：对算法做出一些限制，减少状态数量和可能产生的变动。



### 复制的状态机(Replicated state machine)

**复制的状态机(Replicated state machine)** 的结构如下：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/replicated_state_machine.jpg" width = "60%" height = "60%">
</div>

**相同的初始状态 + 相同的输入 = 相同的结束状态**。多个节点上，从**相同的初始状态开始**，执行相同的一串命令，**产生相同的最终状态**。这些指令序列被保存在 **Replicated log**(operation log)中，**共识算法通过投票选举的方式使得各服务器节点的 Replicated log 保持一致，是实现一致性的手段；从而保证了服务器集群的高可用(只有有超过一半的机器正常，机器就能正常运行)**。

在Raft中，leader将客户端请求（command）封装到一个个log entry中，将这些log entries复制到所有follower节点，然后大家按相同顺序应用log entries中的command，根据复制状态机的理论，大家的结束状态肯定是一致的。log entry 是KV表的形式。

把复制状态机需要同步的数据量按大小进行分类，它们分别适合不同类型
的共识算法。
1. 数据量非常小，如<u>集群成员信息、配置文件、分布式锁、小容量分布式任务队列</u>。
    - 无leader的共识算法(如**Basic Paxos**)。
2. 数据量比较大但可以拆分为不相干的各部分，如<u>大规模存储</u>系统。
    - 有leader的共识算法(如**Multi Paxos，Raft**)，实现有<u>GFS，HDFS</u>等。
3. 不仅数据量大，数据之间还存在关联。
    - 类场景就主要是一些如**Spanner、OceanBase、TiDB**等<u>支持分布式事务的分布式数据库。它们通常会对Paxos或Raft等共识算法进行一定的改造，来满足事务级的要求</u>。

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/raft_summary.jpg)

### 状态简化
在任何时刻，每一个服务器节点都处于**leader，follower 或 candidate**这三个状态**之一**。相比于Paxos，这一点就极大简化了算法的实现，因为Raft只需考虑状态的**切换**，而不用像Paxos那样考虑状态之间的**共存和互相影响**。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/raft_state.jpg)
在正常情况下,只有一个leader, 其他全为 follower。leader 处理客户端的请求，如果客户端联系follower，leader 间接处理客户端请求。follower 对来自leader 和 candidate 的请求做出响应；当需要选举 leader 时，follower 变成 candidate。

Raft把时间分割成任意长度的**任期（term）**，任期用连续的整数标记。每一段任期从一次选举开始。在某些情况下，一次选举无法选出leader（比如两个节点收到了相同的票数），在这种情况下，这一任期会以没有leader结束；一个新的任期（包含一次新的选举）会很快重新开始。**Raft保证在任意一个任期内，最多只有一个leader**(避免脑裂)。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/raft_term.jpg)

Raft算法中服务器节点之间使用RPC进行通信，并且Raft中只有两种主要的RPC：
- **RequestVote RPC（请求投票）**：由candidate在选举期间发起。
- **AppendEntries RPC（追加条目）**：由leader发起，用来复制日志和提供一种心跳机制。
- 服务器之间通信的时候会**交换当前任期号**；如果一个服务器上的当前任期号比其他的小，该服务器会将自己的任期号更新为较大的那个值。
- 如果一个candidate或者leader发现自己的任期号过期了，它会立即回到follower状态。
- 如果一个节点接收到一个包含过期的任期号的请求，它会直接拒绝这个请求。


### 领导者选举(Leader election)

Raft内部有一种**心跳机制**，如果存在leader，那么它就会周期性地向所有follower发送**心跳**(AppendEntries RPCs that carry no log entries)，来维持自己的地位。如果follower一段时间(**election timeout，Raft 随机选举超时时间为 150~300ms**)没有收到 leader心跳或者 candidate 请求，那么他就会认为系统中没有可用的leader了，然后开始进行选举。开始一个选举过程后，follower **先增加自己的当前任期号，并转换到candidate状态，然后投票给自己，并且并行地向集群中的其他服务器节点发送投票请求（RequestVote RPC），选出 Leader 后发送心跳和进行日志备份(AppendEnrties RPC)**。

**Election Safety**: at most one leader can be elected in a given term.

最终会有三种结果：
1. 它获得超过半数选票赢得了选举(要求机器是数为奇数，2f+1 个机器可以允许 f 个机器宕机) -> 成为 leader 并开始发送心跳
2. 其他节点赢得了选举 -> 收到新leader的心跳后，如果新leader的任期号不小于自己当前的任期号，那么就从candidate回到follower状态；如果任期号小于自己的任期号，则保持 candidate 状态
3. 一段时间之后没有任何获胜者 -> 每个candidate都在一个自己的随机选举超时时间后增加任期号开始新一轮投票。

> 对于没有成为candidate的follower节点，对于同一个任期，会按照**先来先得**的原则投出自己的选票。

### 日志备份(Log replication)

Leader被选举出来后，开始为客户端请求提供服务。Leader **并行**发送**AppendEntries RPC** 给 follower，让它们复制该条目。当该条目被**超过半数**的follower复制后，这个 **log entry 被提交**(持久化，最终被可用的状态机执行)，leader就可以在本地执行该指令并把结果返回客户端。如果**超过半数的机器提交了，则认为集群提交**。

**Log Matching Property**： 
1. <u>如果两个 entries 在不同日志中有相同的 log index(日志号) 和 term(任期号)，则他们存放相同的指令</u>
2. <u><font color=Red>如果两个 entries 在不同日志中有相同的 log index(日志号) 和 term(任期号)，则他们之前的 entries 也是一致的</font></u>(复制的一致性)。

**Leader Append-Only**: a leader never overwrites or deletes entries in its log; it only appends new entries. 

Leader或follower随时都有崩溃或缓慢的可能性，Raft必须要在有宕机的情况下继续支持日志复制，并且保证每个副本日志顺序的一致（以保证复制状态机的实现）。具体有三种可能：
1. 如果有follower因为某些原因**没有给leader响应**，那么leader会不断地**重发**追加条目请求（AppendEntries RPC），哪怕leader已经回复了客户端。
2. 如果有follower**崩溃后恢复**或某个follower运行很慢，这时Leader **发送InstallSnapshot RPC** 或 AppendEntries RPC 的**一致性检查生效**，这是一个自动过程，保证follower能按顺序恢复崩溃后的缺失的日志。
    - **一致性检查**：Leader在每一个发往follower的追加条目RPC中，会放入**前一个日志条目的索引位置和任期号**，如果follower在它的日志中找不到前一个日志(**索引和任期号都相同才认为两个 entry 相同**)，那么它就会拒绝此日志，leader收到follower的拒绝后，会发送前一个日志条目(**prevLogIndex index, prevLogTerm**)，从而**逐渐向前定位到follower第一个缺失的日志**。Leader 删除冲突不匹配的entries，Leader 不断向 follower 追加 entries。
    > 单个运行慢的follower不会影响整体的性能。

1. 如果**leader崩溃**，那么崩溃的leader可能已经复制了日志到部分follower但还**没有提交**，而被选出的新leader又可能不具备这些日志，这样就有部分follower中的日志和新leader的日志不相同。
    - Raft在这种情况下，**leader通过强制follower复制它的日志来解决不一致的问题**，这意味着follower中跟leader冲突的日志条目会被新leader的日志条目覆盖（因为没有提交，所以不违背外部一致性）。

### 安全性(Safety)
领导者选举和日志复制两个子问题实际上已经涵盖了共识算法的全程，但这两点还不能完全保证每一个状态机会按照相同的**顺序执行**(没有空洞)相同的命令。

#### Leader宕机处理：选举限制
对领导者选举增加一个限制，**保证被选出来的leader一定包含了之前各任期的所有被提交的日志条目**。通过 RequestVote RPC 来实现这一限制: <u>RPC中包含了candidate的日志信息，如果投票者自己的日志比candidate的还**新**，它会拒绝掉该投票请求</u>。
  - **如果两份日志最后条目的任期号不同，那么任期号大的日志更“新”**。
  - **如果两份日志最后条目的任期号相同，那么日志较长的那个更“新”**。

#### Leader宕机处理：新leader是否提交它自己之前任期内的日志条目

如果某个leader在提交某个日志条目之前崩溃了，以后的leader会试图完成该日志条目的**复制**，而非提交(该 leader 后面有新的 entry log 被提交，则**复制**的entry 才被间接提交)。

- **Raft永远不会通过计算副本数目的方式来提交之前任期内的日志条目**。
- **只有leader当前任期内的日志条目才通过计算副本数目的方式来提交**。
- <font color=Red>一旦当前任期的某个日志条目被提交，那么由于日志匹配特性，</font>**<font color=Red>之前的所有日志条目也都会被间接地提交</font>**(提交的一致性)。

如图所示：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/raft_safety.jpg" width = "60%" height = "60%">
</div>

(a) 中 S1 是 Leader, 2 被复制; (b) 中 S1 宕机，S5 当选 Leader；(c) 中 S1 再次当选 Leader，2， 4均为提交，**复制**2 给 S3; (d) 中 S1 宕机， S5 当选Leader，S5 强制改写 S1-S4中未提交 entry log。如果 (d) 阶段不存在，即 S5 没当选，那么 S1 提交了 4，则 S1 中 2 被间接提交。

#### Follower和Candidate宕机处理
Follower和Candidate崩溃后的处理方式比leader崩溃要简单的多，并且两者的处理方式是相同的。如果follower或candidate崩溃了，那么后续发送给他们的RequestVote和AppendEntriesRPCs都会失败。Raft通过**无限的重试**来处理这种失败。如果崩溃的机器重启了，那么这些RPC就会成功地完成。如果一个服务器在完成了一个RPC，但是还没有相应的时候崩溃了，那么它重启之后就会再次收到同样的请求。（**Raft的RPC都是幂等的**）

#### 时间与可用性限制
Raft算法**整体不依赖客观时间**，也就是说，哪怕因为网络或其他因素，造成后发的RPC先到，也不会影响raft的正确性。（这点和Spanner不同）

只要整个系统满足下面的时间要求，Raft就可以选举出并维持一个稳定的leader：
**广播时间(broadcastTime) << 选举超时时间(electionTimeout) << 平均故障时间(MTBF)**

广播时间和平均故障时间是由系统决定的，但是选举超时时间是我们自己选择的。Raft的RPC需要接受并将信息落盘，所以广播时间大约是**0.5ms到20ms**，取决于存储的技术。因此，选举超时时间可能需要在**10ms到500ms之间**。大多数服务器的平均故障间隔时间都在几个月甚至更长。

### 集群成员变更(Cluster membership changes)
在需要改变集群配置的时候(如增减节点、替换宕机的机器或者改变复制的程度)，Raft可以进行配置变更自动化。

自动化配置变更机制最大的难点是保证转换过程中**不会出现同一任期的两个leader(避免脑裂问题)**，因为转换期间整个集群可能划分为两个独立的大多数。

Raft 使用两阶段的方法：集群先切换到一个过渡的配置，称之为**联合一致（joint consensus）**。
- 第一阶段，leader发起$C_{old,new}$，使整个集群进入联合一致状态。这时，**所有RPC都要在新旧两个配置中都达到大多数才算成功**。
- 第二阶段，leader发起$C_{new}$，使整个集群进入新配置状态。这时，所有RPC只要在新配置下能达到大多数就算成功

> 在Diego Ongaro的博士论文，和后续的大部分对raft实现中，都使用的是另一种更简单的单节点并更方法，即**一次只增减一个节点**，称为**单节点集群成员变更方法**。

### 日志压缩(Log compaction)
为什么要进行日志压缩呢，因为随着raft集群的不断运行，各状态机上的log也在不断地累积，总会有一个时间会把状态机的内存打爆，所以我们需要一个机制来安全地清理状态机上的log。

每个 server 都有一个**快照(Snapshotting)**，将已经提交的 log entries 压缩(删除)，只保留当前的状态，包含:
- ``last included index`` 和 ``last term``(为了一致性检查)
- ``latest configuration``(为了集群成员变更)
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/raft_log_campaction.jpg" width = "70%" height = "70%">
</div>

如果**一个follower落后leader很多**，如果老的日志被清理了，leader怎么同步给follower呢？--> <u>follower 让 leader 给它发 leader 的 Snapshotting，通过 **InstallSnapshot RPC** 的方式</u>。

### Raft性能及与Paxos比较

最根本的，每完成一个日志（命令）的复制与提交，需要的网络（RPC）来回次数。raft在理想情况下，只需要一次AppendEntries RPC来回即可提交日志（理论上的极限）。

**影响Raft性能的因素**: 选举及维持leader所需的代价->合理设置选举超时时间。

**进一步优化方法**：
- Batch：一个日志可以包含多个命令，然后批量进行复制，来节省网络。
- Pipeline：leader不用等待follower的回复，就继续给follower发送下一个日志
- Multi-Raft：将数据分组，每组数据是独立的，用自己的raft来同步。

**Raft与Paxos比较**：<u>raft不允许日志空洞，所以性能没Paxos好</u>。这里的Paxos，实际上指的**是一个能完美处理所有日志空洞带来的边界情况**，并能保证处理这些边界情况的代价要小于允许日志空洞带来的收益的共识算法。
> aft确实有不允许日志空洞这个性能上限，但大部分系统实现，连raft的上限，都是远远没有达到的。所以无需考虑raft本身的瓶颈。

raft允许日志空洞的改造 -> ParallelRaft: ParallelRaft是阿里云原生数据库PolarDB的底层文件PolarFS对Raft的一种优化的实现。


**参考：**
- Ongaro D, Ousterhout J. In search of an understandable consensus algorithm (extended version)[C].Proceeding of USENIX annual technical conference, USENIX ATC. 2014: 19-20.

## Zookeeper
ZooKeeper 是一个开放源码的，协调分布式应用的多个进程的服务(**多进程协助**)。ZooKeeper 的设计目标是将那些复杂且容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语集。解耦了客户端和服务器。<u>客户可以使用一系列简单易用的接口(API)，服务器对应一个个进程，ZooKeeper 通过树分层结构组织和管理每个应用程序对应的多个进程，使用ZAB协议(类似Raft 共识算法)实现高可用和一致性</u>。

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/zookeeper5.jpg)

配合客户端库，Zookeeper 可以提供<u>动态参数配置（configuration metadata）、**分布式锁**、共享寄存器（shared register）、服务发现、集群关系（group membership）、多节点选主（leader election）等一系列分布式系统的协调服务</u>。

总体来看，Zookeeper 有以下特点：
1. Zookeeper 是一个**分布式协调内核**，本身功能比较内聚，以保持 API 的简洁与高效。
2. Zookeeper 提供一组高性能的、保证 FIFO 的、**基于事件驱动的非阻塞 API**。
3. Zookeeper 使用类似文件系统的**目录树**方式对数据进行组织，表达能力强大，方便客户端构建更复杂的协调源语。
4. Zookeeper 是一个自洽的容错系统，使用 **Zab 原子广播（atomic broadcast）协议**保证高可用和一致性。

Zookeeper 的缺点：
1. **Zookeeper 允许客户端直接任意服务器读，不必通过leader，读性能很好**，Zookeeper的读性能随着服务器数量的增加而显著的增加，但是读可能读的不是最新的，不满足顺序一致性(侧重AP)。如果我需要读最新的数据，我需要发送一个sync请求，之后再发送读请求。**写入时，需要 leader 和 follower 同步(sync)，导致写性能不好。raft 读写请求都经过leader。** 写满足顺序一致性(侧重CP)
2. Zookeeper对 session 健康检查不彻底，tcp正常连接就认为服务进程正常。

### Zookeeper 服务设计

#### 术语集

1. **客户端**：client，使用 Zookeeper 服务的用户。
2. **服务器**：server，提供 Zookeeper 服务的进程。
3. **数据树**：data tree，Zookeeper 中所有的数据以树形结构进行组织。
4. **z - 节点**：znode、Zookeeper Node，数据树中的节点，是基本数据单元。
5. **会话**：session，客户端与服务器会新建一个会话来标识一个连接，之后客户端每次请求都会通过该会话句柄来进行。Watch 事件的生命周期也是和会话绑定的。


#### 数据组织

``znode``是客户端应用的抽象，客户端通过 Zookeeper API 操纵(创建、删除、添加子节点)znode，znode只能存储一些小部分信息(meta-data or configration information)，如 reader information, time stamps, version counters 等。不适合大数据存储原因：
1. Zookeeper 把数据加载到内存
2. Zookeeper 提供协同服务，只存储协同服务相关的数据

znodes 的分层结构如下：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/zookper1.jpg" width = "70%" height = "70%">
</div>

树中支持两种类型的 znode：
1. **普通节点**：Regular，生命周期无限，客户端需要调用接口显式的对这类节点进行增删。
2. **暂态节点**：Ephemeral，生命周期绑定到会话上，会话销毁，节点删除。

Zookeeper 使用 **会话机制（session）** 管理客户端一次连接的生命周期。在实现时，会话会关联一个超时间隔（timeout）。如果客户端死掉或者与 Zookeeper 断开连接，超时时限内客户端未进行心跳，Zookeeper 会在服务器端销毁该会话。<u>Zookeeper 使用 ``Watch`` 机制，当watch 了某个节点后，当该节点发生变化时，客户端会收到一次通知（边缘触发），一个订阅是绑定到会话上的，因此会话销毁后，订阅的事件也会消失</u>。

#### Client API
下面是以伪码的形式列出 Zookeeper 对客户端提供的 API 细节和注释。所有操作对象都是路径（ path） 所对应的数据节点（znode）。
```c
// 在路径 path 处创建一个 znode，存入数据 data
// 并设置 regular, ephemeral, sequential 等 flags 标志。
// 返回值：znode 名字
create(path, data, flags) 

// 如果 path 处的 znode 与预期 version 相同，
// 则删除该 znode。
// 指定 version 一般是为了并发安全。
delete(path, version)

// watch 让客户端在此 path 上添加一个监听
// 返回值：路径对应的 znode，存在时返回 true
// 不存在返回 false
exists(path, watch)

// 获取路径 path 对应的 znode 的数据和元信息
// 当 znode 存在时，允许设置 watch 来监听
// znode 数据变化
getData(path, watch)

// 当 version 匹配时，将数据 data 写入
// path 对应的 znode
setData(path, data, version)

// 获取路径 path 对应的 znode 的所有孩子
getChildren(path, watch)

// 同步最新数据，通常放在 getData 前面
sync(path)
```

上面的 API 有以下特点：

1. **异步支持**。所有接口都有 **同步（synchronous）** 和 **异步（asynchronous）** 版本。异步版本以回调函数方式进行执行，客户端可以根据业务需求，选择阻塞等待以获取重要更新，或者异步调用以获得更好性能。
2. **路径而非句柄**。为了简化接口设计，并减少服务端维护的状态， Zookeeper 使用路径而非 znode 句柄的形式来提供对 znode 的操作接口。毕竟，句柄类似于 session，是有状态的，会增加分布式系统的实现复杂度。使用路径，可以配合版本信息做成类似幂等的接口，在处理多客户端并发时，更容易实现。
3. **版本信息**。所有的 **更新操作（set/delete）** 都需要指明对应数据的版本号，版本号不匹配则终止更新并返回异常。但可以通过指定特殊版本号 -1 ，跳过版本号检查。
  
基于以上ZooKeeper API,可以实现一些基本的原语(Primitives)，如：
1. Configuration Management
2. Group Membership
3. Simple Locks
4. Read/Write Locks
5. Double Barrier
6. 等

#### ZooKeeper guarantees

在处理多个客户端向 Zookeeper 发出的并发请求时， API 有两个基本顺序的保证：

1. **线性化写（Linearizable writes）**。所有 Zookeeper 状态的更新请求会被串行化执行。
2. **客户端内的先入先出（FIFO client order）**。给定客户端的请求会按其发送的顺序进行执行。

但这里的线性化是一种异步线性化： A-linearizability。即单个客户端可以同时有多个正在执行的请求（multiple outstanding operations），但是这些请求会按发出顺序进行执行。对于读请求，可以在每个服务器本地（不需要通过主）执行。因此，可以通过增加服务器（Observer）提升读请求的吞吐。

> 强一致性和序列化等同，Client 看到的是当前状态而不是历史状态

此外，Zookeeper 还提供可用性和持久性的保证：

1. **可用性（liveness）**：Zookeeper 集群中过半数节点可用，则可对外正常提供服务。
2. **持久性（durability）**：任何被成功返回给客户端的修改请求，都会作用到 Zookeeper 状态机中。即使不断有节点故障重启，只要 Zookeeper 能正常提供服务，就不会影响这一特性。

### Zookeeper 的实现
为了提供高可靠性，Zookeeper 使用多台服务器对数据进行冗余存取。然后使用 Zab 共识协议处理所有的更新请求，然后写入 WAL，进而应用到本地内存状态机（data tree）。

在 Zab 协议中，所有节点分为两种角色，Leader 和 Followers，前者只有一个，剩余的都是 Followers。但后来实践中，可能有 Observers。

<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/zookper2.jpg" width = "70%" height = "70%">
</div>
如上图所示，当 Server 收到一个请求时，首先进行预处理（Request Processor），如果是写请求，则通过 Zab 协议（Atomic Broadcast）达成一致，然后各自提交到本地数据库（Replicated Database）。对于读请求，直接读取本地数据库中状态后返回。

#### 请求处理（Request Processor）

所有更新请求都会被转为 **幂等（idempotent）** 的事务（txn），具体方法为获取当前状态、计算出目标状态，封装为事务，即可使用类似 CAS 的方式处理并发请求。因此，只要保证所有事务按固定顺序执行，就能避免不同服务器上的数据副本分裂。

#### 原子广播（Atomic Broadcast）

领导者选举--> 同步给 Follower --> 写性能很慢

所有更新请求都会被转给 Zookeeper 的 Leader，Leader 首先将事务追加到本地 WAL，然后将变动使用 Zab 协议广播到各个节点，收到过半成功回复之后，Leader 将变动提交（Commit）到本地内存数据库，并广播该 Commit 给 Followers。

由于 Zab 使用多数票原则，因此 2k+1 个节点的集群最多可以容忍 k 个节点的故障（failures）。

为了提高系统吞吐，Zookeeper 使用流水线（pipelined）方式优化多个请求处理过程。

#### 复制状态机（Replicated Database）

每个服务器都会在本机内存中维护一个 Zookeeper 中所有状态的副本（replica），为了应对宕机重启，ZooKeeper 会定期将状态做快照。不同于普通快照，Zookeeper 称其快照为 _fuzzy_ _snapshots_，即在做快照时并不上锁，通过 DFS 的方式遍历文件树 Dump 到本地。之后由于异常宕机重启时，只需加最新快照，然后重新执行最新快照之后几条 WAL 即可。由于 WAL 中记录的事务的幂等性特点，即使快照和 WAL 的时间点不完全对应，也不会影响副本间的一致性。

#### 客户端服务器交互事宜（Client-Server Interactions）

**串行写**。无论是在全局范围还是具体到一个 Server 本地，所有更新操作都是串行的。在执行某个 Path 数据更新时，该 Server 会触发所有与之连接的 Client 所订阅的 Watch 事件。需要注意，这些事件只保存在 Server 本地，因为他们是和会话关联的，如果 Client 与该 Server 断开连接，会话便会销毁，这些事件也随之消亡。

**本地读**。为了获取极致性能，Zookeeper 的 Server 直接在本地处理读请求。但这有可能造成客户端拿到陈旧数据（比如其他客户端在另外的 Server 更新了同一 Path）。于是 Zookeeper 设计出了 Sync 操作，会将调用 Sync 时刻的最新提交数据同步到与该 Client 连接的 Server 上，然后将最新数据返回给 Client。即，Zookeeper 将**性能**与**时效性**的选择权交给了用户，方法是是否调用 Sync。

**一致性视图**。Zookeeper 全局会维持一个事务自增标识：zxid，它本质上是个逻辑时钟，可以标识 Zookeeper 一个时刻的数据视图。Client 在故障重启后重新连接到一个新的 Server 时，如果该 Server 未执行到客户端所存 zxid，则要么 Server 执行到该 zxid 后再回复 Client，要么 Client 换一个更新的 Server 进行连接。如此，可以保证 Client 不会看到回退的视图。

**会话过期**。会话在 Zookeeper 中本质上标识一个 Client 到 Server 的连接。会话有超时时间，如果 Client 长时间（大于超时间隔）不发**请求**或者**心跳**，Server 便会删除该会话。

### Zookeeper 分布式锁

原单体单机部署的系统被演化成分布式集群系统后，由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效，单纯的应用并不能提供分布式锁的能力。

惊群现象：很多个客户端线程争抢zookeeper znode 的锁(获取锁的过程就是创建唯一 znode 的过程)，当一个线程抢到锁，其他线程阻塞等待(休眠)，该线程结束则唤醒其他线程争抢锁。**同一时间有多个线程争抢(阻塞等待)一把锁，造成极大的浪费**。

为了解决惊群现象，Zookeeper 分布式锁设计逻辑如下：多个客户端线程根据请求顺序来创建临时有序的znode，最小节点编号的线程最先获得锁。如 ABCD 顺序，A检查到自己为最小节点编号，获取锁，然后释放，B发现自己节点编号最小，然后获取锁。**同一时间只有一个线程在阻塞等待一把锁。**

> Zookeeper 分布式锁是CP的

**参考：**
- [分布式系统协调内核 ——Zookeeper](https://www.qtmuniao.com/2021/05/31/zookeeper/)


## More Replication, CRAQ

Chain Replication 具有 HEADER 和 TAIL 节点。
- HEADER 节点处理写请求，逐步传播到TAIL节点，然后进行 commit, <u>在之前的论文中TAIL节点直接返回给客户端</u>，改进的论文考虑已有的TCP连接，通过 HEADER 返回给客户端。
- TAIL 节点处理读请求，容易造成热点问题
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/chain_replication.jpg" width = "70%" height = "70%">
</div>

> 除非写请求到达了TAIL，否则一个写请求是不会commit，也不会向客户端回复确认，也不能将数据通过读请求暴露出来。

**Chain Replication的故障恢复**：
1. 如果HEAD出现故障，下一个节点可以接手成为新的HEAD
2. 如果TAIL出现故障，TAIL的前一个节点可以接手成为新的TAIL。
3. 中间节点出现故障需要做的就是将故障节点从链中移除。

**Chain Replication与Raft比较**:
1. Raft 通过 leader 处理写请求，并将写同步到所有follower, 而Chain Replication HEADER只需要将写同步到下一个节点，减轻了 HEADER 的压力；此外Raft也通过 header 处理读请求，Chain Replication TAIL 节点处理读请求。这两点减轻了HEADER的压力
2. Chain Replication 实现简单
3. Raft在抵御短暂的慢响应方面表现的更好。

Chain Replication**不能抵御网络分区，也不能抵御脑裂**，在实际场景中，这意味它不能单独使用。使用Configuration Manager 作为权威，监测节点存活性，来实现容错和抵御脑裂问题。Configuration Manager 通告给所有参与者整个链的信息，所以所有的客户端都知道HEAD在哪，TAIL在哪，所有的服务器也知道自己在链中的前一个节点和后一个节点是什么。Configuration Manager 通常基于Raft或者Paxos，是容错的，也不会受脑裂的影响。


## Cloud Replicated DB, Aurora

Aurora 是高性能云数据库，云架构。
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/aurora.jpg" width = "70%" height = "70%">
</div>

> Aurora实现了计算和存储解耦

Aurora 有6个数据的副本，位于3个AZ，每个AZ有2个副本。所以现在有了超级容错性，并且每个写请求都需要以某种方式发送给这6个副本。<u>Mirrored MySQL将大的page数据发送给4个副本，而Aurora只是将小的Log条目发送给6个副本，Aurora获得了35倍的性能提升</u>。这里的存储系统不再是通用（General-Purpose）存储(MySQL Log条目的存储系统)，取而代之的是一个应用定制的（Application-Specific）存储系统，存储小的 log 条目。Aurora并不需要6个副本都确认了写入才能继续执行操作-->Quorum复制机制。

### Fault-Tolerant Goals
Aurora的Quorum系统管理了6个副本的容错系统。所以值得思考的是，Aurora的容错目标是什么？
1. 首先是对于写操作，当只有一个AZ彻底挂了之后，写操作不受影响。
2. 其次是对于读操作，当一个AZ和一个其他AZ的服务器挂了之后，读操作不受影响。
3. Aurora期望能够容忍暂时的慢副本。
4. 如果一个副本挂了，在另一个副本挂之前，快速生成新的副本（Fast Re-replication）是争分夺秒的


### Quorum Replication
Aurora使用的是一种经典quorum思想的变种。Quorum系统背后的思想是通过复制构建容错的存储系统，并确保即使有一些副本故障了，读请求还是能看到最近的写请求的数据。通常来说，Quorum系统就是简单的读写系统，支持Put/Get操作。它们通常不直接支持更多更高级的操作。

<u>假设有N个副本。如果要执行写请求，必须要确保写操作被W个副本确认，W小于N。如果要执行读请求，那么至少需要从R个副本得到所读取的信息</u>。

- **W : Write Quorum**
- **R : Read Quorum**
- **R + W = N + 1: 任意W个服务器至少与任意R个服务器有一个重合**
> 如果有 N = 3，每个服务器只有一个对象，W = R = 2, 对象原来值为21，W 将两个副本对象的值设置成23。此时 R中两个副本一个为23， 一个为21，**它们有着不同的版本号，客户端会挑选版本号最高的结果。**

相比Chain Replication，这里的优势是可以轻易的剔除暂时故障、失联或者慢的服务器。

Quorum系统可以调整读写的性能。通过调整Read Quorum和Write Quorum，可以使得系统更好的支持读请求或者写请求。如果想提升读性能，可以将R设置成1

### 数据分片（Protection Group）

为了能支持超过10TB数据的大型数据库。Amazon的做法是将数据库的数据，分割存储到多组存储服务器上，每一组都是6个副本，分割出来的每一份数据是10GB。所以，如果一个数据库需要20GB的数据，那么这个数据库会使用2个PG（Protection Group），其中一半的10GB数据在一个PG中，包含了6个存储服务器作为副本，另一半的10GB数据存储在另一个PG中，这个PG可能包含了不同的6个存储服务器作为副本。


每个Protection Group存储了所有data page的一个子集，以及这些data page相关的Log条目。对于一个特定的存储服务器，它存储了许多Protection Group对应的10GB的数据块。如果一个存储服务器挂了，假设上面有100个数据块，现在的替换策略是：找到100个不同的存储服务器，其中的每一个会被分配一个数据块，也就是说这100个存储服务器，每一个都会加入到一个新的Protection Group中。所以相当于，每一个存储服务器只需要负责恢复10GB的数据。所以在创建新副本的时候，我们有了100个存储服务器，这100个存储服务器可以**并发的恢复**。


## Cache Consistency: Frangipani
Frangipani 是关于分布式文件系统的优秀论文， 大量有关缓存一致性（Cache Coherence），分布式事务（Distributed Transaction）和分布式故障恢复（Distributed Crash Recovery）有关的有趣的并且优秀的设计。从整体架构上来说，Frangipani就是一个网络文件系统（NFS，Network File System）。从一个全局视图来看，它包含了大量的用户（U1，U2，U3）。

### Frangipani 结构

Frangipani 的结构如下：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/frangipani_structure.jpg" width = "60%" height = "60%">
</div>
 
多台机器运行相同的 Frangipani 文件系统(在 Petal virtual disk 顶部)，文件系统的数据结构，例如文件内容、inode、目录、目录的文件列表、inode和块的空闲状态， 所有这些数据都存在一个叫做Petal的共享虚拟磁盘服务中。<u>lock 服务提供 multiple-reader/single-writer 保证多个服务器的缓存一致性</u>。最开始的时候，文件修改只会在本地的缓存中(本地内存，Write-Back缓存), 这些修改要过一会才会写回到Petal。

**Disk layout:**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/frangipani_disk_layout.jpg)

**缓存一致性**： 如果我缓存了一个数据，并且其他人在他的缓存中修改了这个数据，那么我的缓存需要自动的应用那个修改。如何保证缓存一致？如何保证原子性（似于创建文件，删除文件这样的操作表现的就像即时生效的一样，同时不会与相同时间其他工作站的操作相互干扰）？本地缓存崩溃，其他机器缓存正常，如何恢复？

### Frangipani 的锁服务（Lock Server）与缓存一致性（Cache Coherence）

假设文件X最近被工作站WS1使用了，所以WS1对于文件X持有锁。同时文件Y最近被工作站WS2使用，所以WS2对于文件Y持有锁。**锁服务器**会记住每个文件的锁被谁所持有。当然一个文件的锁也有可能不被任何人持有。

在每个**工作站**，会记录跟踪它所持有的锁，和锁对应的文件内容。所以在每个工作站中，Frangipani模块也会有一个lock表单，表单会记录文件名、对应的锁的状态和文件的缓存内容。这里的文件内容可能是大量的数据块，也可能是目录的列表。

当一个Frangipani服务器（工作站）决定要读取文件，比如读取目录 /、读取文件A、查看一个inode，首先，它会向一个锁服务器请求文件对应的锁，之后才会向Petal服务器请求文件或者目录的数据。收到数据之后，工作站会记住，本地有一个文件X的拷贝，对应的锁的状态，和相应的文件内容。在工作站完成了一些操作之后，比如创建文件，或者读取文件，它会随着相应的系统调用（例如rename，write，create，read）释放锁。只要系统调用结束了，工作站会在内部释放锁，现在工作站不再使用那个文件。但是从锁服务器的角度来看，工作站仍然持有锁。工作站内部会标明，这是锁时Idle状态，它不再使用这个锁。所以这个锁仍然被这个工作站持有，但是工作站并不再使用它。

Frangipani规则： 一种提供缓存一致性的方式来使用锁，规则、锁、缓存数据需要配合使用
1. <u>工作站不允许不允许在没有锁保护的前提下缓存数据</u>，只有当工作站持有锁了，工作站才会从Petal读取数据，并将数据放在缓存中
2. <u>如果你在释放锁之前，修改了锁保护的数据，那你必须将修改了的数据写回到Petal，只有在Petal确认收到了数据，你才可以释放锁</u>，也就是将锁归还给锁服务器。所以这里的顺序是，先向Petal存储系统写数据，之后再释放锁,最后再从工作站的lock表单中删除关文件的锁的记录和缓存的数据。

工作站和锁服务器之间的缓存一致协议协议包含了4种不同的消息：
1. **Request消息**： WS(工作站服务器)->LS(锁服务器)。 WS 向LS 请求获取锁
2. **Grant消息**: LS(锁服务器)->WS(工作站服务器)。 如果从锁服务器的lock表单中发现锁已经被其他人持有了，那锁服务器不能立即交出锁。<u>但是一旦锁被释放了，锁服务器会回复一个Grant消息给工作站</u>。这里的Request和Grant是异步的。锁服务器怎样让持有锁的工作站先释放锁？
3. **Revoke消息**：LS(锁服务器)->WS(工作站服务器)。如果锁服务器收到了一个加锁的请求，它查看自己的lock表单可以发现，<u>这个锁现在正被工作站WS1所持有，锁服务器会发送一个Revoke消息给当前持有锁的工作站WS1</u>。并说，现在别人要使用这个文件，请释放锁吧。
4. **Release消息**: LS(锁服务器)->WS(工作站服务器)。如果锁的状态是Idle，首先需要将修改了的缓存数据发回给Petal，工作站才会再向锁服务器发Revoke请求的响应---一条Release消息。如果锁的状态是busy, 工作站收到Revoke消息时，直到工作站使用完了锁为止它才放弃锁，工作站中的锁的状态才会从Busy变成Idle，进行后面操作。

### 原子性（Atomicity）
**原子操作**：中间状态对外不可见，对外可见的是要么成功，要么失败。
**事务**：原子操作的集合，由一个状态切换到另一个状态。如创建文件，重命名文件，删除文件等原子性操作的集合。

Frangipani在内部实现了一个数据库风格的分布式事务系统，并且是以锁为核心。工作站需要获取所有需要读写数据的锁，在完成操作之前，工作站不会释放任何一个锁（保证原子操作），并且为了遵循缓存一致性规则。

Frangipani使用锁实现了两个几乎相反的目标。对于缓存一致性，Frangipani使用锁来确保写操作的结果对于任何读操作都是立即可见的，所以对于缓存一致性，这里使用锁来确保写操作可以被看见。但是对于原子性来说，锁确保了人们在操作完成之前看不到任何写操作，因为在所有的写操作完成之前，工作站持有所有的锁。

### Frangipani Log

我们需要能正确应对这种场景：一个工作站持有锁，并且在一个复杂操作的过程中崩溃了。比如说一个工作站在创建文件，或者删除文件时，它首先获取了大量了锁，然后会更新大量的数据，在其向Petal回写数据的过程中，一部分数据写入到了Petal，还有一部分还没写入，这时工作站崩溃了，并且锁也没有释放（因为数据回写还没有完成）。这是故障恢复需要考虑的有趣的场景。

Frangipani与其他的系统一样，需要通过预写式日志（Write-Ahead Log，WAL）实现故障可恢复的事务（Crash Recoverable Transaction）。

标准WAL: 当一个工作站需要完成涉及到多个数据的复杂操作时，在工作站向Petal写入任何数据之前，工作站会在Petal中自己的Log列表中追加一个Log条目，这个Log条目会描述整个的需要完成的操作。<U>只有当这个描述了完整操作的Log条目安全的存在于Petal之后，工作站才会开始向Petal发送数据</U>。所以如果工作站可以向Petal写入哪怕是一个数据，那么描述了整个操作、整个更新的Log条目必然已经存在于Petal中。

Frangipani在实现WAL时，有一些不同的地方。
1. Frangipani为每个工作站都保存了一份独立的Log，而不是所有工作站共用一个log
2. 工作站的Log存储在Petal，而不是本地磁盘中

当工作站从锁服务器收到了一个Revoke消息，要自己释放某个锁，它需要执行好几个步骤:
1. 首先，工作站需要将内存中还没有写入到Petal的Log条目写入到Petal中。
2. 之后，再将被Revoke的Lock所保护的数据写入到Petal。
3. 最后，向锁服务器发送Release消息。

### 故障恢复（Crash Recovery）
发生故障时可能会有这几种场景：
1. 要么工作站正在向Petal写入Log，所以这个时候工作站必然还没有向Petal写入任何文件或者目录。
    - 工作站WS1在向Petal写入任何信息之前就故障了。这意味着，当其他工作站WS2执行恢复，查看崩溃了的工作站的Log时，发现里面没有任何信息，自然也就不会做任何操作。之后WS2会释放WS1所持有的锁。
2. 要么工作站正在向Petal写入修改的文件，所以这个时候工作站必然已经写入了完整的Log。
    - 执行恢复的工作站WS2会从Log的最开始向后扫描，直到Log的序列号不再增加，因为这必然是Log结束的位置。工作站WS2会检查Log条目的更新内容，并向Petal执行Log条目中的更新内容。

## Distributed Transactions

分布式锁和分布式事务类似，都是由原来的在单一机器的本地锁和本地事务，演变为多机器下多个线程/进程操作的分布式锁和分布式事务。

<u>分布式系统环境下由不同的节点之间通过网络远程协作，在多个节点上执行ACID属性的事务称为**分布式事务**。通常有一个实体协调所有的过程，以确保事务的所有部分都适用于所有相关系统</u>。可能包括数据库、存储管理器、文件系统、消息系统和其他数据管理器等系统，例如用户注册送积分事务、创建订单减库存事务，银行转账事务等都是分布式事务。

例1：本地事务
```
begin transaction；
    //1.本地数据库操作：张三减少金额
    //2.本地数据库操作：李四增加金额
commit transation;
```
分布式事务
```
begin transaction；
    //1.本地数据库操作：张三减少金额
    //2.远程调用：让李四增加金额
commit transation;
```
例2：甲和乙AA吃饭是一个事务，老板是事务协调器。甲和乙各自付钱相当于两个分布式原子操作(两台机器上的原子操作)，只有当甲和乙都付钱了，老板才能让甲和乙吃饭(提交事务)，否则老板退钱给某乙方(回滚事务)

**可串行化（Serializable）**
<u>通常来说，隔离性（Isolated）意味着可串行化（Serializable）</u>。

**可串行化是指：并行的执行这些事务得到的结果，与按照某种串行的顺序来执行这些事务，可以得到相同的结果**（如果事物间存在共同的操作对象，这是考虑可串行化的关键；如果不同的事务没有共同的操作对象，则并发执行结果和执行先后顺序没有关系）。

<u>例：T1(转账事务)，T2(读检查事务)。如果先执行T1，再执行T2，得到X=11，Y=9，打印字符串“11，9”；如果先执行T2，再执行T1，得到X=11，Y=9，打印字符串“10，10”。这是两种串行执行的合法结果。如果我们同时执行这两个事务，看到了这两种结果之外的结果，那么我们运行的数据库不能提供序列化执行的能力（也就是不具备隔离性 Isolated）</u>

<u>分布式事务主要有两部分组成。第一个是并发控制（Concurrency Control），其等同于可串行化--> 两阶段锁是可串行化的工具；第二个是原子提交（Atomic Commit）--> 主要两阶段提交来保证</u>。


### 并发控制（Concurrency Control）
在并发控制中，主要有两种策略。
1. **悲观并发控制（Pessimistic Concurrency Control）**：在事务使用任何数据之前，它需要获得数据的锁。<u>如果一些其他的事务已经在使用这里的数据，锁会被它们持有，当前事务必须等待这些事务结束，之后当前事务才能获取到锁</u>。在悲观系统中，如果有锁冲突，比如其他事务持有了锁，就会造成延时等待。所以这里需要为正确性而牺牲性能。 --> **两阶段锁(Two-Phase Locking)**
2. **乐观并发控制（Optimistic Concurrency Control）**: 直接执行读写操作(不用考虑其他事务是否正在读写你操作的数据)，通常这些执行会在一些临时区域，在事务最后的时候，再检查是不是存在其他干扰的事务。如果没有这样的其他事务，那么你的事务就完成了，并且也不需要承受锁带来的性能损耗，因为操作锁的代价一般都比较高；但是如果有其他的事务在同一时间修改了你操作的数据，并造成了冲突，则必须要Abort当前事务，并重试。

这两种策略哪个更好取决于不同的环境。如果冲突非常频繁，则更倾向于使用悲观并发控制，因为如果冲突非常频繁的话，在乐观并发控制中你会有大量的Abort操作。如果冲突非常少，那么乐观并发控制可以更快，因为它完全避免了锁带来的性能损耗。

#### 两阶段锁(Two-Phase Locking)
数据库中的每一个数据对象都有两种锁：(S)hared lock 和 e(X)clusive lock。shared lock允许多个锁并存；exclusive lock具有排它性，只有一个锁存在。

两阶段锁(Two-Phase Locking):
1. **growing phase (expanding phase)**: <u>在执行任何数据的读写之前，先获取锁。事务必须持有任何已经获得的锁，直到事务提交或者Abort，不允许在事务的中间过程释放锁(持有锁直到事务结束, 持有的锁逐渐减加，这里是一个事务的多个参与者所持有的锁)
    > <font color=Red> 读数据的事务需要持有共享锁，共享锁不允许其他事务更新数据。面对大量读事务，Spanner 读事务不使用共享锁 --> 多版本并发控制 </font>
2. **shrinking phase (contracting phase)**: <u>事务提交或Abort后，释放锁，**如果一个transaction释放了它所持有的任意一个锁，那它就再也不能获取任何锁**(持有的锁逐渐减少)</u>。

两阶段锁非常容易产生死锁。例如我们有两个事务，T1读取记录X，之后再读取记录Y，T2读取记录Y，之后再读取记录X。如果它们同时运行，这里就是个死锁。实际上，事务有各种各样的策略，包括了判断循环，超时来判断它们是不是陷入到这样一个场景中。如果是的话，数据库会Abort其中一个事务，撤回它所有的操作，并表现的像这个事务从来没有发生一样。

### 两阶段提交（Two-Phase Commit）

一个事务的多个操作分布在多个机器上，只有所有的操作都成功，事务才会提交，否则回滚，这保证了事务的原子性。

有一个计算机会用来管理多个事务，它被称为**事务协调者（Transaction Coordinator， TC）**。参与一个分布式事务执行操作的多个服务器被称为**参与者（Participants, Ps）**。在一个完整事务系统，会有很多并发运行的事务，每个事务有一个事务ID(Transaction ID, TD), 每个持有数据的服务器会维护一个锁的表单，用来记录锁被哪个事务所持有，则一个事务的TD会被多台参与者的锁表单记录（这里要满足两阶段锁）。

Two-Phase Commit简称为2PC，原理图如下，当一个客户端请求提交时，TC 进入两阶段提交：
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/2PC.jpg" width = "80%" height = "80%">
</div>

1. **准备阶段（Prepare phase）**：事务管理器给每个参与者发送 Prepare 消息，每个数据库参与者在本地执行事务(这里事务不断获取锁，TD被多个P的锁表单记录)，并写本地的 Undo/Redo 日志，此时事务没有提交。（Undo 日志是记录修改前的数据，用于数据库回滚，Redo 日志是记录修改后的数据，用于提交事务后写入数据文件）
2. **提交阶段（commit phase）**：如果事务管理器收到了参与者的执行失败或者超时消息时，直接给每个参与者发送回滚（Rollback）消息；否则，发送提交（Commit）消息。为了遵守前面的锁规则（两阶段锁），事务参与者会释放锁（这里不论Commit还是Abort都会释放锁）。

两阶段提交之后，TC 使得事务的结果对于外部是可见的，然后向客户端回复。

### 故障恢复（Crash Recovery）
假设有一个事务协调器TC，和两个事务参与者P1和P2，分析以下故障场景，以及应对措施。

#### 服务器崩溃场景
**参与者崩溃**:
1. P2在回复TC的Prepare消息之前崩溃: 从TC的角度来看，B没有回复Yes，TC也就不能Commit。如果P2不能针对Prepare消息发YES，则会恢复No, Abort 事务。
2. P2在回复Prepare消息YES之后，收到TC的commit消息之前崩溃了: P1收到Commit,执行事务分包给它的那一部分，持久化存储结果，并释放锁。P2 的log(持久化到硬盘)记录修改前和修改后的数据，以及事务的锁信息。当再次收到Commit， 直接读取log，将提交的数据写入文件。
3. P2在回复收到 Commit 之后崩溃：因为没有收到ACK，事务协调者会再次发送Commit消息，P2 执行后续操作。

**协调器崩溃**:
1. TC在崩溃前没有发送Commit消息: 直接 Abort 事务
2. TC已经向P1发送了Commit消息，但是还没来得及向P2发送Commit消息就崩溃了: TC会将结果和事务ID写入存在磁盘中的Log。TC必须在重启的时候准备好向B重发Commit消息，以确保两个参与者都知道事务已经提交了。

#### 消息在网络传输的时候丢失
1. TC发送了Prepare消息，但是并没有收到所有的Yes/No消息: TC会重发Prepare消息；TC在等待YES/NO 过程中，如果因为消息丢包或者某个参与者崩溃了，而超时了，它可以直接决定Abort这个事务。
2. B收到了Prepare消息，并回复了Yes，但因为网络问题，一直没收到 TC 的 Commit 消息：如果等待Commit消息超时了，参与者不允许Abort事务，它必须无限的等待Commit消息，这里通常称 **Block**。

如果事务协调者成功的得到了所有参与者的ACK，那么它就知道所有的参与者知道了事务已经Commit或者Abort，TC可以删除Log中相关事务的信息。如果一个参与者收到了Commit或者Abort消息，完成了它们在事务中的相应工作，持久化存储事务结果并释放锁，那么在它发送完ACK之后，参与者也可以完全删除相关的事务log。

### 2PC的局限性与改进

两阶段提交性能很差。
1. 大量交互情况下，TC和Ps 都需要持久化到硬盘，并且存在等待。
2. 遇到故障，可能进入 Block 状态，长时间等待

Raft 和 2PC 有点类似，都是一个 Leader 和 多个 Follower, 但是二者是两个概念的东西, Raft 只有过半参与者可用即可，2PC要求所有参与者都可用。可以二者结合，将每个TC和每个P都分别是一个Raft 集群，消息会在这些集群之间传递。如下图：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/2PC_raft.jpg)
这样很复杂，Spanner使用了这种结构。


## Spanner

## Opimistic Concurrency Control

## Big Data: Spark

## Cache Consistency: Memcached at Facebook

## COPS, Causal Consistency

## Fork Consistency, Certificate Transparency

## Bitcoin

## Blockstack

## RocksDB

















