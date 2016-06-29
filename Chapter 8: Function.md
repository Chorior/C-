#Chapter 8: Function
---
:smile:
1. 如果一个函数很短,可以将其设为内联函数
```C++
inline double square(double x)
{
    return x*x;
}
```
2. 引用变量的主要用途是用作函数的形参,通过将引用变量用作函数,函数将使用原始数据,而不是其副本;
3. 必须在声明引用时将其初始化;
4. 对于形参为const引用的C++函数,如果实参不匹配,则其行为类似于按值传递,为确保原始数据不被修改,将使用临时变量来存储值;
5. 推荐尽可能使用const
    * 使用const可以避免无意中修改数据的编程错误;
    * 使用const使函数能够处理const和非const实参,否则只能接受非const数据;
    * 使用const引用使函数能够正确生成并使用临时变量;
6. 返回引用时,不能返回函数终止时不再存在的内存单元引用;
7. 当传递字符串给`string &str`时,会生成一个string的临时变量;
8. 基类引用无需强制转换便可指向派生类对象;
9. 必须通过函数原型设置默认值
```C++
char * left(const char * str,int n = 1);
```
10. 要为某个参数设置默认值,必须为它右边所有的参数提供默认值
```C++
int chico(int n, int m = 6, int p = 5);
```
11. 函数定义与没有默认参数时完全相同;
12. 函数重载
    * 如果两个函数的参数数目和类型相同,参数的排列顺序也相同,则它们的函数特征标相同,变量名是无关紧要的;
    * 如果两个函数的名称相同,但特征标不同,那么称这两个函数重载了;
    * 如果函数重载,调用的时候找不到正确的匹配,就会拒绝这个调用,并将其视为错误;
    * 特别的是,C++将类型引用和类型本身视为同一个特征标,因为调用的时候不能区分;
13. 函数模板
    * 函数模板是通用的函数描述,它使用泛型来定义函数;
    * 模板函数示例
    ```C++
    template <typename/class AnyType>
    void Function(AnyType &a,AnyType &b)
    {
        statements;
    }
    ```
    * 编译器会根据调用的参数类型,生成相应的函数(隐式实例化);
    * 显式具体化
    ```C++
    ...
    struct job
    {
        char name[40];
        double salary;
        int floor;
    };
    
    template <class AnyType>
    void Function(AnyType &a,AnyType &b);
    
    template <> void function <job>(job &,job &); 
    // template <> void function(job &,job &); 
    
    int main()
    {
        statements;
        return 0;
    }
    
    template <class AnyType>
    void Function(AnyType &a,AnyType &b)
    {
        statements;
    }
    
    template <> void function <job>(job &,job &)
    {
        statements;
    }        
    ```
    * 如果调用函数有多个原型,则编译器在选择原型时,非模板版本优先于显式具体化和模板版本,而显式具体化版本优先于使用模板生成的版本;
    * 显式实例化
    ```C++
    template void function<int>(int &,int &)；
    ```
    * 同一文件中不允许出现同一种类型的显式实例和显式具体化;
    * 示例
    ```C++
    ...
    template <class T>
    void swap(T &,T &);  // template prototype
    
    template<> void swap<job>(job &,job &); // explicit specialization for job
    
    int main()
    {
        template void swap<char>(char &,char &); // explicit instantiation for char
        
        short a,b;
        swap(a,b); // implicit template instantiation for short
        
        job n,m;
        swap(n,m); // use explicit specialization for job
        
        char g,h;
        swap(g,h); // use explicit template instantiation for char
        
        ...
        return 0;        
    }
    ...
    ```
14. 模板函数的局限及解决方案
    * 不能明确模板定义中使用变量的类型,使用关键字decltype
        * `decltype(expression) var`
            * 如果expression是一个单值,则var的类型与expression的类型相同;
            * 如果expression是一个函数调用,则var类型与函数的返回类型相同;
            * 如果expression是用括号括起的单值,那么var为指向其类型的引用;
            * 如果expression是个表达式,那么var类型与表达式最后的类型相同;
            * 示例
            ```C++
            template<class T1, class T2>
            void add(T1 x,T2 y)
            {
                 ...
                decltype(x+y) sum = x + y;
                ...
            }
            ```
    * 未声明形参时无法获取返回值的类型,采用后置返回类型
    ```C++
    template<class T1,class T2>
    auto add(T1 x,T2 y) -> decltype(x+y)
    {
        ...
        return x + y;
    }
    ```              
    
    