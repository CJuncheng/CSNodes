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


### Const限定符
- Const常量不能被修改，否则产生未定行为
- 因为Const对象一旦创建后就不能再改变，故const对象必须初始化
- 
#### Const

### C++的异常处理
```
//方式一
#include <iostream>
#include <exception>
try
{

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
        throw bad_1();
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
{}
catch(bad_2 &be)
{}
catch(bad_3 &be)
{}
catch(...)//捕获所有异常
{}
```

### C++的命名空间
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

### 右值引用&&


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



## 关联容器

## 多态

虚函数，重载实现CPP的多态

定义一个指向基类的指针，然后每次访问之前对这个指针变量进行赋值，
。。。。

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
注：在执行Animal_Say(pa)的时候，虽然参数类型是指向父类Animal的指针，但是实际传入的pa是一个指向子类Dog的对象，这个对象中的虚表指针vptr指向的是子类中自己定义的虚表dog_vtbl，这个虚表中的函数指针say指向的是子类中重新定义的虚函数_Dog_Say，因此this->vptr->say(this)最终调用的函数就是_Dog_Say。


## 动态内存分配

**参考：**
- [C++——动态内存分配new--delete](https://www.cnblogs.com/southcyy/p/10271981.html)


## 标准库特殊设施

### 正则表达式




