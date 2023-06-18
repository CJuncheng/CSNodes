<!---<h1 align="left"> CPP Notes </h3>--->
# CPP Notes
<h2 align="left">目录</h3>
[toc]

## CPP基础
### 数值操作
1. 产生格雷码
```cpp
vector<int> grayCode(int n) {
      vector <int> ans;
      for(int i =0; i<1<<n; i++){
         ans.push_back(i^(i>>1));
      }
      return ans;
}
```
### CPP命名规范
```
namespace name: service_straits   
class name: TcpModbusMag
function name: getStatus()
common variable name(include bool style): num_complated_entry
class variable name: class_name_
typedef, struct name: FuncPointerTypedef
const variable name: kDayInWeek
define, emun variable name : EMUN_VARIABLE_NAME
class ,vector ,struct ,enum object name : VectorTypeName  vectorObject
```

### 指针函数与函数指针

1. 指针函数是一个函数，返回返回变量的指针
```cpp
int* fun(int x,int y)
{

}
```
2. 函数指针是一个指针变量，指向一个函数的地址
```
//使用第一种方式定义一个函数指针变量
int (*fun)(int,int)　  //定义一个函数指针变量，指向一个函数的地址，这个函数返回类型为int,并且有两个int参数

//使用第二种方式定义一个函数指针变量
typedef int (*FunPointersTypedef)(int int);//定义一个函数指针类型
FunPointersTypedef fun;               

//函数指针变量的赋值
fun=&Function
fun=Function

//函数指针变量的调用
int x=(*fun)(a,b)
int x=fun(a, b)   
```   
```cpp
//a classic example 
#include<iostream>
using namespace std;
int addFun(int a,int b)
{
    return a+b;
}
int main(int argc,*char argv[])
{ 
    int (*add)(int,int)=addFun;
    //两种方法都可以
    //int (*add)(int,int)=&addFun;
　　　　　　　　
    int result=(*add)(1,2);
    //建议使用第一种方法
    // int result=add(1,2)
　　
    cout<<"通过函数指针调用返回结果:"<<result<<endl;
    cout<<"通过直接调用函数返回结果:"<<result=addFun(1,2)<<endl;
    return 0;
}
```
     // another example　in "func_pointers_application.cpp"
3. 返回值为函数指针的函数
```cpp
typedef int (*RET_FuncPointersTypedef)(int,int)
RET_FunPointersTypedef testFun(char op)
{
    　
}
```
```cpp
// a complicated function, returning function pointers,I even couldn't understand it...
void ( *set_malloc_handler(void(*f)()) )  ()  //a function,it is equivalent to the following contents
//<=> 
typedef void (* REC_FuncPointersTypedef)()
REC_FuncPointersTypedef set_malloc_handler(void(*f)()) 
//void(*f)() is a actual parameter 
//void ( * ...... )  () 定义返回类型，返回类型为实参
//However, void (*set_malloc_handler_1)() is a function pointers variable
```


### main 函数参数和返回值
main 函数的标准写法
```
int main(int argc, char* argv[])
{}
```
main 函数的参数argv是指针数组，其数组元素的类型是字符指针，每一个字符指针都指向一个特定的字符串; argc表示数组argv的长度。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/main_arg_ret.png)
1. **获取参数**

argv[0]存储函数名称及其路径，而argv[argc]为空指针，编译下列程序``test.cpp``得到``test.exe``
```cpp
#include <iostream>
int main (int argc, char* argv[])
{
	std::cout << "argc: " << argc << std::endl; 
	std::cout << "argv:" << std::endl;
	for(int i = 0; i < argc+1; ++i)
		printf("argv[%d]=%s\n", i, argv[i]);
    return 0;
}
```
在命令行输入：``test.exe par1 par2``,结果如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/main_arg_ret_2.png )

2. **获取返回值**
编译下列代码``test.cpp``得到``test.exe``
```cpp
#include <iostream>
int main (int argc, char* argv[])
{
    return 12345;
}
```
在命令行输入：``test.exe``,回车后， 再输入``echo %errorlevel%``就可以看到打印返回值(实验中没有打印出来)。

main函数执行之前，主要就是初始化系统相关资源：
1. 设置栈指针 
2. 初始化static静态和global全局变量，即data段的内容 
3. 将未初始化部分的赋初值：数值型short，int，long等为0，bool为FALSE，指针为NULL，等等，即.bss段的内容 
4. 运行全局构造器，估计是C++中构造函数之类的吧 
5. 将main函数的参数，argc，argv等传递给main函数，然后才真正运行main函数


**参考：**
- [探寻main函数的“标准”写法，以及获取main函数的参数、返回值](https://www.cnblogs.com/kangjianwei101/p/5219819.html)


### 宏
 #define命令是C语言中的一个宏定义命令，它用来将一个标识符定义为一个字符串，该标识符被称为宏名，被定义的字符串称为替换文本. 使用宏时是简单的代码段替换.
#### #define的概念
##### 简单的宏定义
```
#define <宏名>　　<字符串>
例： #define PI 3.1415926
```
注：使用简单的宏定义可以定义一些常量，区分``简单宏定义``和``const valtype`` 的区别(优先使用const)

##### 带参数的宏定义(宏函数)
```
#define <宏名> (<参数表>) (<宏体>)
例： #define Max(a, b) ( (a)>(b) ? (a) : (b))
```
注：简单代码段使用宏函数比使用函数好，免去了函数调用的开销，提高运行效率
#### 宏的使用情形
1. 头文件包含
    把源程序中的#include 扩展为文件正文，即把包含的.h文件找到并展开到#include 所在处
2. 条件编译
     预处理器根据``#if``和``#ifdef``等编译命令及其后的条件，将源程序中的某部分包含进来或排除在外，通常把排除在外的语句转换成空行
     - #if 命令
        ```
        #if 整型常量表达式1
            程序段1
        #elif 整型常量表达式2
            程序段2
        #elif 整型常量表达式3
            程序段3
        #else
            程序段4
        #endif
        ```
    - #ifdef
        ```
        #ifdef  宏名
            程序段1
        #else
            程序段2
        #endif
        ```
3. 宏展开
    预处理器将源程序文件中出现的对宏的引用展开成相应的宏定义，即本文所说的``#define``的功能，由预处理器来完成，这里是单纯的替换与展开
4. 避免头文件重复引用
    ```
    #ifndef INCLUDE_NAME_H 
    #define INCLUDE_NAME_H 
        //头文件内容 
    #endif
    ```

#### define中的三个特殊符号：``#，##，#@`` 
```
#define Conn(x,y) x##y
#define ToChar(x) #@x
#define ToString(x) #x
```  

- ``x##y``表示x连接y, 如``int n = Conn(123,456); // 结果就是n=123456;``
- ``#@x``表示给x加单引号, 如``char a = ToChar(1); //结果就是a='1';``
- ``#x``表示给x加双引号, 如``std::string str = ToString(12345); //结果就是std="12345";``

### 内联
**1.内联函数的介绍：**

内联函数从源代码层看，有函数结构，而在编译后，却不具备函数性质，内联函数不是在调用时发生控制转移，而是在编译时将函数体嵌入每一个调用处，编译时，类似宏替换，使用函数体替换调用处的函数名，在代码中用inline关键字修饰，在类中声明的成员函数，自动转换为内联函数

> 1）类中的成员函数默认是内联函数(要求成员函数没有循环和递归，当其中存在循环的递归的时候，编译器会将其默认为一个普通函数处理)
> 
> 2）内联函数不允许有循环或者递归语句
> 
> 3）内联函数的定义必须出现在第一次调用内联函数之前

**2.内联函数的优点：**

> 1）有参数类型检测，更加安全
> 
> 2）内联函数是在程序运行时展开，而且是进行参数传递
> 
> 3）inline关键字只是对编译器的一个定义，如果函数本地不符合内联函数的标准，编译器就会将这个函数当作是普通函数

**3.内联函数的缺点：**

> 1）因为内联函数是在调用处展开，所以会使代码边长，占用更多内存

#### 内联函数和宏定义的主要区别

> 1）内联函数在运行时可调试，而宏定义不可以
> 2）编译器会对内联函数的参数类型做安全检查或自动类型转换，而宏定义则不会
> 3）内联函数可以访问类的成员变量，而宏定义则不能
> 4）宏定义是在预处理阶段由预处理器替换，内联函数是在编译时编译器执行。

**参考：**
- [【C++】内联函数(inline)和宏定义(# define)的优劣及其区别](https://www.cnblogs.com/yinbiao/p/11606554.html)




### Const限定符 

#### const 与引用
```cpp
const int a = 5;
const int& r = a; // 对常量的引用
int& r2 = a; // 错误，让非常量的引用指向一个常量对象

int b = 0;
const int& r3 = b; // b可以修改，但不能通过 r3 修改

int cmptr(const int& args){ //常常用引用传参

}
```

#### const 与指针
1. 指向常量的指针(const 在*左边)
指针的指向可以修改，但是指针指向的内容不可以修改。(**可以不初始化**)
```cpp
int a = 5;
const int* p = &5; //表示不能通过p改变p所指的对象的值，p的值是可以变的，所以p可以不用初始化
```
> **指向"常量"的引用或指针，是指不能通过该引用或指针改变其所指的对象，但这个"常量"可以通过其他方式改变**

2. 常量指针(const 在*的右边)
指针的指向不可以修改，指针指向的内容可以修改。(**必须初始化**)
```cpp
int a = 5;
int* const p = &a;
```



#### 顶层 const 与 底层 const
1. 顶层 const：表示指针本身是常量(常量指针)
2. 底层 const: 修饰指针所指的对象，不能通过这个指针修改所指的对象(指向常量的指针)

#### const 修饰函数
1. 返回值
2. 函数参数
3. 成员函数：不能修改成员变量
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/const_member.webp)


#### 常量表达式和 constexpr

const expression 是指值不会改变在编译阶段就能得到计算结果的表达式；

在复杂系统很难分辨一个初始 值到底是不是常量表达式，C++11 规定将变量声明为 constexpr 由编译器来验证变量是不是常量表达式。尽量使用 constexpr。

constexpr 把定义的对象置为顶层 const:
```cpp
int a = 5;
constexpr int * np = &a; // 常量指针 
```

### assert()函数
原型定义：
```
#include <assert.h>
void assert( int expression );
```
运行逻辑：
```
if(条件成立) {
    程序正常运行；
} else {
    向stderr打印一条出错信息，然后通过调用 abort 来终止程序运行
}
```
使用 assert 的缺点是，频繁的调用会极大的影响程序的性能，增加额外的开销。在调试结束后，可以通过在包含 #include 的语句之前插入 #define NDEBUG 来禁用 assert 调用，示例代码如下：
```cpp
#define NDEBUG
#include <assert.h>
```


**参考：**
- [C/C++ assert()函数用法总结](https://www.cnblogs.com/lvchaoshun/p/7816288.html)




### 自定义sort()
```
// 离远点最近的前k个点
// 1.
struct {
    bool operator()(const vector<int>& vec1, const vector<int>& vec2){
        return pow(vec1[0], 2)+pow(vec1[1], 2) < pow(vec2[0], 2)+pow(vec2[1], 2);
    }
} compareLess;

// 2. 推荐方式
bool compare(const vector<int>& vec1, const vector<int>& vec2){
    return pow(vec1[0], 2)+pow(vec1[1], 2)< pow(vec2[0], 2)+pow(vec2[1], 2);
}

// 3. lamda表达式
[](const vector<int>& vec1, const vector<int>& vec2) {
    return pow(vec1[0], 2)+pow(vec1[1], 2)< pow(vec2[0], 2)+pow(vec2[1], 2);
}
vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
    sort(points.begin(), points.end(),compare);
    return vector<vector<int>>(points.begin(), points.begin()+k);
}
```

### 去重
```cpp
#include <algorithm>
#include <vector>

vector<int> arr = {1, 2, 3, 8, 4}
std::sort(arr.begin(), arr.end())
arr.erase(std::unique(arr.begin(), arr.end()), arr.end());
```

### override 关键字
override关键字作用： <u>如果派生类在虚函数声明时使用了override描述符，那么该函数必须重载其基类中的同名函数，否则代码将无法通过编译</u>。override关键字起到限定作用，使得代码更规范。

### static 关键字
**全局（静态）存储区：分为 DATA 段和 BSS 段。DATA 段（全局初始化区）存放初始化的全局变量和静态变量；BSS 段（全局未初始化区）存放未初始化的全局变量和静态变量**。

1. **static 修饰全局变量或者函数时，只能在该文件访问他们**。全局变量默认是有外部链接性的。

2. **静态成员变量实际上就是类域中的全局变量，必须初始化，且只能在类体外，如果不赋值一般会被默认初始化为 0**。初始化时不受private和protected访问限制。
静态成员变量属于类，不属于对象，属于**被很多对象共享的变量**。**static成员变量不占用对象内存，在所有对象外开辟内存**，不会随着某一对象的声明周期结束而结束，**编译时在静态数据区分配内存，到程序结束时才释放**。

3. **static 静态成员函数没有 this 指针，只能访问静态成员变量和成员函数；普通成员函数具有 this 指针，可以访问任意成员**。

> 静态成员变量/函数可以通过对象访问, 可以通过作用域访问




## 顺序容器

## 泛型算法

### lambda 表达式

## 关联容器

## 动态内存与智能指针
### new and delete
```cpp
int* p1 = new int(10);
delete p1; // p1指向一个动态分配的对象或为空

int* p2 = new int[10];
delete [] p2; // p2指向一个动态分配的数组或为空

int* p3 = new int[10]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // 分配内存并构造对象，初始化对象
delete [] p3;

int* const p4 = new int[10]; //分配内存并构造对象
int a;
int* q = p4;
while(cin >> a && q != (p4+10))
    *q++ = a; // 给对象赋值
const size_t size = q - p4;
delete[] p4;
```

**new有灵活上的局限性，其中一方面表现在它将内存分配和对象构造组合在一起**。当需要动态分配大块内存时，需要按需构造对象，allocator类将**内存分配和对象构造分开**
> allocator是封装了molloc的模板类
```cpp
#include<memory>
const int n = 10;
allocator<string> alloc;
string* const p = alloc.allocate(n); // 分配内存
string s;
string* q = p;
while(cin >> s && q != (p+n)){
    alloc.construct(q++, s); // 在已经分配的内存基础上，构造对象
}
const size_t size = q-p;
cout << size << endl;
for(size_t i = 0; i < size; ++i)
    cout << p[i] << endl;

while(q != p){ //释放已经构造的string
    alloc.destroy(--q);
}
alloc.deallocate(p, size); // 释放内存
```

**new / delete 与 malloc / free的异同**
相同点：
- 都可用于内存的动态申请和释放
不同点：
- new/delete 是运算符，malloc/free 是库函数
- new/delete 自动分配内存大小，返回具体类型指针；malloc/free 手动指定分配内存大小，返回 void*
- new/delete 内部封装了malloc/free，会调用构造函数和析构函数；malloc/free 仅仅内存分配和释放

**new / delete 的实现**
- new 内存分配和构造对象一起，先调用 operator new() 分配内存，再调用构造函数构造对象
- delete 先调用析构函数，再调用 operator delete()释放内存


### shared_ptr
shared_ptr 允许多个指针指向同一对象。每个shared_ptr所指对象都关联一个**计数器**。**当用一个shared_ptr 初始化另一个shared_ptr，或者将它作为参数传递给一个函数，或者作为函数返回值**，计数器加1。shared_ptr**被赋予新值或shared_ptr被销毁**（如一个局部shared_ptr离开作用域），计数器减一。当计数器为0时，自动释放其所管理的对象。

```cpp
#include<memory>

shared_ptr<string> p1 = make_shared<string>("hello"); // 推荐使用 make_shared<T>
// or auto p1 = make_shared<string>("hello");
shared_ptr<string> p2 = new string("hello"); //error, 不能将内置指针隐式转换为智能指针
shared_ptr<string> p2(new string("hello")); // right, 只能使用直接初始化形式

// 拷贝赋值
shared_ptr<string> r = make_shared<string>("world");
shared_ptr<string> q(new string("hello"));
r = q;
// or
shared_ptr<string> q1(r, d); // d 是自定义删除器的函数指针

if(!p1.unique()) // 判断所指对象的被引用次数是否为1(true)
    p1.reset(new string(*p1)); // p1不是对象的唯一用户；分配新的拷贝, 及 p1指向拷贝的对象
*p1 += newVal; // 此刻 p1 是唯一用户，可以修改对象的值
```

### unique_ptr
unique_ptr独占其所指的对象。
```cpp
#include<memory>
/*unique_ptr管理对象的生命周期，离开作用域时自动销毁*/
std::unique_ptr<Cat> cat(new Cat("maoliang"));
//std::unique_ptr<int[]> arrp(new int [10]); //指向数组的unique_ptr
std::unique_ptr<Cat> cat = make_unique<Cat>("maoliang"); //c++14

/*unique_ptr 不可拷贝，不可复制，因为不能多个unique_ptr指向同一个对象（资源）*/
cat1->eat(); //调用成员函数
(*cat1).eat();

std::unique_ptr<Cat> cat2 = cat1; // error
std::unique_ptr<Cat> cat2(cat1); // error
std::unique_ptr<Cat> cat2 = std::move(cat1); //ca1 的控制权移交给cat2, cat1 被置成空指针 
std::unique_ptr<Cat> cat2(cat1.release()); //ca1 的控制权移交给cat2, cat1 被置成空指针 
// OR cat2.reset(cat1.release())
```
可以拷贝或赋值一个将要被销毁的unique_ptr。如：
```cpp
unique_ptr<int> clone(int p)
{
    return unique_ptr<int> (new int(p));
}
```

自定义删除器
```
unique_ptr<objT, delT> p(new objT, fcn)
//delT代表函数指针类型，如 decltype(delT)*
```

### weak_ptr
weak_ptr 不单独使用（没有实际用处），只能和 shared_ptr 类型指针搭配使用， 获取 shared_ptr 指针的一些状态信息。

weak_ptr 是一种不控制所指对象生存周期的智能指针，它指向一个由 share_ptr 管理的对象。将一个 weak_ptr 绑定到一个 share_ptr， 不会改变 share_ptr 的引用计数。
```
weak_ptr<T> w;
weak_ptr<T> w(sp); 使用share_ptr sp直接初始化w
w = p;  
w.use_count(); // 与w共享对象的shared_ptr数量
```

```cpp
auto p = make_shared<int>(42);
weak_ptr<int> wp(p);// p的引用计数并未增加
```

不能直接调用 weak_ptr 直接访问对象， 必须调用lock来检查weak_ptr所指对象是否为空。如果存在，lock返回一个指向共享对象的 shared_ptr
```cpp
if(shared_ptr<int> sp = wp.lock()){//如果 np 不为空则条件成立
    // 在 if 中 sp 与 wp 共享对象
} 
```

**参考：**
- [C++——动态内存分配new--delete](https://www.cnblogs.com/southcyy/p/10271981.html)

## 拷贝控制
类通过定义特殊的成员函数控制**对象拷贝，赋值，移动或销毁**操作，包括：拷贝构造函数，拷贝赋值运算符，析构函数，移动构造函数，移动赋值运算符
### 拷贝构造函数

如果**构造函数的第一个参数是自身类类型的引用，且所有其他参数(如果有的话)都有默认值**，则此构造函数时就是拷贝构造函数。
```cpp
class Foo{
public:
    Foo(); // 默认构造函数
    Foo(const Foo&); // 拷贝构造函数
}
```
拷贝构造函数参数必须是引用类型，因为如果不是引用类型，则参数传递
时值传递，调用拷贝构造函数，从而造成死循环。
> 拷贝构造函数通常被隐式的使用，因此构造函数不应该是explicit(explicit要求显示使用，不允许隐式转换)
> 如果类未定义自己的拷贝构造函数，编译器为他合成一个

拷贝构造函数在以下几种情况下会被使用：
- **拷贝初始化(用 = 定义变量)**
  - **直接初始化调用普通构造函数** 
- 将一个对象作为实参传递给非引用类型的形参
- 一个返回类型为非引用类型的函数返回一个对象
- 用花括号列表初始化一个数组中的元素或一个聚合类中的成员
- 初始化标准容器或调用其 insert/push 操作时，容器会对其元素进行拷贝初始化

### 拷贝赋值运算符
类可以控制其对象如何赋值。拷贝赋值运算符是一个重载运算符，定义为类的成员函数，左侧运算对象绑定到隐含的 this 参数。它会将**右侧运算对象的每个非 static 成员赋予左侧运算对象的对应成员**

```cpp
Sales_data trans, accum;
trans = accum; // 使用 Sales_data 的拷贝赋值运算符
```
如果类未定义自己的拷贝赋值运算符，编译器会为它合成一个。
> **赋值运算符通常应该返回一个指向其左侧运算对象的引用**
```cpp
Sales_data&
Sales_data::operator=(const Sales_data &rhs)
{
    bookNo = rhs.bookNo; // 调用 string::operator=
    units_sold = rhs.units_sold; //使用内置的 int 赋值
    revenue = rhs.revence; // 使用内置 double 赋值
    return *this; // 返回此对象的一个引用
}
```

### 析构函数
析构函数是类的一个成员函数，释放对象使用的资源，销毁非静态数据成员。名字由波浪号接类名构成。它**没有返回值，也不接受参数**。**析构函数不能被重载**。

当析构函数为空，空函数执行完后，非静态数据成员会逐个销毁，**成员函数在析构函数体之后隐含的析构阶段中进行销毁**。

**内置类型（整型，实型）没有析构函数，销毁内置类型什么也不需要做。**
> 隐式销毁一个内置指针类型成员不会 delete 它所指的对象（不会执行析构函数）。
> 当指向一个对象的引用或指针离开作用域时，析构函数不会被执行。

无论何时一个对象销毁，就会自动调用其析构函数：
- 变量离开其作用域时被销毁
- 当一个对象被销毁时，其成员被销毁
- 容器(无论是标准容器还是数组)被销毁时，其元素被销毁
- 对于动态分配的对象，当对指向它的指针应用 delete 运算符时被销毁
- 对于临时对象，当创建它的完整表达式结束时被销毁

总结：
- 如果一个类需要析构函数，几乎可以肯定它也需要一个拷贝构造函数和以拷贝赋值运算符。
- 如果一个类需要一个拷贝构造函数，几乎可以肯定它也需要一个拷贝赋值运算符


### 移动对象

#### 右值引用&左值引用

**右值引用**就是必须绑定到右值的引用, 通过 \&\&获得。右值引用只能绑定到**临时对象**，所引用的对象将要销毁，该对象没有其他用户。因此可以自由地移动其资源，我们可以从绑定到右值引用的对象“窃取”状态。

**左值引用**, 也就是 “常规引用”, **不能**绑定到**要转换的表达式、字面常量或返回右值的表达式**。而右值引用恰好相反, 可以绑定到这类表达式, 但**右值引用不能绑定到一个左值对象**上。

返回左值的表达式包括返回左值引用的函数及赋值、下标、解引用和前置递增/ 递减运算符, 返回右值的包括返回非引用类型的函数及算术、关系、位和后置递增/ 递减运算符。可以看到, **左值的特点是有持久的状态, 而右值则是短暂的**。

```cpp
int i = 42;
int &r = i; // 正确
int &&rr = i; // 错误： 不能将一个右值引用绑定到一个左值上
int &r2 = i*42; // 错误，不能将一个左值引用绑定到右值
const int &r3 = i*42; // 正确，我们可以将一个 const 的左值引用绑定到右值上
int &&rr2 =i*42; // 正确：将右值引用绑定到右值
```

**变量是左值**， 不能将右值引用绑定到一个变量上，但可以使用 ``move()``函数**显示地将一个左值转换成对应的右值引用类型**。
```cpp
#include<utility>
int &&rr3 = std::move(rr1);
```
对于使用 move 后的移后源对象 rr1, 我们**可以销毁它，也可以给他赋值，但不能再使用它**。

#### 移动构造函数与移动赋值运算符

移动构造函数不分配任何新内存。移动构造函数要确保移后源对象被销毁是无害的。
```cpp
StrVec::StrVec(StrVec&& s) noexcept // 移动操作不应该抛出异常
: elements(s.element), first_free(s.first_free), cap(s.cap){
    s.elements = s.first_free = s.cap = nullptr;
}
```
> 不要随意使用移动操作，当我们调用 move 时，必须绝对确认移后源对象没有其他用户。

移动赋值运算符不抛出任何异常，并且要正确处理自赋值
```cpp
StrVec& StrVec::StrVec(StrVec&& rhs) noexcept 
{
    //直接检测自赋值
    if(this!=rhs){
        free; // 释放已有元素
        elements = rhs.elements; // 从 rhs 接管资源
        first_free = rhs.first_free;
        cap = rhs.cap;
        s.elements = s.first_free = s.cap = nullptr;
    }
    return *this;
}
```

> 只用当一个类没有定义任何自己版本的拷贝控制成员，且它所有数据成员都能移动构造或移动赋值时，编译器才会为他合成移动构造函数或移动赋值运算符。

#### 成员函数与右值引用,左值引用
区分移动和拷贝的重载函数通常有一个版本接受一个 ``const T&``，而另一个版本接受一个 ``T&&``。

不管一个右值还是左值都可以调用成员函数。
可以向右值赋值
```cpp
s1+s2 = "wow!";
```
但可以使用**引用限定符**避免向右值赋值。
```cpp
T& operator=(const T&) &; //只能向可修改的左值赋值
```

一个函数可以同时使用 const 和引用限定符，但引用限定符必须跟在 const 之后
```cpp
T f() const &;
```
> 如果一个成员函数有引用限定符，则具有相同参数列表的所有版本都必须有引用限定符。

## 重载运算与类型转换

### 函数对象
如果类重载了函数调用运算符()，则该类的对象称作**函数对象**(function object)，也叫仿函数。我们使用这种对象，像使用函数一样，即**函数对象是一个类对象，但行为像函数**。函数对象内部可以记录状态。**函数对象通常不定义构造函数和析构函数**
- 一元仿函数：重载 operator() 获得一个参数
- 二元仿函数：重载 operator() 获得两个参数

```cpp
struct absInt {
    int operator() (int val) const { 
        return val < 0 ? -val : val;
    }
}

int i = -42;
absInt absObj;
int ui = absObj(i) // 一元仿函数

cout << absObj.count << endl; // 打印 函数对象被调用的次数
```

**仿函数和 lambda 表达式类似，常常作为函数参数**, lambda表达式就是函数对象
```cpp
// 1. 使用 lambda 表达式
stable_sort(words.begin(), words().end(),[](const string& lhs, const string& rhs){
    return lhs.size() < rhs.size();
});

// 2. 使用 函数对象
class ShorterString{
public:
    bool operator() (const string& lhs, const string& rhs) const {
        return lhs.size() < rhs.size();
    }
}

stable_sort(words.begin(), words().end(), ShorterString());
// ShorterString() 作为实参
```


**函数对象可以作为类型与模板结合使用**，而普通函数没有类型(函数名类型是函数指针)
```cpp
int SumSquares(int total, int value)
{
    return total + value * value;
}

template<class T>
class SumPowers
{
private:
    int power;
public:
    SumPowers(int p) :power(p) { }
    const T operator() (const T & total, const T & value)
    { //计算 value的power次方，加到total上
        T v = value;
        for (int i = 0; i < power - 1; ++i)
            v = v * value;
        return total + v;
    }
};

vector<int> v = { 1,2,3,4,5,6,7,8,9,10 };
int res1 = accumulate(v.begin(), v.end(), 0, SumSquares);
int res2 = accumulate(v.begin(), v.end(), 0, SumPowers<int>(3));
```
对于 ``accumulate(v.begin(), v.end(), 0, SumSquares)``，函数名是函数指针，编译器编译时将 accumulate 模板实例化后得到的模板函数定义如下：
```cpp
int accumulate(vector <int>::iterator first, vector <int>::iterator last, int init, int(*op)(int, int))
{
    for (; first != last; ++first)
        init = op(init, *first);
    return init;
}
```

对于 ``accumulate(v.begin(), v.end(), 0, SumPowers<int>(3))``，SumPowers 是类模板的名字，SumPowers<int>(3) 就是一个 SumPowers<int> 类的临时对象。编译器编译时将 accumulate 模板实例化后得到的模板函数定义如下：
```cpp
int accumulate(vector<int>::iterator first, vector<int>::iterator last, int init, SumPowers<int> op)
{
    for (; first != last; ++first)
        init = op(init, *first);
    return init;
}
```



## 类&继承

C++ 通过 public、protected、private三个关键字来控制成员变量和成员函数的访问权限，分别表示：公有的、受保护的、私有的。

### 类访问权限
在类的内部，无论成员属于哪一种，都可以互相访问；但是在类的外部，如果通过类的对象(object)访问类的成员，只能访问 public 属性的成员，不能访问  protected、private 属性的成员。

派生类可以访问（继承）基类中所有的**非私有成员**。因此基类成员如果不想被派生类的成员函数访问（继承），则应在基类中声明为 private。

我们可以根据访问权限总结出不同的访问类型，如下所示：
|访问      | public成员 | protected成员 | private成员 |
|:----:    | :---    |    :----:  |  ----:  |
|类内      | 可以    | 可以        | 可以    |
|类的对象  | 可以   | 不可以      | 不可以  |
|派生类成员| 可以   | 可以        | 不可以  |
|友元函数  | 可以   | 可以        | 可以    |
|外部的类  | 可以   | 不可以      | 不可以  |

一般而言，成员变量和成员函数的子函数设置为``private``属性。

### 基类 & 派生类
一个类可以派生自多个类，它可以从多个基类继承数据和函数。定义一个派生类，我们使用一个类派生列表来指定基类。类派生列表以一个或多个基类命名，形式如下：
```bash
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,...
{
    <派生类类体>
};
```

### 继承类型

|继承方式/类成员| 基类的公共成员       |  基类的保护成员     | 基类的私有成员 |
|:----:        | :---               |   :----:          |  ----:        |
|公有继承       | 派生类的**公有**成员 | 派生类的**保护**成员 | 派生类中不可见 |
|保护继承       | 派生类的**保护**成员 | 派生类的**保护**成员 | 派生类中不可见 |
|私有继承       | 派生类的**私有**成员 | 派生类的**私有**成员 | 派生类中不可见 |


### 注意事项
- **class** 成员默认是**私有**的，默认继承方式是**私有**继承；**struct** 成员默认是**公有**的，默认继承方式是**公有**继承
- **class**一些成员需要隐藏不被外界访问，但又需要被派生类的成员访问（不是间接访问），需要将基类的这种成员设置为保护类型；
- 声明 public、protected、private 的顺序可以任意；
- 在一个类中，public、protected、private 可以出现多次，每个限定符的有效范围到出现另一个限定符或类结束为止。但为了使程序清晰，应该使每种限定符只出现一次。
- 友元关系不能继承，也就是说基类友元不能访问子类私有和保护成员。
- 当创建子类对象时, 构造函数的调用顺序：**静态数据成员的构造函数 -> 父类的构造函数 -> 非静态的数据成员的构造函数 -> 自己的构造函数**

## 多态

虚函数，重载实现CPP的多态

多态： **基类指针动态调用子类的虚函数**

定义一个指向基类的指针，然后每次访问之前对这个指针变量进行赋值（将子类的指针赋值给基类）
- 基类指针通过动态绑定调用子类的虚函数(子类重载父类虚函数，开发主动权由子类决定)
- 这里非虚函数是稳定的，虚函数是不稳定的

> **抽象基类的析构函数应该写成虚函数**，因为基类的指针动态绑定指向子类，如果基类析构函数是非虚的，则基类的指针无法通过动态绑定调用子类的析构函数。
> 构造函数不能是虚函数，析构函数常常是虚函数。如果构造函数时虚函数，那么调用构造函数就需要去找vptr，而此时vptr还没有初始化。



## 静态绑定与动态绑定

几个名词定义：
- **静态类型：对象在声明时采用的类型，在编译期既已确定；**
- **动态类型：通常是指一个指针或引用目前所指对象的类型，是在运行期决定的；**
- **静态绑定：绑定的是静态类型，所对应的函数或属性依赖于对象的静态类型，发生在编译期；**
- **动态绑定：绑定的是动态类型，所对应的函数或属性依赖于对象的动态类型，发生在运行期（用父类的指针调用子类的方法）；**

从上面的定义也可以看出，非虚函数一般都是静态绑定，而虚函数都是动态绑定（如此才可实现多态性）
```cpp
 class A
{
public:
    /*virtual*/ void func(){ std::cout << "A::func()\n"; }
};
class B : public A
{
public:
    void func(){ std::cout << "B::func()\n"; }
};
class C : public A
{
public:
    void func(){ std::cout << "C::func()\n"; }
};

//测试结果
C* pc = new C(); //pc的静态类型是它声明的类型C*，动态类型也是C*；
B* pb = new B(); //pb的静态类型和动态类型也都是B*；
A* pa = pc;      //pa的静态类型是它声明的类型A*，动态类型是pa所指向的对象pc的类型C*；
pa = pb;         //pa的动态类型可以更改，现在它的动态类型是B*，但其静态类型仍是声明时候的A*；
C *pnull = NULL; //pnull的静态类型是它声明的类型C*,没有动态类型，因为它指向了NULL；

pa->func();      //A::func() pa的静态类型永远都是A*，不管其指向的是哪个子类，都是直接调用A::func()；
pc->func();      //C::func() pc的动、静态类型都是C*，因此调用C::func()；
pnull->func();   //C::func() 不用奇怪为什么空指针也可以调用函数，因为这在编译期就确定了，和指针空不空没关系；
```
如果注释掉类C中的func函数定义，其他不变，即
```cpp
class C : public A
{
};

pa->func();      //A::func() 理由同上；
pc->func();      //A::func() pc在类C中找不到func的定义，因此到其基类中寻找；
pnull->func();   //A::func() 原因也解释过了；
```
如果为A中的void func()函数添加virtual特性，其他不变，即
```cpp
class A
{
public:
    virtual void func(){ std::cout << "A::func()\n"; }
};

pa->func();      //B::func() 因为有了virtual虚函数特性，pa的动态类型指向B*，因此先在B中查找，找到后直接调用；
pc->func();      //C::func() pc的动、静态类型都是C*，因此也是先在C中查找；
pnull->func();   //空指针异常，因为是func是virtual函数，因此对func的调用只能等到运行期才能确定，然后才发现pnull是空指针；
```
**分析**：
1. 如果基类A中的func不是virtual函数，那么不论pa、pb、pc指向哪个子类对象，对func的调用都是在定义pa、pb、pc时的静态类型决定，早已在编译期确定了。
同样的空指针也能够直接调用no-virtual函数而不报错（这也说明一定要做空指针检查啊！），因此静态绑定不能实现多态；

2. 如果func是虚函数，那所有的调用都要等到运行时根据其指向对象的类型才能确定，比起静态绑定自然是要有性能损失的，但是却能实现多态特性；

**本文代码里都是针对指针的情况来分析的，但是对于引用的情况同样适用。**

**静态绑定和动态绑定的区别：**
1. <font color=Red>静态绑定发生在编译期，动态绑定发生在运行期；</font>
2. <font color=Red>对象的动态类型可以更改，但是静态类型无法更改；</font>
3. <font color=Red>要想实现多态，必须使用动态绑定；</font>
4. <font color=Red>在继承体系中只有虚函数使用的是动态绑定，其他的全部是静态绑定；</font>

**建议：**
1. <font color=Red>绝对不要重新定义继承而来的非虚(non-virtual)函数</font>（《Effective C++ 第三版》条款36），因为这样导致函数调用由对象声明时的静态类型确定了，而和对象本身脱离了关系，没有多态，也这将给程序留下不可预知的隐患和莫名其妙的BUG；
2. <font color=Red>绝对不要重新定义一个继承而来的virtual函数的缺省参数值，因为缺省参数值都是静态绑定（为了执行效率），而virtual函数却是动态绑定。</font>
> 缺省参数指在定义函数时为形参指定缺省值（默认值）。

示例如下：
```cpp
class E
{
public:
    virtual void func(int i = 0)
    {
        std::cout << "E::func()\t"<< i <<"\n";
    }
};
class F : public E
{
public:
    virtual void func(int i = 1)
    {
        std::cout << "F::func()\t" << i <<"\n";
    }
};

void test2()
{
    F* pf = new F();
    E* pe = pf;
    pf->func(); //F::func() 1  正常，就该如此；
    pe->func(); //F::func() 0  哇哦，这是什么情况，调用了子类的函数，却使用了基类中参数的默认值！
}
```
## 关于vptr(虚表指针)和vtbl(虚函数表)
在CPP中，如果一个类中定义了虚函数，则编译器就会在内存空间中开辟空间以存放vtbl(指针数组), 每个vtbl对应一个指针，即vptr。vtbl存放虚函数的地址，以指向虚函数，因此能够通过虚函数表调用虚函数。关于vtbl的对象模型如下图(**侯捷(C++课件): 面向对象高级编程(下)**)所示：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/vtbl_1.png)

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/vtbl_2.png)

**总结：**
1. 当子类继承了父类虚函数，但没有重新定义该虚函数，则该虚函数**不需要被重新分配地址**(继承的普通函数也不需要被重新分配地址)，子类的虚函数表存放该虚函数的地址，图中蓝色框（0x401F10）所示。
2. 当子类继承了父类虚函数，并且重新定义了该虚函数，则该虚函数**需要被重新分配地址**，子类的虚函数表存放该虚函数的地址，图中非蓝色框（0x401F10）所示。由于虚函数对应的是动态绑定，从而实现多态。
3. <u>虚函数表(编译阶段)在构造函数之前生成， c++ 构造函数**调用完**(不是执行完)后才生成对象，对象内存空间开辟后写入对象的虚表指针(这样才能访问虚函数表)，**类对象的虚函数指针vptr是在运行阶段确定的**。因此**构造函数不能是虚函数，但构造函数内部可以调用虚函数**。析构函数往往写成虚函数，子类往往需要重载析构函数</u>

叙述说明，每个类有虚函数，就会创建一个虚函数表，这个**虚函数表在编译阶段生成**，在构造函数调用之前就生成了。子类从父类继承虚函数表，如果子类重载了虚函数，在子类虚函数表的那个虚函数指针就会被新的虚函数指针覆盖。类对象的内存模型，在**编译阶段**，确定好虚表指针vptr(编译阶段为空)，和其他普通成员的地址(不同的对象公用编译阶段的代码，可能产生不同的结果，每个对象绑定一个this指针)，这里的普通成员的地址也包含从父类继承的普通成员地址，这是静态绑定的关键。在运行阶段，让每个类的 vptr 指向虚函数表，这是动态绑定的关键。因为先调用构造函数，才生成对象，然后对象的vptr才不为空，才能调用虚函数，如果构造函数是虚函数就会有矛盾。但在构造函数内部可以调用虚函数，因为构造函数调用之后，生成了对象，对象在运行阶段可以调用虚函数。


## C模仿CPP实现面向对象

面向对象三个部分：封装、继承、多态
**封装：**

```cpp
// ====================== 定义父类，实现封装 ======================
// 定义父类结构体
typedef struct {
    int age;
    int weight;
}Animal;
//构造函数
void AnimalCtor(Animal *this, int age, int weight)
{
    this->age = age;
    this->weight = weight;
}
int AnimalGetAge(Animal *this) { return this->age; }
int AnimalGetWeight(Animal *this) { return this->weight; }
```
```cpp
// 测试
int main ( int argc, char* argv[] ) 
{
   Animal a;
   AnimalCtor(&a, 5, 4);
   printf("Animal:\n age = %d, weight = %d\n\n", AnimalGetAge(&a), AnimalGetWeight(&a));

   return 0;
}
```
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/c_packaging.jpg)

**继承：**

```cpp
// ====================== 定义子类，实现继承 ======================
typedef struct {
    Animal parent;
    int legs;
} Dog;
void DogCtor(Dog *this, int age, int weight, int legs){
    // 首先调用父类构造函数，来初始化从父类继承的数据
    AnimalCtor(&this->parent, age, weight);
    this->legs = legs;
}
int DogGetAge(Dog *this) { return AnimalGetAge(&this->parent); }
int DogGetWeight(Dog *this) { return AnimalGetWeight(&this->parent); }
int DogGetLegs(Dog *this) { return this->legs; }
```

```cpp
// 测试
int main ( int argc, char* argv[] ) 
{
   Dog d;
   DogCtor(&d, 2, 3, 5);
   printf("Dog:\n age = %d, weight = %d, legs = %d", DogGetAge(&d), DogGetWeight(&d), DogGetLegs(&d));

   return 0;
}
```
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/c_inheritance.jpg)

**多态：**
CPP通过虚函数的动态绑定来实现多态。那么可以使用C语言模仿CPP实现多态：创建虚函数表和虚表指针。
<!--子类在继承父类之后，在内存中又会开辟一块空间来放置子类自己的虚表，然后让继承而来的虚表指针指向子类自己的虚表。-->
```cpp
//vtbl.h
#include "stdio.h"
#include "assert.h"
// ====================== 父类 ======================
struct AnimalVTable; //父类虚函数表的前置声明
typedef struct //父类结构体
{
    struct AnimalVTable *vptr; //虚函数表指针
    int age;
    int weight;
} Animal;
struct AnimalVTable //父类虚函数表
{
    void (*say)(Animal *this); //定义虚函数指针变量
    void (*eat)(Animal *this);
};

static void _Animal_Say(Animal *this) //父类虚函数声明+定义
{
    // 因为父类Animal是一个抽象的东西，不应该被实例化。
    // 父类中的这个虚函数不应该被调用，也就是说子类必须实现这个虚函数。
    // 类似于C++中的纯虚函数。
    assert(0);
}
static void _Animal_Eat(Animal *this) { assert(0); } //父类虚函数声明+定义

void AnimalCtor(Animal *this, int age, int weight) //父类构造函数
{
    static struct AnimalVTable animal_vtbl = {_Animal_Say, _Animal_Eat}; //定义一个父类虚函数表变量并初始化
    this->vptr = &animal_vtbl; //让父类虚表指针指向上面的虚函数表
    this->age = age;
    this->weight = weight;
}
// ====================== 子类 ======================
typedef struct
{
    Animal parent;
    int legs;
} Dog;

static void _Dog_Say(Dog *this) { printf("dog say\n"); }       //子类虚函数声明+定义
static void _Dog_Eat(Dog *this) { printf("dog eat: meat\n"); } //子类虚函数声明+定义

void DogCtor(Dog *this, int age, int weight, int legs) //子类构造函数
{
    AnimalCtor(&this->parent, age, weight); // 首先调用父类构造函数，来初始化从父类继承的数据
    static struct AnimalVTable dog_vtbl = {(void (*)(Animal *))_Dog_Say, (void (*)(Animal *))_Dog_Eat}; //定义一个子类虚函数表变量并初始化
    this->parent.vptr = &dog_vtbl;  // 把从父类中继承得到的虚表指针指向子类自己的虚表
    this->legs = legs;
}
```
```cpp
//测试
#include "vtbl.h"
// 测试多态：传入的参数类型是父类指针
void Animal_Say (Animal *this)
{
   // 如果this实际指向一个子类Dog对象，那么这个虚表指针指向子类自己的虚表，
   // 因此，this->vptr->say将会调用子类虚表中的函数。this->vptr
   this->vptr->say(this);
}
void Animal_Eat(Animal *this) { this->vptr->eat(this); }

int main (int argc, char* argv[]) 
{
   Dog d;  // 在栈中创建一个子类Dog对象
   DogCtor(&d, 2, 3, 5);
   Animal *pa = (Animal *)&d; // 把子类对象赋值给父类指针
   Animal_Say(pa);            // 传递父类指针，将会调用子类中实现的虚函数。
   Animal_Eat(pa);

   return 0;
}
```
> 注：在执行Animal_Say(pa)的时候，虽然参数类型是指向父类Animal的指针，但是实际传入的pa是一个指向子类Dog的对象，这个对象中的虚表指针vptr指向的是子类中自己定义的虚表dog_vtbl，这个虚表中的函数指针say指向的是子类中重新定义的虚函数_Dog_Say，因此this->vptr->say(this)最终调用的函数就是_Dog_Say。




## 模板与泛型编程

虚函数是运行时多态，动态绑定； 函数对象与模板泛型编程结合是编译时多态，静态绑定。



## 标准库特殊设施

### 正则表达式


## 用于大型程序的工具
### 异常处理
```
//方式一
#include <iostream>
#include <exception>
try
{
    //某些语句隐式抛出异常
}
catch(std::exception &e)//0r catch(std::exception const &e)
{
    std::cout<<e.what()<<std::endl;    //基类异常
}
```
```
//方式二
class bad_1{};
class bad_2{};
class bad_3{};
...
void duper(void)
{
	...
	if(oh_no)
        throw bad_1(); // bad_1表示异常类型
    if(rats)
        throw bad_2();
    if(drat)
	    throw bad_3();
}   
　　　...
try
{
    duper(void);
}
catch(bad_1 &be)
{
    throw; // 重新抛出异常
}
catch(bad_2 &be)
{}
catch(bad_3 &be)
{}
catch(...)//捕获所有异常， 一般放最后
// or catch(exception)
{}
```

C++通过 ``try{} catch(){}`` 处理异常。
**try{}**
try{} 里面是可能发生异常的程序。在 try{} 里面通过 ``throw 异常对象``来**显式抛出异常**。当执行一个throw时，throw后面的语句将不再执行。**throw 异常对象的类型可以是自己定义的，也可以是标准异常类型**，异常对象可以直接初始化，如使用语句``throw exception_type(exception_type_val)``。在 try{} 里面某些语句触发标准异常，则该异常被隐式抛出。
> 1. throw抛出的对象一定要是可以复制的（C++ Primer中的原话是: 异常对象是通过复制被抛出表达式的结果创建, 该结果必须是可以复制的类型）
> 2. 不要抛出(throw)一个数组或者函数, 原因是, 和函数参数一样, 数组和函数类型实参, 该实参自动转换为一个指针.
> 3. C++异常说明: void func(int) throw(exception type list), 表明函数func会且仅会抛出list中列举的异常对象类型, throw()表示不会抛出任何异常(空异常类型列表)

**catch(){}**
无论是通过 throw 显示抛出异常，还是通过隐式抛出异常，都可以通过 ``catch(exception_type e){}``来捕获异常。一个try可以对应许多个catch(){}, 通过栈展开的方式，沿着嵌套函数调用链不断查找，直到找到与异常匹配的 catch 语句为止。如果异常抛出后没有被正常捕获，则系统将调用 ``terminate`` 函数来终止当前函数。在搜寻与异常匹配的catch语句时，挑选出来的应该是第一个与异常匹配的 catch 语句，但不一定是最佳匹配catch, **越是专门的 catch 越应该放置整个catch列表前端**。一般要求异常类型和catch声明的类型是精准匹配的。 

``catch(exception_type e){}``类似一个函数参数类似，catch的参树类型可以是引用类型，也可以是非引用类型：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/catch.png)


> 如果多个catch语句类型之间存在继承关系。应该把继承链最低端的类放最前面

**重新抛出异常**
一条catch语句通过**重新抛出**的操作将将异常传递给另一个catch。重新抛出使用空 ``throw;`` 语句。空的throw语句只能出现在catch语句或catch 直接或间接调用的函数中

**noexcept异常**
noexcept有两层含义：**当跟在函数参数列表后面时它是异常说明符；而当作为 noexcept 异常说明的 bool 实参出现时，它是一个运算符**
```cpp
void regroup(int) noexcept; //不会抛出异常。 noexcept是异常说明符

void regroup(int) noexcept（false）; //会抛出异常。 noexcept是运算符
void regroup(int) noexcept（true）; //不会抛出异常。 noexcept是运算符
void f() noexcept(noexcept(g())) // f 和 g的异常说明符一致
```

> 如果一个基类虚函数承诺不抛出异常。则后续派生的虚函数也不能抛出异常；如果一个基类虚函数承诺抛出异常，则后续派生的虚函数可以抛出异常，也可以不抛出异常。

**异常类型**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/stdexcept2.png)
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cpp/exception.png)

### 命名空间
1. 命名空间的定义
```
#include <iostream>
using namespace std;
 
// 定义第一个命名空间
namespace firstnamespace
{
  　void func()
　　{
        cout << "Inside first_space" << endl;
  　}

    // 定义第二个命名空间，第二个命名空间嵌套在第一个命名空间内
    namespace second_space
    {
        void func()
　　    {
            cout << "Inside second_space" << endl;
        }
    }
}
　　　
//注：命名空间的定义可以放在不同的文件下，即命名空间不连续，如下：
namespace net     |  namespace net                      
{                 |  {
    1...          |     2...
}                 |  } 
```
> 通常情况下，不把 ``#include`` 放在命名空间内部
2. 命名空间的调用
```　　
//方式一：使用第二个命名空间，先声明所需要的命名空间e
using namespace first_space::second_space;
int main ()
{
    // 调用第二个命名空间中的函数
    func();
    return 0;
}

//方式二：不声明使用命名空间来调用命名空间的类和函数，即: first_space::second_space::func();
```
> using 指示使得特定命名空间中的所有名字都成为可见的；而一个 using 声明只能引入特定命名空间中的成员 

### 多重继承与虚继承

## C++ 多线程

```cpp
/**多线程并发**/
#include <thread>
#include <iostream>
#include <vector>
#include <mutex>
void work(int i)
{ 
    std::cout << "do some work... arg:" << i << " " << "thread id: " << std::this_thread::get_id()<< std::endl;  
}
int main(int argc, char *argv[])
{
    int thread_num = std::thread::hardware_concurrency();
    /*
    std::thread worker(work, 10);
    //worker.join(); // 等待worker线程结束，才继续往下执行
    worker.detach(); // 分离子线程，join() 和 detach() 不能同时使用
    if(worker.joinable()){
        worker.join(); 
    }
    std::cout << "main thread" << std::endl;
    */

    // 多线程并发
    std::vector<std::thread> workers;
    for(int i = 0; i < thread_num; ++i)
        workers.emplace_back(work, i); // 函数指针， 传入参数
    for(int i = 0; i < thread_num; ++i){
        workers[i].join();
    }
    //std::cout << cnt << std::endl;
    std::cout << "main thread id: " << std::this_thread::get_id() << std::endl;    
 
    return 0;
}
```

```cpp
/**数据竞争与锁**/
#include <thread>
#include <iostream>
#include <vector>
#include <mutex>

std::mutex mtx;
static int cnt = 1;  // 最多有一个线程同时访问全局变量, 避免数据竞争
void work(int i)
{ 
    for(int i = 0; i < 100; ++i){ 
        mtx.lock();
        ++cnt;
        mtx.unlock();
    }
}
int main(int argc, char *argv[])
{
    int thread_num = std::thread::hardware_concurrency(); //检测系统最多支持的线程数
    // 多线程并发
    std::vector<std::thread> workers;
    for(int i = 0; i < thread_num; ++i)
        workers.emplace_back(work, i); // 函数指针， 传入参数
    for(int i = 0; i < thread_num; ++i){
        workers[i].join();
    }
    std::cout << cnt << std::endl;
    return 0;
}
```

```cpp
/**并发bug--带锁跑路，及应对**/
#include <thread>
#include <iostream>
#include <vector>
#include <mutex>
std::mutex mtx;
void work(int i)
{ 
    std::lock_guard<std::mutex> my_lock(mtx); //1. my_lock构造时给 mtx 加锁， 析构时解锁
    for(int i = 0; i < 100; ++i){ 
        mtx.lock();
        try{
            int* arr = new int[1000];
            throw "exception";
        } catch (...) {
            std::cout << "handle exception" << std::endl;
            mtx.unlock(); //2. 如果没有解锁，造成死锁。 1. 和 2. 任选一个
            return;  // 1. my_lock 析构函数被调用
        }
        mtx.unlock();
    }
}
int main(int argc, char *argv[])
{
    int thread_num = std::thread::hardware_concurrency(); //检测系统最多支持的线程数

    std::vector<std::thread> workers;
    for(int i = 0; i < thread_num; ++i)
        workers.emplace_back(work, i); // 函数指针， 传入参数
    for(int i = 0; i < thread_num; ++i){
        workers[i].join();
    }
    return 0;
}
```

