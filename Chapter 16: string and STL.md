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
    * 基本容器特征
        * `a.begin()`: 返回指向容器第一个元素的迭代器;
        * `a.end()`: 返回超尾值迭代器;
        * `a.size()`: 返回元素个数,等价于`a.end() - a.begin()`;
        * `a.swap(b)`: 交换a,b的内容;        
    * 序列;
        * 序列中的元素具有确定的顺序;
        * 序列类型
            * deque
                * `<deque>`;
                * 如果多数操作发生在序列的起始和结尾处,使用deque最佳;
            * forward_list;
                * 单链表;
                * 不可反转容器;
            * list;
                * `<list>`;
                * 双向链表;
                * 不支持随机访问和数组表示;
                * 强调元素的快速插入和删除;
                * list成员函数
                    * `void merge(list<T,Alloc> &x)`: 将链表x与调用链表合并,**两个链表必须已经排序**,合并后的经过排序的链表保存在调用链表中,x为空;
                    * `void remove(const T &val)`: 从链表中删除val的所有实例;
                    * `void sort()`: 使用<运算符对链表进行排序,N个元素的复杂度为NlogN;
                    * `void splice(iterator pos, list<T, Alloc> x)`: 将链表x的内容插入到pos的前面,x将为空;
                    * `void unique()`: 将连续的相同元素压缩为单个元素;
            * queue;
                * `<queue>`;
                * 不允许随机访问队列元素;
                * 不允许遍历;
                * 函数
                    * `bool empty() const`: 如果队列为空,返回true,否则返回false;
                    * `size_type size() const`: 返回队列中元素数目;
                    * `T& front()`: 返回指向队首元素的引用;
                    * `T& back()`: 返回指向对尾元素的引用;
                    * `void push(const T& x)`: 在队尾插入x;
                    * `void pop()`: 删除队首元素;
            * priority_queue;
                * `<queue>`;
                * 支持的操作与queue一样;
                * 最大的元素被移到队首;
                * 构造函数
                ```C++
                priority_queue<int> test1;
                priority_queue<int> test2(greater<int>);
                ```
                * `greater<>()`是一个预定义的函数对象;
            * stack;
                * `<stack>`;
                * 不允许随机访问;
                * 不允许遍历;
                * 函数
                    * `bool empty() const`: 如果栈为空,返回true,否则返回false;
                    * `size_type size() const`: 返回栈中元素数目;
                    * `T& top()`: 返回指向栈顶元素的引用;
                    * `void push(const T& x)`: 在栈顶部插入x；
                    * `void pop()`: 删除栈顶元素;
            * vector
                * `<vector>`;
                * 可反转容器;
                * 强调元素的快速访问;
        * 序列函数(X表示序列类型)
            * `X a(n,t)`: 声明一个名为a的由n个t值组成的序列;
            * `X a(i,j)`: 声明一个名为a的序列,并将其初始化为区间[i,j)的内容;
            * `a.insert(p,t)`: 将t插入到p的前面;
            * `a.insert(p,n,t)`: 将n个t插入到p的前面;
            * `a.insert(p,i,j)`: 将区间[i,j)中的元素插入到p的前面;
            * `a.erase(p)`: 删除p指向的元素;
            * `a.erase(p,q)`: 删除区间[p,q)中的元素;
            * `a.clear()`: 等价于`erase(begin(), end())`;
            * `a.front()`: 等价于`*a.begin()`,可用于vector,list,deque;
            * `a.back()`: 等价于`*--a.end()`,可用于vetor,list,deque;
            * `a.push_front(t)`: 等价于`a.insert(a.begin(),t)`,可用于list,deque;
            * `a.push_back(t)`: 等价于`a.insert(a,end(),t)`,可用于vector,list,deque;
            * `a.pop_front()`: 等价于`a.erase(a.begin())`,可用于list,deque;
            * `a.pop_back()`: 等价于`a.erase(--a.end())`,可用于vector,list,deque;
            * `a[n]`: 等价于`*(a.begin() + n)`,可用于vector,deque;
            * `a.at(n)`: 等价于`*(a.begin() + n)`,可用于vector,deque;
        * a[n] 和 a.at(n)都返回一个指向容器中第n个元素的引用,a.at(n)会执行边界检查,,但a[n]不会;                   
    * 关联容器
        * 关联容器将值与键关联在一起,并使用键来查找值;
        * 优点在于提供了对元素的快速访问;
        * 关联容器不能指定插入的位置;
        * 四种关联容器
            * set
                * `<set>`;
                * 值类型与键相同,键是唯一的;                
                * 集合中不会有多个相同的键;
                * 可反转;
                * 集合被排序;
                * 两个有用的方法
                    * lower_bound(): 将键作为参数并返回一个指向集合中第一个不小于键参数的成员的迭代器;
                    * upper_bound(): 将键作为参数并返回一个指向集合中第一个大于键参数的成员的迭代器;                        
            * multiset
                * `<set>`;
                * 类似于set;
                * 可能有多个值的键相同;
            * map
                * `<map>`;
                * 值与键的类型不同,键是唯一的,每个键只对应一个值;
            * multimap
                * `<map>`;
                * 类似于map；
                * 一个键可以与多个值相关联;
                * 被排序;
                * 构造函数
                    * `multimap<int,string> codes;`: 键类型为int,值类型为string;
                * STL使用模板类pair<class T,class U>将键和值存储到一个对象中,前面声明的codes对象的值类型为`pair<const int, string>`
                ```C++
                pair<const int,string> item(213,"Los Angeles");
                codes.insert(item);
                ```
                * 对于pair对象,使用first和second成员来访问其两个部分;
                * 成员函数
                    * `count()`: 接受键作为参数,返回具有该键的元素数;
                    * `lower_bound()`;
                    * `upper_bound()`;
                    * `equal_range()`: 用键作为参数,返回两个迭代器,并将它们封装在一个pair对象中,这里pair的两个模板参数都是迭代器,它们表示的区间与该键匹配
                    ```C++
                    pair<multimap<KeyType,string>::iterator, 
                     multimap<KeyType,string>::iterator> range = 
                        codes.equal_range(718);
                    cout < "Cities with area code 718:\n";
                    std::multimap<KeyType,string>::iterator it;
                    for(it = range.first; it != range.second; ++ it)
                        cout << (*it).second << endl;
                    ```    
        * 四种无序关联容器
            * unordered_set;
            * unordered_multiset;
            * unordered_map;
            * unordered_multimap;                                                         
        * 通用函数
            * set_union()
                * 并集;
                * 五个迭代器参数;
                * 前两个参数定义第一个集合的区间;
                * 第三个和第四个参数定义第二个集合区间;
                * 最后一个参数指出将结果集合复制到什么位置;  
                * 示例
                ```C++
                set_union(A.begin(),A.end(),B.begin(),B.end(),
                  insert_iterator<set<string> >(C,C.begin()));
                ```
            * set_intersection()
                * 查找交集;
                * 接口与set_union()相同;
            * set_difference()
                * 获得两个集合的差;
                * 接口与set_union()相同;                
                                             
                                     
        
                                         
                               
         
          
        
        
        
