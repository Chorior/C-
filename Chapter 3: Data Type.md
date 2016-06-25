#Chapter 3: Data Type
---
1. 以一个下划线开头的名称被保留给实现,用作全局标识符;
2. C++对名称的长度没有限制;
3. 整型:
    * short至少16位;
    * int至少与short一样长;
    * long至少32位,且至少与int一样长;
    * long long至少64位,且至少与long一样长;
4. 一个字节8位;
5. sizeof运算符返回字节长度
    * 对类型名使用sizeof时,应该使用括号;
    * 对变量名使用sizeof时,可以省略括号;
6. bool类型
    * 任何非零值被转换为true;
    * 零值被转换为false;
7. 浮点类型
    * float至少32位;
    * double至少48位,且不少于float,通常64位;
    * long double至少和double一样多;
8. 浮点运算速度比整数运算速度慢,且精度会降低;
9. 强制转换
    * (typeName)value; // C style
    * typeName (value); //C++ style
    * static_cast<typeName\>(value)比传统强制类型转换更为严格;     