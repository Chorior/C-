#Chapter 6: Branch and Logic operator
---
1. 判断相等条件时,将常量写在左边用以捕获将相等运算符==写为赋值运算符=的错误;
2. and,or,not是C++保留字,它们是逻辑运算符&&,||,!的另一种表达式;在C中需要包含头文件`<ios646.h>`，但C++中不需要;
3. 文件的读写
```
// write file

    #include <iostream>
    #include <fstream>
    int main()
    {
        using namespace std;
        
        fstream outFile;    
        outFile.open("test.txt"); //if test.txt exist,clear;else create a new text named test.txt
        outFIle<<"This is just a test!!"<<endl;
        outFile.close();
        
        return 0;
    }
```
    
```
//read file

    #include <iostream>
    #include <fstream>
    #include <cstdlib>
    int main()
    {
        using namespace std;
        
        ifstream inFile;
        inFile.open("test.txt");
        if(!inFile.is_open())
        {
            cout<<"Could not open the File"<<endl;
            exit(EXIT_FAILURE); // defined in <cstdlib>
        }
        
        char ch;
        int count = 0;
        while(inFile >> ch)
        {
            ++count;
        }
        
        cout<<"There are "<<count<<"charactors in test.txt"<<endl;
        inFile.close();
        
        return 0;
    }
```
