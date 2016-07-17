#Chapter 16: string and STL
---
:apple:
---
* string类
    * string类由头文件`<string>`支持;
    * `<string.h>`或`<cstring>`只支持对C风格字符串进行操作的C库字符串函数,不支持string类;
    * string类实际上是模板具体化`basic_string<char>`的一个typedef;
    * basic_string有四个具体化
    ```Ｃ++
    typedef basic_string(char) string;
    typedef basic_string(wchar_t) wstring;
    typedef basic_string(char16_t) u16string; // C++ 11
    typedef basic_string(char32_t) u32string; // C++ 11
    ```
    * string::npos定义为字符串的最大长度,通常为unsigned int的最大值;
    * string类的构造函数
        * `string(const char *s)`: 将string对象初始化为s指向的传统C字符串;
        * `string(size_type n, char c)`: 创建一个包含n个元素的string对象,其中每个元素都被初始化为字符c;
        * `string(const string &str)`: 将一个string对象初始化为string对象str;
        * `string()`: 默认构造函数,创建一个默认的string对象,长度为0;
        * `string(const char *s,size_type n)`: 将string对象初始化为s指向的传统c字符串的前n个字符,即使超过了其结尾;
        * `template<class Iter> string(Iter begin,Iter end)`: 将string对象初始化为区间[begin,end)内的字符;
        ```C++
        char array_test[] = "This is just a test!!!";
        string str_test0(array_test + 3, array_test + 10);
        string str_test1(&str_test0[1], &str_test0[6]);
        ```
        * `string(const string &str,size_type pos = 0, size_type n = npos)`: 将一个string对象初始化为对象str中从pos开始的n个字符;
    * string类输入,指定分界符后,换行符被视为常规字符
    ```C++
    char info[100];
    cin >> info; // read a word
    cin.getline(info,100); // read a line, discard \n
    cin.getline(info,100,':'); // read up to :, discard :
    cin.get(line,100); // read a line,leave \n in queue
    
    string stuff;
    cin >> stuff; // read a word
    getline(cin,stuff); // read a line, discard \n
    getline(cin,stuff,':'); // read up to :, discard : 
    ```   
    * 使用字符串
        * size()和length()成员函数都返回字符串中的字符数;
        * length()来自较早版本的string类,size()是为提供STL兼容性而添加的;
        * 在string中搜索给定的字符串或字符
            * find方法
                * `size_type find(const string &str,size_type pos = 0) const`: 从字符串的pos位置开始,查找字符串str,成功返回str首次出现的位置,失败返回string::npos;
                * `size_type find(const char *s,size_type pos = 0) const`: 从字符串的pos位置开始,查找字符串*s;
                * `size_type find(const char *s,size_type pos = 0,size_type n)`: 从字符串的pos位置开始,查找s的前n个字符组成的子字符串;
                * `size_type find(char ch,size_type pos = 0) const`: 从字符串的pos位置开始,查找字符ch;       
            * rfind()方法: 查找子字符串或字符最后一次出现的位置;
            * find_first_of()方法: 查找参数中任意一个字符首次出现的位置;
            * find_last_of()方法: 查找参数中任意一个字符最后出现的位置;
            * find_first_not_of()方法: 查找字符串中第一个不包含在参数中的字符;
            * find_last_not_of()方法: 查找字符串中最后一个不包含在参数中的字符;
        * 方法capacity()返回当前分配给字符串的内存块的大小;
        * 方法reserve()请求内存块的最小长度;
        ```C++
        string str_test0;
        string str_test1("test");
        
        cout << "str_test0.size() = " << str_test0.size() << endl;
        cout << "str_test1.size() = " << str_test1.size() << endl;  
        
        cout << "str_test0.capacity() = " << str_test0.capacity() << endl;          
        cout << "str_test1.capacity() = " << str_test1.capacity() << endl;
        
        str_test0.reserve(50);
        cout << "str_test0.capacity() = " << str_test0.capacity() << endl;
        ```
        * c_str()方法返回一个指向c字符串的指针,该c字符串与string内容相同;
* 智能指针模板类
    * 模板auto_ptr已被Ｃ++11摒弃;
    * 将new获得的指针赋给智能指针,那么当智能指针过期时,其析构函数将使用delete来释放内存;
    * 头文件: `<memory>`;
    * 将new出来的指针赋给智能指针
    ```C++
    shared_ptr<double> pd;
    double *p_reg = new double;
    pd = p_reg; // not allowed
    pd = shared_ptr<double>(p_reg); // allowed
    shared_ptr<double> pshared(p_reg); // allowed
    ```
    * 不能将delete运算符赋给非堆内存
    ```C++
    string str_test("This is a test!");
    shared_ptr<string> sp(&str_test); // NO!
    ```
    * unique_ptr
        * 对于特定的对象,只有一个智能指针可拥有它,只有拥有对象的智能指针的构造函数会删除该对象,赋值操作会转让所有权;
        * 如果unique_ptr是一个临时右值,那么允许将其赋给另一个unique_ptr，否则不行;        
    * shared_ptr: 赋值时,指针数加一,过期时,指针数减一,仅当最后一个指针过期时,调用delete;
    * 如果程序要使用多个指向同一个对象的指针,选择shared_ptr,否则使用unique_ptr;
* 标准模板库(STL)
    * 迭代器
        * 每个容器都定义了一个迭代器;
        * 超过结尾(past-the-end): 迭代器,指向容器最后一个元素后面的那个元素,end()成员函数返回超过结尾;
    * push_back(): 将元素添加到矢量末尾;
    * 三个具有代表性的ＳＴＬ函数
        * 头文件: `<algorithm>`
        * for_each
            * 三个参数;
            * 前两个为容器中区间的迭代器,最后一个是指向函数的指针;
            * 将被指向的函数应用于区间中的各个元素;
            * 被指向的函数不能修改容器函数的值;
        * random_shuffle
            * 两个参数;
            * 参数为容器中区间的迭代器参数;
            * 随机排列该区间的元素;
        * sort
            * 两个版本,在Ｃ++11中可用于常规数组;
            * 第一个版本
                * 两个参数;
                * 参数为容器中区间的迭代器参数;
                * 使用内置的<运算符对值进行比较,按升序排列区间中的元素;                
                * 如果是用户自定义的对象,那么必须定义能够处理自定义对象的operator<()函数;
            * 第二个版本
                * 三个参数;
                * 前两个为容器中区间的迭代器,最后一个是指向函数的指针;
                * 指向的函数与第一个版本的operator<()类似,返回true表示参数区间正确;
    * 基于范围的for循环(C++11)
    ```C++
    double prices[5] = {4.99, 10.99, 6.87, 7.99, 8.49);
    for(double x: prices)
        std::cout << x << std::endl;
    ```
* 泛型编程
    * 泛型编程注重算法;
    * C++将`operator++()`作为前缀版本,`operator++(int)`作为后缀版本;
    * 最好避免使用迭代器,应尽可能使用STL函数来处理细节;
    * 五种迭代器
        * 输入迭代器;
        * 输出迭代器;
        * 正向迭代器;
        * 双向迭代器;
        * 随机访问迭代器;
    * 如果迭代器相同,则它们指向的内容肯定相同;
    * copy()函数
        * 三个参数;
        * 前两个参数为迭代器指向的区间,最后一个参数表示要将第一个元素复制到什么位置;
        * 用区间中的数据覆盖目标容器中的数据;
        ```C++
        int casts[10] = { 6, 7, 2, 9, 4, 11, 8, 7, 10, 5 };
        vector<int> dice[10];
        copy(casts, casts + 10, dice.begin());
        ```
    * 输出流迭代器
        * 包含头文件`<iterator>`;
        * `ostream_iterator<int,char> out_iter(std::cout, " ");
        * 第一个模板参数指出被发送给输出流的数据类型,第二个参数指出输出流使用的字符类型;
        * 构造函数的第一个参数指出要使用的输出流,可以是文件输出流;
        * 构造函数的第二个参数指出在发送给输出流的每个数据项后显示的分隔符;
        * 使用输出流迭代器
        ```C++
        *out_iter++ = 15; // work like cout << 15 << " ";
        copy(dice.begin(), dice,end(), out_iter);
        ```  
    * 输入流迭代器: `copy(istream_iterator<int,char>,(cin), istream_iterator<int,char>(). dice.begin())`;
    * 反向迭代器
        * rbegin()和end()返回相同的值,但类型不同;
        * rend()和begin()返回相同的值,但类型不同;   
        * 反向输出: `copy(dice.rbegin(), dice.rend(), out_iter)`;
        * 对反向迭代器进行递增操作导致递减;
    * 插入迭代器
        * back_insert_iterator: 将元素插入到容器尾部;
        * front_insert_iterator: 将元素插入到容器最前端;
        * insert_iterator: 将元素插入到指定位置前;
        ```C++
        vector<string> test;
        back_inset_iterator<vector<string> > back_iter(test);
        insert_iterator<vector<string> > insert_iter(test, test.begin());
        string str_test[4] = { "This", "is", "a", "test" );
        copy(str_test,str_test + 4, back_iter);
        //copy(str_test,str_test + 4, insert_iter);
        ```   
    * 序列;
    * 关联容器;
                                     
        
                                         
                               
         
          
        
        
        