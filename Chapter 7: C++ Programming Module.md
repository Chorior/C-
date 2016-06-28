#Chapter 7: C++ Programming Module
---
1. 在C++中,原型必不可少;
2. C++标准使用参数(argument)来表示实参,使用参量(parameter)表示形参,因此参数传递将参数赋给参量;
3. 在C++中,当且仅当在函数头或函数原型中,`int *arr`与`int arr[]`的含义相同,当arr是一个数组的头指针时,便于阅读和理解;
4. const的用法
    * age对于pt而言,是个常量,不能通过pt修改age,但是可以通过age直接修改
    ```C++
        int age = 23;
        const int *pt = &age;
    ```
    * pt只能指向age,可以通过pt修改age的值,但是pt不能修改,只能指向age  
    ```C++
        int age = 23;
        int * const pt = &age;
    ```
5. 模板类array除了能存储基本数据类型之外,还可以存储类对象;