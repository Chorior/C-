
#Chapter 18: C++11
---
:blue_car:
---
* 右值引用: 可关联到右值,但不能对其应用地址运算符的值;
* 移动语义: 避免移动原始数据,只是修改记录;
* 使用移动语义时,若含有指针,最好将原指针设为nullptr;
* 移动赋值运算符参数不能是const引用;
* 如果提供析构函数、赋值构造函数或复制赋值运算符,编译器将不会自动提供移动构造函数和移动赋值运算符;
* 如果提供移动构造函数或移动赋值运算符,编译器将不会自动提供赋值构造函数和复制赋值运算符;
* 使用关键字default可以显式的声明上面的函数
```C++
class Someclass
{
public:
  Someclass(Someclass &&);
  Someclass() = dafault;
  Someclass(const Someclass &) = default;
  Someclass &operator=(const Someclass &) = default;
  ...
};
```
* 使用关键字delete用于禁止编译器使用特定方法
```C++
class Someclass
{
public:
  Someclass(Someclass &&);
  Someclass() = delete;
  Someclass(const Someclass &) = delete;
  Someclass &operator=(const Someclass &) = default;
  ...
};
```
* 要禁止复制,可将复制构造函数和赋值运算符放在private部分,使用delete也能达到这个目地,且更简单,不容易出错;
* 虚函数说明符override指出要覆盖一个虚函数,将其放在参数列表后面;
* 禁止覆盖特定的虚函数,在参数列表后面加上final;
* lambda表达式
  * 能够使用匿名函数;
  * 使用[]替代了函数名,并省去了返回类型;
  * 当lambda表达式完全由一条返回语句组成时,自动类型推断才管用;否则使用后置返回语法;
* 正则表达式库: `<regex>`;
* lexical_cast能够在数值和字符串之间进行简单的转换
```C++
#include "boost/lexical_cast.hpp"
...
int age = 23;
float weight = 55;
std::string name = "pengzhen";
std::string me = name +
  "'age = '" + boost::lexical_cast<string>(age) +
  ",weight = " + boost::lexical_cast<string>(weight);
...
```  
* 建模语言: Unified Modeling Lauguage,UML,一种表示编程项目的分析和设计语言.  
