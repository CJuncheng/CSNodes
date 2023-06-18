# Data Structure And Algorithm Notes

<h2 align="left">目录</h3>
[toc]

## 复杂度

复杂度分析是数据结构与算法的核心精髓，指在不依赖硬件、宿主环境、数据集的情况下，粗略推导，考究出算法的效率和资源消耗情况, 包括时间复杂度和空间复杂度
### 时间复杂度
首先从CPU的角度看待程序，每行代码执行的操作都包括：``读程序``，``写程序``，``运算``。这里粗略估计，忽略每行代码读程序和写程序的时间
假设**每行代码执行（运算）的时间都是一样**的，为**单位时间**（即假设程序执行一次均消耗单位时间）

#### 大$O$记号
$$T(n)=O(f(n))$$
其中：
- $T(n)$: 程序执行总时间
- $n$: 数据规模大小
- $f(n)$: 程序执行的总次数
- $O$：程序执行总时间$T(n)$与$f(n)$成正比
> 大$O$记号按最坏的情况来估计程序执行时间；大$\Omega$记号按最好情况估计程序执行时间；大$\Theta$记号按平均情况来估计程序执行时间。后文只分析大$O$记号。

![三种不同记号比较](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/big_O_sign.png)
 
<!--
<div align="center">
<img src=images/default/table.png width=60%>
</div>
-->
 
大$O$记号的性质：
> - 对任意常数$c>0$, $O(f(n))=O(cf(n))$
> - 对任意常数$a>b>0$, $O(n^a+n^b)=O(n^a)$

#### 常见时间复杂度
1. 常数阶$O(1)$
```
public void sum(int n) {
    int sum = 0; // 执行一次
    sum = n*2; // 执行一次
    System.out.println(sum); // 执行一次
}
```
程序执行常数次，与问题规模$n$无关，复杂度记为$O(1)$

2. 对数阶$O(logn)$
```
public void logarithm(int n) {
    int count = 1; // 执行一次
    while (count <= n) { // 执行logn次
        count = count*2; // 执行logn次
    }
}
```
该段代码什么时候会停止执行呢？是当count大于n时。也就是说多少个2相乘后其结果值会大于$n$，即$2^x=n$。由$2^x=n$可以得到$x=logn$，所以这段代码时间复杂度是$O(logn)$。

3. 线性阶$O(n)$
```
public void circle(int n) {
    for(int i = 0; i < n; i++) { // 执行n次
        System.out.println(i); // 执行n次
    }
}
```

4. 线性对数阶 $O(nlogn)$

线性对数阶$O(nlogn)$就是将一段时间复杂度为$O(logn)$的代码执行$n$次
```
public void logarithm(int n) {
    int count = 1;
    for(int i = 0; i < n; i++) { // 执行n次
        while (count <= n) { // 执行logn次
            count = count*2; // 执行nlogn次
        }
    }
}
```

5. 平方阶 $O(n^2)$

双重for循环
```
public void square(int n) {
    for(int i = 0; i < n; i++){ // 执行n次
        for(int j = 0; j <n; j++) { // 执行n次
            System.out.println(i+j); // 执行n方次
        }
    }
}
```

![复杂度比较](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/complexity_compare.png )

#### 复杂度分析
示例如下(限定条件: $0<n$ 且 $0<x$ 且$n$和$x$为整数)：
```
 public int Function(int n, int x)
 {
    int sum = 0;
    for (int i = 1; i <= n; ++i)
    {
        if (i == x)
            break;
            sum += i;
    }
    return sum;
  }
```
> - 当$x>n$时，此代码的时间复杂度是$O(n)$
> - 当$1<=x<=n$时，时间复杂度是一个我们不确定的值，取决于x的值
> - 当$x=1$时，时间复杂度是$O(1)$

这段代码在不同情况下，其时间复杂度是不一样的。所以为了描述代码在不同情况下的不同时间复杂度，我们引入了最好、最坏、平均时间复杂度。

1. 最好情况时间复杂度(best case)

最好情况时间复杂度，表示在最理想的情况下，执行这段代码的时间复杂度。
上述示例就是当x=1的时候，循环的第一个判断就跳出，这个时候对应的时间复杂度就是最好情况时间复杂度。

2. 最坏情况时间复杂度(worst case)

最坏情况时间复杂度，表示在最糟糕的情况下，执行这段代码的时间复杂度。
上述示例就是$n<x$的时候，我们要把整个循环执行一遍，这个时候对应的时间复杂度就是最坏情况时间复杂度。

3. 平均情况时间复杂度(average case)

好和最好情况是极端情况，发生的概率并不大。为了更有效的表示平均情况下的时间复杂度，引入另一个概念：**平均情况时间复杂度**。
分析上面的示例代码，判断x在循环中出现的位置，有$n+1$种情况：$1<=x<=n$ 和$n<x$。将所有情况下代码执行的次数累加起来，然后再除以所有情况数量，就可以得到**平均情况时间复杂度**，即
$$\frac{((1+2+3+\cdots+n)+n)}{(n+1)}=\frac{n(n+3)}{2(n+1)}$$
大$O$表示法会省略系数、低阶、常量，所以平均情况时间复杂度是$O(n)$。

**考虑概率的平均情况复杂度：**

$x$要么在$1\sim n$中，要么不在$1\sim n$中，所以它们的概率都是$1/2$。同时数据在$1\sim n$中各个位置的概率都是一样的为1/n。根据概率乘法法则，$x$在$1\sim n$中任意位置的概率是$1/(2n)$。因此在前面推导过程的基础上，我们把每种情况发生的概率考虑进去，得到**考虑概率的平均情况复杂度**：
\[(1\dfrac{1}{2n}+2\dfrac{1}{2n}+3\dfrac{1}{2n}+\cdots+n\dfrac{1}{2n})+n\dfrac{1}{n}=\frac{3n+1}{4}
\]
引入概率之后，平均复杂度变为$O(\frac{3n+1}{4})$，忽略系数及常量后，最终得到加权平均时间复杂度$O(n)$。

> 多数情况下，我们**不需要区分最好、最坏、平均情况时间复杂度**。只有同一块代码在不同情况下时间复杂度有量级差距，我们才会区分3种情况，为的是更有效的描述代码的时间复杂度。


4. 均摊情况时间复杂度

均摊复杂度是一个更加高级的概念，它是一种特殊的情况，应用的场景也更加特殊和有限。对应的分析方式称为：``摊还分析``或``平摊分析``。
示例如下（限定条件：$0\leq x\leq n$且$0\leq n$且$n$, $x$为整数）：
```
int n;
public int Function2(int x)
{
    int count = 0;
    if (n == x)
    {
        for (int i = 0; i < n; i++)
            count += i;
    }
    else
        count = x;
    return count;
 }
```
共有$n+1$种情况，每种情况的概率均为$\dfrac{1}{n+1}$当$0\leq x<n$时，时间复杂度为$O(1)$; 当$x=n$时，时间复杂度为$O(n)$。
**平均时间复杂度为：**
$$(1+1+\cdots+1)\dfrac{1}{n+1}+n\dfrac{1}{n+1}=\dfrac{2n}{n+1}$$
当省略系数及常量后，平均时间复杂度为$O(1)$。
通过分析可以看出，上述示例代码复杂度遵循一定的规律，对应1个$O(n)$，和n个$O(1)$。针对这样一种特殊场景使用更简单的分析方法：**摊还分析法**, 即把耗时多的复杂度均摊到耗时低的复杂度，得到对应的均摊时间复杂度为O(1)。
> - 均摊时间复杂度是将高高复杂度均摊到其余低复杂度，故一般均摊时间复杂度就等于最好情况时间复杂度。
> - 均摊时间复杂度是一种特殊的平均复杂度（特殊应用场景下使用）

### 空间复杂度
空间复杂度定性地描述该算法或程序运行所需要的存储空间大小，算法空间复杂度的计算公式记作：$S(n)=O(f(n))$，其中$n$为问题的规模，$f(n)$为语句关于$n$所占存储空间的函数。

**参考：**
- <https://www.cnblogs.com/jonins/p/9956752.html>
- [邓俊辉数据结构（C++语言版）](https://dsa.cs.tsinghua.edu.cn/~deng/ds/dsacpp/)
- <https://zhuanlan.zhihu.com/p/361636579>
- <https://zh.m.wikipedia.org/zh-hans/%E7%A9%BA%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6>

## 线性表
### 数组
C语言中的数组和C++中的vector
#### 滑动窗口
#### 双指针

#### 前缀和

#### 旋转矩阵
```cpp
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> res;
    if(matrix.empty()) return res;
    int u = 0, d = matrix.size()-1, l = 0, r = matrix[0].size()-1;
    while(true){
        for(int i=l; i<=r; ++i) res.push_back(matrix[u][i]); // 向右
        if(++u>d) break;
        for(int i=u; i<=d; ++i) res.push_back(matrix[i][r]); // 向下
        if(--r<l) break;
        for(int i=r; i>=l; --i) res.push_back(matrix[d][i]); //向左
         if(--d<u) break;
        for(int i=d; i>=u; --i) res.push_back(matrix[i][l]); //向上
        if(++l>r) break;
    }
    return res;
}
```

### 链表
链表是一种通过指针串联在一起的线性结构。
#### 链表的组成
如图，一个完整的链表需要由以下几部分构成：
![](./images/link_structure.png)
- 头指针：一个普通的指针，它的特点是永远指向链表第一个节点的位置，用于指明链表的位置；
- 节点：链表的每一个节点由两部分组成：数据域和指针域。链表中的节点可细分为头节点、首元节点和其他节点：
    - 头节点：通常作为链表的第一个节点，是不存放数据的空节点。头节点不是必须的，它的作用只是为了方便解决某些实际问题；
    - 首元节点：链表中称第一个存有数据的节点；
    - 其他节点：链表中其他的节点；最后一个节点的指针域指向null（空指）。
> 链表中有头节点时，头指针指向头节点；反之，若链表中没有头节点，则头指针指向首元节点。下文不考虑头节点。

#### 链表的类型
1. 单链表
之前的是单链表

2. 双向链表
双链表：每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。双链表 既可以向前查询也可以向后查询。
![](./images/double_linked_list.png)

3. 循环链表
循环链表首尾相连，可以用来解决约瑟夫环问题。
![](./images/circular_linked_list.png)

#### 链表的存储方式和性能分析
数组是在内存中是连续分布的，但链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理，通过指针链接在一起。因此，链表只能按位置顺序访问，不能随机访问，链表这种存储方式最大缺点就是容易出现断链。
![](./images/linked_list_memory.png)

比较数组和链表的性能：
- 数组在定义的时候，长度就是固定的，查询元素方便，如果想改动数组的长度，就需要重新定义一个新的数组。
- 链表的长度可以是不固定的，并且可以动态增删，适合数据量不固定，频繁增删，较少查询的场景。


|       | 插入/删除（时间复杂度）| 查询（时间复杂度）| 适用场景 |
|:----: | :---:                 |   :----:         |  :----     |
|数组   | $O(n)$                | $O(1)$          | 数量固定，频繁查询，较少增删 |
|链表   | $O(1)$               | $O(n)$           | 数量不固定，频繁增删，较少查询 |

#### 链表的创建
1. 单链表
```cpp
struct LinkedListNode {
    int val;
    ListNode *next;
    LinkedListNode() : val(0), next(nullptr) {}
    LinkedListNode(int x) : val(x), next(nullptr) {}
    LinkedListNode(int x, LinkedListNode *next) : val(x), next(next) {}
};

template<typename T>
inline LinkedListNode* creatLinledList(const T& val){ //可以用 vector创建链表
    LinkedListNode* dummyHead = new LinkedListNode(0);
    LinkedListNode* cur = dummyHead;
    const size_t len = val.size();
    for(size_t i = 0; i < len; ++i){
        cur->next = new LinkedListNode(val[i]);
        cur = cur->next;
    }
    return dummyHead->next;
}
```

#### 链表的操作
1. 插入节点
```cpp
// prev 之后插入新的节点
LinkedListNode* tmp = prev->next
prev->next = new LinkedListNode(0);
prev->next->next = tmp
```
2. 删除节点
```cpp
// 删除 cur 节点
prev->next = cur->next;
delete cur
```
3. 打印节点
```cpp
inline void printLinkedList(LinkedListNode* node)
{
    std::cout << "Print linked list: " << std::endl;
    while(node!=nullptr) {
        std::cout << node->val << " ";
        node = node->next;
    }
    std::cout << std::endl;
}
```

## 字符串
字符串结构简单，规模庞大，元素重复率高，存储在连续空间中（可以通过指针偏移量访问）

### 基本操作
1. 访问
```
下标访问： s[i]
front(); back();
迭代器访问：
for(auto & c : s)

```
2. 添加
```
str1.push_back('y');该函数只能在字符串后面添加字符；```

str += 'a'; 
str += "abc"
   
basic_string &append( const basic_string &str );
basic_string &append( const basic_string &str, size_type index, size_type len );
basic_string &append( input_iterator start, input_iterator end );
basic_string &append( size_type num, char ch ); 添加n个重复的字符

str.push_back()
```
3. 删除
```
str.pop_back()
```
4. 查找
```
size_type find( const basic_string& str, size_type pos = 0 ) const; //返回起始的index
```
5. 插入
```
// insert(size_type index, size_type count, char ch)
// insert(size_type index, const char* s)
// insert(size_type index, const char* s)
// insert(const_iterator pos, size_type count, char ch)
// insert(const_iterator pos, InputIt first, InputIt last)
```
6. 子串
```
basic_string substr( size_type pos = 0, size_type count = npos ) const;
```
7. 子串替换
```
#include <string>
#include <regex>

std::string test = "abc def abc def";
test = std::regex_replace(test, std::regex("def"), "klm"); // replace 'def' -> 'klm'
// test = "abc klm abc klm"
```
8. 反转
```
//调用标准库函数
std::reverse(InputIt first, InputIt last)

//自己手写 
template <typename T>
void swap_(T &x, T &y)
{
    x = x^y;
    y = x^y;
    x = x^y;
}
template <typename Iter>
inline void reverseK(Iter first, Iter last) {
    for(--last; first < last; ++first, --last) {
        //cout << i << endl;
        swap_(*first, *last);
    }
}
```
### 字符串运算

字符串相加和相乘 leetcode 415, 43.
```cpp
class Solution {
private:
    string addStrings(string str1, string str2)
    {
        int i = str1.size()-1, j = str2.size()-1;
        string res;
        int add = 0;
        while(i>=0||j>=0||add!=0){
            int x = i>=0 ? str1[i]-'0' : 0;
            int y = j>=0 ? str2[j]-'0' : 0;
            int tmp = x+y+add;
            res.push_back('0'+tmp%10);
            add = tmp/10;
            --i; --j;
        }
        reverse(res.begin(), res.end());
        return res;
    }
public:
    string multiply(string num1, string num2) {
        string res;
        if(num1=="0"||num2=="0") return "0";
        for(int i = num2.size()-1; i>=0; --i){
            string tmp; // num2 第i位与num1相乘的结果放在字符串中
            for(int j = 0; j < num2.size()-1-i; ++j)
                tmp.push_back('0');
            int add = 0;
            for(int j = num1.size()-1; j>=0||add!=0; --j){
                int mult = (num2[i]-'0')*(j<0?0:num1[j]-'0')+add;
                tmp.push_back('0'+mult%10);
                add = mult/10;
            }
            reverse(tmp.begin(), tmp.end());
            res = addStrings(res, tmp);
        }
        return res;
    }
};
```


### 串匹配算法
在设计字符串的众多实际应用中，串匹配的最常用的一项基本操作。
**串匹配**：对基于同一字符表的任何**文本串T(|T|= n)**和**模式串P(|P|= m)**(n>>m>>2):判定T中是否存在某一字串与P相同，若存在匹配，则报告该字串在T中的起始位置。
**串匹配分类**：
|   分类    | 问题描述          | 应用                   |
|  :----:  |  :---:            |  :----                 |
|模式监测   |  匹配是否出现      | 垃圾邮件的监测          |
|模式定位   |匹配起始位置在哪出现 | 带病毒程序的鉴定与修复   |
|模式计数   | 匹配共有几次出现    | 网络热门词汇排行榜的更新 |
|模式枚举   | 各匹配出现在哪里    | 网络搜索引擎           |

#### 蛮力算法
1. 实现1: ``O(m(n-m+1))``时间复杂度
```cpp
int bruteForce1(const char* T, const char* P) { //字符串蛮力算法实现1,
    size_t n = strlen(T), i = 0; //文本串长度、当前接受比对字符的位置
    size_t m = strlen(P), j = 0; //模式串长度、当前接受比对字符的位置
    for(i=0; i < n-m+1; ++i){
        for(j=0; j < m; ++j)
            if(T[i+j] != P[j]) break;
        if(j==m) return i; // found
    } 
    return -1; // not found
}
```
2. 实现2:  ``O(m(n-m+1))``时间复杂度
```cpp
int bruteForce2(const char* T, const char* P) { //字符串蛮力算法实现2, O(m(n-m+1))时间复杂度
    size_t n = strlen(T), i = 0; //文本串长度、当前接受比对字符的位置
    size_t m = strlen(P), j = 0; //模式串长度、当前接受比对字符的位置
    while(i<n && j<m){
        if(T[i]==P[j]) {++i; ++j;}
        else {
            i+=1-j; j=0; //T回退，P复位
        }
    }
    return j == m ? i-j : -1; //return i-m, found; return -1, not found
}
```
#### KMP算法
蛮力算法存在大量的局部匹配，重复匹配较多，导致蛮力算法效率低。KMP算法来源于蛮力算法，避免了回溯，最坏情况下不超过O(n)时间复杂度。

![](./images/string_kmp1.png)
如图，``T[i] != P[j]``时 已有局部匹配：
```
P[0, j) = T[i-j, i)
```
向右滑动模式串P后使得新的P[t]与T[i]对齐, 以开启下一轮操作。P滑动 j-t 个单位后，满足
```
P[0, t) = T[i-t, i) = P[j-t, j)
```
> 这里 T[i - j, i - t) 被舍弃，不值得被重新匹配。

如何确定t? t = next[j]代替j。构建关于模式串P的next[]表，从而能够根据 j 获得t。

**构造next[]表**  

next数组其实就是构造的**前缀表**，前缀表可以暂时理解为：**字符串最长公共前缀后缀长度**。
```
“ABCDA”的前缀为[A, AB, ABC, ABCD]，后缀为[BCDA, CDA, DA, A]，共有元素为”A”，长度为1；
```
next[i] 表示以 P[i] 结尾的子串的最长公共前缀后缀长度

在P[0]左侧附加"P[-1]",该字符"*"与任何字符都匹配。等效于"next[0]=-1"。
用递推方式构造next[]表
- P[j]=P[next[j]] 时， next[j+1]=next[j]+1
- P[j]!=P[next[j]] 时，令 t = next[j], 反复用 next[t] 替换 t ，直到 P[j]=P[t], next[j+1] = next[t]+1。实现程序如下：
  
```cpp
int* buildNext(const char* P)
{
    int m = strlen(P), j = 0; 
    int* next = new int[m];
    int t = next[0]=-1;
    while(j<m-1) {
        if(t==-1||P[j]==P[t])  //匹配； -1为通配哨兵
            next[++j] = ++t;    
        else t = next[t];  //失配
    }
    return next;
}
```

构造next[]表改进：
```cpp
int* buildNext(const char* P)
{
    int m = strlen(P), j = 0; 
    int* next = new int[m];
    int t = next[0]=-1;
    while(j<m-1) {
        if(t==-1||P[j]==P[t])  //匹配； -1为通配哨兵
        {
            ++j; ++t;
            next[j] = (P[j]!=P[t]? t : next[t]);
        }
        else t = next[t];  //失配
    }
    return next;
}
```

**KMP算法**
在蛮力算法实现2的基础上改进得到KMP算法如下：
```cpp
int kmp(const char* T, const char* P) // O(m+n)时间复杂度
{ 
    int* next = buildNext(P);    //O(m)
    int n = strlen(T), i = 0; //文本串长度、当前接受比对字符的位置
    int m = strlen(P), j = 0; //模式串长度、当前接受比对字符的位置
    while(i<n && j<m){   //O(n)
        if(j==-1||T[i]==P[j]) {++i; ++j;} //若匹配，或P已移出最左侧（两个判断的次序不可交换）
        else j = next[j];  //模式串右移（注意：文本串不用回退）
    }
    delete [] next;
    return j == m ? i-j : -1; //return i-m, found; return -1, not found
}
```
> **c++ string 中使用 s.find(substr) == -1 判断没有该子串**

## 栈和队列
### 栈
后进先出, 对顶操作，地址从栈底到栈顶递减，向下生长
**接口**
```
#include <stack>
push(); pop(); top(); empty(); size();
```

**栈的应用**
1. 单调栈

单调递增栈：
```cpp
stack<int> st;
for(int i = 0; i < nums.size(); ++i)
{
	while(!st.empty() && st.top() >= nums[i])
	{
		st.pop();
	}
	st.push(nums[i]);
}
```

单调递减栈：
```cpp
stack<int> st;
for(int i = 0; i < nums.size(); ++i)
{
	while(!st.empty() && st.top() <= nums[i])
	{
		st.pop();
	}
	st.push(nums[i]);
}
```
2. 括号匹配
```cpp
bool parenthesMatching(string s) {
    size_t len = s.size();
    if (len&0x1) return false; // 如果s的长度为奇数，一定不符合要求
    stack<char> S;
    for(size_t i = 0; i < len; ++i){
        switch(s[i]) {
            case '(': case '[': case '{': S.push ( s[i] ); break;
            case ')': if ( S.empty() ||'(' != S.top() ) return false; S.pop(); break;
            case ']': if ( S.empty() ||'[' != S.top() ) return false; S.pop();  break;
            case '}': if ( S.empty() ||'{' != S.top() ) return false; S.pop();break;
            default: break; //非括号字符一徇忽略
        }
    }
    return S.empty();
}
```
3. 计算逆波兰表达式（RPN）的值
```cpp
int evalRPN(vector<string>& tokens) {
    stack<long> st;
    const size_t len = tokens.size();
    for(size_t i = 0; i < len; ++i) {
        if(tokens[i]=="+"||tokens[i]=="-"||tokens[i]=="*"||tokens[i]=="/"){
            long tmp1 = st.top(); st.pop();
            long tmp2 = st.top(); st.pop();
            switch(tokens[i][0]) {
                case '+': st.push(tmp2+tmp1); break;
                case '-': st.push(tmp2-tmp1); break;
                case '*': st.push(tmp2*tmp1); break;
                case '/': st.push(tmp2/tmp1); break;
                default: break;
            }
        }
        else st.push(stoi(tokens[i]));
    }
    return st.top(); //只剩下一个元素
}
```

### 队列
先进先出：从头 (队头) 取出，从尾 (队尾) 插入
**接口**
1. 单向队列
```
#include <queue>
// 对头操作
front(); //access the first element
pop(); //removes the first element
// 对尾操作
back(); push();

empty(); size();
```
2. 双向队列
```
#include <deque>
// 对头操作
front(); push_front(); pop_front()
// 对尾操作
back(); push_back(); pop_back();

empty(); size();
```

**队列的应用**
1. 滑动窗口最大值
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    const int len = nums.size();
    vector<int> res;
    deque<int> dq;
    for(int i = 0; i < len; ++i) {
        if(i>=k&&nums[i-k]==dq.front()) dq.pop_front(); // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
        while(!dq.empty()&&nums[i]>dq.back()) dq.pop_back();
        dq.push_back(nums[i]);
        if(i>=k-1) res.push_back(dq.front());
    }
    return res;
}
```
2. 求前 K 个高频元素
```cpp
struct CompareMore{ //最大堆
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs){
        return lhs.second < rhs.second;
    }
};
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> umap;
    const int len = nums.size();
    for(int i = 0; i < len; ++i) ++umap[nums[i]];
    priority_queue<pair<int, int>, vector<pair<int, int>>, CompareMore> priQue(umap.begin(), umap.end());
    vector<int> res(k);
    for(int i = 0; i < k; ++i){
        res[i] = priQue.top().first;
        priQue.pop();
    }
    return res;
}
```

## 树
### 树的基本概念
1. 有根树
    树是特殊的图 T = (V, E)，节点数 |V| = n, 边数 |E| = e, 指定任一节点 r ∈ V 作为根后，T 即称作**有根树**。小型有根树（子树）可以形成具有一定规模的有根树。
2. 有序树
    - 节点 v 的度数或（出）度 degree(v): 表示 V 孩子的总数，也表示 v连出去的边数
    - 若指定 $T_i$ 作为 $T$ 的第 $i$ 棵子树，$r_i$ 作为 $r$ 的第 $i$ 个孩子，则 $T$ 称作**有序树**
3. 深度与层次
    - 路径的长度等于边数，任一节点V 与根之间存在唯一路径，path(v, r) = path(v)
    - **深度 depth(v)** : 沿每个节点 v 到根 r 的唯一通路所经过边的数目。特别地，根节点的深度 depth(r) = 0，属于第 0 层
    - 任一节点 v 在通往树根沿途所经过的每个节点都是其祖先（ancestor）, v 是它们的后代 (descendant)。特别地，v 的祖先和后代包括其本身，而 v 本身以外的祖先/后代称作真祖先 (proper ancestor) /真后代 (proper descendant)。根节点是所有节点的公共祖先。
    - v 的前一层称作 v 的父亲，v 是儿子
4. 高度
    <div align=center>
    <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_height.png" width = "60%" height = "60%">
    </div>
    - 树 T 中所有节点深度最大值称作该数的高度 (height) ，记作 height(T) ，单个节点树的高度为 0，空树高度为-1
    - 任一节点 v 所对应子树 subtree(v) 的高度，为该节点的高度 height(v)
    - 全树高度即其根节点的高度， height(T) = height(r)
    - depth(v) + height(v) ≤ height(T)



### 二叉树
对于**有根且有序** 的多叉树，根据 **长子-兄弟** 表示法，可以将**多叉树**转化为**二叉树** ( **长子-左孩子，兄弟-右孩子** )

二叉树节点定义
```cpp
struct BinTreeNode {
    int val;
    BinTreeNode *left;
    BinTreeNode *right;
    BinTreeNode() : val(0), left(nullptr), right(nullptr) {}
    BinTreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    BinTreeNode(int x, BinTreeNode *left, BinTreeNode *right) : val(x), left(left), right(right) {}
};

```
#### 先序遍历
先访问**父节点**再访问**左右孩子**
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_preorder.png"/>

**递归版本:**
```cpp
inline
void travPre_R(BinTreeNode* parent, vector<int> res)
{
    if(parent == nullptr) return;
    res.push_back(parent->val);
    travPre_R(parent->left, res);
    travPre_R(parent->right, res);
}
vector<int> travPre_R(BinTreeNode* root) {
    vector<int> res;
    travPre_R(root, res);
    return res;
}
```
**迭代版本:**
参考递归版算法，引入辅助栈消除尾递归，得到先序遍历的迭代版本, 不易推广
```cpp
vector<int> travPre_I(BinTreeNode* root) {
    vector<int> res;
    if (root == nullptr) return res;
    stack<BinTreeNode*> S;
    S.push(root);  // root不为空
    while(!S.empty()){
        root = S.top();
        S.pop();
        res.push_back(root->val);
        if(root->right); S.push(root->right);
        if(root->left); S.push(root->left);
    }
    return res;
}
```
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_preorder_I.png"/>

#### 中序遍历
先访问**左孩子**，再访问**父节点**，最后访问**右孩子**
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_inorder.png"/>

**递归版本:**
```cpp
inline
void travIn_R(BinTreeNode* parent, vector<int>& res)
{
    if(parent == nullptr) return;
    travIn_R(parent->left, res);
    res.push_back(parent->val);
    travIn_R(parent->right, res);
}
vector<int> travIn_R(BinTreeNode* root) {
    vector<int> res;
    travIn_R(root, res);
    return res;
}
```
**迭代版本:**

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_inorder_I.png"/>

```cpp
vector<int> travIn_I(BinTreeNode* root) {
    vector<int> res;
    if (root == nullptr) return res;
    stack<BinTreeNode*> S;
    while(true){
        while (root) { //从当前节点出发，沿左分支不断深入，直至没有左分支的节点
            S.push(root);
            root = root->left;
        } //当前节点入栈后随即向左侧分支深入，迭代直到无左孩子
        if (S.empty())
            break; //直至所有节点处理完毕
        root = S.top(); 
        S.pop();
        if(root) {
            res.push_back(root->val);
            root = root->right;    
        } 
    }
    return res;
}
```
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_inorder_I_example.png"/>


#### 后序遍历
先访问**左右孩子**再访问**父节点**

<img src="images/tree_post.png"/>

**递归版本:**
```cpp
inline
void travPost_R(BinTreeNode* parent, vector<int>& res)
{
    if(parent == nullptr) return;
    travPost_R(parent->left, res);
    travPost_R(parent->right, res);
    res.push_back(parent->val);
}
vector<int> travPost_R(BinTreeNode* root) {
    vector<int> res;
    travPost_R(root, res);
    return res;
}
```
**迭代版本:**
在先序遍历迭代版本的基础上改进
```cpp
vector<int> travPost_I(BinTreeNode* root) {
    vector<int> res;
    if (root == nullptr) return res;
    stack<BinTreeNode*> S;
    S.push(root);  // root不为空
    while(!S.empty()){
        root = S.top();
        S.pop();
        if(root){
            res.push_back(root->val);
            if(root->left); S.push(root->left);
            if(root->right); S.push(root->right);
        }
    }
    reverse(res.begin(), res.end()); //中右左 -> 左右中 
    return res;
}
```

#### 层次遍历
即先上后下，先左后右，借助队列实现。
 

```cpp
vector<vector<int>> travLevel(BinTreeNode* root) {
    vector<vector<int>> res;
    if (root == nullptr) return res;
    queue<BinTreeNode*> que;
    que.push(root);
    while(!que.empty()){
        vector<int> curVec;
        const size_t len = que.size();
        for(size_t i = 0; i < len; ++i){
            BinTreeNode* curNode = que.front(); que.pop();
            curVec.push_back(curNode->val);
            if(curNode->left) que.push(curNode->left);
            if(curNode->right) que.push(curNode->right);
        }
        res.push_back(curVec);
    }
    return res;
}
```

#### 二叉树的两种形式

**完全二叉树**
叶节点只能出现在最底部的两层，且最底层叶节点均处于次底层叶节点的左侧。

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_complete.png"/>

对于高度为$h$ 的完全二叉树，规模应在 $2^h$和 $2^{h+1} - 1$之间。
完全二叉树可借助向量结构实现紧凑存储和高效访问。

**满二叉树**
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/tree_full.png"/>

第 \( {k} \) 层的节点数是 \( 2^{k} \) ，当高为 \( {h} \) 时，总结点数是 \( 2^{0}+2^{1}+\cdots+2^{h}=2^{h+1}-1 \) ，内部节点是 \( 2^{h+1}-1-2^{h}=2^{h}-1 \) ，叶结点为 \( 2^{h} \) ，叶 节点总是恰好比内部节点数多 1 。


### 二叉搜索树
二叉搜索树（binary search tree），处处都满足顺序性：**任一节点r的左（右）子树中，所有节点（若存在）均小于（大于）r**(假定所有节点互不相等)。
> 在实际应用中，对相等元素的禁止既不自然也不必要

**中序遍历序列**

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/search_tree_inorder.png"/>

任何一棵二叉树是二叉搜索树，**当且仅当其中序遍历序列单调非降**


### 平衡二叉搜索树
平衡二叉搜索树（英语：Balanced Binary Search Tree, BBST）是一种结构平衡的二叉搜索树，它是一种每个节点的左右两子树高度差都不超过 1 的二元树。它能在$O(log n)$内完成**插入、查找和删除**操作。

二叉搜索树的性能主要取决于**高度**，故应尽可能地使兄弟子树的高度彼此接近，即全树尽可能地平衡。当然，包含n个节点的二叉树，高度不可能小于 $log_2n$。若树高恰好为 $log_2n$，则称作**理想平衡的二叉搜索树**。在渐进意义下适当放松标准之后的平衡性，称作**适度平衡的二叉搜索树**。AVL树、伸展树、红黑树、kd-树等，都属于适度平衡的二叉搜索树。这些变种，因此也都可归入平衡二叉搜索树。

> C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树，所以map、set的增、删、查操作时间时间复杂度是$O(log n)$。unordered_map、unordered_set，unordered_map、unordered_map底层实现是哈希表。

#### AVL树
**AVL树**（Adelson-Velsky and Landis Tree）是[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6 "计算机科学")中最早被发明的[自平衡二叉查找树](https://zh.wikipedia.org/wiki/%E8%87%AA%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91 "自平衡二叉查找树")。在AVL树中，任一节点对应的两棵子树的最大高度差为1，因此它也被称为**高度平衡树**。查找、插入和删除在平均和最坏情况下的[时间复杂度](https://zh.wikipedia.org/wiki/%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6 "时间复杂度")都是 O(logn) 。增加和删除元素的操作则可能需要借由一次或多次[树旋转](https://zh.wikipedia.org/wiki/%E6%A0%91%E6%97%8B%E8%BD%AC "树旋转")，以实现树的重新平衡。

#### 红黑树
红黑树是一种**自平衡的二叉查找树**，是一种高效的查找树。它是由 Rudolf Bayer 于1972年发明，在当时被称为对称二叉 B 树(symmetric binary B-trees)。后来，在1978年被 Leo J. Guibas 和 Robert Sedgewick 修改为如今的红黑树。红黑树具有良好的效率，它可**在 O(logN) 时间内完成查找、增加、删除等操作**。因此，红黑树在业界应用很广泛，比如 Java 中的 TreeMap，JDK 1.8 中的 HashMap、C++ STL 中的 map 均是基于红黑树结构实现的。

红黑树通过如下的性质定义实现自平衡：
1. 根节点是黑色，叶节点是不存储数据的黑色空节点；
2. 任何相邻的两个节点不能同时为红色，即每个红色节点必须有两个黑色的子节点。
3. 任意节点到其可到达的叶节点包含相同数量的黑色节点

**AVL 树和红黑树比较：**

- **AVL 树旋转是非常耗时的，由此我们可以知道AVL树适合用于插入与删除次数比较少，但查找多的情况**
- **红黑树的旋转次数少，适合搜索，插入，删除操作较多的情况**。
- 因此 epoll 在内存维护一个红黑树而不是AVL树

### 字典树
在计算机科学中，trie，又称**前缀树或字典树**，是一种有序树，用于保存关联数组，其中的键通常是字符串。与二叉查找树不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的前缀，也就是这个节点对应的字符串，而根节点对应空字符串。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。
```cpp
struct Trie{
    bool isEnd = false;
    Trie* child[26] = {nullptr};
};
class StreamChecker {
public:
    Trie* root = new Trie();
    string str;
    StreamChecker(vector<string>& words) {
        for(auto& s : words){
            Trie* cur = root;
            int n = s.size();
            for(int i = n-1; i >= 0; --i){
                int idx = s[i] - 'a';
                if(cur->child[idx]==nullptr) cur->child[idx] = new Trie();
                cur = cur->child[idx];
            }
            cur->isEnd = true;
        }
    }
    
    bool query(char letter) {
        str.append(1, letter);
        Trie* cur = root;
        int n = str.size();
        for(int i = n-1; i>=0; --i){
            int idx = str[i] - 'a';
            if(cur->child[idx] == nullptr) break;
            cur = cur->child[idx];
            if(cur->isEnd) return true;
        }
        return false;
    }
};
```

实现前缀树(Leecode 208)
```cpp
struct TrieNode {
    bool isEnd = false;
    TrieNode* child[26] = {nullptr};
};

class Trie {
public:
    TrieNode* root = new TrieNode(); // root
    Trie() {}
    
    void insert(string word) {
        TrieNode* node = root;
        for(char c : word){
            if(node->child[c-'a']==nullptr) {
                node->child[c-'a'] = new TrieNode();
            }
            node = node->child[c-'a'];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        TrieNode* node = root;
        for(char c : word){
            if(node->child[c-'a']==nullptr) return false;
            node = node->child[c-'a'];
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for(char c : prefix){
            if(node->child[c-'a']==nullptr) return false;
            node = node->child[c-'a'];
        }
        return true;
    }
};
```


### 线段树
 



### B+树

## 图
### 图的存储方式
#### 邻接矩阵
```cpp
struct Edge
{
    int begin; //对应顶点索引
    int end;
    int weight = 1;
};
/******************************************************************************************
 * 无向简单图的邻接矩阵表示
 ******************************************************************************************/
template<class VertexType>
struct AdjMatGraph
{
    std::vector<std::vector<int>> adjMat;
    int numEdges;

    AdjMatGraph(const std::vector<VertexType>& vexs, const std::vector<Edge>& edges)
        : numEdges(edges.size())
    {
        const int len = vexs.size();
        adjMat.resize(len, std::vector<int>(len, INT_MAX));
        for(int i = 0; i < len; ++i) adjMat[i][i] = 0;
        for(int k = 0; k < numEdges; ++k){
            int i = edges[k].begin, j = edges[k].end;
            adjMat[i][j] = adjMat[j][i] = edges[k].weight;
        }
    }
    int numVex(void) const {return adjMat.size();}
};
```

#### 邻接表

可以使用二维vector.

### 图的遍历
树和图都有BFS和DFS, 回顾下图的BFS和DFS:
![](images/graph_bfs_dfs.png)

#### DFS
深度优先搜索算法（英语：Depth-First-Search，DFS）是一种用于遍历或搜索树或图的算法。这个算法会尽可能深地搜索树的分支。当节点v的所在边都己被探寻过，搜索将回溯到发现节点v的那条边的起始节点。这一过程一直进行到已发现从源节点可达的所有节点为止。如果还存在未被发现的节点，则选择其中一个作为源节点并重复以上过程，整个进程反复进行直到所有节点都被访问为止。

基本思想：
1. 首先将根节点放入stack中。
2. 从stack中取出第一个节点，并检验它是否为目标。
    - 如果找到目标，则结束搜寻并回传结果。
    - 否则将它某一个尚未检验过的直接子节点加入stack中。
3. 重复步骤2。
    - 如果不存在未检测过的直接子节点。
    - 将上一级节点加入stack中。
4. 重复步骤2。
5. 重复步骤4。
6. 若stack为空，表示整张图都检查过了——亦即图中没有欲搜寻的目标。结束搜寻并回传“找不到目标”。

**DFS与回溯**
```cpp
// leecode 1219. 黄金矿工
class Solution {
private:
    const int dirs[4][2] = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
   
public:
    int getMaximumGold(vector<vector<int>>& grid) {
        const int m = grid.size(), n = grid[0].size();
        int res = 0;

        function<void(int, int, int)> backTracking = [&](int x, int y, int gold){
            gold += grid[x][y];
            res = max(res, gold);
            int tmp = grid[x][y];
            grid[x][y] = 0;
            for(int i = 0; i < 4; ++i){
                int x_ = x + dirs[i][0];
                int y_ = y + dirs[i][1];
                if(x_>=0 && x_<m && y_>=0 && y_<n&&grid[x_][y_]>0)
                    backTracking(x_, y_, gold);
            }
            grid[x][y] = tmp;
        };

        for(int i = 0; i < m; ++i)
            for(int j = 0; j < n; ++j){
                if(grid[i][j]!=0)
                    backTracking(i, j, 0);
            }
        return res;
    }
};
```

**DFS与记忆搜索**
```cpp
// leetcode 329. 矩阵中的最长递增路径
class Solution {
private:
    static constexpr int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        const int nRows = matrix.size(), nCols = matrix[0].size();
        if(nRows==0 || nCols == 0) return 0;
        vector<vector<int>> mem(nRows, vector<int>(nCols));
        int res;
        function<int(int, int)> dfs = [&](int x, int y)->int{
            if(mem[x][y]!=0) return mem[x][y];
            mem[x][y] = 1;
            for(int i = 0; i < 4; ++i){
                int x_ = x+dirs[i][0], y_ = y + dirs[i][1];
                if(x_>=0 && x_ < nRows && y_ >= 0 && y_ < nCols && matrix[x_][y_] > matrix[x][y]){
                    mem[x][y] = max(mem[x][y], dfs(x_, y_)+1);
                }
            }
            return mem[x][y];
        };
        for(int i = 0; i < nRows; ++i)
            for(int j = 0; j < nCols; ++j)
                res = max(res, dfs(i, j));
        return res;
    }
};
```


#### BFS
广度优先搜索（breadth-first search, BFS）: 反复从队列中取出最早被访问的顶点v。若存在没有被访问的邻居节点，则将没有被访问的节点依次加入队列；若v 的所有邻居节点都已被访问，则从队列中取出下一个节点。
![](images/graph_bfs.png)

基本思想：
1. 首先将根节点放入队列中。
2. 从队列中取出第一个节点，并检验它是否为目标。
    - 如果找到目标，则结束搜索并回传结果。
    - 否则将它所有尚未检验过的直接子节点加入队列中。
3. 若队列为空，表示整张图都检查过了——亦即图中没有欲搜索的目标。结束搜索并回传“找不到目标”。
4. 重复步骤2。

DFS和BFS代码实现：
```cpp
/******************************************************************************************
 * 无向图的遍历
 ******************************************************************************************/
static std::vector<bool> visited;

template <class T>
void DFS(const AdjMatGraph<T>& graph, int i)
{
    visited[i] = true;
    //std::cout << graph.vexs[i] << " ";
    for (int j = 0; j < graph.numVex(); ++j)
        if (graph.adjMat[i][j] == 1 && !visited[j]) DFS(graph, j);
}

template <class T>
void BFS(const AdjMatGraph<T>& graph, int i)
{
    std::queue<int> vexQue;
    visited[i] = true;
    //std::cout << graph.vexs[i] << " ";
    vexQue.push(i);
    while (!vexQue.empty())
    {
        i = vexQue.front(); vexQue.pop();
        for (int j = 0; j < graph.numVex(); ++j)
        {
            if (graph.adjMat[i][j] == 1 && !visited[j])
            {
                visited[j] = true;
                //std::cout << graph.vexs[j] << " ";
                vexQue.push(j);
            }
        }
    }
}

template <class Graph>
void DFSTraverse(const Graph& graph)
{
    visited.resize(graph.numVex());
    std::fill(visited.begin(), visited.end(), false);
    for (int i = 0; i < graph.numVex(); ++i)
        if (!visited[i]) DFS(graph, i); // 可能存在多个连通分支   
}

template <class Graph>
void BFSTraverse(const Graph& graph)
{
    visited.resize(graph.numVex());
    std::fill(visited.begin(), visited.end(), false);
    for (int i = 0; i < graph.numVex(); ++i)
        if (!visited[i]) BFS(graph, i); //可能存在多个连通分支  
}
```

### 并查集
并查集 (Union-find Sets) 是一种非常精巧而实用的数据结构, 它主要用于处理一些不相交集合的合并问题。一些常见的用途有求连通子图、求最小生成树的 Kruskal 算法和求最近公共祖先 (LCA) 等。

**初始化：**
每个节点的父节点设置为它本身
```
function Init(x)
    for i = 1...n
        x.parent = x
end function
```
**查询：**
找到 i 的祖先直接返回，未进行路径压缩。
```
function Find(x)
    if x.parent = x then //递归出口，找到i的祖先直接返回
        return x
    else
        return Find(x.parent) //不断往上查找祖先
    end if
 end function
```
路径压缩
在集合很大或者树很不平衡时，上述代码的效率很差，最坏情况下（树退化成一条链时），单次查询的时间复杂度高达$O(n)$。一个常见的优化是**路径压缩**：在查询时，把被查询的节点到根节点的路径上的所有节点的父节点设置为根结点，从而减小树的高度。也就是说，在向上查询的同时，**把在路径上的每个节点都直接连接到根上**，以后查询时就能直接查询到根节点。
```
function Find(x)
    if x.parent = x then //递归出口，找到i的祖先直接返回
        return x
    else
        x.parent := Find(x.parent)
        return x.parent //不断往上查找祖先
    end if
 end function
```

**合并：**
合并操作 Union(x, y) 把元素 x 所在的集合与元素 y 所在的集合合并为一个。
```
function Union(x, y)
    xRoot := Find(x) // 找到x祖先
    yRoot := Find(y) // 找到y祖先
     
    if xRoot ≠ yRoot then
        xRoot.parent := yRoot
    end if
end function
```
按秩合并(优化)
```cpp
void union(int x, int y){
    int xRoot = find(x);
    int yRoot = find(y);
    if(xRoot!=yRoot) {
        if (rank[xRoot] < rank[yRoot]) {
            swap(xRoot, yRoot);
        }
        parent[yRoot] = xRoot;
        if (rank[xRoot] == rank[yRoot]) rank[xRoot] += 1;
        --count; //记录连通分支数
    }
}
```

### 最短路算法
![](images/graph_shortest_path.jpeg)

#### Dijkstra
Dijkstra算法通常是求解单源最短路中最快的算法，但它无法处理存在负权边的情况（原因在正确性证明中）。Dijkstra本质上是一种贪心算法，通过不断调整每个点的“当前距离”最终得到最优结果。

假设现在要求出从某一点s到其他所有点的最短距离，对于每个点v均维护一个“当前距离”（dist[v]）（节点s到当前节点的距离）和“是否访问过”(visited[v])。首先将dist[s]初始化为0，将其他点的距离初始化为无穷，并将所有点初始化为未访问的。记u->v的边权为weight[u->v]。然后进行以下步骤：

1. 从所有未访问的点中，找出当前距离最小的，设为u，并将其标记为已访问的。
2. 调整u的所有边（若是有向图则为出边）连接的并且**未被访问过的点**：若weight[u->v] + dist[u] < dist[v], 则将dist[v]更新为dist[u]+weight[u->v]。
3. 重复1和2步骤，直到所有点都被标   记为已访问的，则dist[i]即s到i的最短距离。如果只想求从s到某一点的最短距离，那么当该点被标记为访问过之后可直接退出。
4. 补充：如果除了最短距离之外还想求出具体的路径，只需建立一个pre数组，在步骤2后添加操作：pre[v] = u（前提是dist[v]被更新）。

使用小堆优化的代码实现如下：
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <utility>
using namespace std;   
// 加边
void addEdge(vector<pair<int, int>> adj[], int u, int v, int wt) 
{ 
    adj[u].push_back(make_pair(v, wt)); 
    adj[v].push_back(make_pair(u, wt)); 
} 

/*  template<typename T> 
 *  struct greater {
 *      constexpr bool operator()(T const& a, T const& b) const {
 *          return a.first>b.first || ( (a.first<b.first) && (a.second>b.second));
 *      }
 *  };
 */   

// 计算最短路
void dijkstra(vector<pair<int,int>> adj[], int V, int src) 
{ 
    // 关于stl中的优先队列如何实现，参考下方网址：
    // http://geeksquiz.com/implement-min-heap-using-stl/ 
    priority_queue<pair<int, int>, vector<pair<int, int>> , greater<pair<int, int>>> pq; // 小堆
  
    // 距离置为正无穷大
    vector<int> dist(V, INT_MAX); 
    vector<bool> visited(V, false);

    // 插入源点，距离为0
    pq.push(make_pair(0, src)); 
    dist[src] = 0; 
  
    /* 循环直到优先队列为空 */
    while (!pq.empty()) 
    { 
        // 每次从优先队列中取出顶点事实上是这一轮最短路径权值确定的点
        int u = pq.top().second; 
        pq.pop(); 
        if (visited[u]) {
            continue;
        }
        visited[u] = true;
        // 遍历所有边
        for (auto x : adj[u]) 
        { 
            // 得到顶点边号以及边权
            int v = x.first; 
            int weight = x.second; 
  
            //可以松弛
            if (dist[v] > dist[u] + weight) 
            { 
                // 松弛 
                dist[v] = dist[u] + weight; 
                pq.push(make_pair(dist[v], v)); 
            } 
        } 
    } 
  
    // 打印最短路
    /*
    cout << "Vertex Distance from Source" << endl;; 
    for (int i = 0; i < V; ++i) 
        cout << i << " " << dist[i] << endl;
    */
} 
int main() 
{ 
    int V = 9; 
    vector<pair<int, int>> adj[V]; 
    addEdge(adj, 0, 1, 4); 
    addEdge(adj, 0, 7, 8); 
    addEdge(adj, 1, 2, 8); 
    addEdge(adj, 1, 7, 11); 
    addEdge(adj, 2, 3, 7); 
    addEdge(adj, 2, 8, 2); 
    addEdge(adj, 2, 5, 4); 
    addEdge(adj, 3, 4, 9); 
    addEdge(adj, 3, 5, 14); 
    addEdge(adj, 4, 5, 10); 
    addEdge(adj, 5, 6, 2); 
    addEdge(adj, 6, 7, 1); 
    addEdge(adj, 6, 8, 6); 
    addEdge(adj, 7, 8, 7); 
  
    dijkstra(adj, V, 0); 
  
    return 0; 
}
```
应用例题leecode 743

https://zh.wikipedia.org/wiki/%E6%88%B4%E5%85%8B%E6%96%AF%E7%89%B9%E6%8B%89%E7%AE%97%E6%B3%95
https://zhuanlan.zhihu.com/p/357580063

#### Floyd
Floyd-Warshall算法（英语：Floyd-Warshall algorithm），中文亦称弗洛伊德算法或佛洛依德算法，是解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权（但不可存在负权回路）的最短路径问题，同时也被用于计算有向图的传递闭包。

以带权有向图为例，用邻接矩阵表示该图。
![](images/floyd.png)
A为最短距离矩阵，初始化为邻接矩阵；Path为路径矩阵，初始化都为-1，表示不经过中间点。

对于每个顶点 \( v \), 和任一顶点对 \( (i, j), i \neq j, v \neq i, v \neq j \), 如果 \( A[i][j]>A[i][v]+A[v][j] \), 则将 \( A[i][j] \) 更新为 \( A[i][v]+A[v][j] \) 的值, 并且将  \(Path[i][j]\) 改为 \( v \)。

v可以看成对角线元素的位置，做十字，依次遍历对角线元素。在遍历过程中，**v所在的行和列不需要访问，v行和列中的 INF 对应的行与列也不需要访问。**

根据邻接矩阵floyd算法的实现如下：
```cpp
void printPath(int u, int v, const std::vector<std::vector<int>>& Path) // 打印任意两点之间的最短路径
{
   
    if(Path[u][v]==-1) {
        std::cout << "<" << u << "," << v << ">" << std::endl; 
        return;
    }
    int mid = Path[u][v];
    printPath(u, mid, Path);
    printPath(mid, v, Path);
}

void floyd(const std::vector<std::vector<int>>& adjMat)
{
    const int n = adjMat.size();
    std::vector<std::vector<int>> A(adjMat);  // 最短距离矩阵
    std::vector<std::vector<int>> Path(n, std::vector<int>(n, -1)); // 路径矩阵
    for(int v = 0; v < n; ++v) // 依次访问对角线元素对应的坐标
        for(int i = 0; i < n; ++i){ // v所在的行和列不需要访问，v行和列中的 INF 对应的行与列也不需要访问
            if(i==v) continue;
            for(int j = 0; j < n; ++j){
                if(j==v) continue;
                if(A[i][v] < INT_MAX && A[v][j] < INT_MAX && A[i][j] > A[i][v]+A[v][j]){
                    A[i][j] = A[i][v]+A[v][j];
                    Path[i][j] = v;
                }
            }
        }
    printPath(1, 0, Path);
}
```




### 最小生成树
在连通边赋权图 $G$ 中求一棵总权值最小的生成树. 该生成树称为**最小生成树**或最小代价树.
#### Kruskal(克鲁斯克尔)算法

算法步骤（以边为基础，比较适合于稀疏图）：
1. 选择边 $e_1$, 使得其权值最小;
2. 若已经选定边 $e_1, e_2, · · · , e_k$, 则从 $E − {e_1, e_2, · · · , e_k}$ 中选择边 $e_{k+1}$, 使得：
    - $G[e_1, e_2, · · · , e_{k+1}]$ 为无圈图；
    - $e_{k+1}$ 的权值 $w(e_{k+1})$ 尽可能小.
3. 当 2. 不能进行时，停止.

>由克鲁斯克尔算法得到的任何生成树一定是最小生成树.

```cpp
class UnionFindSet{
public:
    std::vector<int> parent; // 记录节点的根
    std::vector<int> rank;   // 记录根节点的深度（用于优化）
    std::vector<int> size;   // 记录每个连通分量的节点个数
    std::vector<int> len;    // 记录每个连通分量里的所有边长度
    int num;            // 记录节点个数
    UnionFindSet(int n): parent(n), rank(n), len(n, 0), size(n, 1), num(n) {
        for (int i = 0; i < n; i++) 
            parent[i] = i;
    }
    int find(int x) {
        return x == parent[x] ? x : parent[x] = find(parent[x]);
    }

    int merge(int x, int y, int length) {
        int xRoot = find(x);
        int yRoot = find(y);
        if (xRoot != yRoot) {
            if (rank[xRoot] < rank[yRoot]) {
                std::swap(xRoot, yRoot);
            }
            parent[yRoot] = xRoot;
            if (rank[xRoot] == rank[yRoot]) rank[xRoot] += 1;
            // yRoot 的父节点设置为 xRoot,同时将 yRoot 的节点数和边长度累加到 xRoot,
            size[xRoot] += size[yRoot];
            len[xRoot] += len[yRoot] + length;
            // 如果某个连通分量的节点数 包含了所有节点，直接返回边长度
            if (size[xRoot] == num) return len[xRoot];
        }
        return -1;
    }
};
struct Edge {
    int start; // 顶点1
    int end;   // 顶点2
    int len;   // 长度
};

class Solution {
public:
    int kruskal(const std::vector<std::vector<int>>& points) {
        int res = 0;
        int n = points.size();
        std::vector<Edge> edges;
        // 建立点-边式数据结构
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                Edge edge = {i, j, abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])};
                edges.emplace_back(edge);
            }
        }
        // 按边长度排序
        sort(edges.begin(), edges.end(), [](const auto& a, const auto& b) {
            return a.len < b.len;
        });

        UnionFindSet unionFindSet(n);
        // 连通分量合并
        for (auto& e : edges) {
           res = unionFindSet.merge(e.start, e.end, e.len);
           if (res != -1) return res;
        }
        return 0;
    }
};
```

#### Prim 算法

算法步骤（以顶点为基础，比较适合于稠密图）：
1. 对于连通赋权图 $G$ 的任意一个顶点 $u$，选择与点$u$关联的且权值最小的边作为最小生成树的第一条边 $e1$;
2. 在接下来的边 $G[e_2, e_3, · · · , e_m]$, 在与一条已经选取的边只有一个公共端点的的所有边中，选取权值最小的边. 直到找到最小生成树.

基于邻接矩阵的代码实现如下：
```cpp
int prim(const std::vector<std::vector<int>>& adjMat)
{
    const int n = adjMat.size();
    int sumLowCost = 0;
    std::vector<int> lowcost(n, INT_MAX); // -1 代表加入最小生成树
    int start = 0; // 可以设置任意起点
    lowcost[start] = -1;
    for(int i = 0; i < n; ++i){ // 初始化lowcost[]
        if(i==start) continue;
        lowcost[i] = adjMat[i][start];
    }
    for(int i = 1; i < n; ++i){
        int minIdx = -1, minVal = INT_MAX; // 找到离已有的最小生成树最近的点
        for(int j = 0; j < n; ++j)
            if(lowcost[j]!=-1&&lowcost[j]<minVal){
                minVal = lowcost[j];
                minIdx = j;
            }
        sumLowCost += minVal;
        lowcost[minIdx] = -1;
        for(int j = 0; j < n; ++j)
            if(lowcost[j]!=-1 && adjMat[j][minIdx] < lowcost[j])
                lowcost[j] = adjMat[j][minIdx]; //更新lowcost[]
    }
    return sumLowCost;
}
```


### 图的着色

```cpp
class Solution {
private:
    bool dfs(int curNode, int curColor, const vector<vector<int>>&adj, vector<int>& color)
    {
        color[curNode] = curColor;
        for(auto& nextNode : adj[curNode]){
            if(color[nextNode]&&color[curNode]==color[nextNode]) return false;
            if(!color[nextNode]&& !dfs(nextNode, 3^curColor, adj, color)) return false;
        }
        return true;
    }
public:
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        vector<vector<int>> adj(n);
        vector<int> color(n, 0);
        for(auto& i : dislikes){
            adj[i[0]-1].push_back(i[1]-1);
            adj[i[1]-1].push_back(i[0]-1);
        }
        for(int i = 0; i < n; ++i)
            if(!color[i]&&!dfs(i, 1, adj, color)) return false;
        return true;
    }
};
```

### 拓扑排序
当且仅当图中没有定向环时（即**有向无环图**），才有可能进行拓扑排序。有向无环图如下所示：
<div align=center>
    <img src="images/graph_DAG.png" width = "40%" height = "40%">
</div>




## 哈希表
### 哈希表介绍
哈希表（hash table）也叫散列表，是一种以 "key-value" 形式存储数据的数据结构。哈希表可以看成一个数组, 每个数组下标对应一个桶（bucket）,因此哈希表也可以叫桶数组。所谓以 "key-value" 形式存储数据，是指任意的键(key) 都唯一对应到内存中的某个位置。使用嵌套接口entry将键值对的对应关系封装成对象, 然后把entry放到哈希表对应的桶(bucket)中，根据哈希表的索引就可以访问到entry，从而访问到"value"。**哈希表插入、删除和查找的时间复杂度是$O(1)$**。

### 哈希函数
假设哈希表的长度为M ( 即有M个bucket )，键（Key）个数为N, 但键值的取值范围很大，为[0, R),且满足 R >> M > N。如下图所示，要让键（Key）对应到内存中的位置，就要为键（Key）计算出在哈希表的下标。哈希函数hash()的作用是根据key计算出其在hashtable中的下标（哈希值），也就是enrty在哈希表的下标（哈希值）。这是一种映射，也可看成将键（Key）取值范围空间[0, R)压缩到哈希表数组空间[0, M)。

看一个例子，关于学生信息，假设一个班有40人，每个学生对应一个学号，六位学号随机分布在 000000-999999 范围内, 可以根据学号后两位查询学生姓名。根据前文可设 N = 40, key的取值范围为[000000, R)，且 R = 1000000； 哈希表数组长度为 100，即 M = 100。联系哈希表和哈希函数，可得到下图：

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/hashtable.png )
> 根据键值对的 key，经过 hashtable[hash(key)] 即在内存中可访问到key对应的 value。

hash表数组不一定能装满，hash表数组空间的覆盖情况与hash函数的设计紧密相关。

#### 哈希函数的设计
好的哈希函数满足假设：
- 确定：同一 key被映射到同一下标位置
- 快速：理想时间复杂度为$O(1)$
- 满射：尽可能充分地覆盖整个哈希表数组空间
- 均匀：避免过度集中的映射，减少哈希表冲突

1. **除余法**
```
hash(key) = key mod M, 其中 M 表示哈希表的大小，mod 为取余操作
```
> M 要取为素数，否则 key 映射到[0, M)的均匀性将大幅降低，发生冲突的概率随 M 所含素因子的增大而迅速增大
2. **MAD法(multiply-add-divide method)** 

```
hash(key) = (a x key + b) mod M, 其中 M 为素数，a > 0, b > 0, 且 a mod M 不等于 0
```
3. **(伪)随机数法**
```
hash(key) = rand(key) mod M, 其中 rand(key) 为系统定义的随机数
```
> <font color=Red>越是随机、越是没有规律，就越是好的散列函数</font>

4. **数字分析法**

假设关键字是以r为基的数，并且哈希表中可能出现的关键字都是事先知道的，则可取关键字的若干数位组成哈希表下标。

5. **折叠法**

将关键字分割成位数相同的几部分（最后一部分的位数可以不同），然后取这几部分的叠加和（舍去进位）作为哈希表下标。

6. **位异或法**

将key的二进制展开分割成等宽的若干段，经异或运算得到哈希表下标。

### 解决哈希冲突

如前面学生信息，如果两个学生的学号的后两位相同，则它们被哈希函数映射到哈希表的同一桶（bucket） or 槽位，这样就产生冲突，解决哈希冲突常用两种方法，即**开放寻址法**和**链接技术法**。
#### 开放寻址法
开放寻址法（Open Addressing），将所有的entry都存放在哈希表内的数组中，不使用额外的数据结构。

1. 线性探查（Linear Probing）

实现步骤：
- 当插入新的元素时，使用哈希函数在哈希表中定位元素位置；
- 检查哈希表中该位置是否已经存在元素。如果该位置内容为空，则插入并返回，否则转向下一步；
- 如果该位置为 i，则检查 i+1 是否为空，如果已被占用，则检查 i+2，依此类推，直到找到一个内容为空的位置。
> 线性探查方式虽然简单，但它会导致同类哈希的聚集（Primary Clustering）
2. 二次探查

二次探查在线性探查基础上进行改进，即每次检查位置空间的步长为平方倍数。也就是说，如果位置 i 被占用，则首先检查 i + 1^2 处，然后再检查 i - $1^2$，i + $2^2$，i - $2^2$，i + $3^2$ 依此类推，而不是像线性探查那样以 i + 1，i + 2 $\cdots$ 方式增长。尽管如此，二次探查同样也会导致同类哈希聚集问题。

3. 二度哈希

二度哈希是另一种改进的开放寻址方法， 二度哈希的工作原理如下：有一个包含一组哈希函数 $H_1, H_2\cdots H_n$ 的集合。当需要从哈希表中添加或获取元素时，首先使用哈希函数 $H_1$。如果导致冲突，则尝试使用 $H_2$，以此类推，直到 $H_n$。所有的哈希函数都与 $H_1$ 十分相似，不同的是它们选用的乘法因子（multiplicative factor）。

#### 链接技术法

在链接技术法中，把映射到哈希表的同一桶（bucket）的所有元素都放到一个链表中，即每个桶都包含一个链表以存储相同哈希值（对应哈希表的下标）的元素。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/hash_link-table.png )

> 使用链接技术添加(哈希值计算和链表操作)、查询，删除元素的时间复杂度均为$O(1)$

**参考：**
- <https://zhuanlan.zhihu.com/p/95156642>
- [邓俊辉数据结构（C++语言版）](https://dsa.cs.tsinghua.edu.cn/~deng/ds/dsacpp/)
- [HashMap实现原理及源码分析](https://www.cnblogs.com/chengxiao/p/6059914.html)
- [维基百科$\cdot$哈希表](https://zh.wikipedia.org/wiki/%E5%93%88%E5%B8%8C%E8%A1%A8)
- [哈希表和完美哈希](https://www.cnblogs.com/gaochundong/p/hashtable_and_perfect_hashing.html)



## 堆和优先级队列

堆（Heap）是计算机科学中的一种特别的完全二叉树。若是满足以下特性，即可称为堆：“给定堆中任意节点P和C，若P是C的母节点，那么P的值会小于等于（或大于等于）C的值”。若母节点的值恒小于等于子节点的值，此堆称为最小堆（min heap）；反之，若母节点的值恒大于等于子节点的值，此堆称为最大堆（max heap）。在堆中最顶端的那一个节点，称作根节点（root node），根节点本身没有母节点（parent node）。

**优先级队列**
优先队列（priority queue）是计算机科学中的一类抽象数据类型。优先队列中的每个元素都有各自的优先级，优先级最高的元素最先得到服务；优先级相同的元素按照其在优先队列中的顺序得到服务。优先队列通常使用“堆”（heap）实现。

```
//"<"为从大到小排列，">"为从小打到排列
struct CompareLess{ //最小堆， 队列中元素按从小到大排序
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs){
        return lhs.second > rhs.second;
    }
};
struct CompareMore{ //最大堆， 队列中元素按从大到小排序
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs){
        return lhs.second < rhs.second;
    }
};
//unordered_map<int, int> umap;
// 添加1--iterator
priority_queue<pair<int, int>, vector<pair<int, int>>, CompareMore> priQue(umap.begin(), umap.end());
// 添加2--push
priority_queue<pair<int, int>, vector<pair<int, int>>, CompareLess> priQue;
for(auto it = umap.begin(); it != umap.end(); ++it) priQue.push(*it);
// 弹出
priQue.pop();
//打印
while(!priQue.empty()){
    cout << priQue.top() <<endl;
    priQue.pop();
}
```

## 多路归并

某些情况下使用优先级队列方便优化

``264.`` 丑数||
```cpp
int nthUglyNumber(int n) {
    priority_queue<long, vector<long>, greater<double>> priQue;
    long res = 1;
    for(int i = 0; i < n-1;++i){
        priQue.push(res*2);
        priQue.push(res*3);
        priQue.push(res*5);
        res = priQue.top(); priQue.pop();
        while(!priQue.empty()&&res==priQue.top()) priQue.pop();
    }
    return res;
}

int nthUglyNumber(int n) {
    vector<int> res(n+1);
    res[1]=1;
    for(int p2 = 1, p3 = 1, p5 = 1, idx = 2; idx <=n; ++idx){
        int num2 = res[p2]*2, num3 = res[p3]*3, num5 = res[p5]*5;
        int minNum = min(min(num2, num3), num5);
        res[idx] = minNum;
        if(minNum==num2) ++p2;
        if(minNum==num3) ++p3;
        if(minNum==num5) ++p5;
    }
    return res[n];
}
```
``313.`` 超级丑数

```cpp
int nthSuperUglyNumber(int n, vector<int>& primes) {
    const int m = primes.size();
    vector<long> res(n+1);
    vector<int> p(m, 1);
    res[1] = 1;
    for(int i = 2; i <= n; ++i){
        long minNum = LONG_MAX;
        for(int j = 0; j<m; ++j)
            minNum = min(minNum, res[p[j]]*primes[j]);
        res[i] = minNum;
        for(int j = 0; j<m; ++j)
            if(minNum==res[p[j]]*primes[j]) ++p[j];
            
    }
    return res[n];
}
```


``373.`` 查找和最小的K对数字
```cpp
vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k){
    const int m = nums1.size(), n =nums2.size();
    auto cmp = [&nums1, &nums2](const pair<int, int>& lhs, const pair<int, int>& rhs){
        return nums1[lhs.first]+nums2[lhs.second] > nums1[rhs.first]+nums2[rhs.second];
    };
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> priQue(cmp);
    vector<vector<int>> res;
    for(int i = 0; i < min(m, k); ++i) priQue.push({i, 0});
    while(k-->0 && !priQue.empty()){
        auto [x, y] = priQue.top(); priQue.pop();
        res.push_back(vector<int>{nums1[x], nums2[y]});
        if(y+1<n) priQue.push({x, y+1});
    }
    return res;
}
```
``632.`` 最小区间

``719.`` 找出第 k 小的距离对

```cpp
int smallestDistancePair(vector<int>& nums, int k) {
    const int n = nums.size();
    sort(nums.begin(), nums.end());
    int lo = 0, hi = nums[n-1]-nums[0];
    while(lo<=hi){
        int cnt = 0, mid = lo + ((hi-lo)>>1);
        for(int i = 0, j = 0; j<n; ++j){
            while(nums[j]-nums[i]>mid) ++i;
            cnt += j-i;
        }
        cnt >= k ? hi = mid -1 : lo = mid+1;
    }
    return lo;
}
```

``1439.`` 有序矩阵中的第 k 个最小数组和

**难**

``1508.`` 子数组和排序后的区间和
```cpp
int rangeSum(vector<int>& nums, int n, int left, int right) {
    int MOD = 1e9+7;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> priQue;
    for(int i = 0; i < n; ++i) priQue.push({nums[i], i});
    int res = 0;
    for(int i = 1; i <= right; ++i){
        auto [x, y] = priQue.top(); priQue.pop();
        if(i>=left) res = (res +x) % MOD;;
        if(y+1<n) priQue.push({x+nums[y+1], y+1});
    }
    return res;
}
```


## 排序算法
![](images/sort.png)
### 冒泡排序
冒泡排序（英语：Bubble Sort）又称为泡式排序，是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。
冒泡排序算法的运作如下：
- 比较相邻的元素。如果第一个比第二个大，就交换它们两个。
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

伪代码：
```
函数 冒泡排序 输入 一个数组名称为array 其长度为length
    i 从 0 到 (length - 1)
        j 从 0 到 (length - 2 - i)
            如果 array[j] > array[j + 1]
                交换 array[j] 和 array[j + 1] 的值
            如果结束
        j循环结束
    i循环结束
函数结束
```

冒泡排序通过不断扫描交换实现，**对于长度为n的序列，共需做n - 1次比较和不超过n - 1次交换，这一过程称作一趟扫描交换**。下图中由7个整数组成的序列A[7] = {5, 2, 7, 4, 6, 3, 1}。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_bubble.png"/>

冒泡排序代码如下：
```cpp
void bubbleSort(vector<int>& vec, int lo, int hi) // [lo, hi), hi = A.size()
{
    while(lo < --hi)
        for(int i = lo; i < hi; ++i)
            if(vec[i] > vec[i+1]) 
                std::swap(vec[i], vec[i+1]);
}
```

### 选择排序
选择排序（Selection sort）是一种简单直观的排序算法, 它适用于向量与列表之类的序列结构。该算法将序列划分为**无序前缀**和**有序后缀**两部分；此外，还要求前缀不大于后缀。如此，每次只需从前缀中选出最大者，并作为最小元素转移至后缀中，即可使有序部分的范围不断扩张。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_selection_sort.png"/>
**在任何时刻，后缀S(r, n)已经有序，且不小于前缀S[0, r]**。

选择排序的实现代码（基于向量）如下：
```cpp
void selectionSort(vector<int>& vec, int lo, int hi) // [lo, hi), hi = A.size()
{
    while(lo < --hi){
        int mx = hi, last = hi; // mx 对应[lo, hi]中的最大者的索引
        while(lo < last--) 
            if(vec[last]>vec[mx]) mx = last;
        swap(vec[mx], vec[hi]); //将[hi]与[lo, hi]中的最大者交换
    }   
}
```

### 插入排序
插入排序（英语：Insertion Sort）是一种简单直观的排序算法，它适用于包括向量与列表在内的任何序列结构。该算法始终将整个序列视作并切分为两部分：**有序的前缀**和**无序的后缀**；通过迭代，反复地将后缀的首元素转移至前缀中。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_insert_sort.png"/>
**在任何时刻，相对于当前节点e = S[r]，前缀S[0, r)总是业已有序**。

插入排序的完整代码（基于向量）实现如下：
```cpp
void insertionSort(vector<int>& vec, int lo, int hi) // [lo, hi), hi = A.size()
{
    for(int i = lo+1; i < hi; ++i){
        int key = vec[i], j = i-1;
        while(j>=lo&&vec[j]>key) {
            vec[j+1] = vec[j]; 
            --j;
        }
        vec[j+1] = key;
    }
}
```
插入排序的完整代码（基于列表）实现如下：
```cpp
ListNode* insertionSortList(ListNode* head) {
    ListNode* dummyHead = new ListNode(0);
    dummyHead->next = head;
    while(head && head->next) {
        if(head->val < head->next->val) {
            head = head->next; continue;
        }
        ListNode* pre = dummyHead;
        while(pre->next->val < head->next->val) pre = pre->next; // 将 head->next 插入到 pre后面
        ListNode* tmp = head->next;
        head->next = head->next->next;
        tmp->next = pre->next;
        pre->next = tmp;
    }
    return dummyHead->next;
}
```

### 希尔排序
希尔排序（Shellsort），也称**递减增量排序算法**，是插入排序的一种更高效的改进版本。希尔排序是非稳定排序算法。
希尔排序是基于插入排序的以下两点性质而提出改进方法的：
- 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率
- 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

**步长的选择是希尔排序的重要部分。只要最终步长为1任何步长序列都可以工作**。Donald Shell最初建议步长选择为$\frac{n}{2}$并且对步长取半直到步长达到1。虽然这样取可以比$\mathcal {O}(n^{2})$类的算法（插入排序）更好，但这样仍然有减少平均时间和最差时间的余地。

希尔排序的完整代码（基于向量）实现如下：
```cpp
void shellSort(vector<int>& vec, int lo, int hi)
{
    int len = hi - lo;
    for(int inc = len/2; inc>0; inc /= 2){
        for(int i = lo + inc; i < hi; ++i){ // 每一趟采用插入排序
            int key = vec[i], j = i - inc;
            for(; j>=lo&&vec[j]>key; j-=inc)
                vec[j+inc] = vec[j];
            vec[j+inc] = key;
        }
    }
}
```

### 归并排序
归并排序（英语：Merge sort，或mergesort），是创建在归并操作上的一种有效的排序算法，效率为$\displaystyle O(n\log n)$（大$O$符号）。1945年由约翰·冯·诺伊曼首次提出。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。
**归并排序**也可以理解为是通过反复调用所谓**二路归并**（2-way merge）算法而实现的。

所谓二路归并，就是将两个**有序序列**合并成为一个有序序列。有序向量{ 5, 8, 13, 21 }和{ 2, 4, 10, 29 }的二路归并实现如下：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_two_way_merge.png"/>

归并排序的主体结构属典型的**分治策略**。下图归并排序递归实例：S = { 6, 3, 2, 7, 1, 5, 8, 4 }。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_merge_sort.png"/>
如上图所示，上半部分对应于递归的不断深入过程：不断地均匀划分（子）向量，直到其规模缩减至1从而抵达递归基。下半部分，开始递归返回。通过反复调用二路归并算法，相邻且等长的子向量不断地捉对合并为规模更大的有序向量，直至最终得到整个有序向量。由此可见，归并排序可否实现、可否高效实现，关键在于二路归并算法。

归并排序使用递归方法的完整代码（基于向量）实现如下：
```cpp
// merge sort
// 方式1
void twoWayMerge(vector<int>& vec, int lo, int mi, int hi) // [lo, mi)和[mi, hi)各自有序，lo < mi < hi
{
    int index = lo; // vec对应的索引 [lo, hi)
    int i = 0, lenA = mi - lo;
    vector<int> A(lenA); // A 需要新建
    for(int i=0; i<lenA; ++i) A[i] = vec[lo+i]; //前子向量A = vec[lo, mi)
    int j = mi; //后子向量 vec[mi, hi)
    while(i<lenA && j<hi)
        vec[index++] = A[i] <= vec[j] ? A[i++] : vec[j++];
    while(i<lenA) // 后子向量 vec[mi, hi)先耗尽
        vec[index++] = A[i++];  //若前子向量先耗尽，vec直接保持后子向量的内容
}
// 方式2
void twoWayMerge(vector<int>& vec, int lo, int mid, int hi) // [lo, mi)和[mi, hi)各自有序，lo < mi < hi
{
    vector<int> tmpVec(hi-lo);
    int index = 0, i = lo, j = mid;
    while(i<mid && j < hi)
        tmpVec[index++] = vec[i]>vec[j] ? vec[i++] : vec[j++];
    while(i<mid) tmpVec[index++] = vec[i++];
    while(j < hi) tmpVec[index++] = vec[j++];
    for(int i = 0; i < hi-lo; ++i)
        vec[lo+i] = tmpVec[i];
}
void mergeSort(vector<int>& vec, int lo, int hi)
{
    if(hi-lo < 2) return;
    int mi = (lo+hi)/2; //以中点为界
    mergeSort(vec, lo, mi); mergeSort(vec, mi, hi); //分别排序
    twoWayMerge(vec, lo, mi, hi); //归并
}
```

归并排序使用递归方法的完整代码（基于列表）实现如下：
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
ListNode* twoWayMerge(ListNode* list1, ListNode* list2) { //list1, list2各自有序
    ListNode dummyHead(0);
    ListNode* cur = &dummyHead;
    while(list1 && list2){
        ListNode* &tmpNode = list1->val < list2->val ? list1 : list2;
        cur = cur->next = tmpNode;
        tmpNode = tmpNode->next;
    }
    cur->next = list1 ? list1 : list2;
    return dummyHead.next; 
}
ListNode* mergeSort(ListNode* head) {
    if (!head || !head->next) return head;
    ListNode* slow = head, *fast = head;
    while (fast->next && fast->next->next)
        slow = slow->next, fast = fast->next->next;
    fast = slow->next, slow->next = nullptr; // 切链
    return twoWayMerge(mergeSort(head), mergeSort(fast));
}
```

### 快速排序
快速排序（英语：Quicksort），又称分区交换排序（partition-exchange sort），简称快排，一种排序算法，最早由东尼·霍尔提出。在平均状况下，排序n个项目要$O(n\log n)$（大O符号）次比较。在最坏状况下则需要$O(n^{2})$次比较，但这种状况并不常见。事实上，快速排序$\Theta (n\log n)$通常明显比其他算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地达成。

快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为较小和较大的2个子序列，然后递归地排序两个子序列。
步骤为：
1. 挑选基准值：从数列中(随机)挑出一个元素，称为**基准（pivot）**
2. 分割：重新排序数列，**所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面**（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准值的排序就已经完成。
3. 递归排序子序列：**递归地**将小于基准值元素的子序列和大于基准值元素的子序列排序。

递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。
**选取基准值（pivot）有数种具体方法，此选取方法对排序的时间性能有决定性影响**。

快速排序使用递归方法的完整代码（基于向量）实现如下：
```cpp
// https://www.bilibili.com/video/BV1at411T75o/?spm_id_from=333.337.search-card.all.click&vd_source=7e207334419dc8142fdc0d30691a9168
int partition(vector<int>& vec, int lo, int hi) // [lo, hi)
{   
    swap(vec[lo], vec[lo+rand()%(hi-lo)]); //任选一个元素与首元素交换, rand()*(hi-lo)取值[0, hi-lo-1]
    --hi;
    int pivotValue = vec[lo];  //以首元素为候选轴点——经以上交换，等效于随机选取
    while(lo<hi){ //从向量的两端交替地向中间扫描
        while(lo<hi) 
            if(pivotValue < vec[hi]) --hi; // hi直接向左移动
            else { vec[lo++] = vec[hi]; break;} //vec[hi]移到lo, lo向前移动一步
        while(lo<hi)
            if(pivotValue > vec[lo]) ++lo;
            else { vec[hi--] = vec[lo]; break;}
    } //assert: lo == hi 
    vec[lo] = pivotValue; //将备份的 pivotValue 记录置于前、后子向量之间
    return lo; //返回 pivot
}
void quickSort(vector<int>& vec, int lo, int hi) // [lo, hi), hi = A.size()
{
    if(hi-lo < 2) return;
    int pivot = partition(vec, lo, hi); //在[lo, hi)内构造轴点
    quickSort(vec, lo, pivot);  //[lo, pivot)
    quickSort(vec, pivot+1, hi); //[pivot+1, hi)  
}
```

### 堆排序
堆排序（英语：Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆是一个近似完全二叉树的结构，并同时满足堆的性质：即子节点的键值或索引总是小于（或者大于）它的父节点。
- 大顶堆：每个结点的值都大于或等于其左右孩子结点的值
- 小顶堆：每个结点的值都小于或等于其左右孩子结点的值
  
如图，(a)为大顶堆，(b)为对应的下标, (c)为大顶堆对应的有序数组。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_heap_sort.png"/>

**堆节点的访问**
通常堆是通过一维数组来实现的。在数组起始位置为**0**的情形中：
- 父节点 i 的左子节点在位置 (2i+1);
- 父节点 i 的右子节点在位置 (2i+2);
- 子节点 i 的父节点在位置 $\lfloor (i-1)/2\rfloor$;

**堆的操作**
在堆的数据结构中，堆中的最大值总是位于根节点。堆中定义以下几种操作：
- 最大堆调整（heapify）：维护某一节点的堆性质，使得子节点永远小于父节点
- 创建最大堆（buildHeap）：将堆中的所有数据重新排序，将无序数组调整为有序的大顶堆
- 堆排序（sort）：将堆顶元素（最大值）与最后一个元素交换，移除最后一个元素，最后对交换过的堆顶做最大堆调整的递归运算
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_heap_sort_algorithm.png"/>

堆排序的完整代码（基于向量）实现如下：
```cpp
/* 维护某一节点 idx 的堆性质: O(logn)
 * @param vec : 存储堆的向量
 * @param inx : 待维护节点的下标， inx < hi
 */
void heapify(vector<int>& vec, int idx, int end)   // idx < end
{
    int largest = idx, left = 2*idx+1, right = 2*idx+2;
    if(left < end && vec[largest] < vec[left]) largest = left;
    if(right < end && vec[largest] < vec[right]) largest = right; // 此时，largest为三个点最大值的下标
    if(largest!=idx) {
        swap(vec[idx], vec[largest]);
        heapify(vec, largest, end);
    }
}
// O(nlogn)
void heapSort(vector<int>& vec) // [lo, hi)
{
    const int len = vec.size();
    // build heap: O(n), 将无序数组调整为有序的大顶堆, 下表为0的位置对应最大值
    for(int i = len/2-1; i >=0; --i) // 初始化，i从最后一个父节点开始调整
        heapify(vec, i, len);
    // sort
    for(int end = len - 1; end > 0; --end) // i = lo 时，就剩一个元素不用管
    {
        swap(vec[0], vec[end]); // 堆顶元素和最后一个元素交换
        heapify(vec, 0, end); // 维护剩余vec元素的大堆性质
    }
}
```

### 计数排序
计数排序（Counting sort）是一种稳定的线性时间排序算法。该算法于1954年由 Harold H. Seward 提出。计数排序使用一个额外的数组C ，其中第i个元素是待排序数组A中值等于i的元素的个数。然后根据数组 C 来将A中的元素排到正确的位置。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_counting_sort.png"/>

计数排序的完整代码实现如下：
```cpp
// counting sort
void countingSort(vector<int>& vec, int lo, int hi)
{
    int max = vec[lo];
    for(int i = lo+1; i < hi; ++i)
        if(vec[i]>max) max = vec[i]; // 找到[lo, hi)中最大值
    vector<int> output(hi-lo), count(max+1);
    for(int i = lo; i < hi; ++i)
        ++count[vec[i]];
    for(int j = 1; j < max + 1; ++j)
        count[j] += count[j-1];
    for(int i = hi-1; i>=lo; --i){
        output[count[vec[i]]-1] = vec[i];
        --count[vec[i]];
    }
    for(int i = lo; i < hi; ++i) 
        vec[i] = output[i-lo];     
}
```



### 桶排序
桶排序（Bucket sort）或所谓的箱排序，是一个排序算法，工作的原理是将数组分到有限数量的桶里。每个桶再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。

桶排序以下列程序进行：
- 设置一个定量的数组当作空桶子。
- 寻访序列，并且把项目一个一个放到对应的桶子去。
- 对每个不是空的桶子进行排序。
- 从不是空的桶子里把项目再放回原来的序列中。


例子：假设数据分布在[0，25)之间，桶的数量设定为5个，每个桶内部用链表（局部有序）表示，在数据入桶的同时插入排序。然后把各个桶中的数据合并（整体有序）。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_bucket_sort.png"/>

桶排序的完整代码（桶内基于链表）实现如下：
```cpp
#include<iostream>
#include<vector>
using namespace std;
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
ListNode* insert(ListNode* head, int val)
{
    ListNode dummyHead(0);
    dummyHead.next = head;
    ListNode* prev = &dummyHead, *cur = head;
    while(cur && cur->val <= val){
        prev = cur;
        cur = cur->next;
    }
    ListNode* newNode = new ListNode(val);
    prev->next = newNode;
    newNode->next = cur;
    return dummyHead.next;
}
ListNode* twoWayMerge(ListNode* list1, ListNode* list2) { //list1, list2各自有序
    ListNode dummyHead(0);
    ListNode* cur = &dummyHead;
    while(list1 && list2){
        ListNode* &tmpNode = list1->val < list2->val ? list1 : list2;
        cur = cur->next = tmpNode;
        tmpNode = tmpNode->next;
    }
    cur->next = list1 ? list1 : list2;
    return dummyHead.next; 
}
void bucketSort(vector<int>& vec, int lo, int hi) // [lo, hi)
{
    //const int BUCKET_NUM = 5;
    const int maxValue = *(max_element(vec.begin()+lo, vec.begin()+hi));
    const int minValue = *(min_element(vec.begin()+lo, vec.begin()+hi));
    const int BUCKET_NUM = maxValue-minValue+1;
    vector<ListNode*> buckets(BUCKET_NUM, (ListNode*)(0));
    for(int i = lo; i < hi; ++i){
        //int index = vec[i]/BUCKET_NUM;
        int index = vec[i]-minValue;
        ListNode* head = buckets[index];
        buckets[index] = insert(head, vec[i]);
    }
    ListNode* head = buckets[0];
    for(int j = 1; j < BUCKET_NUM; ++j)
        head = twoWayMerge(head, buckets[j]);
    for(int i = lo; i < hi; ++i){
        vec[i] = head->val;
        head = head->next;
    }
}
```



### 基数排序
基数排序（英语：Radix sort）是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。基数排序的发明可以追溯到1887年赫尔曼·何乐礼在列表机（Tabulation Machine）上的贡献。它是这样实现的：**将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序**(放入桶，先放先取)。这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列。基数排序的方式可以采用**LSD（Least significant digital）**或MSD（Most significant digital），LSD的排序方式由键值的**最右边**开始，而MSD则相反，由键值的最左边开始。

例子：通过基数排序对数组{53, 3, 542, 748, 14, 214, 154, 63, 616}，它的示意图如下：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/sort_radix_sort.jpg"/>

基数排序的完整代码实现（反复调用计数排序）如下：
```cpp
// radix sort
void radixSort(vector<int>& vec, int lo, int hi)
{
    int max = vec[lo];
    for(int i = lo+1; i < hi; ++i)
        if(vec[i]>max) max = vec[i]; // 找到[lo, hi)中最大值
    for(int exp = 1; max/exp > 0; exp*=10) // 按指数 exp 进行排序，exp 按个位。。。
    {
        vector<int> output(hi-lo), buckets(10); // buckets记录对应桶中数据的次数
        for(int i = lo; i < hi; ++i) 
            ++buckets[vec[i]/exp%10];
        for(int j = 1; j < 10; ++j) // 更改buckets[i]。目的是让更改后的buckets[i]的值，是该数据在output[]中的位置。
            buckets[j]+=buckets[j-1];
        for(int i = hi-1; i>=lo; --i){  // 将数据存储到临时数组output中
            output[buckets[vec[i]/exp%10] - 1] = vec[i];
            --buckets[vec[i]/exp%10];
        }
        for(int i = lo; i < hi; ++i) 
            vec[i] = output[i-lo];        
    }
}
```
**参考：**
- [冒泡排序](https://www.runoob.com/w3cnote/bubble-sort.html)
- [基数排序](https://www.cnblogs.com/skywang12345/p/3603669.html)
- [Radix Sort](https://www.geeksforgeeks.org/radix-sort/)
- [Heap Sort](https://www.oreilly.com/library/view/algorithms-in-a/9780596516246/ch04s06.html)
- [堆排序讲解](https://www.bilibili.com/video/BV1fp4y1D7cj/?spm_id_from=333.788&vd_source=7e207334419dc8142fdc0d30691a9168)


## 查找算法
### 二分查找
在计算机科学中，二分查找算法（英语：binary search algorithm），也称折半搜索算法（英语：half-interval search algorithm）、对数搜索算法（英语：logarithmic search algorithm，是一种在**有序数组**中查找某一特定元素的搜索算法。二分查找算法在最坏情况下是对数时间复杂度的，需要进行 O(logn)次比较操作。
```cpp
// 迭代版本 1
int binarySearch(const vector<int>& vec, int target, int lo, int hi) // [lo, hi)内查找元素 target 的索引
{
    while(lo < hi){
        int mid = lo + ((hi-lo)>>1);
        if(vec[mid] > target) hi = mid; //深入前半段[lo, mi)继续查找
        else if(vec[mid] < target) lo = mid + 1; //深入后半段(mi, hi)继续查找
        else return mid;
    }
    return -1;
}
int binarySearch(const vector<int>& vec, int target, int lo, int hi) // [lo, hi]内查找元素 target 的索引
{
    while(lo <= hi){
        int mid = lo + ((hi-lo)>>1);
        if(vec[mid] > target) hi = mid-1; 
        else if(vec[mid] < target) lo = mid + 1;
        else return mid;
    }
    return -1;
}

// 迭代版本 2 -- 推荐
int binarySearch(const vector<int>& vec, int target, int lo, int hi) // [lo, hi)内查找元素 target 的索引
{
    while(1 < hi-lo){
        int mid = lo + ((hi-lo)>>1);
        vec[mid] > target ? hi = mid : lo = mid; //经比较后确定深入[lo, mi)戒[mi, hi)
    } //出口时hi = lo + 1，查找区间仅含一个元素A[lo]
    return target == vec[lo] ? lo : -1;
}
int binarySearch(const vector<int>& vec, int target, int lo, int hi) // [lo, hi]内查找元素 target 的索引
{
    while( hi>lo){
        int mid = lo + ((hi-lo)>>1);
        vec[mid] < target ? lo = mid+1 : hi = mid; //经比较后确定深入[lo, mi]和[mi+1, hi]
    } //出口时hi = lo ，查找区间仅含一个元素A[lo, lo]
    return target == vec[lo] ? lo : -1;
}

// 递归版本 1
int binarySearch(const vector<int>& vec, int target, int lo, int hi) // [lo, hi)内查找元素 target 的索引
{
    if(lo>=hi) return -1;
    int mid = lo + (hi - lo) / 2;
    if(vec[mid] > target) return binarySearch(vec, target, lo, mid); //深入前半段[lo, mi)继续查找
    else if(vec[mid] < target) return binarySearch(vec, target, mid+1, hi); //深入后半段(mi, hi)继续查找
    else return mid;
}


// 递归版本 2
int binarySearch(const vector<int>& vec, int target, int lo, int hi)
{
    if (hi > lo) {
        int mid = lo + (hi - lo) / 2;
        if (vec[mid] == target)
            return mid;
        if (vec[mid] > target)
            return binarySearch(vec, target, lo, mid);
        return binarySearch(vec, target, mid + 1, hi);
    }
    return -1;
}
```


## 贪心算法
### 基本概念
   
**贪心的本质是选择每一阶段的局部最优，从而达到全局最优**， 而并非从整体考虑。贪心算法没有固定的算法框架，算法设计的关键是贪心策略的选择。必须注意的是，贪心算法不是对所有问题都能得到整体最优解，选择的贪心策略必须具备**无后效性**，即某个状态以后的过程不会影响以前的状态，只与当前状态有关。

### 基本思路

1. 建立数学模型来描述问题。
2. 把求解的问题分成若干个子问题。
3. 对每一子问题求解，得到子问题的局部最优解。
4. 把子问题的解局部最优解合成原来解问题的一个解。

### 基本要素
对于一个具体的问题，怎么知道是否可用贪心算法解此问题，以及能否得到问题的最优解呢?这个问题很难给予肯定的回答。可以用贪心算法求解的问题具有两个性质：
1. 贪心选择性质
所谓贪心选择性质是指所求问题的整体最优解可以通过一系列局部最优的选择，即贪心选择来达到。这是贪心算法可行的第一个基本要素，也是贪心算法与动态规划算法的主要区别。
2. 最优子结构性质
当一个问题的最优解包含其子问题的最优解时，称此问题具有最优子结构性质。问题的最优子结构性质是该问题可用动态规划算法或贪心算法求解的关键特征。

**参考：**
- [贪心算法](https://www.cnblogs.com/chinazhangjie/archive/2010/11/23/1885330.html)


## 动态规划

动态规划（英语：Dynamic programming，简称DP）是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。动态规划背后的基本思想非常简单。大致上，若要解一个给定问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。通常许多子问题非常相似，为此动态规划法试图仅仅解决每个子问题一次，从而减少计算量：一旦某个给定子问题的解已经算出，则将其记忆化存储，以便下次需要同一个子问题解之时直接查表。这种做法在重复子问题的数目关于输入的规模呈指数增长时特别有用。

动态规划常常适用于**有重叠子问题，无后效性的问题和最优子结构性质**的问题：
- **最优子结构性质**。如果问题的最优解所包含的子问题的解也是最优的，我们就称该问题具有最优子结构性质（即满足最优化原理）。最优子结构性质为动态规划算法解决问题提供了重要线索。
- **无后效性**。即子问题的解一旦确定，就不再改变，不受在这之后、包含它的更大的问题的求解决策影响。
- **子问题重叠性质**。子问题重叠性质是指在用递归算法自顶向下对问题进行求解时，每次产生的子问题并不总是新问题，有些子问题会被重复计算多次。动态规划算法正是利用了这种子问题的重叠性质，对每一个子问题只计算一次，然后将其计算结果保存在一个表格中，当再次需要计算已经计算过的子问题时，只是在表格中简单地查看一下结果，从而获得较高的效率，降低了时间复杂度。**动态规划保存递归时的结果，因而不会在解决同样的问题时花费时间**。

> 动态规划中每一个状态一定是由上一个状态推导出来的，这一点就区分于贪心，贪心没有状态推导，而是从局部直接选最优的

动态规划的三要素是**最优子结构、状态转移方程和边界**
**动规五部曲分析如下**：
- 确定dp数组（dp table）以及下标的含义
- 确定递推公式
- dp数组如何初始化
- 确定遍历顺序
- 举例推导dp数组

使用动态规划的算法:
- 最长公共子序列
- Floyd-Warshall算法
- Viterbi算法
- 求解马可夫决策过程下最佳策略

**实例应用**
### 01背包问题
有$n$件物品和一个最多能背重量为$w$ 的背包。第i件物品的重量是$weight[i](0\leq i \leq n-1)$，得到的价值是$value[i](0\leq i \leq n-1)$。**每件物品只能用一次**，求解将哪些物品装入背包里物品价值总和最大。

- 使用二维数组，即dp[i][j] 表示**从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少**。
  - 不放物品 i : ``dp[i][j] = dp[i-1][j]``
  - 放入物品 i : ``dp[i][j] = dp[i-1][j-weight[i]] + value[i]``
  
  因此，递推公式： ``dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i])``

- 使用一维度数组，即dp[j]表示**容量为j的背包，所背的物品价值可以最大为dp[j]**
  递推公式：``dp[j] = max(dp[j], dp[j - weight[i]] + value[i])``

**例：** 4件物品，weight[] = {1, 2, 3, 4}, value[] = {3, 4, 5, 6}, 书包载重5。
写出使用二维数组的01背包动态规划程序：
```cpp
vector<vector<int>> knapSack(void)
{
    int wKnapSack = 5; // 背包载重 5
    vector<int> weight = {1, 2, 3, 4}, value = {3, 4, 5, 6}; // 物品
    const int len = weight.size();
    vector<vector<int>> dp(len, vector<int>(wKnapSack+1, 0));
    
    for(int j = weight[0]; j <= wKnapSack; ++j) dp[0][j] = value[0];  //初始化
    for(int i = 1; i < len; ++i)
        for(int j = 0; j <= wKnapSack; ++j) {
            if(j < weight[i]) dp[i][j] = dp[i-1][j];
            else dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i]);
        }
    return dp;
}
```

写出使用一维数组的01背包动态规划程序：
```cpp
int knapSack(void)
{
    int wKnapSack = 5; // 背包载重 5
    vector<int> weight = {1, 2, 3, 4}, value = {3, 4, 5, 6}; // 物品
    const int len = weight.size();
    vector<int> dp(wKnapSack+1, 0);
    
    for(int i = 0; i < len; ++i)
        for(int j = wKnapSack; j >= weight[i]; --j) 
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    return dp[wKnapSack];
}
```

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/os/dp_01blag.png"/>


### 完全背包问题
有$n$件物品和一个最多能背重量为$w$ 的背包。第i件物品的重量是$weight[i](0\leq i \leq n-1)$，得到的价值是$value[i](0\leq i \leq n-1)$。**每件物品都有无限个（也就是可以放入背包多次）**，求解将哪些物品装入背包里物品价值总和最大。

写出使用一维数组的背包动态规划程序：
- 如果求组合数就是外层for循环遍历物品，内层for遍历背包。
- 如果求排列数就是外层for遍历背包，内层for循环遍历物品。
```cpp
int completeKnapSack(void)
{
    int wKnapSack = 5; // 背包载重 5
    vector<int> weight = {1, 2, 3, 4}, value = {3, 4, 5, 6}; // 物品
    const int len = weight.size();
    vector<int> dp(wKnapSack+1, 0);
    
    // 先遍历物品，再遍历背包
    for(int i = 0; i < len; ++i)
        for(int j = weight[i]; j <= wKnapSack; ++j) 
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    /*
    // 先遍历背包，再遍历物品
    for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
        for(int i = 0; i < weight.size(); i++) { // 遍历物品
            if (j - weight[i] >= 0) 
                dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    */
    return dp[wKnapSack];
}
```

### 打家劫舍
```cpp
// dp[i]：考虑下标i（包括i）以内的房屋，最多可以偷窃的金额为dp[i]。
int rob(vector<int>& nums) {
    const int len = nums.size();
    if(len == 0) return 0;
    if(len == 1) return nums[0];
    vector<int> dp(len, 0);
    dp[0] = nums[0];
    dp[1] = max(nums[0], nums[1]);
    for(int i = 2; i < len; ++i)
        dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
    return dp[len-1];
}
```
### 股票问题
最佳买卖股票时机含冷冻期
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/dsa/dp_sell_stock.png"/>

```cpp
/* 动态规划
 * 0 状态一：买入股票状态（今天买入股票，或者是之前就买入了股票然后没有操作）
 * 卖出股票状态，这里就有两种卖出股票状态
 *   1 状态二：两天前就卖出了股票，度过了冷冻期，一直没操作，今天保持卖出股票状态
 *   2 状态三：今天卖出了股票
 * 3 状态四：今天为冷冻期状态，但冷冻期状态不可持续，只有一天！
 * dp[i][j]中 i表示第i天，j为 [0 - 3] 五个状态，dp[i][j]表示第i天状态j所剩最大现金。
 */
int maxProfit(vector<int>& prices) {
    const int len = prices.size();
    if(len == 0) return 0;
    vector<vector<int>> dp(len, vector<int>(4));
    dp[0][0] = - prices[0];
    for(int i = 1; i < len; ++i) {
        dp[i][0] = max(dp[i-1][0], max(dp[i-1][3], dp[i-1][1])-prices[i]);
        dp[i][1] = max(dp[i-1][1], dp[i-1][3]);
        dp[i][2] = dp[i-1][0] + prices[i];
        dp[i][3] = dp[i-1][2];
    }
    return max(dp[len-1][3], max(dp[len-1][1], dp[len-1][2]));
}
```

### 子序列问题
最长公共子序列（不连续）
```cpp
/* 动态规划
 * dp[i][j] ：长度为[0, i - 1]的字符串text1与长度为[0, j - 1]的字符串text2的最长公共子序列为dp[i][j]
 *  dp[i][0] 和 dp[0][j]其实都是没有意义的！均为0
 * 如果text1[i - 1] 与 text2[j - 1]相同，那么找到了一个公共元素，所以dp[i][j] = dp[i - 1][j - 1] + 1;
 * 如果text1[i - 1] 与 text2[j - 1]不相同，那就看看text1[0, i - 2]与text2[0, j - 1]的最长公共子序列 
 *              和 text1[0, i - 1]与text2[0, j - 2]的最长公共子序列，取最大的。
 */
int longestCommonSubsequence(string text1, string text2) {
    const int lenA = text1.size(), lenB = text2.size();
    vector<vector<int>> dp(lenA + 1, vector<int>(lenB + 1));
    for(int i = 1; i <= lenA; ++i) {
        for(int j = 1; j <= lenB; ++j){
            if(text1[i-1] == text2[j-1]) dp[i][j] = dp[i-1][j-1] +1;
            else  dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[lenA][lenB];
}
```
**参考：**
- [维基百科$\cdot$动态规划](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92)
- [图解|原来这就是动态规](https://www.cnblogs.com/flashsun/p/14448939.html)
  
       

## 分治法
分治法，字面意思是“分而治之”，就是把一个复杂的1问题分成两个或多个相同或相似的子问题，再把子问题分成更小的子问题直到最后子问题可以简单地直接求解，原问题的解即子问题的解的合并，这个思想是很多高效算法的基础，例如排序算法(快速排序，归并排序)，傅里叶变换(快速傅里叶变换)等。

分治法的基本思想：将一个难以直接解决的大问题，分割成一些规模较小的相同问题，以便各个击破，分而治之。

通过循环递归实现：在每一层递归上都有三个步骤：
- 分解：将原问题分解为若干个规模较小，相对独立，与原问题形式相同的子问题。
- 解决：若子问题规模较小且易于解决时，则直接解。否则，递归地解决各子问题。
- 合并：将各子问题的解合并为原问题的解。
  
分治法适用的情况：分治法所能解决的问题一般具有以下几个特征：
1. 该问题的规模缩小到一定的程度就可以容易地解决
2. 该问题可以分解为若干个规模较小的相同问题，即该问题具有最优子结构性质。
3. 利用该问题分解出的子问题的解可以合并为该问题的解；
4. 该问题所分解出的各个子问题是相互独立的，即子问题之间不包含公共的子子问题。


使用分治法求解的经典问题：
- 二分搜索
- 大整数乘法
- Strassen矩阵乘法
- 棋盘覆盖
- 合并排序
- 快速排序
- 线性时间选择
- 最接近点对问题
- 循环赛日程表
- 汉诺塔

**总结：** 分治算法的一个核心在于子问题的规模大小是否接近，如果接近则算法效率较高。分治算法和动态规划都是解决子问题，然后对解进行合并；但是分治算法是寻找远小于原问题的子问题（因为对于计算机来说计算小数据的问题还是很快的），同时分治算法的效率并不一定好，而动态规划的效率取决于子问题的个数的多少，子问题的种类个数远小于子问题的总数的情况下（也就是重复子问题多），算法才会很高效。



## 回溯法
回溯算法的求解过程实质上是一个先序遍历一棵"**状态树**"的过程,只是这棵树不是遍历前预先建立的,而是隐含在遍历过程中。在不断**试探**中回溯，回溯算法的本质是穷举，效率并不高。可以通过**剪枝**处理，提高效率。

**回溯三部曲：回溯函数、回溯函数终止条件、回溯搜索遍历过程**

```
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

### 组合问题
给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。
```cpp
vector<vector<int>> res;
vector<int> path;
void backTracking(int n, int k, int start){
    if((int)path.size()==k){
        res.push_back(path);
        return;
    }
    for(int i = start; i <= n-(k-(int)path.size())+1; ++i){ //剪枝优化
        path.push_back(i);
        backTracking(n, k, i+1);
        path.pop_back();
    }
}

vector<vector<int>> combine(int n, int k) {
    backTracking(n, k, 1);
    return res;
}
```

### 切割问题
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。返回 s 所有可能的分割方案。
```cpp
template<typename Iter>
inline bool isPalindrome(Iter first, Iter last)
{
    for(; first < last; ++first, --last)
        if(*first != *last) return false;
    return true;
}

vector<vector<string>> res;
vector<string> path;
void backtracking(const string& str, string::iterator first){
    if(first >= str.end()){
        res.push_back(path);
        return;
    }
    for(auto last = first; last != str.end(); ++last){
        if(isPalindrome(first, last)){
            string s = string(first, last+1);
            path.push_back(s);
            backtracking(str, last+1);
            path.pop_back();
        }
    }
}

vector<vector<string>> partition(string s) {
    backtracking(s, s.begin());
    return res;
}
```

### 子集问题
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。说明：解集不能包含重复的子集。
```cpp
vector<vector<int>> res;
vector<int> path;
void backtracking(const vector<int>& nums, int start)
{
    res.push_back(path);
    //if(start>=(int)nums.size()) return;
    for(int i = start; i < (int)nums.size(); ++i){
        path.push_back(nums[i]);
        backtracking(nums, i+1);
        path.pop_back();
    }
}
vector<vector<int>> subsets(vector<int>& nums) {
    backtracking(nums, 0);
    return res;
}
```

### 排列问题
给定一个 没有重复 数字的序列，返回其所有可能的全排列。
```cpp
vector<vector<int>> res;
vector<int> path;
void backtracking(const vector<int>& nums, vector<bool>& used)
{
    if(path.size()==nums.size()){
        res.push_back(path);
        return;
    }
    for(int i = 0; i < (int)nums.size(); ++i){
        if(used[i] == true ) continue; // path里已经收录的元素，直接跳过
        path.push_back(nums[i]);
        used[i]= true;
        backtracking(nums, used);
        used[i]= false;
        path.pop_back();
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<bool> used(nums.size(), false);
    backtracking(nums, used);
    return res;
}
```

### 棋盘问题
**N皇后**：不能同行、不能同列、不能同斜线
```cpp
vector<vector<string>> res;
inline
bool isValid(int row, int col, vector<string>& chessboard, int n)
{
    for(int i = 0; i < row; ++i) // 检查列
        if(chessboard[i][col] == 'Q') return false;
    for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) // 检查45度
        if(chessboard[i][j] == 'Q') return false;
    for(int i = row - 1, j = col + 1; i >= 0 && j >= 0; --i, ++j) // 检查135度数
        if(chessboard[i][j] == 'Q') return false;
    return true;
}
void backtracking(int n, int row, vector<string>& chessboard)
{
    if(row == n) { 
        res.push_back(chessboard);
        return;
    }
    for(int col = 0; col < n; ++col) {
        if(isValid(row, col, chessboard, n)){
            chessboard[row][col] = 'Q';
            backtracking(n, row+1, chessboard);
            chessboard[row][col] = '.';
        }
    }
}
vector<vector<string>> solveNQueens(int n) {
    vector<string> chessboard(n, string(n, '.'));
    backtracking(n, 0, chessboard);
    return res;
}
```

**解数独**
```cpp
inline
bool isValid(int row, int col, char val, vector<vector<char>>& board)
{
    for(int i = 0; i < 9; ++i) {
        if(board[row][i]==val) return false;
        if(board[i][col]==val) return false;
    }
    int startRow = (row/3) * 3;
    int startCol = (col/3) * 3;
    for(int i = startRow; i < startRow + 3; ++i)
        for(int j = startCol; j < startCol + 3; ++j)
            if(board[i][j]==val) return false;
    return true;
}

bool backtracking(vector<vector<char>>& board)
{
    for(int i = 0; i < 9; ++i)
        for(int j = 0; j < 9; ++j) {
            if(board[i][j] != '.') continue;
            for(char k = '1'; k <= '9'; ++k) {
                if(isValid(i, j, k, board)) {
                    board[i][j] = k;
                    if(backtracking(board)) return true;
                    board[i][j] = '.';
                }
            }
            return false;
        }
    return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
}
void solveSudoku(vector<vector<char>>& board) {
    backtracking(board);
}
```
## 分支界定


> 注意回溯和分支界定的区别



## 排列组合







