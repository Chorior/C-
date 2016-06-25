#Chapter 4: Composite Type
---
1. 编译器不会检查下标是否有效;
2. 初始化数组
    * 只有在定义数组时才能使用初始化;
    * 例: `int array[128] = {1,2,3,4}`;
    * 剩余的元素会被初始化为0;
3. 初始化数组时让编译器计算数组个数是很糟糕的做法;
4. strlen函数计算从当前位置到空字符的长度,而且不计算空字符;
5. 每次读取一行字符串输入
    1. getline()
        * 丢弃换行符;
        * 例: `cin.getline(array,sizeof(array))`;
        * 存储字符串时,getline()用空字符替换换行符;
    2. get()
        * 不丢弃换行符;
        * 使用不带参数的`cin.get()`可以读取下一个字符(包括换行符);
        * 例: `cin.get(array,sizeof(array))`;
    3. get()可以通过下一个输入字符来获取错误信息;
    4. get()读取空行后会设置失效位,接下来的输入会被阻断,使用`cin.clear()`恢复输入;
    5. 如果输入行包含字符数比指定的多,getline()和get()会把余下的字符留在输入队列中,getline()还会设置失效位,关闭后面的输入;
    6. cin会将换行符留在输入队列中;
6. string
    * 头文件: `<cstring>`或`<string.h>`;
    * 使用string比使用数组更方便也更安全;
    * 获取string长度: `int length = str.size()`;
    * 使用printf()输出string时: `printf("%s\n",str.c_str())`,将string转换为数组;
7. 可以将结构赋给另一个结构;
8. 共用体能够存储不同的数据类型,但只能同时存储其中的一种类型;
9. 枚举量是整型,可以被提升为int型,int类型不能自动转换成枚举类型,但有效的int值可以强制转换为枚举变量;
10. 枚举范围
    * 上限: 大于枚举最大值,最小的2的幂，减一;
    * 下限
        * 枚举最小值不小于零,下限为0;
        * 枚举最小值小于零,与上限相同的寻找方法,加上负号;
11. 使用new分配内存: `typeName * pointer = new typeName`;
12. 使用delete释放内存: `delete pointer`;
13. new与delete需要配对使用,不能释放已经释放的内存块;
14. 只能使用delete来释放使用new分配的内存;
15. 动态数组的创建与释放
    * 创建: `int * array_int = new int[128]`;
    * 释放: `delete [] array_int`;
    * 对空指针使用delete是安全的;
16. 对指针变量加1后,增加的量等于它指向的类型的字节数;
17. 对数组名使用sizeof得到的是数组的长度;对指针使用sizeof得到的是指针的长度;
18. 模板类
    * vector
        * 头文件: `<vector>`
        * 包含在std中;
        * 创建一个名为vt的vector对象,它可以存储n_elem个类型为typeName的元素: `vector<typeName> vt(n_elem)`;n_elem可以为整型常量或者整型变量;
    * array
        * 头文件: `<array>`
        * 包含在std中;
        * 创建一个名为arr的array对象,它包含n_elem个类型为typeName的元素: `array<typeName,n_elem> arr`;n_elem不能为变量;
        * 可以将一个array对象赋给另一个array对象;
        * array效率与数组相同,但更安全和方便;
    * vector和array可以使用中括号`[]`指定元素,但不能保证下标是否越界;
    * vector和array可以使用成员函数`at()`指定元素,并且会捕获非法索引;`begin()`和`end()`指向第一个和最后一个元素;                
        