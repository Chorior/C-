#Chapter 15: Friend class,Exception and Others
---
:blush:
---
* 友元类
    * 友元声明
    ```C++
    friend class friend_class;
    ```
    * 友元声明可以位于公有,保护,私有部分,其所在的位置无关紧要;
    * 友元类的所有方法都可以访问原始类的私有成员和保护成员;
    * 可以选择将特定的类成员成为另一个类的友元,方法是使用前向声明,并将友元类放在**前面声明**,将友元类定义放在目标类后面;
    ```C++
    class Tv; // forward declaration
    class Remote
    {
        ...
        void set_chan(Tv &t, int c);
        ...
    };
    class Tv
    {
    private:
        int channel;
        ...
    public:        
        friend void Remote::set_chan(Tv &t, int c);
        ...
    };    
    
    inline void Remote::set_chan(Tv &t, int c)
    {
        t.channel = c;
    }      
    ```
    * 如果把定义放在实现文件中,必须删除inline,因为inline函数是內链的;
    * 可以让类成为彼此的友元类;
    ```C++
    class Tv
    {
        friend class Remote;
    public:
        void buzz(Remote &r);
        ...
    };
    class Remote
    {
        friend class Tv;
    public:
        void Bool volup(Tv &t) { t.volup(); }
        ...
    };
    
    inline void Tv::buzz(Remote &r)
    {
        ...
    }
    ```
    * 可以将函数声明为两个类共同的友元,方法是添加**前向声明**,并在两个类里面声明友元函数;
* 异常
    * 异常的三个组成部分
        * 引发异常;
        * 使用处理程序捕获异常;
        * 使用try块;
    * 示例
    ```C++
    try{
        test();
    }
    catch(...)
    {
        // statements;
    }
    
    void test()
    {
        ...
        throw somthing;
        ...
    }
    * exception类;
    * 捕获异常的catch语句顺序: 将捕获基类的catch语句放在最後面,然后向上捕获;如果有捕获所有异常的catch块,将其放在最後面;
    * **虽然异常处理对于某些项目极为重要,但它会增加编程的工作量,增大程序,降低程序的速度**;
    * **不进行错误检查的代价可能非常高**;
* **失败时返回空指针的new**
    * 示例
    ```C++
    int *pi = (std::nothrow) int;
    int *pa = (std::nothrow) int[500];
    ```
* RTTI
    * 运行阶段类型识别(Runtime Type Identification)的简称;
    * 只适用于包含虚函数的类层次结构;
    * dynamic_cast;
    * typeid;
    * type_info;
* 类型转换运算符
    * const_cast: 改变值为const或volatile,除const或volatile特征外,type_name和expression的类型必须相同;
    ```C++
    const_cast <type_name>(expression);
    ```
    * static_cast: 仅当type_name可被隐式转换为expression所属的类型或expression可被隐式转换为type_name所属的类型时,下述转换合法;
    ```C++
    static_cast <type_name>(expression);
    ```
           
                                    
                                                    
              
        
           
            