#Chapter 14: C++ Code Reuse
---
:+1:
---
* 模板类valarray
    * 头文件<valarray>
    * 声明示例
    ```C++
    valarray<int> q_values; // an array of int, size 0
    valarray<double> weights; // an array of double, size 0
    valarray<int> v2(8); // an array of 8 int elements, each set to 10
    
    double gpa[5] = {3.1, 3.5, 3.8, 2.9, 3.3};
    valarray<double> v4(gpa,4); // an array of 4 elements, initialized to the first 4 elements of gpa
    
    valarray<int> v5 = {20, 32, 17 ,9}; // C++ 11
    ```
    * 方法
        * `operator[]()`: 访问各个元素;
        * `size()`: 返回包含的元素数;
        * `sum()`: 返回所有元素的总和;
        * `max()`: 返回最大的元素;
        * `min()`: 返回最小的元素;
* 对于继承的对象,构造函数在成员初始化列表中使用类名来调用基类的构造函数;
* 对于成员对象,构造函数在成员初始化列表中使用成员名来调用相应构造函数;
* 当对象需要使用下标进行输入输出时,为使输出时不会改变对象的值,需要重载两个下标函数,C++区分常量和非常量特征标
```C++
Type_name & operator[] (parameter);
Type_name operator[] (parameter) const;
```
* 私有继承
    * 基类的公有成员和保护成员都将成为派生类的私有成员
    * 使用类名来构造函数;
    * 要使用基类方法时,使用类名和作用域解析运算符来调用;
    * 要使用基类对象,使用强制类型转换;
    * 未进行显式类型转换的派生类引用或指针,无法赋值给基类的引用或指针;
    * 只有在新类需要访问原有类的保护成员,或需要重新定义虚函数时,使用私有继承,否则使用包含; 
* 保护继承
    * 基类的公有成员和保护成员都将成为派生类的保护成员;
* 要让基类的方法在派生类外可用
    * 可以定义一个使用该基类方法的派生类方法;
    * 还可以使用`using`声明来指出特定的基类成员,即使使用的是私有派生
    ```C++
    class class_name: private std::valarray<double>
    {
    ...
    public:
        using std::valarray<double>::min;
        using std::valarray<double>::max;
        ...
    };
    ```
    * 这样, std::valarray<double>::min和using std::valarray<double>::max就像是class_name的公有方法一样;
    * using声明只使用成员名,没有圆括号,函数特征标和返回类型,如果有同名函数,则它们都可用;
    * using声明只适用于继承,不适用于包含;
* 谨慎使用多重继承(Multiple Inheritance,MI);
* 多重继承要点--虚基类;
* 类模板
    * 开头
    ```C++
    template <class Type>
    ```
    * 所有模板类型要改为Type;
    * 所有模板成员函数定义要加上`template <class Type>`;
    * 所有类限定符从`class_name::`改为`class_name<Type>::`;
    * 不能将模板成员函数放在独立的实现文件中;
    * 最好把所有模板信息放在一个头文件中,并在要使用这些模板的文件中包含该头文件;
    * 声明一个类型为模板类的对象
    ```C++
    class_name<int> test1;
    class_name<doble> test2;
    ```
    * 必须显式的提供所需的类型,而模板函数则可以根据参数类型来确定模板类型;
    * 在模板声明或模板函数定义内可以使用类限定符class_name,但在类的外面,必须使用完整的calss_name<Type>;
    * 模板可以使用指针类型;
    * 模板可以使用模板参数: `template <class T,int n>`,模板参数的值只能被使用,不能被更改,其地址也不能使用;
    * 模板类可以被用作基类;
    * 模板可以递归,__模板维的顺序与等价的二维数组相反__,P579;
    * 模板可以包含多个类型参数: `template <class T1, class T2>`;
    * 模板类显式实例化,声明必须位于模板定义所在的名称空间中
    ```C++
    template class class_name<...>;
    ```    
    * 模板类显式具体化
    ```C++
    template <> class class_name<specialized-type-name> { ...};
    ```
    * 当具体化和通用版本都与实例化匹配时,编译器将使用具体化版本;
    * 模板类部分具体化
    ```C++
    // general template
    template <class T1, class T2> class Pair { ... };
    // specialization with T2 set to int
    template <class T1> class Pair<T1,int> { ... };
    // specialization with T2 set to T1
    template <class T1> class Pair<T1,T1> { ... };
    ```
    * 如果有多个版本可供选择,编译器将使用具体化程度最高的模板;
    * 模板可以嵌套;
    * 模板可以用作参数
    ```C++
    template < template <class T> class thing>
    class class_name
    {
        ...
    }
    ```
    * 模板友元分为三类
        * 非模板友元: 所有模板实例化的友元;
        * 约束(bound)模板友元: 友元的类型取决于类被实例化的类型;
        * 非约束(unbound)模板友元: 友元的所有具体化都是类的每一个具体化的友元;
    * 模板的静态成员意味着这个类的每一个特定的具体化都将有自己的静态成员;
    * 可以使用typedef为模板具体化制定别名
    ```C++
    typedef class_name<...> alias_name;
    alias_name test;
    ```
    * 在C++11中可以使用模板提供别名
    ```C++
    template <class T>
        using alias_name = class_name<T>;
    alias_name<T> test;
    ```
    * 在C++11中将using 用于非模板时,用法与typedef等价
    ```C++
    typedef const char * pc1;
    using pc2 = const char *;
    ```                                
                   
