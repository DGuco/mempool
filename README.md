# mempool
- 用内存池管理c++对象，结合数组和双向链表实现对象内存池（注:使用该内存的类必须有无参的构造函数），创建内存池时需要一个初始化的大小参数，这个参数需要根据
- 应用场景给一个合适的预估大小值，当内存池大小不够用时会在内部自动的申请另外的初始大小的内存区。
- 示例
```
//
//  main.cpp
//  Demo
//
//  Created by 杜国超 on 17/6/22.
//  Copyright © 2017年 杜国超. All rights reserved.
//
#include <vector>
#include <iostream>
#include <string>
#include "memsafety.h"

using namespace std;

class Demo
{
public:
    Demo()
    {
        name = "";
        age = 0;
    }
    
    void init(string _name,int _age)
    {
        name = _name;
        age = _age;
    }
    
    string GetName() {return name;}
    int GetAge()  {return age;}
private:
    string name;
    int age;
};

int main()
{
    CMemoryPool<Demo> memPool;
    //初始化内存池大小
    memPool.Create(5);
    std::vector<Demo*> demoArray;
    
    for (int i = 0; i< 10;i++) {
        Demo *demo = memPool.Alloc();
        demo->init(to_string(i), i);
        demoArray.push_back(demo);
    }
    
    for (int i = 0; i< 10;i++) {
        std::cout<< "name:" << demoArray[i]->GetName() << "   age:" << demoArray[i]->GetAge() << std::endl;
        memPool.Free(demoArray[i]);
    }
    
    memPool.Destroy();
    std::cout << "Hello word" <<  std::endl;
}
```
