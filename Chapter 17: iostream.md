#Chapter 17: iostream
---
:banana:
---
* cout.put(ch)
    * ostream &put(char);
    * 显示字符
* cout.write(s,n)
    * basic_ostream<charT,traits>&write(const char_type *s,streamsize n);
    * 第一个参数提供要显示的字符串的地址,第二个参数指出要显示多少字符;
* 控制符flush刷新缓冲区: `cout << "Hello!\n" << flush;`
* 默认浮点类型被显示为6位,末尾0不显示;
* 当指数大于等于6或小于等于-5时,使用科学计数法;
* cout.width(n)
    * 设置字段宽度;
    * int width(int);
    * 只影响将显示的下一个项目;
* 默认使用空格填充字段中未使用的部分;
* cout.fill(ch)
    * 将填充字符改为ch;
    * 设置将一直有效,直到更改;
* cout.precision(n)
    * 设置浮点显示精度;
    * 设置将一直有效,直到更改;
* cout.setf()
    * fmtflags是bitmask类型的typedef名,用于存储格式标记;
    * 两个原型
        *第一个原型: fmtflags setf(fmtflags);
            * 返回值指出标记以前的设置;
            * 参数fmtflags格式常量
                * ios_base::boolalpha : 输入输出bool值,可以为true或false;
                * ios_base::showbase : 对输出使用C++基数前缀(0,0x);
                * ios_base::showpoint : 显示末尾小数点,未显示的0将显示;
                * ios_base::uppercase : 对16进制使用大写字母;
                * ios_base::showpos : 在正数前加+;
            * 设置将一直有效,直到更改;
        * 第二个原型: fmtflags setf(fmtflags,fmtflags);
            * 第一参数包含所需设置的fmtflags值;
            * 第二参数指出要清除第一参数的哪些位;
            * 第二参数  第一参数    含义
               ios_base::basefield  ios_base::dec   使用基数10
               ios_base::basefield  ios_base::oct   使用基数8
               ios_base::basefield  ios_base::hex   使用基数16
               ios_base::floatfield ios_base::fixed 使用定点计数法
               ios_base::floatfield ios_base::scientific    使用科学计数法
               ios_base::adjustfield    ios_base::left 使用左对齐
               ios_base::adjustfield    ios_base::right 使用右对齐
               ios_base::adjustfield    ios_base::internal  符号或基数前缀左对齐,值右对齐
            * 定点表示法和科学表示法连个特征
                * 精度指的是小数位数;
                * 显示末尾的0;    
            * 设置将一直有效,直到更改;
* cout.unsetf()
    * 消除setf()的设置;
    * void unsetf(fmtflags mask);
    * setf将位设置为1,unsetf将位设置为0;
    * 无论cout的当前状态如何,`cout.unsetf(ios_base::floatfield);`都将切换到默认模式;
    ```C++
    cout.setf(ios_base::showpoint);
    cout.unsetf(ios_base::showpoint);
    cout.setf(ios_base::boolalpha);
    cout.unsetf(ios_base::boolalpha);
    ```
* 使用setf()不是进行格式化的,对用户最为友好的方法,使用控制符进行格式化比较好;
* 控制符
    * boolalpha: 调用setf(ios_base::boolalpha)
    * noboolalpha: 调用unsetf(ios_base::boolalpha)
    * showbase: 调用setf(ios_base::showbase)
    * noshowbase: 调用unsetf(ios_base::showbase)
    * showpoint
    * noshowpoint
    * showpos
    * noshowpos
    * uppercase
    * nouppercase
    * internal: 调用setf(ios_base::internal,ios_base::adjustfield)
    * left
    * right
    * dec
    * hex
    * oct
    * fixed
    * scientific
    * 其它控制符
        * `<iomanip>`;
        * setprecision(n): 指定精度,参数为整数;
        * setfill(ch): 指定填充字符;
        * setw(): 指定字段宽度,参数为整数;
    ```C++
    cout << boolalpha << true << setw(6) << "test  " << setprecision(3) << 12.00 << endl;
    ```   
* 检查文件是否打开成功
```C++
if(!fin.is_open())
{   
    statements;
}
```
* 文件模式
    * ios_base::in : 打开文件,以便读取;
    * ios_base::out : 打开文件,以便写入;
    * ios_base::ate : 打开文件,并移到文件尾;
    * ios_base::app : 追加到文件尾;
    * ios_base::trunc : 如果文件存在,则截短文件;
    * ios_base::binary : 二进制文件;
    * 运算符OR(|)用于将两个位值合并成一个可用于设置两个位的值;
    ```C++
    ifstream fin("test.txt", ios_base::in);
    //ifstream fin("test.txt");
    fin.close();
    ofstream fout("test.txt", ios_base::out | ios_base::app);
    fout.close();
    ```
* 创建临时文件名
```C++
#include <cstdio>
#include <iostream>

int main()
{
	using namespace std;
	
	cout << "This system can generate up to " << TMP_MAX
	     << " temporary names of up to " << L_tmpnam
	     << " charactors.\n";
	
	char pszName[] = "test_XXXXXX";
	
	cout << "Here are ten names: \n";
	for(int i = 0; i < 10; ++ i)
	{
		mkstemp(pszName);
		cout << pszName << endl;
	}
	
	return 0;
}   
```
* ostringstream
    * `<sstream>`;
    * 成员函数str()返回一个初始化为缓冲区内容的字符串对象
    ```C++
    string name = "pengzhen";
    string age = "23";
    ostringstream outstr;
    outstr << name << "'s age is " << age << endl;
    string result = outstr.str();
    cout << result;
    ```
* istringstream
    * `<sstream>`;
    * 可以使用istream方法读取instr中的数据
    ```C++
    string numlist = " 1 2 3 4 5";
    istringstream instr(numlist);
    int n = 0,sum = 0;
    while(instr >> n)
        sum += n;
    ```
* fout.seekg(n): 将输入指针移到指定的文件位置;
* fin.seekp(n): 将输出指针移到指定的文件位置;                     
* fout.tellg(): 对输入流,返回一个表示当前位置的streampos值,以字节为单位,从文件开始处算起;
* fin.tellp(): 对输出流,返回一个表示当前位置的streampos值,以字节为单位,从文件开始处算起;    
                                                  
                                           