#Chapter 11: Using Class
---
:dolphin:
---
* 如果方法通过计算得到一个新的类对象,则应考虑是否可以使用类构造函数来完成这种工作;
* C++允许将运算符重新扩展到用户定义的类型,运算符函数的格式: `operatorop(argument-list)`,其中op必须是有效的C++运算符,不能虚构一个新的符号;
```C++
    class Time
    {
    private:
        ...
    public:
        Time operator+(const Time &t) const;
        ...
    };
    Time Time::operator+(const Time &t) const
    {
        ...
    }
    int main()
    {
        Time time1,time2,time3;
        ...
        time3 = time1 + time2;
        ...
    }                        
```
* C++对用户定义的运算符重载的限制
    * 重载后的运算符必须至少有一个操作数是用户定义的类型;
    * 使用运算符时不能违反运算符原来的句法规则;
    * 不能修改运算符的优先级;
    * 不能创建新运算符;
    * 有些运算符不能被重载;
    * 有些运算符只能通过成员函数重载;
* 友元
    * 友元分为三种
        * 友元函数;
        * 友元类;
        * 友元成员函数;
    * 通过让函数成为类的友元,可以赋给该函数与类的成员函数相同的访问权限;
    * 创建友元函数的第一步是将其原型放在类声明中,并在原型声明前加上关键字friend;
    * 友元函数不是成员函数,不能使用成员运算符来调用;
    * 只有类声明可以决定哪一个函数是友元;
    * 将友元函数看作类的扩展接口;
    * 友元函数的定义不能使用关键字friend;
    ```C++
        class Time
        {
        private:
            ...
        public:
           friend Time operator+(double m,const Time &t);
            ...
        };
        Time operator+(double m,const Time &t)
        {
            ...
        }
    ```   
* 如果要为类重载运算符,并将非类的项作为其第一个操作数,则可以用友元函数来反转操作数的顺序;
* 定义运算符函数时,如果要使其第一个操作数不是类对象,必须使用友元函数;
* 重载`<<`运算符
```C++
    class Time
    {
    private:
        ...
    public:
        friend ostream & operator<<(ostream &os,const Time &t);
        ...
    };
    ostream & operator<<(ostream &os,const Time &t)
    {
        os << ... ;
        return os;
    }
    int main()
    {
        Time time1,time2;
        ...
       os<<time1<<endl<<time2<<endl;
        ...
    } 
```   
* 使用成员函数实现运算符重载所需的参数数目比使用非成员函数的版本少一个,因为其中一个操作数是被隐式的传递的调用对象;
* 不要使重载的运算符在被调用时出现两个或者两个以上的匹配原型;
* 对于只有二元形式的运算符,只能将其重载为二元运算符;
* 关键字explicit
    * 如果构造函数只有一个参数或者构造函数只有一个参数没有默认值,则它可以作为转换函数,如`Time time = 30;`
    * 在构造函数前添加explicit可以避免这种隐式转换,但是可以显式转换,如`Time time = Time(99);`
    * 定义时不能使用关键字`explicit`;
* 将类转换为其它类型,使用转换函数
    * 函数形式: `operator typeName();`
    * 转换函数必须是类方法;
    * 转换函数不能指定返回类型;
    * 转换函数不能有参数;
    * 转换函数必须返回转换后的值;
    ```C++
        class Time
        {
        private:
            ...
        public:
            operator int() const;
            operator double() const;
            ...
        };
        Time::operator int() const
        {
            ...
            return int(...);
        }
        Time::operator double() const
        {
            ...
            return double(...);
        }
        int main()
        {
            Time time1,time2;
            int type_int = int(time1);
            double type_double = double(time2);
            ...
        } 
    ```    
* 最好使用显式转换,避免隐式转换;              
