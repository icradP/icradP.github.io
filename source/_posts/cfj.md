---
title: C++复健(未整理)
tags: [C++]
date: 2022/7/24
categories: C++
comments: false
---

C++复健（侯捷）



overloading (重载)

重载的条件：

同名

同作用域

不同的返回值

或者不同的参数（形参）



同名：其实在编译器来看是不同名的

作用域：一个类内，一个全局域内，一个函数域内





当构造函数放在私有中：

设计模式：Singleton

外界不能创建这个类

但是我们可以通过这个类提供的外界接口。

示例：

```c++
class A{
  public:
    static A& getInstance(); //关键点在 static
    setup(){...}
  private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance()
{
    static A a;
    return a;
}

void test()
{
    A::getInstance().setup();//调用方式
}
```







成员函数,内联（inline）定义，调用是最快的 

但内联是编译器决定的，写的定义过于复杂就不能一定是内联函数 

我们在类外定义函数实现的时候：需要inline在最外进行修饰，才有可能修饰成内联函数



构造函数：初始化值最好使用初始化列表的方式



常量成员函数

修饰符：const

要确定，是否要更改其中对象中数据，
如果要不需要更改 ，那必须需要修饰成员函数 添加const修饰

因为这是正规的正式合法的修饰，不加说明你是学习C++的杂牌军

const的意义

在使用该类的途中如果使用了const修饰，你却没有给这个函数修饰，编译器会报错



所有的成员函数都隐藏着一个参数 this; 这个this是调用这个成员函数的对象的指针

示例：

运算符的重写

```c++
//表面写法
operator ()(const int &a)

//实际上
operator ()(this,const int &a)
    
```









引用是可以理解为一个指针常量

可以改内容不可以改名字

我们可以用const修饰引用，既不可以该指针地址，也不可以改值

传指针才多大，当你要传的东西，大于指针地址的大小，为了速度呢前提是你不更改你传的东西，可以使用常量引用作为传值，若果要修改，就不要加常量修饰

注意了，返回引用本质是上返回指针，别啥玩意都返回引用，返回个野指针空指针啥的都是非法操作



**传递者无许知道接受者是以引用（reference)的形式接受**









标准库不是圣经,~~圣经还有错的~~呢，不是完美无缺的





构造函数的特殊语法初始化列表



**相同class的各个objects互为友元**



**一个好的C++代码习惯：**

**总结：内联函数最佳，const能加就加，引用能用就用，类中变量数据最好放私有区域** **用构造函数的特殊语法初始化列表**



**相当优雅**

类的匿名对象可以用作temp object（临时对象） ,一般用在生命周期比较短的作用域

```
return int( (int) char( 'a'));
```





拷贝构造编译器默认有给。

但是遇到以下情况：就不能用默认的：

类带指针成员

自定义一个string

```c++
class String
{
  public:
    String(onst char* cstr=0);
    ~String();
  private:
    char* m_data;
    
};
```

函数实现

```c++

inline//内联
    String::String(const char* cstr=0)//拷贝构造
{
    if(cstr)
    {
        m_data= new char[strlen(cstr)+1];//+1的意义是结束符号
        strcpy(m_data, cstr);//数据的挪移
    }
    else{
        m_data =new char[1];
        *m_data = '\0';//结束符号
    }
}

inline 
    String& String::operator=(const String& str)
{
    if(this == &str){
        return &this;//检测自我赋值
    }
    //这个判断是有必要的，不止是为了效率，还为了正确性假设有这个么笨蛋
    //这么使用语法 s1=s1 如果不进行检测，就把自己先delete掉了。接下来的操作都是非法操作.
    
    delete[] m_data;//指针数组串需要加中括号
    m_data = new char[ strlen(str.m_data) + 1];
    strcpy(m_data, str.m_data);//深拷贝
    return *this;
}

inline 
    String::~String()
{
    delete[] m_data;//释放
}
```

函数调用

```c++
String s1();
String s2(s1);
String s3=s2;
```



注意了有拷贝构造，VS的编译器就不给你生成别的默认的构造函数了（你就自己用你自己设定的吧！哼



啥事栈(stack)？啥事堆（heap）啥事作用域（scope）？

~~若不知道，回炉重造~~

栈：

存在于某作用域(scope)的一块内存空间。调用一个函数，函数本身会形成一个栈来方式所接还送肚饿参数和返回地址。



堆：

系统分配的（具体怎么分配问你操作系统）提供的一块全局的内存空间

程序可以动态分配区块，你得手动释放这块内存，才会死，否则只能就交给操作系统处理，具体怎么处理，还是得问问操作系统。



static静态修饰的玩意在全局区，直到程序结束



new 不是操作符，实际上是一个函数

而malloc才是一个操作符

new的底层调用了malloc,然后调用构造函数ctor

delete同理

底层原理：先调用析构函数dtor  后调用free





VC编译器习的动态内存分配

注意是32位的操作系统下。

分配的内存块(in VC)，必须是16的倍数,除了本身的数据以外，还有两个4字节的头与尾cokie    值为总内存块的大小转16进制最后一位为1

1代表着，系统给出内存。0就没有给出，可以理解为一个标志



注意，分配的若是数组，将会分配的内存块中会有一个整数（4字节），值是数组的长度

调用delete时 也会先调用数组的长度次数的析构函数，再释放free内存

