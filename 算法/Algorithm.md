# 素数的筛法

## 埃式筛和欧拉筛

**素数的倍数不是素数。**

### 埃式筛

```c++
#include <iostream>
using namespace std;
#include <time.h>
const int N = 1e6;

bool shai[N];
int Prime[N];

int main()
{
    int cnt = 0;
    double t1 = clock();
    for(int i = 2;i<=N;i++)
    {
        if(!shai[i])
        {
            shai[++cnt] = i;
            for(int j = 2;j<=N;j++)
            {
                if(i*j>N)break;
                shai[i*j] = 1;//**素数的倍数不是素数。
            }
        }
    }
    double t2 = clock();
    cout<<cnt<<endl;
    cout<<t2-t1;
    return 0;
}
```

### 欧拉筛

```c++
//欧拉筛
#include <iostream>
using namespace std;
const int N = 1e6;
#include <time.h>
bool IsPrime[N];
int a[N];

int main()
{
    int cnt = 0;
    double t1 = clock();
    for(int i = 2;i<=N;i++)
    {
        if(!IsPrime[i]) a[++cnt] = i; 
            for(int j = 1;j<=cnt;j++)
            {
                if(i*a[j]>N)break;
                IsPrime[i*a[j]] = 1;
                if(i%a[j]==0) break;//这一步非常关键，可以去除重复筛元素
            }
        
    }
    double t2 = clock();
    cout<<cnt<<endl;
    cout<<t2-t1;
    return 0;
}

```





## 素数的判断方法

```c++
#include <iostream>
#include <time.h>
using namespace std;
const int N = 1e6;

bool IsPrime(int x)
{
        for (int i = 2; i <= x / i; i++)//用除法防溢出
        {
            if (x % i == 0)
            return false;
        }
    return true;
}
int main()
{
    int cnt = 0;
    double t1 = clock();
    for(int i = 2;i<=N;i++)
    {
        if(IsPrime(i)) cnt++;
    }
    double t2 = clock();
    cout<<cnt<<endl;
    cout<<t2-t1;
    return 0;
}
```

# 数据结构

## 线性表

### 循环链表

#### 约瑟夫环问题

[约瑟夫环]: https://www.luogu.com.cn/problem/P1996	"1"

![image-20240529173305725](https://cdn.jsdelivr.net/gh/Github/hongjianMa/picture/master/202405291733338.png)

本题主要是熟悉循环链表的建立与插入与删除

```c++
#include <iostream>
using namespace std;
class Node//节点类
{
    public:
    int data;
    Node *next;
    Node(int x):data(x),next(nullptr){};
};

class CirecleList
{
    public:
    CirecleList():head(nullptr),tail(nullptr){};
    ~CirecleList()//循环链表 的析构函数
    {
        if(!head) return ;//如果头节点都为空，则返回
        Node *current = head;//定义current来删除链表中的元素
        while(current->next!=head)//如果只剩下一个元素，就停下
        {
            Node* temp = current;
            current = current->next;
            delete temp;
        }
        delete current;//删除最后一个元素
    }

    void CreateList(int x)//用尾插法创建循环链表
    {
        Node* last = nullptr;//用一个指向尾部的指针
        for(int i = 1;i<=x;i++)
        {
            Node *node = new Node(i);
            if(!head)
            {
                head = node;
                last = node;
                head->next = head;//初始化时自己指向自己形成环
            }else{
                last ->next = node;
                node->next = head;//新节点指向头部，形成环
                last = node;
            }
        }
        tail = last;// 最后一个节点成为尾节点
    }

    void func(int y)//解绝约瑟夫环问题
    {
        Node *cur = head;
        Node *pre = tail;//pre是cur前一个节点
        while(cur!=cur->next)
        {
            for(int i = 1;i<y;i++){// 循环y-1次找到第y个节点的前一个节点
                pre = cur;
                cur = cur->next;
            }
            //此时cur是第y个节点
            pre->next = cur->next;//把将cur前一个节点连接到cur后一个节点
            if(cur==head)head = cur->next;//如果删除的是头节点
            if(cur==tail)tail = pre;//如果删除的是尾节点
            cout<<cur->data<<" ";
            delete cur;//删除
            cur = pre->next;//更新
        }
        cout<<cur->data<<endl;//输出最后一个元素
        delete cur;
        head = tail = nullptr;//重置头指针和尾指针
    }
    private:

    Node *head,*tail;
};


int main()
{
    int n,m;
    cin>>n>>m;
    CirecleList list;
    list.CreateList(n);
    list.func(m);
    return 0;
}
```

