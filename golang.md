# Golang Notes
<h2 align="left">目录</h2>
[toc]

## 简介

Go 语言被设计成一门应用于搭载 Web 服务器，存储集群或类似用途的巨型中央服务器的系统编程语言。对于高性能分布式系统领域而言，Go 语言无疑比大多数其它语言有着更高的开发效率。它提供了海量并行的支持，这对于游戏服务端的开发而言是再好不过了。具有简洁、快速、安全、并行、有趣、开源、内存管理、数组安全、编译迅速等优势。新增Goroutine, channel, defer等；舍弃枚举，泛型等。

**Go语言的优点**
1. **语法先进**。在语言层面支持线程（goroutine）和管道（channel）。对线程间的加锁、同步支持良好。
2. **类型安全（type safe）**。内存访问安全（memory safe），很难写出像 C++ 一样内存越界访问等 bug。
3. **支持垃圾回收（GC）**。不需要用户手动管理内存，这一点在多线程编程中尤为重要，因为在多线程中你很容易引用某块内存，然后忘记了在哪引用过。
4. **简洁直观**。没 C++ 那么多复杂的语言特性，并且在报错上很友好。

## Go基础
### 语言结构
```go
package main

import (
    "fmt"
    "time"
)

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```
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
    在编码过程中，可能会遇到没有名称的变量、类型或方法。虽然这不是必须的，但有时候这样做可以极大地增强代码的灵活性，这些变量被统称为匿名变量。匿名变量的特点是一个下画线，下画线本身就是一个特殊的标识符，被称为空白标识符。
    ```go
    a, _ := GetData()
    _, b := GetData()
    fmt.Println(a, b)
    ```
    > <font color=Red>匿名变量不占用内存空间，不会分配内存。匿名变量与匿名变量之间也不会因为多次声明而无法使用。</font>



4. 指针变量
    ```go
    var ptr_name *v_type = &v //v的类型是v_type
    var ptr_name = &v
    ptr_name := &v
    ```
    > 类似C++中nullptr表示空指针， nil代表golang中的空指针；类似C/C++, *ptr表示取地址内容，解引用
5. 数组变量
    ```go
    // 1. 声明与定义分开
    var arrName0 [10] int;
    arrName0 = [10] int{12, 1, 3, 6, 7, 4, 8, 4, 2, 1};

    // 2. 声明与定义一起
    var arrName1 [10] int = [16] int{12, 1, 3, 6, 7, 4, 8, 4, 2, 1};
    // or
    var arrName2 = [10] int{12, 1, 3, 6, 7, 4, 8, 4, 2, 1};
    // or
    arrName3 := [10] int{12, 1, 3, 6, 7, 4, 8, 4, 2, 1};
    ```

6. 结构体变量
    结构体的定义
    ```
    type struct_variable_type struct {
        member definition
        member definition
        ...
        member definition
    }
    ```
    结构体的变量
    ```
    //声明与定义分开
    var struct_variable_name structure_variable_type // 结构体变量声明
    var struct_pointer_variable_name *structure_variable_type // 结构体指针变量声明
    struct_variable_name = structure_variable_type {value1, value2...valuen}
    struct_pointer_variable_name = &struct_variable_name

    //声明与定义一起
    struct_variable_name := structure_variable_type {value1, value2...valuen} 

    var struct_pointer_variable_name *structure_variable_type = &struct_variable_name
    var struct_pointer_variable_name = &struct_variable_name
    struct_pointer_variable_name := &struct_variable_name
    ```
    结构体的访问成员
    ```
    //均是.
    struct_variable_name.member
    struct_pointer_variable_name.member
    ```
    例子
    ```go
    package main
    import "fmt"
    type Books struct {
        title string
        author string
        subject string
        book_id int
    }
    func main() {
        book1 := Books{"Go 语言",
                "www.w3cschool.cn",
                "Go 语言教程",
                6495407}
        book2 := &book1
        
        fmt.Printf( "Book title : %s\n", book2.title)
    }
    ```
> <font color=Red>Golang 抛弃了枚举变量</font> 

### 常量
常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。
**常量的定义格式**
```
const identifier [type] = value
```
你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。
- 显式类型定义： ``const b string = "abc"``
- 隐式类型定义： ``const b = "abc"``

**常量用作枚举**
```go
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```
**常量与iota**
iota，特殊常量，可以认为是一个可以被编译器修改的常量。在每一个const关键字出现时，被重置为0，然后再下一个const出现之前，每出现一次iota，其所代表的数字会自动增加1。
```go
const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    // 结果： 0 1 2 ha ha 100 100 7 8
```

### 函数
和其他编程语言类似，需要一个main函数。
Go 语言函数定义格式如下：
```
func function_name( [parameter list] ) [return_types]{
   函数体
}
```
```go
func swap(x, y string) (string, string) {
   return y, x
}
```
go语言传递参数可以使用值传递，也可以使用指针(引用)传递

**defer语句**
Go语言中的defer语句会将其后面跟随的语句进行延迟处理
在defer所属的函数即将返回时，将延迟处理的语句按照defer定义的顺序逆序执行，即先进后出
```go
package main
import "fmt"
func main() {
	fmt.Println("开始")
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)
	fmt.Println("结束")
}
// 输出： 开始 -> 结束 -> 3 -> 2 -> 1
```


### 流程控制
```
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
}
//if还有一种特殊的写法，可以在if表达式之前添加一个执行语句，再根据变量值进行判断，代码如下：
if zt:=getStatus();zt!=0 {
    fmt.Println(zt) 
    return

switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
} 
/*变量 var1 可以是任何类型，而 val1 和 val2 则可以是同类型
的任意值。类型不被局限于常量或整数，但必须是相同的类型*/

for init; condition; post { }  /* 和 C 语言的 for 一样 */
for condition { } /*和 C 的 while 一样*/
for { } /*和 C 的 for(;;) 一样*/

```

### 关键字
#### type

type既可以定义类型，也可以起别名
**定义新类型**
type可以定义新类型，包括结构体、接口、函数、通道等
```go
type Person struct {
    Name string
    Age int
}

type Reader interface{
    Read() ([]byte, error)
}

type MiddlewareFunc func(ctx context.Context, next Handler) error

type IntChan chan int
```

**定义别名**
```go
type PersonType = Person
```


## 切片(Slice)

Go 语言切片是对数组的抽象。Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

### 定义切片
1. 你可以声明一个未指定大小的数组来定义切片：
    ```
    var identifier []type
    ```

2. 切片不需要说明长度。或使用make()函数来创建切片:
    ```
    var slice1 []type = make([]type, len)
    ```
    也可以简写为
    ```
    slice1 := make([]type, len)
    ```
    也可以指定容量，其中capacity为可选参数。

    ```
    make([]type, length, capacity)
    ```
    这里 len 是数组的长度并且也是切片的初始长度。


### 切片初始化
1. 直接初始化切片，\[\]表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3
    ```
    s :=[] int {1,2,3 } 
    ```
2. 初始化切片s,是数组arr的引用
    ```
    s := arr[:] 
    ```
3. 将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片
    ```
    s := arr[startIndex:endIndex] 
    ```
4. 缺省endIndex时将表示一直到arr的最后一个元素
    ```
    s := arr[startIndex:] 
    ```

5. 缺省startIndex时将表示从arr的第一个元素开始
    ```
    s := arr[:endIndex] 
    ```
6. 通过切片s初始化切片s1
    ```
    s1 := s[startIndex:endIndex] 
    ```
7. 通过内置函数make()初始化切片s,\[\]int 标识为其元素类型为int的切片
    ```
    s :=make([]int,len,cap) 
    ```

### len() 和 cap() 函数

切片是可索引的，并且可以由 len() 方法获取长度。切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。
以下为具体实例：
以下为具体实例：
```go
package main
import "fmt"
func main() {
   var numbers = make([]int,3,5)
   printSlice(numbers)
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
输出：
```
len=3 cap=5 slice=[0 0 0]
```

### 空(nil)切片
一个切片在未初始化之前默认为 nil，长度为 0，实例如下：

```go
package main
import "fmt"
func main() {
   var numbers []int
   printSlice(numbers)
   if(numbers == nil){
      fmt.Printf("切片是空的")
   }
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

### 切片截取
可以通过设置下限及上限来设置截取切片 ``[lower-bound:upper-bound]``
```
numbers := []int{0,1,2,3,4,5,6,7,8}  
number2 := numbers[:2]
```
### append() 和 copy() 函数
如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

```go
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
输出：
```
len=0 cap=0 slice=[]
len=1 cap=1 slice=[0]
len=2 cap=2 slice=[0 1]       
len=5 cap=6 slice=[0 1 2 3 4] 
len=5 cap=12 slice=[0 1 2 3 4]
```

## Map(集合)

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。

### 声明与初始化

**声明和初始化分开**
```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type
```
但是要注意，这样声明得到的是一个空的map，map的零值是nil，可以理解成空指针。所以我们不能直接去操作这个m，否则会得到一个panic。

我们声明了一个map之后，想要使用它还需要对它进行初始化。使用它的方法也很简单，就是使用make方法创建出一个实例来。它的用法和之前通过make创建元组非常类似：
```go
//创建一个空的 Map
map_variable = make(map[key_data_type]value_data_type)

map_variable = make(map[key_data_type]value_data_type, initialCapacity) 
```
> initialCapacity 是可选参数，用于指定 Map 的初始容量， 如果不指定 initialCapacity，Go 语言会根据实际情况选择一个合适的值。

**声明和初始化同时**
也可以使用字面量创建 Map：
```go
// 使用字面量创建 Map
m := map[string]int{
    "apple": 1,
    "banana": 2,
    "orange": 3,
}
```

### 添加元素

```go
map_variable[key] = value
```

### 其他
1. 获取元素
    ```go
    // 获取键值对
    v1 := m["apple"]
    v2, ok := m["pear"]  // 如果键不存在，ok 的值为 false，v2 的值为该类型的零值
    ```
2. 遍历Map
    ```go
    for k := range map_variable {
        fmt.Println("key:", k, "value:", map_variable[k])
    }
    for k, k := range countryCapitalMap {
        fmt.Println("key:", k, "value:", v)
    }
    ```

3. 删除元素：
delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。
    ```go
    delete(m, k)
    ```
4. 获取 Map 的长度：
    ```go
    // 获取 Map 的长度
    len := len(m)
    ```
5. 修改元素：
    ```go
    // 修改键值对
    m["apple"] = 5
    ```




## 范围(Range)
Go 语言中 range 关键字用于for循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引值，在集合中返回 key-value 对的 key 值。

对于映射，它返回下一个键值对的键。Range返回一个值或两个值。如果在Range表达式的左侧只使用了一个值，则该值是下表中的第一个值。  

<table class="reference"><tbody><tr><td>&nbsp;Range表达式</td><td><span>第一个值</span></td><td>&nbsp;第二个值[可选的]</td></tr><tr><td>&nbsp;Array or Slice: a [n]E</td><td>&nbsp;索引 i int</td><td>&nbsp;a[i] E</td></tr><tr><td>&nbsp;String: s string type</td><td>&nbsp;索引 i int</td><td>&nbsp;rune int</td></tr><tr><td>&nbsp;Map: m map[K]V</td><td>&nbsp;键 k K</td><td>&nbsp;值 m[k] V</td></tr><tr><td>&nbsp;Channel: c chan E</td><td>&nbsp;元素 e E</td><td>&nbsp;none</td></tr></tbody></table>

实例：
```go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```
输出：
```go
sum: 9
index: 1
a -> apple
b -> banana
0 103
1 111
```




## 接口(Interface)
Go 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

```go
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
```

实例
```go
package main
import "fmt"
type Phone interface { /* define interface type */
	speak() /* interfce method 1 */
	read()  /* interfce method 2 */
}

/* 接口的某一属性 */
type IPhone struct {
	name string
	id   int
}
type Samsung struct {
	name string
	id   int
}

func (myphone IPhone) speak() { /* implement method 1 based on IPhone */
	fmt.Printf("phone name %s, phone type %d\n", myphone.name, myphone.id)
}
func (myphone IPhone) read() { /* implement method 2 based on IPhone */
	fmt.Printf("IPhone read()")
}

func (myphone Samsung) speak() { /* implement method 1 based on Samsung */
	fmt.Printf("phone name: %s, phone type: %d\n", myphone.name, myphone.id)
}
func (myphone Samsung) read() { /* implement method 2 based on Samsung */
	fmt.Printf("Samsung read()")
}

func main() {
	//iphone := IPhone{"IPhone", 1}
	//samsung := Samsung{"samsung", 2}
	var phone1 Phone = IPhone{"IPhone", 1}
	phone1.speak()
	phone2 := Samsung{"Samsung", 2}
	phone2.speak()
}
```

## 反射



## 错误与异常处理


## 并发和同步原语
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

## 反射
Go语言提供了一种机制，在不知道具体类型的情况下，可以用反射来更新变量值，查看变量类型
```go
bookdetail := make(map[string]string)
fmt.Println(reflect.TypeOf(bookdetail))
```

**使用建议：**
1. 大量使用反射的代码通常会变得难以理解
2. 反射的性能低下，基于反射的代码会比正常的代码运行速度慢一到两个数量级
## Util
### 排序

**排序整数、浮点数和字符串切片**
对于[]int, []float, []string这种元素类型是基础类型的切片使用sort包提供的下面几个函数进行排序。
- `sort.Ints`
- `sort.Floats`
- `sort.Strings`

```go
import "sort"
s := []int{4, 2, 3, 1}
sort.Ints(s)
fmt.Println(s) // 输出[1 2 3 4]
```

**排序任意数据结构**
- 使用`sort.Sort`或者`sort.Stable`函数。
- 他们可以排序实现了sort.Interface接口的任意类型

一个内置的排序算法需要知道三个东西：序列的长度，表示两个元素比较的结果，一种交换两个元素的方式；这就是sort.Interface的三个方法：
```go
/*sort 内置接口与方法*/
type Interface interface { /* define interface type */
	Len() int           /* interfce method 1 */
	Less(i, j int) bool /* interface method 2 */
	Swap(i, j int)      /* interface method 3 */
}
```

自定义的类型上实现 srot.Interface 接口
```go
package main
import (
	"fmt"
	"sort"
)
type Person struct {
	Name string
	Age  int
}
type ByAge []Person                /* 调用接口的某一属性 */

func (a ByAge) Len() int           { return len(a) } /* implement method 1*/
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age } /* implement method 2*/
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] } /* implement method 3*/

func main() {
	family := []Person{
		{"David", 2},
		{"Alice", 23},
		{"Eve", 3},
		{"Bob", 25}}

	sort.Sort(ByAge(family))
	fmt.Println(family)
}
```