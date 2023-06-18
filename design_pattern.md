# Design Pattern Notes
<h2 align="left">目录</h2>


[toc]


设计模式使得代码更高效，条理更清晰，开拓思维。很多项目使用了设计模式。**既有稳定部分也有变化部分使用设计模式才有意义**。设计模式是可复用面向对象软件的基础软件设计目标是**复用性**，运用抽象，可扩展性高，将变化减为最小。

**可能考察问题**

基础考点：
设计模式实现，代码库
设计模式应用，某些场景下应该使用什么设计模式

理论考点
1. 简单介绍设计模式软件开发原则
2. 设计模式如何分类
3. 单例设计模式有什么优缺点？什么时候使用它？
4. 代理模式。。。
5. 策略模式。。。
6. 简单工厂、工厂方法和抽象工厂模式有什么区别？

实践能力考点：
1. 举例说明你在做项目时，如何应用软件开发原则？
2. 你使用过哪些设计模式？
3. 你在哪些情况下会使用设计模式
4. 单例模式有哪些实现方式？分别有什么优缺点？请手写其中一种

源码应用考察：
1. 举例说明设计模式在某个框架源码中的应用
2. 介绍一下代理模式，说明静态代理和动态代理的区别
3. 举例建造者模式在框架源码中的应用？



## 面向对象设计原则

1. 依赖倒置原则（DIP）
    - 高层模块(稳定)不应该依赖于低层模块(变化)，二者都应该依赖于抽象(稳定) 。
        - 可以通过隔离
    - 抽象(稳定)不应该依赖于实现细节(变化) ，实现细节应该依赖于抽象(稳定)。
2. 开放封闭原则(OCP)
    - 对扩展开放，对更改封闭
    - 类模块应该是可扩展的，但是不可修改
3. 单一职责原则(SRP)
    - 一个类应该仅有一个引起它变化的原因
    - 变化的方向隐含类的责任
4. Liskov替换原则(LSP)
    - 子类必须能够替换他们的基类(IS-A)
    - 继承表达类型抽象
5. 接口隔离原则(ISP)
    - 不应该强迫客户程序依赖它们不用的方法。
    - 接口应该小而完备。
6. 优先使用对象组合，而不是类继承
    - 类继承通常为“白箱复用”, 对象组合通常为“黑箱复用”
    - 继承在某种程度上破坏了封装性，子类父类耦合度高
    - 而对象组合则要求被组合的对象具有良好定义的接口，耦合度低
7. 封装变化点
    - 使用封装来创建对象之间的分界层，让设计者可以在分界层的一侧进行修改，而不会对另一侧产生不良影响，从而实现层次间的松耦合
8. 针对接口编程，而不是针对实现编程
    - 不将变量类型声明为某个具体类，而是声明为某个接口
    - 客户程序无需获知对象的具体类型，只需要知道对象所具有的接口
    - 减少系统中各部分的依赖关系，从而实现“高内聚、松耦合”的类型设计方案

## 设计模式分类介绍

**从目的来看**：
1. 创建型（Creational）模式：将对象的部分创建工作延迟到子类或者其他对象，从而应对需求变化为对象创建时具体类型实现引来的冲击。
    - （5种）工厂模式、抽象工厂模式、单例模式、建造者模式、原型模式
2. 结构型（Structural）模式：通过类继承或者对象组合**获得更灵活的结构**，从而应对需求变化为对象的结构带来的冲击。
    - （7种）适配器模式、装饰者模式、代理模式、外观模式、桥接模式、组合模式、享元模式
3. 行为型（Behavioral）模式：通过类继承或者对象组合**来划分类与对象间的职责**，从而应对需求变化为多个交互的对象带来的冲击。
    - （11种）策略模式、模板方法模式、观察者模式、迭代器模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式

**从范围来看**：
1. 类模式处理类与子类的静态关系。
2. 对象模式处理对象间的动态关系。

**从封装变化角度对模式分类**：

<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/design_pattern_class.jpg" width = "85%" height = "85%">
</div>

1. **“组件协作”模式**: 现代软件专业分工之后的第一个结果是“框架与应用程序的划分”，“组件协作”模式通过晚期绑定，来实现框架与应用程序之间的松耦合，是二者之间协作时常用的模式。
2. **“单一职责”模式**: 在软件组件的设计中，如果责任划分的不清晰，使用继承得到的结果往往是随着需求的变化，子类急剧膨胀，同时充斥着重复代码，这时候的关键是划清责任。 
3. **“对象创建”模式**：通过“对象创建” 模式绕开new，来避免对象创建（new）过程中所导致的紧耦合（依赖具体类），从而支持对象创建的稳定。它是接口抽象之后的第一步工作。
4. **“对象性能”模式**：面向对象很好地解决了“抽象”的问题，但是不可避免地要付出一定代价。对于通常情况来讲， 面向对象地成本大都可以忽略不计。但是某些情况，面向对象所带来地成本必须谨慎考虑。
5. **“接口隔离”模式**：在组件构建过程中,某些接口之间直接的依赖常常会带来很多问 题、甚至根本无法实现。采用添加一层**间接**(稳定)接口，来**隔离**本来互相紧密关联的接口是一种常见的解决方案。（OS是软件和硬件的间接接口）
6. **“状态变化”模式**： 在组件构建过程中，某些对象的状态经常面临变化，如何对这些变化进行有效的管理？同时又维持高层模块的稳定？ “状态变化”模式为这一问题提供了一种解决方案。
7. **“数据结构”模式**：常常有一些组件在内部具有特定的数据结构，如果让客户程序依赖这些特定的数据结构，将极大地破坏组件的复用。这时候，将这些特定数据结构封装在内部, 在外部提供统一的接口，来**实现与特定数据结构无关的访问**，是一种行之有效的解决方案。
8. **“行为变化”模式**：在组件构建过程中，组件行为的变化经常导致组件本身剧烈的变化。<u>“行为变化”模式将组件的行为和组件本身进行解耦，从而支持组件行为的变化</u>，实现两者之间的松耦合。
9. **“领域规则”模式**：在特定领域中,<u>某些变化虽然频繁，但可以抽象为某种规则</u>。这时候，结合特定领域，将问题抽象为语法规则，从而给出在该领域下的一般性解决方案。
   

计模式的应用不宜先入为主，一上来就使用设计模式是对设计模式的最大误用。没有一步到位的设计模式。敏捷软件开发实践提倡的“Refactoring to Patterns”是目前普遍公认的最好的使用设计模式的方法。**重构的关键技法**：
- 静态->动态
- 早绑定->晚绑定
    ![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/bind.jpg)
    - 早期绑定：子类通过继承调用父类成员函数，并且子类实现程序流程函数
    - 晚期绑定：父类通过动态绑定调用子类重载的虚函数，并且程序流程函数由父类实现
- 继承->组合
- 编译时依赖->运行时依赖
- 紧耦合->松耦合


## Template Method 模式

**动机：** 在软件构建过程中，对于某一项任务，<u>它常常有稳定的整体操作结构，但各个子步骤却有很多改变的需求</u>，或者由于固有的原因（比如框架与应用之间的关系）而无法和任务的整体结构同时实现。

Template Method模式是一种非常基础性的设计模式，在面向对象系统中有着大量的应用。**它用最简洁的机制(虚函数的多态性)** 为很多应用程序框架提供了灵活的扩展点，是代码复用方面的基本实现结构。

**在基类中接口中定义非虚函数(稳定的)，定义(纯)虚函数(不稳定的)。稳定部分通过基类实现，不稳定部分通过子类 overwrite 虚函数来实现。将子类的指针赋值给基类指针，这样基类指针可以通过动态绑定调用子类重载的虚函数。此外，基类定义程序流程的函数，这样子类只需要重载实现从父类继承的虚函数即可。** 这是一种**晚期绑定**方式。程序实现如下：

```cpp
class Library{
public:
    void Run(){
        Step1();
        if (Step2()) { 
            Step3(); 
        }
        for (int i = 0; i < 4; i++){
            Step4(); 
        }
        Step5();
    }
    virtual ~Library(){ } 

protected:
    void Step1() {
        //... 
    }
	void Step3() {
        //... 
    }
	void Step5() {
        //... 
    }
    virtual bool Step2() = 0;
    virtual void Step4() =0; 
};

class Application : public Library {
protected:
    virtual bool Step2(){
		//... 
    }
    virtual void Step4() {
		//... 
    }
};

int main()
{
    Library* pLib=new Application();
    lib->Run();
    delete pLib;
}

```
> 1. **基类的析构函数应该写成虚函数**，因为基类的指针动态绑定指向子类，如果基类析构函数是非虚的，则基类的指针无法通过动态绑定调用子类的析构函数
> 2. 被Template Method(基类)调用的虚方法可以具有实现，也可以没有任何实现（抽象方法、纯虚方法），但一般推荐将它们设置为protected方法。

程序分工如下：
1. Library 开发人员：
    - 开发1, 3, 5三个步骤
    - 程序主流程
2. Application 开发人员：
    - 开发2, 4步骤

与之相对的结构化设计流程使用**早期绑定**，子类直接调用父类的成员函数，并且实现程序主流程。没有使用Template Method 模式，这是本人之前常使用的方式。代码实现如下：
```cpp
class Library{

public:
    void Step1(){
        //... 
    }
    void Step3(){
        //... 
    }
    void Step5(){
        //... 
    }
};

class Application{
public:
    bool Step2(){
        //... 
    }
    void Step4(){
        //... 
    }
};

int main()
{
    Library lib();
    Application app();
    lib.Step1();
    if (app.Step2()){
        lib.Step3();
    }
    for (int i = 0; i < 4; i++){
        app.Step4();
    }
    lib.Step5();
}
```
程序分工如下：
1. Library 开发人员：
    - 开发1, 3, 5三个步骤
2. Application 开发人员：
    - 开发2, 4步骤
    - 程序主流程

## Strategy 策略模式

**动机**
在软件构建过程中，某些对象使用的算法可能多种多样，经常改变，如果将这些算法都编码到对象中，将会使对象变得异常复杂；而且**有时候支持不使用的算法也是一个性能负担**(多个if else 就需要判断)。

如何在运行时根据需要透明地更改对象的算法？将算法与对象本身解耦，从而避免上述问题？

**模式定义**
定义一系列算法，把它们一个个封装起来，并且使它们可互相替换（变化）。该模式**使得算法可独立于使用它的客户程序(稳定)而变化**（扩展，子类化）。

**结构（Structure）**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/strategy_model2.png)

**代码实现**
普通方式违背了开放封闭原则(OCP)，实现如下：
```cpp
enum TaxBase {
    CN_Tax,
    US_Tax,
    DE_Tax,
    FR_Tax       //更改
};

class SalesOrder{
    TaxBase tax;
public:
    double CalculateTax(){
    //...
        
        if (tax == CN_Tax){
            //CN***********
        }
        else if (tax == US_Tax){
            //US***********
        }
        else if (tax == DE_Tax){
            //DE***********
        }
        else if (tax == FR_Tax){  //更改
            //...
        }
        //....
     }  
};
```
使用策略模式实现如下：
```cpp
class TaxStrategy{
public:
    virtual double Calculate(const Context& context)=0;
    virtual ~TaxStrategy(){}
};


class CNTax : public TaxStrategy{
public:
    virtual double Calculate(const Context& context){
        //***********
    }
};

class USTax : public TaxStrategy{
public:
    virtual double Calculate(const Context& context){
        //***********
    }
};

class DETax : public TaxStrategy{
public:
    virtual double Calculate(const Context& context){
        //***********
    }
};

//扩展
//*********************************
class FRTax : public TaxStrategy{
public:
    virtual double Calculate(const Context& context){
	    //.........
    }
};

class SalesOrder{
private:
    TaxStrategy* strategy; // 多态，使用指针

public:
    SalesOrder(StrategyFactory* strategyFactory){
        this->strategy = strategyFactory->NewStrategy(); // 由工厂方法返回那个堆对象，即调用哪个类对象: CN, US, DE, FR
    }
    ~SalesOrder(){
        delete this->strategy;
    }
    public double CalculateTax(){
        //...
        Context context();
        
        double val = strategy->Calculate(context); //多态调用
        //...
    }
};
```

**要点总结**
1. Strategy及其子类为组件提供了一系列可重用的算法，从而可以使得类型在**运行时**方便地根据需要在各个算法之间进行切换。
2. Strategy模式提供了用条件判断语句以外的另一种选择，消除条件判断语句，就是在解耦合。含有许多条件判断语句的代码通常都需要Strategy模式。
3. 如果Strategy对象没有实例变量，那么各个上下文可以共享同一个Strategy对象，从而节省对象开销。

## Observer 观察者模式

**动机**
在软件构建过程中，我们需要为某些对象建立一种“通知依赖关系” —— **一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知**。如果这样的依赖关系过于紧密，将使软件不能很好地抵御变化。

使用面向对象技术，可以将这种依赖关系弱化，并形成一种稳定的依赖关系。从而实现软件体系结构的松耦合。

**模式定义**
**观察者模式**又叫发布-订阅模式。定义对象间的一种一对多（变化）的依赖关系，以便**当一个对象(Subject)的状态发生改变时，所有依赖于它的对象都得到通知并自动更新**。观察者可以决定是否订阅目标对象的消息。
> Ceph文件系统通信机制使用了观察者模式

**结构（Structure）**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/observer_model.png)

通过抽象观察者进行软隔离，实现松耦合

**代码实现**
更新多个文件的进度条程序。普通方式违背了违背依赖倒置原则（DIP），实现如下：
```cpp
class FileSplitter
{
    string m_filePath;
    int m_fileNumber;
    ProgressBar* m_progressBar;
public:
    FileSplitter(const string& filePath, int fileNumber, ProgressBar* progressBar) :
        m_filePath(filePath), 
        m_fileNumber(fileNumber),
        m_progressBar(progressBar){
    }
    void split(){
        //1.读取大文件  
        //2.分批次向小文件中写入
        for (int i = 0; i < m_fileNumber; i++){
            //...
            float progressValue = m_fileNumber
            progressValue = (i + 1) / progressValue;
            m_progressBar->setValue(progressValue);
        }
    }
};

class MainForm : public Form
{
    TextBox* txtFilePath;
    TextBox* txtFileNumber;
    ProgressBar* progressBar;
public:
    void Button1_Click(){
        string filePath = txtFilePath->getText();
        int number = atoi(txtFileNumber->getText().c_str());
        FileSplitter splitter(filePath, number, progressBar);
        splitter.split();
    }
};
```

使用观察者模式实现如下：
```cpp
class IProgress{ // 抽象 observer
public:
    virtual void DoProgress(float value)=0;  // Update
    virtual ~IProgress(){}
};
class FileSplitter
{
    string m_filePath;
    int m_fileNumber;
    List<IProgress*>  m_iprogressList; // 抽象通知机制，支持多个观察者
public:
    FileSplitter(const string& filePath, int fileNumber) : 
        m_filePath(filePath), 
        m_fileNumber(fileNumber) {}
    
    
    void split(){
        //1.读取大文件
        //2.分批次向小文件中写入
        for (int i = 0; i < m_fileNumber; i++){
            //...
            float progressValue = m_fileNumber;
            progressValue = (i + 1) / progressValue;
            onProgress(progressValue);//发送通知, Notify(), 自动通知，不需要指定具体的观察者
        }
    }
    void addIProgress(IProgress* iprogress){
        m_iprogressList.push_back(iprogress); // 这里使用了多态
    }
    void removeIProgress(IProgress* iprogress){
        m_iprogressList.remove(iprogress);
    }
protected:
    virtual void onProgress(float value){
        List<IProgress*>::iterator itor=m_iprogressList.begin(); 
        while (itor != m_iprogressList.end() ){
            (*itor)->DoProgress(value); //更新进度条
            itor++;
        }
    }
};

//........................
class MainForm : public Form, public IProgress  // 具体观察者， 订阅
{
    TextBox* txtFilePath;
    TextBox* txtFileNumber;
    ProgressBar* progressBar;
public:
    void Button1_Click(){
        string filePath = txtFilePath->getText();
        int number = atoi(txtFileNumber->getText().c_str());
        ConsoleNotifier cn;
        FileSplitter splitter(filePath, number);
        splitter.addIProgress(this); //订阅通知， 可以决定要不要订阅
        splitter.addIProgress(&cn)； //订阅通知
        splitter.split();
        splitter.removeIProgress(this);
    }
    virtual void DoProgress(float value){
        progressBar->setValue(value);
    }
};
class ConsoleNotifier : public IProgress {  // 具体观察者，订阅
public:
    virtual void DoProgress(float value){
        cout << ".";
    }
};
```

**要点总结**
1. 使用面向对象的抽象，Observer模式使得我们可以独立地改变目标与观察者，从而使二者之间的依赖关系达致松耦合。
2. 目标发送通知时，无需指定观察者，通知（可以携带通知信息作为参数）会自动传播。
3. 观察者自己决定是否需要订阅通知，目标对象对此一无所知
4. Observer模式是基于事件的UI框架中非常常用的设计模式，也是MVC模式的一个重要组成部分。

## Decorator 装饰模式
**动机**
在某些情况下我们可能会“**过度地使用继承来扩展对象的功能**”，由于继承为类型引入的**静态特质**，使得这种扩展方式**缺乏灵活性**；并且随着子类的增多（扩展功能的增多），各种子类的组合（扩展功能的组合）会导致更多子类的膨胀。

如何使“对象功能的扩展”能够根据需要来动态地实现？同时避免“扩展功能的增多”带来的子类膨胀问题？从而使得任何“功能扩展变化”所导致的影响将为最低？

**模式定义**
动态（组合）地给一个对象增加一些额外的职责。就增加功能而言，**Decorator模式比生成子类（继承）更为灵活**（消除重复代码 & 减少子类个数）。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/decorator_0.png)

**代码实现**
实现IO流操作设计程序。普通方式使用大量的类继承操作，代码结构如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/decorator_1.png)

使用 Decorator 设计模式采用组合方式，实现在运行时动态扩展对象功能的能力。代码架构如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/decorator_2.png)

代码实现如下：

```cpp
class Stream{
public：
    virtual char Read(int number)=0;
    virtual void Seek(int position)=0;
    virtual void Write(char data)=0;
    
    virtual ~Stream(){}
};
//主体类
class FileStream: public Stream{
public:
    virtual char Read(int number){ //读文件流
    }
    virtual void Seek(int position){//定位文件流
    }
    virtual void Write(char data){//写文件流
    }
};

class NetworkStream :public Stream{
public:
    virtual char Read(int number){//读网络流
    }
    virtual void Seek(int position){//定位网络流
    }
    virtual void Write(char data){//写网络流
    }
};

class MemoryStream :public Stream{
public:
    virtual char Read(int number){//读内存流
    }
    virtual void Seek(int position){//定位内存流
    }
    virtual void Write(char data){//写内存流
    }  
};
//扩展操作
DecoratorStream: public Stream{ //1. 
protected:
    Stream* stream;//2. ...核心，使用组合方式实现多态。 1. 继承， 2. 组合 同时进行 -> decorator model 
    DecoratorStream(Stream * stm):stream(stm){}
};

class CryptoStream: public DecoratorStream {
public:
    CryptoStream(Stream* stm):DecoratorStream(stm){}
    virtual char Read(int number){  
        //额外的加密操作...
        stream->Read(number);//读文件流
    }
    virtual void Seek(int position){
        //额外的加密操作...
        stream::Seek(position);//定位文件流
        //额外的加密操作...
    }
    virtual void Write(byte data){
        //额外的加密操作...
        stream::Write(data);//写文件流
        //额外的加密操作...
    }
};
class BufferedStream : public DecoratorStream{
    Stream* stream;//...
public:
    BufferedStream(Stream* stm):DecoratorStream(stm){}
    //...
};
void Process(){
    //运行时装配
    FileStream* s1=new FileStream();
    CryptoStream* s2=new CryptoStream(s1);
    BufferedStream* s3=new BufferedStream(s1); 
    BufferedStream* s4=new BufferedStream(s2);
}
```
> DecoratorStream **即继承了 Stream 基类(虚函数，接口一致性)，也使用组合方式定义了 Stream 对象指针(Decorator Model 的核心)**

**要点总结**
1. 通过采用**组合**而非继承的手法， Decorator模式实现了**在运行时动态扩展对象功能**的能力，而且可以根据需要扩展多个功能。**避免了使用继承带来的“灵活性差”和“多子类衍生问题”**。
2. Decorator类在接口上表现为is-a Component的**继承关系**，即Decorator类继承了Component类所具有的接口。但在实现上又表现为has-a Component的**组合关系**，即Decorator类又使用了另外一个Component类。
3. Decorator模式的目的并非解决“多子类衍生的多继承”问题，Decorator模式应用的要点在于**解决“主体类在多个方向上的扩展功能”**——是为“装饰”的含义。


## Bridge 桥模式
 
**动机**
由于某些类型的固有的实现逻辑，使得它们 **具有两个变化的维度，乃至多个纬度的变化**。

如何应对这种“多维度的变化”？如何利用面向对象技术来使得类型可以轻松地沿着两个乃至多个方向变化，而不引入额外的复杂
度？

**模式定义**
将**抽象部分(业务功能)与实现部分(平台实现)分离，使它们都可以独立地变化**。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/bridge_model.jpg)

**代码实现**
实现message设计程序。普通方式使用大量的类继承操作，产生 1 + n + n*m 个类， 其中 n 表示平台个数，m 表示 每个平台对应的实现个数。

使用 Bridge 设计模式采用组合方式，解耦了抽象与实现固有的绑定。代码实现如下：

```cpp
class Messager{ // Abstraction， 稳定
protected:
     MessagerImp* messagerImp;//...
public:
    virtual void Login(string username, string password)=0;
    virtual void SendMessage(string message)=0;
    virtual void SendPicture(Image image)=0;
    virtual ~Messager(){}
};
class MessagerImp{ // Implementor， 稳定
public:
    virtual void PlaySound()=0;
    virtual void DrawShape()=0;
    virtual void WriteText()=0;
    virtual void Connect()=0;
    virtual MessagerImp(){}
};
//平台实现 n
class PCMessagerImp : public MessagerImp{
public:
    
    virtual void PlaySound(){/**********/}
    virtual void DrawShape(){/**********/}
    virtual void WriteText(){/**********/}
    virtual void Connect(){/**********/}
};
class MobileMessagerImp : public MessagerImp{
public:
    virtual void PlaySound(){/**********/}
    virtual void DrawShape(){/**********/}
    virtual void WriteText(){/**********/}
    virtual void Connect(){/**********/}
};

//业务抽象 m
//类的数目：1+n+m
class MessagerLite :public Messager {
public: 
    virtual void Login(string username, string password){  
        messagerImp->Connect(); //........
    }
    virtual void SendMessage(string message){    
        messagerImp->WriteText();//........
    }
    virtual void SendPicture(Image image){    
        messagerImp->DrawShape();//........
    }
};
class MessagerPerfect  :public Messager {
public:
    virtual void Login(string username, string password){    
        messagerImp->PlaySound();
        //********
        messagerImp->Connect();
        //........
    }
    virtual void SendMessage(string message){ 
        messagerImp->PlaySound();
        //********
        messagerImp->WriteText();
        //........
    }
    virtual void SendPicture(Image image){ 
        messagerImp->PlaySound();
        //********
        messagerImp->DrawShape();
        //........
    }
};
void Process(){
    //运行时装配
    MessagerImp* mImp=new PCMessagerImp();
    Messager *m =new Messager(mImp);
}
```

**要点总结**
1. Bridge模式使用“**对象间的组合关系**”**解耦了抽象和实现之间固有的绑定关系，使得抽象和实现可以沿着各自的维度来变化**。所谓抽象和实现沿着各自纬度的变化，即“子类化”它们。
2. Bridge模式有时候类似于多继承方案，但是多继承方案往往违背单一职责原则（即一个类只有一个变化的原因），复用性比较差。Bridge模式是比多继承方案更好的解决方法。
3. Bridge模式的应用一般在“两个非常强的变化维度”，有时一个类也有多于两个的变化维度，这时可以使用Bridge的扩展模式。

## Factory Method 工厂方法模式

**动机**
在软件系统中，经常面临着创建对象的工作；由于需求的变化，
需要创建的对象的具体类型经常变化。

如何应对这种变化？如何**绕过常规的对象创建方法(new)，提供一种“封装机制”来避免客户程序和这种“具体对象创建工作”的紧耦合**？

**模式定义**
定义一个用于创建对象的接口，让子类决定实例化哪一个类。**Factory Method使得一个类的实例化延迟**（目的：解耦，手段：虚函数）到子类。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/factory_method.png)

**代码实现**
实现更新多个文件的进度条程序(观察者模式章节程序绪)。普通方式使用了 new 创建对象，依赖了具体类，是紧耦合操作。

使用 Factory method 定义创建对象的接口，隔离类对象使用者和具体类型之间的耦合关系。代码实现如下：

```cpp
//抽象类
class ISplitter{
public:
    virtual void split()=0;
    virtual ~ISplitter(){}
};
//工厂基类
class SplitterFactory{
public:
    virtual ISplitter* CreateSplitter()=0;
    virtual ~SplitterFactory(){}
};

//具体类
class BinarySplitter : public ISplitter{  
};
class TxtSplitter: public ISplitter{   
};
class PictureSplitter: public ISplitter{   
};
class VideoSplitter: public ISplitter{   
};
// ...可以扩展

//具体工厂
class BinarySplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new BinarySplitter();
    }
};
class TxtSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new TxtSplitter();
    }
};
class PictureSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new PictureSplitter();
    }
};
class VideoSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new VideoSplitter();
    }
};
// ...可以扩展

class MainForm : public Form
{
    SplitterFactory* factory;//工厂, 动态，交给未来

public:
    MainForm(SplitterFactory*  factory){
        this->factory=factory;
    }
    void Button1_Click(){
    	ISplitter * splitter = factory->CreateSplitter(); //多态new
        splitter->split();
    }
};
```

**要点总结**
1. Factory Method模式**用于隔离类对象的使用者和具体类型之间的耦合关系**。面对一个经常变化的具体类型，紧耦合关系(new)会导致软件的脆弱。
2. Factory Method 模式通过**面向对象的手法，将所要创建的具体对象工作延迟到子类，从而实现一种扩展（而非更改）的策略**，较好地解决了这种紧耦合关系。
3. Factory Method模式解决“单个对象”的需求变化。缺点在于要求创建方法/参数相同。

## Abstract Factory 抽象工厂模式

**动机**
在软件系统中，经常**面临着“一系列相互依赖的对象”的创建工作**；同时，由于需求的变化，往往存在更多系列对象的创建工作。

如何应对这种变化？如何绕过常规的对象创建方法(new)，提供一种“封装机制”来避免客户程序和这种“多系列具体对象创建工作”的紧耦合？

**模式定义**
提供一个接口，**让该接口负责创建一系列“相关或者相互依赖的对象”**，无需指定它们具体的类。
> 抽象工厂是工厂方法模式的进一步操作，将相关的对象工厂合并，减少工厂数量

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/abstract_factory.png)

**代码实现**
实现数据库相关操作程序程序。使用 Abstract Factory 将某一类型操作的一些列操作合并为一个工厂，实现代码如下：
```cpp
//数据库访问有关的基类
class IDBConnection{  
};
class IDBCommand{
};
class IDataReader{
};
class IDBFactory{
public:
    virtual IDBConnection* CreateDBConnection()=0;
    virtual IDBCommand* CreateDBCommand()=0;
    virtual IDataReader* CreateDataReader()=0;
};

//支持SQL Server
class SqlConnection: public IDBConnection{  
};
class SqlCommand: public IDBCommand{  
};
class SqlDataReader: public IDataReader{  
};

class SqlDBFactory:public IDBFactory{
public:
    virtual IDBConnection* CreateDBConnection(){
        return new SqlConnection();
    }
    virtual IDBCommand* CreateDBCommand(){
        return new SqlCommand();
    }
    virtual IDataReader* CreateDataReader(){
        return new SqlDataReader();
    }
};

//支持Oracle
class OracleConnection: public IDBConnection{
};
class OracleCommand: public IDBCommand{  
};
class OracleDataReader: public IDataReader{ 
};

class EmployeeDAO{
private:
    IDBFactory* dbFactory; // 工厂，决定创建哪个类型的工厂
    
public:
    EmployeeDAO(IDBFactory* dbFactory){
        this->dbFactory = dbFactory;
    }
    vector<EmployeeDO> GetEmployees(){
        IDBConnection* connection = dbFactory->CreateDBConnection();
        connection->ConnectionString("...");

        IDBCommand* command = dbFactory->CreateDBCommand();
        command->CommandText("...");
        command->SetConnection(connection); //关联性

        IDBDataReader* reader = command->ExecuteReader(); //关联性
        while (reader->Read()){

        }

    }
};
```

**要点总结**
1. 如果没有应对“多系列对象构建”的需求变化，则没有必要使 Abstract Factory模式，这时候使用简单的工厂完全可以。
2. **“系列对象”指的是在某一特定系列下的对象之间有相互依赖、或作用的关系**。不同系列的对象之间不能相互依赖。
3. Abstract Factory模式主要在于应对“新系列”的需求变动。其缺点在于难以应对“新对象”的需求变动。

## Prototype 原型模式
**动机**
在软件系统中，经常**面临着“某些结构复杂的对象”的创建工作**（与工厂方法的区别，方便复杂状态的传递）；同时，由于需求的变化，这些对象经常面临着剧烈的变化，但是它们却拥有比较稳定一致的接口

如何应对这种变化？如何向“客户程序（使用这些对象的程序）”隔离出“这些易变对象”，从而使得“依赖这些易变对象的客户程序”不随着需求改变而改变？

**模式定义**
使用原型实例指定创建对象的种类，然后通过**拷贝**这些原型来创建新的对象

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/prototype.png)

**代码实现**
实现更新多个文件的进度条程序(是工厂方法模式程序的改进)。代码实现如下：
```cpp
//抽象类, 相当于将工厂方法抽象基类与工厂基类合并
class ISplitter{
public:
    virtual void split()=0;
    virtual ISplitter* clone()=0; //通过克隆自己来创建对象
    
    virtual ~ISplitter(){}
};

//具体类
class BinarySplitter : public ISplitter{
public:
    virtual ISplitter* clone(){
        return new BinarySplitter(*this);  // 深克隆
    }
};
class TxtSplitter: public ISplitter{
public:
    virtual ISplitter* clone(){
        return new TxtSplitter(*this);
    }
};
class PictureSplitter: public ISplitter{
public:
    virtual ISplitter* clone(){
        return new PictureSplitter(*this);
    }
};
class VideoSplitter: public ISplitter{
public:
    virtual ISplitter* clone(){
        return new VideoSplitter(*this);
    }
};

class MainForm : public Form
{
    ISplitter*  prototype;//原型对象
public:
    MainForm(ISplitter*  prototype){
        this->prototype=prototype;
    }
	void Button1_Click(){
		ISplitter * splitter = prototype->clone(); //克隆原型
        splitter->split();
	}
};
```

**要点总结**
1. Prototype模式同样用于隔离类对象的使用者和具体类型(易变类)之间的耦合关系，他们同样要求这些“易变类”拥有“稳定的接口”。
2. Prototype模式对于“如何创建易变类的实体对象”采用“原型克隆”的方法来做，他是的我们可以非常灵活地动态创建“拥有某些稳定接口”的新对象--所需工作仅仅是创建一个新类的对象(即原型)，然后再任何需要的地方Clone。
3. Prototype模式中Clone 方法可以利用某些框架中的序列化来实现**深拷贝**。

## Builder 构建器
**动机**
在软件系统中，有时候面临着“一个复杂对象”的创建工作，其通常由各个部分的子对象用一定的算法构成；由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是将它们组合在一起的算法却相对稳定。

如何应对这种变化？如何提供一种“封装机制”来隔离出“复杂对象的各个部分”的变化，从而保持系统中的“稳定构建算法”不随着需求改变而改变？

**模式定义**
将一个复杂对象的构建与其表示相分离，使得同样的构建过程(稳定)可以创建不同的表示(变化)。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/builder.png)

**代码实现**

实现游戏建房子程序，不同房子窗子，门不一样等， 构建流程稳定，与模板方法模式类似。代码实现如下：
```cpp
class House{
    //.... 
};

class HouseBuilder {
public:
    House* GetResult(){
        return pHouse;
    }
    virtual ~HouseBuilder(){}
protected: 
    House* pHouse;
	virtual void BuildPart1()=0;
    virtual void BuildPart2()=0;
    virtual void BuildPart3()=0;
    virtual void BuildPart4()=0;
    virtual void BuildPart5()=0;	
};

class StoneHouse: public House{  
};

class StoneHouseBuilder: public HouseBuilder{
protected: 
    virtual void BuildPart1(){
        //pHouse->Part1 = ...;
    }
    virtual void BuildPart2(){   
    }
    virtual void BuildPart3(){  
    }
    virtual void BuildPart4(){
    }
    virtual void BuildPart5(){  
    }
};

class HouseDirector{
public:
    HouseBuilder* pHouseBuilder;
    HouseDirector(HouseBuilder* pHouseBuilder){
        this->pHouseBuilder=pHouseBuilder;
    }
    House* Construct(){
        pHouseBuilder->BuildPart1();
        for (int i = 0; i < 4; i++){
            pHouseBuilder->BuildPart2();
        }
        bool flag=pHouseBuilder->BuildPart3();
        if(flag){
            pHouseBuilder->BuildPart4();
        }
        pHouseBuilder->BuildPart5();
        return pHouseBuilder->GetResult();
    }
};
```

**要点总结**
1. Builder 模式主要用于“分步骤构建一个复杂的对象”。在这其中“分步骤”是一个稳定的算法，而复杂对象的各个部分则经常变化。
2. 变化点在哪里，封装哪里—— Builder模式主要在于应对“复杂对象各个部分”的频繁需求变动。其缺点在于难以应对“分步骤构建算法”的需求变动。
3. 在Builder模式中，要注意不同语言中构造器内调用虚函数的差别（C++ vs. C#) 。


## Singleton 单件模式

**动机**
在软件系统中， 经常有这样一些特殊的类，必须保证他们在系统中只存在一个实例，才能确保他们的逻辑正确性，以及良好的效率。

如何绕过常规的构造器，提供一种机制来保证一个类只有一个实例？

**模式定义**
保证一个类**仅有一个实例，并提供一个该实例的全局访问点**。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/singleton.jpg)

**代码实现**
Singleton 模式代码实现如下：

```cpp
class Singleton{
private:
    Singleton();   // 私有构造函数
    Singleton(const Singleton& other);
public:
    static Singleton* getInstance();
    static Singleton* m_instance;
};

Singleton* Singleton::m_instance=nullptr;

//线程非安全版本
Singleton* Singleton::getInstance() {  // 永远返回最初创建地对象
    if (m_instance == nullptr) {
        m_instance = new Singleton();
    }
    return m_instance;
}

//全局唯一，线程安全版本，但锁的代价过高（高并发场景)
Singleton* Singleton::getInstance() {
    Lock lock; // 该方法全局加锁
    if (m_instance == nullptr) {
        m_instance = new Singleton();
    }
    return m_instance;
}

//双检查锁，但由于内存读写reorder不安全(出现概率高)
/*
reorder: 指令序列到汇编层次和假设不一样，先分配内存，把地址给m_instance， 再执行构造器。正常就该是先分配内存, 再执行构造器， 最后再地址赋值。
         这导致某一个线程只获取了内存地址，但没执行构造器，欺骗了双检查锁，造成创建无效对象问题
*/
Singleton* Singleton::getInstance() { 
    if(m_instance==nullptr){
        Lock lock; // 在空的时候才加锁， 减少代价
        if (m_instance == nullptr) { // m_instance 仍然空的，才创建对象，必要部分，保证逻辑正确
            m_instance = new Singleton();
        }
    }
    return m_instance;
}

//C++ 11版本之后的跨平台实现 (volatile)
std::atomic<Singleton*> Singleton::m_instance;
std::mutex Singleton::m_mutex;
Singleton* Singleton::getInstance() {
    Singleton* tmp = m_instance.load(std::memory_order_relaxed); // 屏蔽编译器 reorder
    std::atomic_thread_fence(std::memory_order_acquire);//获取内存fence
    if (tmp == nullptr) {
        std::lock_guard<std::mutex> lock(m_mutex);
        tmp = m_instance.load(std::memory_order_relaxed);
        if (tmp == nullptr) {
            tmp = new Singleton;
            std::atomic_thread_fence(std::memory_order_release);//释放内存fence
            m_instance.store(tmp, std::memory_order_relaxed);
        }
    }
    return tmp;
}
```

**要点总结**
1. Singleton模式中的实例构造器可以设置为protected以允许子类派生。
2. Singleton模式一般**不要支持拷贝构造函数和Clone接口**，因为这有可能导致多个对象实例，与Singleton模式的初衷违背。
3. 如何实现多线程环境下安全的Singleton? 注意双检查锁的正确实现。

## Flyweight 享元模式

**动机**
在软件系统采用纯粹对象方案的问题在于大量细粒度的对象会很快充斥在系统中，从而带来很高的运行时代价--只要指内存需求方面的评价。对象数量太多严重影响性能，使得开销比较大

如何在避免大量细粒度对象问题的同时，让外部客户程序仍然能够透明地使用面向对象的方式来进行操作？

**模式定义**
运用**共享技术**有效地支持大量细粒度的对象。比如线程开销很大，使用线程池。使用对象池，已有的可重复使用的对象可以不用创建。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/flyweight.png)

**代码实现**
实现系统字体管理程序。Flyweight 模式代码实现如下：
```cpp
class Font {
private:
    //unique object key
    string key;
    //object state
    //....
public:
    Font(const string& key){
        //...
    }
};

class FontFactory{
private:
    map<string,Font* > fontPool;
public:
    Font* GetFont(const string& key){
        map<string,Font*>::iterator item=fontPool.find(key);     
        if(item!=footPool.end()){
            return fontPool[key];
        }
        else{
            Font* font = new Font(key); // 为字体对象返回字体，而不是返回所有字体
            fontPool[key]= font;
            return font;
        }
    }
 
    void clear(){
        //...
    }
};
```

**要点总结**
1. 面向对象解决很好地解决了抽象性的问题，但是作为一个运行在机器中的程序实体，我们需要考虑对象的代价问题。Flyweight 主要解决面向对象的代价问题，一般不触及面向对象的抽象性问题。
2. Flyweight 采用**对象共享的做法来降低系统中对象的个数，从而降低细粒度对象给系统带来的内存压力**。在具体实现方面，要注意对象状态的处理。共享减少数量，共享的尽量是**只读的**
3. 对象的数量太大从而导致对象内存开销加大--什么样的数量才算大？10w个对象大概5M, Flyweight内部实现也需要开销，需要根据具体情况评估。


## Facade 门面模式
稳定的接口来隔离，使得外部的变化不会受内部变化的影响
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/facade.png)

**动机**
上述A方案的问题在于组件的客户和组件中各种复杂的子系统有了过多的耦合，随着外部客户程序和各子系统的演化,这种过多的耦合面临很多变化的挑战。

如何简化外部客户程序和系统间的交互接口？如何将外部客户程序的演化和内部子系统的变化之间的依赖相互解耦？

**模式定义**
为子系统中的一组接口提供一个**一致(稳定)的界面**， Facade 模式定义了一个**高层接口**，这个接口使得这一子系统更加容易使用(**复用**)，隔离了子系统内部的变化，将内部子系统和外部客户端解耦。

**要点总结**
1. 从客户程序的角度， Facade 模式简化了整个组件系统的接口，对于组件内部与外部客户程序来说, 达到一种“**解耦**”的效果--**内部子系统的变化不会影响到Facade接口的变化**。
2. Facade设计模式更注重从架构的底层看整个系统，而不是单个类的层次。Facade很多时候更是一种**架构设计模式**。
3. Facade 设计模式并非一个集装箱，可以任意地放进任何多个对象。Facade 模式中组件的内部应该是“**相互耦合关系比较大的一系列组件**”，而不是一个简单的功能集合。

## Proxy 代理模式

**动机**
在面向对象系统中，有些对象由于某种原因(**比如创建对象的开销很大，或者某些操作需要安全控制，或者需要进程外的访问等**)，直接访问会给使用者、或者系统结构带来很多麻烦。

如何在不失去透明操作（一致）对象的同时来管理/控制这些对象特有的复杂性？增加一层**间接层**是软件开发中常见的解决方式。

**模式定义**
为其他对象提供一种**代理以控制**(**隔离**，使用接口)对这个对象的访问

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/proxy.jpg)

Proxy结构看起来简单，用起来不简单。一般情况下，Subject 对象可以直接调用 RealSubject，但因为某些不为外部所知原因，需要使用代理访问，Subject 通过 Proxy 间接访问 RealSubject。

**代码实现**
Proxy 模式代码实现如下：
```cpp
class ISubject{
public:
    virtual void process();
};

//Proxy的设计--> 常常很复杂
class SubjectProxy: public ISubject{   
public:
    virtual void process(){
        //对RealSubject的一种间接访问
        //....
    }
};

class ClientApp{ 
    ISubject* subject;
public:  
    ClientApp(){
        subject=new SubjectProxy();
    } 
    void DoTask(){
        //...
        subject->process();    
        //....
    }
};
```

**要点总结**
1. “**增加一层间接层**”是**软件系统中对许多复杂问题的一种常见解决方法**。在面向对象系统中，直接使用某些对象会带来很多问题，作为间接层的 Proxy 对象便是解决这一问题的常用手段。
2. 具体 **Proxy 设计模式的实现方法，实现力度都相差很大**，有些可能对单个对象做细粒度的控制，如 Copy-on-write 技术，有时可能对组件模块提供抽象代理层，在架构层次对对象做Proxy。
3. Proxy 并**不一定要求保持接口完整的一致性**，只要能够实现间接控制，有时候损及一些透明性是可以接受的。

## Adapter 适配器模式

**动机**
在软件系统中，由于应用环境的变化, 常常需要将“一些现存的对象”放在新的环境中应用，但是新环境要求的接口是这些现存对象所不满足的。

如何应对这种“迁移的变化” ？如何既能利用现有对象的良好实现，同时又能满足新的应用环境所要求的接口？

**模式定义**
将**一个类的接口转换成客户希望的另一个接口**。Adapter 模式使得原本由于接口不兼容而不能在一起工作的哪些类可以一起工作。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/adapter2.jpg)
Adaptee 被适用接口， Target 是目标， Adapter 继承 Target 实现 Adaptee 到 Target 的转换。具体 Adapter 实现可能很复杂。

**代码实现**
Adapter 模式代码实现如下：
```cpp
//目标接口（新接口）
class ITarget{
public:
    virtual void process()=0;
};
//遗留接口（老接口）
class IAdaptee{
public:
    virtual void foo(int data)=0;
    virtual int bar()=0;
};

//遗留类型
class OldClass: public IAdaptee{
    //....
};

//对象适配器
class Adapter: public ITarget{ //对象组合方式（松耦合），推荐
protected:
    IAdaptee* pAdaptee;//组合  
public: 
    Adapter(IAdaptee* pAdaptee){
        this->pAdaptee=pAdaptee;
    }   
    virtual void process(){
        int data=pAdaptee->bar();
        pAdaptee->foo(data);   
    }
};

//类适配器
class Adapter: public ITarget, // 多继承方式  ，不推荐
               protected OldClass{ //多继承             
}
int main(){
    IAdaptee* pAdaptee=new OldClass(); 
    ITarget* pTarget=new Adapter(pAdaptee);
    pTarget->process();  
}
```

**要点总结**
1. Adapter 模式主要应用于“希望复用一些现存的类，但是接口又与复用环境要求不一致的情况”，在**遗留代码复用、类库迁移**等方面非常有用。
2. GOF23定义了两种 Adapter 模式的实现结构：对象适配器和类适配器。但类适配器采用“多继承”的实现方式，一般不推荐使用。**对象适配器**采用“对象组合”的方式，更符合松耦合的精神。
3.  Adapter 模式可以实现非常灵活，不必拘泥于GOF23中定义的两种结构。例如，完全可以将 Adapter 模式中的“现存现象”作为新的接口方法参数，来达到适配器目的。

## Mediator 中介者模式

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/Mediator1.png)

**动机**
在软件构建过程中，经常会出现多个对象互相关联交互的情况，对象之间常常会维持一种复杂的引用关系,如果遇到一些需求的更改，这种直接的引用关系将面临不断的变化。

在这种情况下，我们可使用一个“中介对象”来管理对象间的关联关系，避免相互交互的对象之间的紧耦合引用关系，从而更好地抵御变化。交换机机制很像 Mediator 。

**模式定义**
用一个中介对象来封装(**封装变化**)一些列的对象交互。中介者**使各对象不需要显式的相互引用**(编译时依赖->运行时依赖)，从而使整合松散（管理变化），而且可以独立地改变它们之间的交互。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/mediator2.png)
Colleague 指针指向 Mediator 。ConcreteMediator 指针指向ConcreteMediator1，ConcreteMediator2。ConcreteMediator1和ConcreteMediator2没有关联。达到双向依赖目的。

**要点总结**
1. 将多个对象间复杂的关联关系解耦，**Mediator模式将多个对象间的控制逻辑进行集中管理，变“多个对象互相关联”为“多个对象和一个中介者关联”**，简化了系统的维护，抵御了可能的变化。
2. 随着控制逻辑的复杂化，Mediator具体对象的实现可能相当复杂。 这时候可以对Mediator对象进行分解处理。
3. **Facade 解耦系统内部和系统外部(单向)的对象关联关系；Mediator 解耦系统内部各对象之间(双向)的关联关系**。

## State 状态模式 

**动机**
在软件构建过程中，某些**对象的状态如果改变，其行为也会随之而发生变化**，比如文档处于只读状态，其支持的行为和读写状态支持的行为就可能完全不同。

如何在运行时根据对象的状态来透明地更改对象的行为？而不会 为对象操作和状态转化之间引入紧耦合？

**模式定义**
允许一个**对象在其内部状态改变时改变它的行为。从而使对象看起来似乎修改了其行为**。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/state.png)

> State 模式和策略模式很像，不需要在意模式的差异性

**代码实现**
实现网络状态应用程序。State 模式代码实现如下：
```cpp
class NetworkState{
public:
    NetworkState* pNext;
    virtual void Operation1()=0;
    virtual void Operation2()=0;
    virtual void Operation3()=0;

    virtual ~NetworkState(){}
};

class OpenState :public NetworkState{
    static NetworkState* m_instance;
public:
    static NetworkState* getInstance(){
        if (m_instance == nullptr) {
            m_instance = new OpenState();
        }
        return m_instance;
    }
    void Operation1(){
        //**********
        pNext = CloseState::getInstance();
    }
    void Operation2(){
        //..........
        pNext = ConnectState::getInstance();
    }
    void Operation3(){  
        //$$$$$$$$$$
        pNext = OpenState::getInstance();
    }
};

class CloseState:public NetworkState{ }
//...

class NetworkProcessor{ // 稳定部分
    NetworkState* pState; 
public:
    NetworkProcessor(NetworkState* pState){    
        this->pState = pState;
    }
    void Operation1(){
        //...
        pState->Operation1();
        pState = pState->pNext;
        //...
    }
    void Operation2(){
        //...
        pState->Operation2();
        pState = pState->pNext;
        //...
    }
    void Operation3(){
        //...
        pState->Operation3();
        pState = pState->pNext;
        //...
    }
};
```

**要点总结**
1. State模式**将所有与一个特定状态相关的行为都放入一个State的子类对象**中，在对象状态切换时，切换相应的对象；但同时维持 State 的接口，这详实现了具体操作与状态转换之间的解耦。
2. 为不同的状态引入不同的对象使得状态转换得更加明确，而且可以保证不会出现状态不一致的情况, 因为转换是原子性的—— 即要么彻底转换过来,要么不转换。
3. 如果 State 对象没有实例变量，那么各上下文可以共享一个State 对象，从而节省对象开销

## Memento 备忘录模式

**动机**
在软件构建过程中，某些对象的状态在转换过程中，可能由于某种需要，要求**程序能够回溯到对象之前处于某个点时的状态(状态快照)**。如果使用一些公有接口来让其他对象得到对象的状态，便会暴露对象的细节实现。

如何实现对象状态的良好保存与恢复？但同时又不会因此而破坏对象本身的封装性。

**模式定义**
在不破坏封装性的前提下, **捕获一个对象的内部状态，并在该对象之外保存这个状态**。这样以后就可以将该对象恢复到原先保存的状态。
> 相对落后的设计模式

**结构（Structure）**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/memento.png)


**代码实现**
实现网络状态应用程序。State 模式代码实现如下：
```cpp
class Memento // 只要快照部分状态，而不是全部状态
{
    string state;
    //..
public:
    Memento(const string & s) : state(s) {}
    string getState() const { return state; }
    void setState(const string & s) { state = s; }
};

class Originator
{
    string state;
    //....
public:
    Originator() {}
    Memento createMomento() {
        Memento m(state);  // 不同平台实现可能相当复杂
        return m;
    }
    void setMomento(const Memento & m) {
        state = m.getState();
    }
};

int main()
{
    Originator orginator;
    //捕获对象状态，存储到备忘录，不破坏原对象的封装性
    Memento mem = orginator.createMomento(); // 深拷贝，但针对复杂情况不太好实现
    //... 改变orginator状态
    //从备忘录中恢复
    orginator.setMomento(mem);
}
```

**要点总结**
1. 备忘录(Memento) 存储原发器(Originator) 对象的内部状态，在需要时恢复原发器状态
2. Memento 模式的核心是**信息隐藏**，即 Originator 需要向外隐藏信息，保持其封装性。但同时又需要将状态保持到外界(Memento)
3. 由于现代语言运行时（如C#, JAVA等）都具有相当的对象序列化支持，因此往往采用效率更高、又容易正确实现的序列化方案(而不是深拷贝方式)来实现 Memento 模式。

## Composite 组合模式

**动机**
在软件在某些情况下，客户代码过多地依赖于对象容器复杂的内部实现结构,对象容器内部实现结构(而非抽象接口)的变化将引起客户代码的频繁变化，带来了代码的维护性、扩展性等弊端。

如何将“客户代码与复杂的对象容器结构”解耦？让对象容器自己来实现自身的复杂结构，从而使得客户代码就像处理简单对象样来处理复杂的对象容器？

**模式定义**
将对象组合成树形结构以表示“部分-整体”的层次结构。 **Composite使得用户对单个对象和组合对象的使用具有一致性（稳定）**。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/composite.png)

**代码实现**
Composite 模式代码实现如下：
```cpp
#include <iostream>
#include <list>
#include <string>
#include <algorithm>

using namespace std;

class Component
{
public:
    virtual void process() = 0;
    virtual ~Component(){}
};

//树节点
class Composite : public Component{
    
    string name;
    list<Component*> elements;  // 子节点
public:
    Composite(const string & s) : name(s) {}
    
    void add(Component* element) {
        elements.push_back(element); // 可能是树节点，也可能是叶节点
    }
    void remove(Component* element){
        elements.remove(element);
    }
  
    void process(){
        //1. process current node
        //2. process leaf nodes
        for (auto &e : elements) 
            /*
                e 是 Composite*； 调用树节点的process
                e 也可以是 Leaf*  调用叶子节点的process
            */
            e->process(); //多态调用   
    }
};

//叶子节点
class Leaf : public Component{
    string name;
public:
    Leaf(string s) : name(s) {}
            
    void process(){
        //process current node
    }
};

void Invoke(Component & c){
    //...
    c.process();
    //...
}

int main()
{
    Composite root("root");
    Composite treeNode1("treeNode1");
    Composite treeNode2("treeNode2");
    Composite treeNode3("treeNode3");
    Composite treeNode4("treeNode4");
    Leaf leat1("left1");
    Leaf leat2("left2");
    
    root.add(&treeNode1);
    treeNode1.add(&treeNode2);
    treeNode2.add(&leaf1);
    
    root.add(&treeNode3);
    treeNode3.add(&treeNode4);
    treeNode4.add(&leaf2);
    
    process(root);  // 具有一致性
    process(leaf2);
    process(treeNode3);
}
```

**要点总结**
1. Composite 模式采用树形结构来实现普遍存在的对象容器，从而**将“一对多”的关系转化为“一对一”关系，使得客户代码可以一致地(复用)处理对象和对象容器，无需关心处理的是单个对象，还是组合的对象容器**。
2. 将“客户代码与复杂代的对象容器结构”解耦是 Composite 的核心思想，解耦之后，客户代码将与纯粹的抽象接口--而非对象容器的内部实现结构--发生依赖，从而更能“应对变化”
3. Composite 模式在具体实现中，可以让父对象中的子对象反向追溯；如果父对象有频繁的遍历需求，可以使用缓存技巧来改善效率

## Iterator 迭代器模式

**动机**
在软件构建过程中，集合对象内部结构常常变化各异。但对于这些集合对象，我们希望在不暴露其内部结构的同时,可以让外部客户代码透明地访问其中包含的元素；同时这种“透明遍历”也为 “同一种算法在多种集合对象上进行操作”提供了可能。

使用面向对象技术**将这种遍历机制抽象为“迭代器对象”**为“应对变化中的集合对象”提供了一种优雅的方式。

**模式定义**
提供**一种方法顺序访问一个聚合对象中的各个元素，而又不暴露（稳定）该对象的内部表示**。
> 隔离算法和容器

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/iterator.png)

**代码实现**
面向对象定义的 Iterator 模式代码实现如下：
```cpp
// 面向对象定义的迭代器--> 过时(面向对象虚函数的缺陷，运行时多态)--> STL 泛型编程迭代器使用模板（编译时多态）；运行时多态性能(虚函数表指针的寻址)不如编译时多态
template<typename T>
class Iterator
{
public:
    virtual void first() = 0;
    virtual void next() = 0;
    virtual bool isDone() const = 0;
    virtual T& current() = 0;
};

template<typename T>
class MyCollection{
public:
    Iterator<T> GetIterator(){
        //...
    } 
};

template<typename T>
class CollectionIterator : public Iterator<T>{
    MyCollection<T> mc;
public:
    CollectionIterator(const MyCollection<T> & c): mc(c){ }
    void first() override { 
    }
    void next() override {
    }
    bool isDone() const override{  
    }
    T& current() override{ 
    }
};

void MyAlgorithm()
{
    MyCollection<int> mc;
    Iterator<int> iter= mc.GetIterator();
    for (iter.first(); !iter.isDone(); iter.next()){
        cout << iter.current() << endl;
    }
}
```
面向对象迭代器到目前为止已经过时，因为面向对象虚函数的缺陷，即它是运行时多态，存在虚函数表指针寻址开销；目前广泛采用 STL 泛型编程迭代器，它使用模板，是编译时多态；
> **运行时多态性能(虚函数表指针的寻址)不如编译时多态**

**要点总结**
1. 迭代抽象：**访问一个聚合对象的内容而无需暴露它的内部表示**。
2. 迭代多态：为遍历不同的集合结构提供一个统一的接口，从而**支持同样的算法在不同的集合结构上进行操作**。
3. 迭代器的健壮性考虑：遍历的同时更改迭代器所在的集合结构，会导致问题。

## Chain of Resposibility 职责链模式

**动机**
在软件构建过程中，<u>一个请求可能被多个对象处理，但是每个请求在运行时只能有一个接受者</u>，如果显式指定，将必不可少地带来 请求发送者与接受者的紧耦合。

如何使请求的发送者<u>不需要指定具体的接受者</u>？让请求的接受者 自己在运行时决定来处理请求，从而使两者解耦。

**模式定义**
使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将**这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止**。
>  在C++中使用，偏过时，在其他语言中广泛应用

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/chain_of_resposibility.png)

**代码实现**
Chain of Resposibility 模式代码实现如下：
```cpp
#include <iostream>
#include <string>
using namespace std;
enum class RequestType
{
    REQ_HANDLER1,
    REQ_HANDLER2,
    REQ_HANDLER3
};

class Reqest
{
    string description;
    RequestType reqType;
public:
    Reqest(const string & desc, RequestType type) : description(desc), reqType(type) {}
    RequestType getReqType() const { return reqType; }
    const string& getDescription() const { return description; }
};

class ChainHandler{
    ChainHandler *nextChain;
    void sendReqestToNextHandler(const Reqest & req)
    {
        if (nextChain != nullptr)
            nextChain->handle(req);
    }
protected:
    virtual bool canHandleRequest(const Reqest & req) = 0;
    virtual void processRequest(const Reqest & req) = 0;
public:
    ChainHandler() { nextChain = nullptr; }
    void setNextChain(ChainHandler *next) { nextChain = next; }
    void handle(const Reqest & req)
    {
        if (canHandleRequest(req))
            processRequest(req);
        else
            sendReqestToNextHandler(req); // 当前不能处理，交给下一个节点
    }
};

class Handler1 : public ChainHandler{
protected:
    bool canHandleRequest(const Reqest & req) override
    {
        return req.getReqType() == RequestType::REQ_HANDLER1;
    }
    void processRequest(const Reqest & req) override
    {
        cout << "Handler1 is handle reqest: " << req.getDescription() << endl;
    }
}; 
class Handler2 : public ChainHandler{
protected:
    bool canHandleRequest(const Reqest & req) override
    {
        return req.getReqType() == RequestType::REQ_HANDLER2;
    }
    void processRequest(const Reqest & req) override
    {
        cout << "Handler2 is handle reqest: " << req.getDescription() << endl;
    }
};
class Handler3 : public ChainHandler{
protected:
    bool canHandleRequest(const Reqest & req) override
    {
        return req.getReqType() == RequestType::REQ_HANDLER3;
    }
    void processRequest(const Reqest & req) override
    {
        cout << "Handler3 is handle reqest: " << req.getDescription() << endl;
    }
};

int main(){
    Handler1 h1;
    Handler2 h2;
    Handler3 h3;
    h1.setNextChain(&h2); //建立链表
    h2.setNextChain(&h3);
    Reqest req("process task ... ", RequestType::REQ_HANDLER3); // 决定将来哪个链表节点处理
    h1.handle(req);
    return 0;
}
```

**要点总结**
1. Chain of Resposibility 模式的应用场景在于“一个请求有可能有多个接收者，但是最后真正的接收者只有一个”，这时候请求发送者与参与者的耦合有可能出现“变化脆弱”的症状，职责链的目的就是将二者解耦，从而更好地应对变化。
2. 应用了 Chain of Resposibility 模式后，对象的职责分工将更具灵活性。我们可以运行时动态添加/修改请求的处理职责。
3. 如果请求传递到职责链的末尾仍得不到处理，应该有一个合理的缺省机制。这也是每个接收对象的责任，而不是发出请求的对象的责任。

## Command 命令模式

**动机**
在软件构建过程中，“行为请求者”与“行为实现者”通常呈现一种“紧耦合”。但在某些场合——比如需要对行为进行“记录、 撤销/重(undo/redo)、事务”等处理，这种无法抵御变化的紧耦合是不合适的。

在这种情况下,如何将“行为请求者”与“行为实现者”解耦？ 将一组行为抽象为对象，可以实现二者之间的松耦合。

**模式定义**
**将一个请求(行为)封装为一个对象**，从而使得你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤销的操作。

> 在C++中使用，偏过时，在其他语言中广泛应用；C++使用函数对象

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/command.png)

**代码实现**
Command 模式代码实现如下：
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Command // 抽象对象
{
public:
    virtual void execute() = 0;
};

class ConcreteCommand1 : public Command // 行为对象
{
    string arg;
public:
    ConcreteCommand1(const string & a) : arg(a) {}
    void execute() override
    {
        cout<< "#1 process..."<<arg<<endl;
    }
};

class ConcreteCommand2 : public Command
{
    string arg;
public:
    ConcreteCommand2(const string & a) : arg(a) {}
    void execute() override
    {
        cout<< "#2 process..."<<arg<<endl;
    }
};
         
class MacroCommand : public Command
{
    vector<Command*> commands;
public:
    void addCommand(Command *c) { commands.push_back(c); }
    void execute() override
    {
        for (auto &c : commands)
        {
            c->execute();  // 使用组合设计模式
        }
    }
};
    
int main()
{
    ConcreteCommand1 command1(receiver, "Arg ###");
    ConcreteCommand2 command2(receiver, "Arg $$$");
    
    MacroCommand macro;  // 对象，表征的是行为
    macro.addCommand(&command1);
    macro.addCommand(&command2);
    
    macro.execute();

}
```

**要点总结**
1. Command模式的根本目的在于将“行为请求者”与“行为实现者”解耦, 在面向对象语言中，常见的实现手段是“**将行为抽象为对象**”。
2. 实现Command接口的具体命令对象ConcreteCommand有时候根据需要可能会保存一些额外的状态信息。通过使用Composite模式，可以将多个“命令”封装为一个“复合命令” MacroCommando
3. Command模式与C++中的函数对象有些类似。
    - <u>Command 模式使用虚函数是运行时多态，动态绑定，函数对象与模板泛型编程结合是编译时多态，静态绑定</u>。
    - 但两者定义行为接口的规范有所区别：Command以面向对象中的“接口-实现”来定义行为接口规范，<u>更严格，但有性能损失</u>；C++函数对象以函数签名来定义行为接口规范，<u>更灵活，性能更高</u>。
    - 使得C++函数对象(仿函数)应用更广泛，但 Command 模式在其他语言中应用广泛


## Visitor 访问器模式

**动机**
在软件构建过程中，由于需求的改变，某些类层次结构中常常需 要增加新的行为(方法)，如果直接在基类中做这样的更改，将会 给子类带来很繁重的变更负担，甚至破坏原有设计。

如何在不更改类层次结构的前提下，在运行时根据需要透明地为 类层次结构上的各个类**动态添加新的操作**，从而避免上述问题？

**模式定义**
表示一个作用于某对象结构中的各元素的操作。使得<u>可以在不改变（稳定）各元素的类的前提下定义（扩展）作用于这些元素的**新操作（变化）**</u>。


**结构（Structure）**

![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/visitor.png)

> **Element 的所有子类都确定是非常大的缺点，也是使用 Visitor 模式的前提**

**代码实现**
Visitor 模式代码实现如下：
```cpp
#include <iostream>

using namespace std;
class Visitor;
class Element
{
public:
    virtual void accept(Visitor& visitor) = 0; //第一次多态辨析, 预先设计将来可能增加的操作

    virtual ~Element(){}
};

class ElementA : public Element
{
public:
    void accept(Visitor &visitor) override {
        visitor.visitElementA(*this);
    }
};
class ElementB : public Element
{
public:
    void accept(Visitor &visitor) override {
        visitor.visitElementB(*this); //第二次多态辨析
    }
};

class Visitor{
public:
    virtual void visitElementA(ElementA& element) = 0;
    virtual void visitElementB(ElementB& element) = 0;
    virtual ~Visitor(){}
};

//================================== 将来
//扩展1
class Visitor1 : public Visitor{
public:
    void visitElementA(ElementA& element) override{
        cout << "Visitor1 is processing ElementA" << endl;
    }
     
    void visitElementB(ElementB& element) override{
        cout << "Visitor1 is processing ElementB" << endl;
    }
};
     
//扩展2
class Visitor2 : public Visitor{
public:
    void visitElementA(ElementA& element) override{
        cout << "Visitor2 is processing ElementA" << endl;
    }
    void visitElementB(ElementB& element) override{
        cout << "Visitor2 is processing ElementB" << endl;
    }
};
   
int main()
{
    Visitor2 visitor;
    ElementB elementB;
    elementB.accept(visitor);// double dispatch
    
    ElementA elementA;
    elementA.accept(visitor);

    return 0;
}
```

**要点总结**
1. Visitor 模式通过所谓的双重分发(double dispatch) 来实现不更改(不添加新操作-编译时) Element 类层次结构的前提下，在运行时透明地为类层次结构上的各类动态添加新的操作(支持变化)
2. 所谓双重分发即 Visitor 模式中间包括两个多态分发(注意其中的多态机制)：第一个为 visitor 方法(accept)的多态辨析；第二个为 visitElementX 方法的多态辨析
3. Visitor 模式的最大缺点在于扩展类层次结构(增添新的 Element 子类)，会导致 Visitor 类的改变。因此 Visitor 模式适用于 “**Element 类层次结构稳定，而其中的操作却经常面临频繁改动**”

## Interpreter 解析器模式

**动机**
在软件构建过程中，如果某一特定领域的问题比较复杂，类似的 结构不断重复出现，如果使用普通的编程方式来实现将面临非常频繁的变化。

在这种情况下，将特定领域的问题表达为某种语法规则下的句子，然后构建一个解释器来解释这样的句子，从而达到解决问题的目的。

**模式定义**
给定一个语言，<u>定义它的文法的一种表示，并定义一种解释器 ，这个解释器使用该表示来解释语言中的句子</u>。

**结构（Structure）**
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/design_pattern/interpreter.webp)

**代码实现**
实现加减运算程序。Interpreter 模式代码实现如下：
```cpp
#include <iostream>
#include <map>
#include <stack>

using namespace std;

class Expression {
public:
    virtual int interpreter(map<char, int> var)=0;
    virtual ~Expression(){}
};

//变量表达式
class VarExpression: public Expression {
    char key;
public:
    VarExpression(const char& key)
    {
        this->key = key;
    }
    
    int interpreter(map<char, int> var) override {
        return var[key];
    }
    
};

//符号表达式
class SymbolExpression : public Expression { 
    // 运算符左右两个参数
protected:
    Expression* left;
    Expression* right;
    
public:
    SymbolExpression( Expression* left,  Expression* right):
        left(left),right(right){    
    }
};

//加法运算
class AddExpression : public SymbolExpression { 
public:
    AddExpression(Expression* left, Expression* right):
        SymbolExpression(left,right){   
    }
    int interpreter(map<char, int> var) override {
        return left->interpreter(var) + right->interpreter(var);
    } 
};

//减法运算
class SubExpression : public SymbolExpression {  
public:
    SubExpression(Expression* left, Expression* right):
        SymbolExpression(left,right){  
    }
    int interpreter(map<char, int> var) override {
        return left->interpreter(var) - right->interpreter(var);
    }
};

Expression*  analyse(string expStr) {
    stack<Expression*> expStack;
    Expression* left = nullptr;
    Expression* right = nullptr;
    for(int i=0; i<expStr.size(); i++)
    {
        switch(expStr[i])
        {
            case '+':
                // 加法运算
                left = expStack.top();
                right = new VarExpression(expStr[++i]);
                expStack.push(new AddExpression(left, right));
                break;
            case '-':
                // 减法运算
                left = expStack.top();
                right = new VarExpression(expStr[++i]);
                expStack.push(new SubExpression(left, right));
                break;
            default:
                // 变量表达式
                expStack.push(new VarExpression(expStr[i]));
        }
    }
    Expression* expression = expStack.top();
    return expression;
}

void release(Expression* expression){
    //释放表达式树的节点内存...
}

int main(int argc, const char * argv[]) {
    string expStr = "a+b-c+d-e";
    map<char, int> var;
    var.insert(make_pair('a',5));
    var.insert(make_pair('b',2));
    var.insert(make_pair('c',1));
    var.insert(make_pair('d',6));
    var.insert(make_pair('e',10));

    Expression* expression= analyse(expStr);
    int result=expression->interpreter(var);  // 最后一个虚函数递归调用
    cout<<result<<endl;
    release(expression);
    
    return 0;
}
```

**要点总结**
1. Interpreter 模式的应用场景是 Interpreter 模式应用中的难点，只有满足“**业务规则频繁变化，且类似的结构不断重复出现, 并且容易抽象成语法规则的问题**”，才适合使用Interpreter 模式
2. 使用 Interpreter 模式来表示文法规则，从而可以使用面向对象技巧**方便地“扩展”文法**
3. Interpreter 模式比较适合简单的文法表示，对于复杂的文法表示，Interpreter 模式会产生比较大的类层次结构，需要求助于语法分析器这样的标准工具(复杂的文法表示涉及复杂的虚函数递归调用，影响性能)。不常用。

## 总结

一个目标：管理变化，提高复用

两种手段：分解 & 抽象

不常用设计模式：
- Builder
- Mediator
- Memento
- Iterator
- Chain of Resposibility
- Command
- Visitor
- Interpreter

C++ 内存模型尽量少使用继承，多用组合(组合对象指针，指针指向多态对象，增加灵活性)。组合对象和继承相差不大。

看出模式的稳定部分和不稳定部分，将变化点和稳定点隔离，审视依赖关系。区分抽象(稳定)和具体(变化)，多写抽象类接口。

什么时候不使用设计模式：
- 代码可读性差
- 需求理解还很浅显
- 变化没有显现时
- 不是系统的关键依赖点
- 项目没有复用价值
- 项目将要发布时





