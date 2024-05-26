# 构造函数/拷贝构造函数/析构函数

## 构造函数

### 构造函数的概念

#### 注意点：

1. 在类对象进入其作用域时调用构造函数。

2. 构造函数<u>没有返回值</u>，因此也不需要在定义构造函数时声明类型。

3. 构造函数<u>不需用户调用，也不能被用户调用</u>。一般不提倡在构造函数体中加入与初始化无关的内容，以保持程序的清晰。

4. 如果用户自己没有定义构造函数，则C++系统会自动生成一个构造函数，只是这个构造函数的函数体是空的，也没有参数，不执行初始化操作

5. 没有给出形参的构造函数，称为<u>默认构造函数</u>。一个类只能有一个默认构造函数。

6. 尽管在一个类中可以包含多个构造函数，但是对于每一个对象来说，建<u>立对象时只执行其中一个构造函数，并非每个构造函数都被执行。</u>

7. <u>应该在声明构造函数时指定默认值，而不能只在定义构造函数时指定默认值。</u>

8.  在声明构造函数时，形参名可以省略。

9.  如果构造函数的全部参数都指定了默认值，则在定义对象时可以给一个或几个实参，也可以不给出实参。

10.  在一个类中定义了<u>全部是默认参数的构造函数后，不能再定义重载构造函数。</u>

    ##### 在C++中，如果你在一个类中定义了一个构造函数，它所有的参数都有默认值，这意味着你可以通过不提供任何实参来调用这个构造函数。这种情况下，如果再定义一个与之重载的构造函数，可能会导致编译器无法确定应该调用哪个构造函数，因为即使没有提供实参，编译器也可以认为是在调用那个带有默认参数的构造函数。因此，这样做通常会导致编译错误或产生未预期的行为。

    ```c++
    class MyClass {
    public:
        // 这是一个全有默认参数的构造函数
        MyClass(int a = 1, int b = 2, int c = 3) {
            // ...
        }
    
        // 这是一个试图重载的构造函数
        // 但是这会导致编译错误，因为上面的构造函数可以接受0个、1个、2个或3个参数
        // 由于默认参数的存在，这个重载是不必要的，也是不允许的
        MyClass(int a, int b) {
            // ...
        }
    };
    ```

    

```cpp

//1.创建对象的时候，构造函数会自动调用，而且只调用一次
//2.析构函数： 析构函数没有参数，析构函数可以手工调用

#include <iostream>
using namespace std;
class Person
{
public:
    Person()
    {
        cout << "Person构造函数的调用！" << endl;
    }
    ~Person()
    {
        cout << "Person析构函数的调用！" << endl;
    }
};

void test01()
{
    Person p;
}
int main()
{
    test01();
    return 0;
}

```

### 构造函数的分类及调用

//1.构造函数的分类及调用

&#x20;//  --无参构造（默认构造）和有参构造

&#x20;//  --按照类型： 普通构造函数和拷贝构造函数&#x20;

```cpp
// 构造函数.cpp : 此文件包含 "main" 函数。程序执行将在此处开始并结束。
//
//1.构造函数的分类及调用
//  --无参构造（默认构造）和有参构造
//  --按照类型： 普通构造函数和拷贝构造函数
//  

#include <iostream>
using namespace std;
class Person
{
public:
    Person()
    {
        cout << "Person无参构造函数的调用！" << endl;
    }
    Person(int a)
    {
        age = a;
        cout << "Person有参构造函数的调用！" << endl;
    }

    //拷贝构造函数
    Person(const Person &p)
    {

        age = p.age;
        cout << "Person拷贝构造函数的调用！" << endl;
    }
    ~Person()
    {
        cout << "Person析构函数的调用！" << endl;
    }

private:
    int age;
};
//调用
void test01()
{
    //1.括号法
    //Person p1;//默认构造函数的调用方法
    //Person p2(10);
    //Person p3(p2);
    //注意事项
    // --调用默认构造函数的时候，不要加（）
    // --不要利用拷贝构造函数初始化匿名对象 编译器会认为 Person(p3)==Person p3

    //2.显示法
    //Person p1;
    //Person p2 = Person(10);//有参构造
    //Person p3 = p2;

    //Person(10);//匿名对象：：  特点：当前执行结束后，系统立即回收掉匿名对象
    //cout << "aaaa" << endl;
    /*运行结果
    * Person有参构造函数的调用！
      Person析构函数的调用！
      aaaa
    */

    //3.隐式转换法
    Person p4 = 10;
    Person p5 = p4;
}
int main()
{
    test01();
    return 0;
}

```

### 自动调用拷贝构造函数的情况：

```cpp
//自动调用拷贝构造函数的情况：
//1.用类的一个对象去初始化另一个对象
//2.值传递的方式给函数参数传值
//3.函数的返回值是类的对象

#include <iostream>
using namespace std;
class Person
{
public:
    Person()
    {
        cout << "Person无参构造函数的调用！" << endl;
    }
    Person(int a)
    {
        age = a;
        cout << "Person有参构造函数的调用！" << endl;
    }

    //拷贝构造函数
    Person(const Person& p)
    {

        age = p.age;
        cout << "Person拷贝构造函数的调用！" << endl;
    }
    ~Person()
    {
        cout << "Person析构函数的调用！" << endl;
    }


    int age;
};
//调用
void test01()
{
    //1.用类的一个对象去初始化另一个对象
    Person p1(20);
    Person p2(p1);
    cout << "p2的年龄为：" << p2.age << endl;

}

//2.值传递的方式给函数参数传值
void doWork(Person p)
{

}
void test02()
{
    
    Person p1;
    doWork(p1);
}

//3.函数的返回值是类的对象
Person func()
{
    Person p1;
    return p1;
}
void test03()
{
    Person p3 = func();
}
int main()
{
    //test01();
    //test02();
    test03();
    
    return 0;
}


```

### 构造函数的调用规则

```cpp
//一般情况下，c++会默认创建默认的构造函数、析构函数、拷贝构造函数
//
//构造函数的调用规则：
//如果用户自定义了了 有参构造函数 那么c++不提供默认构造函数
//如果用户自定义了 拷贝构造函数 那么c++不提供其他构造函数
#include <iostream>
using namespace std;
class Person
{
public:
    Person()
    {
        cout << "Person无参构造函数的调用！" << endl;
    }
    Person(int a)
    {
        age = a;
        cout << "Person有参构造函数的调用！" << endl;
    }
    //拷贝构造函数
    Person(const Person& p)
    {

        age = p.age;
        cout << "Person拷贝构造函数的调用！" << endl;
    }
    ~Person()
    {
        cout << "Person析构函数的调用！" << endl;
    }
    int age;
};

void test01()
{
    Person p;
    p.age = 18;
    Person p2(p);
    cout<<"p2的年龄为："<<p2.age<<endl;

}

//如果用户自定义了了 有参构造函数 那么c++不提供默认构造函数
void test02()
{
    Person p;
}

int main()
{
    test01();
    
    return 0;
}
```

### 深拷贝与浅拷贝问题

```c++
//深拷贝与浅拷贝问题
//浅拷贝：简单的赋值拷贝操作  带来的问题：堆区的内存重复释放
//深拷贝：在堆区重新申请内存，进行拷贝操作  ——>解决堆区重复释放问题
#include <iostream>
using namespace std;
class Person
{
public:
    Person()
    {
        cout << "Person默认构造函数的调用！" << endl;
    }
    Person(int a,int Hight)
    {
        age = a;
        height = new int(Hight); 
        cout << "Person有参构造函数的调用！" << endl;
    }
    //拷贝构造函数
    Person(const Person& p)
    {

        age = p.age;
        //height = p.height;编译器默认实现是这行代码

        height = new int(*p.height);//深拷贝
        cout << "Person拷贝构造函数的调用！" << endl;
    }
    ~Person()
    {
        //析构函数：将堆区开辟的内存释放
        if(height!=NULL)
        {
            delete height;
            height = NULL;
        }
        cout << "Person析构函数的调用！" << endl;
    }
    int age;
    int *height;
};

void test01()
{
    Person p1(20,180);
    cout<<"p1的年龄为："<<p1.age<<" p1的身高为"<<*p1.height<<"cm"<<endl;
    Person p2(p1);
    cout<<"p2的年龄为："<<p2.age<<" p2的身高为"<<*p2.height<<"cm"<<endl;
}
int main()
{
    test01();
    
    return 0;
}
```

## 析构函数注意事项

1. 析构函数不返回任何值，没有函数参数。因此它不能被重载。

2. 一个类可以有多个构造函数，但<u>只能有一个析构函数</u>。

   ### 以下四种情况，程序会执行析构函数

   1. 如果在一个函数中定义了一个对象(<u>它是自动局部对象)，当这个函数被调用结束时</u>，对象应该释放，在对象释放前自动执行析构函数。
   2. <u>static局部对象</u>在函数调用结束时对象并不释放，因此也不调用析构函数，只在main函数结束或调用exit函数结束程序时，才调用static局部对象的析构函数
   3. 如果定义了一个全局对象，则在程序的流程离开其作用域时(<u>如main函数结束或调用exit函数)</u> 时，调用该全局对象的析构函数。
   4. 如果用new运算符动态地建立了一个对象，当用delete运算符释放该对象时，先调用该对象的析构函数。
   5. 析构函数的作用并不是删除对象，而是在撤销对象占用的内存之前完成一些清理工作，使这部分内存可以被程序分配给新对象使用。

## 对象特性

### 初始化列表

语法：构造函数（）：属性1（值），属性2（值）.....{

....

}

```c++

#include <iostream>
using namespace std;
class Person
{
public:
    int a,b,c;
    Person()
    {
        cout << "Person无参构造函数的调用！" << endl;
    }
    Person(int A,int B,int C):a(A),b(B),c(C)
    {
        cout << "Person有参构造函数的调用！" << endl;
    }
    ~Person()
    {
        cout << "Person析构函数的调用！" << endl;
    } 
};

void test01()
{
    Person p(11,22,33);
    cout<<p.a<<" "<<p.b<<" "<<p.c<<endl;
}

int main()
{
    test01();
    return 0;
}
```

## 对象数组的初始化语法

### 分别调用构造函数

```c++
Box a[3] = {Box(10, 12, 15), Box(15, 18, 20), Box(16, 20, 26)};
```

### 用{}法

```c++
 Box a[3] = {{10,12,15},{11,11,11},{22,22,22}};
```



### 类包含类对象成员

```c++
/* Phone的构造函数调用！
Person的构造函数调用！
张三的手机名为：小米手机
Person的析构函数调用！
Phone的析构函数调用！
 */
//构造函数：先调用类对象成员
//析构函数：后析构类对象成员
#include <iostream>
#include <cstring>
using namespace std;
class Phone
{
    public:
    Phone(string a):phoneName(a)
    {
        cout<<"Phone的构造函数调用！"<<endl;
    }

    ~Phone()
    {
        cout<<"Phone的析构函数调用！"<<endl;
    }
    string phoneName;
};

class Person
{
    public:   
    Person(string a,string b):Name(a),PName(b)
    {
        cout<<"Person的构造函数调用！"<<endl;
    }

    ~Person()
    {
        cout<<"Person的析构函数调用！"<<endl;
    }
    string Name;
    Phone PName;
};

void test01()
{
    Person p1("张三","小米手机");
    cout<<p1.Name<<"的手机名为："<<p1.PName.phoneName<<endl;
}
signed main()
{
    test01();
    return 0;
}
```

### 静态成员

```c++
//静态成员变量：
//1.在编译阶段分配内存
//2.类内声明，类外初始化
//3.所有对象共享同一份数据
#include <iostream>
using namespace std;
class Person
{
public:
    static int a;
};
int Person::a = 10;//类外初始化
void test01()
{
    Person p1;
    p1.a = 200;
    cout<<p1.a<<endl;
    Person p2;
    p2.a = 2000;
    cout<<Person::a<<endl;//两种访问方式
}
int main()
{
    test01();
    return 0;
}
```

### 静态成员函数

```c++
//静态成员函数
//所有对象共享同一个函数
//静态成员函数只能访问静态成员变量
#include <iostream>
using namespace std;
class Person
{
public:

    static void func()
    {
        cout<<"func()的函数调用"<<endl;
        a = 1000;
    }
    static int a;
};
int Person::a = 10;//类外初始化
void test01()
{
   Person p1;
   p1.func();
   cout<<Person::a<<endl;
}
int main()
{
    test01();
    return 0;
}
```

## 友元

### 全局函数做友元

```c++
#include <iostream>
using namespace std;
class A
{
    friend void func(A &m);
    public:
    A():a(10),b(20)
    {
        
    }
    int a;
    private:
    int b;
};
void func(A &m)
{
   cout<<m.a<<endl;
   cout<<m.b;
}
void test()
{
    A p1;
    func(p1);
}
int main()
{
    test();
    return 0;
}
```

# 常见基础知识点

## 常对象

### 常对象的定义方式

类名  const  对象名 [(实参表列)];

const   类名 对象名[(实参表列)];

### 如果一个对象被声明为常对象，则不能调用该对象的非const型的成员函数。

![image-20240517191438201](https://cdn.jsdelivr.net/gh/Github/hongjianMa/picture/master/202405171915199.png)

编译系统只检查函数的声明，只要发现调用了常对象的成员函数，而且该成员函数<u>未被声明为const，</u>就报错。

### 引用常对象中的数据成员很简单，只需将该成员函数声明为const即可

```c++
 void get_time( ) const;         
这表示get_time是一个const型函数，即常成员函数。
常成员函数可以访问常对象中的数据成员，但不允许修改常对象中数据成员的值。
```

有时在编程时有要求，一定要修改常对象中的某个数据成员的值。   C++考虑到实际编程时的需要，对此作了特殊的处理，对该数据成员声明为mutable，如 mutable int count; 把count声明为可变的数据成员，这样就可以用声明为const的成员函数来修改它的值。

## 常函数

### 注意事项

1.  如果在一个类中，有些数据成员的值允许改变，另一些数据成员的值不允许改变，则可以将一部分数据成员声明为const，以保证其值不被改变，可以用非const的成员函数引用这些数据成员的值，并修改非const数据成员的值。
2. 如果要求所有的数据成员的值不都允许改变，则可以将所有的数据成员声明为const，或将对象声明为const(常对象)，然后用const成员函数引用数据成员，这样起到“双保险”的作用，切实保证了数据成员不被修改。
3. 如果已定义了一个<u>常对象</u>，<u>只能调用其中的const成员函数</u>，而不能调用非const成员函数(不论这些函数是否会修改对象中的数据)。这是为了保证数据的安全。
4. 常成员函数不能调用另一个<u>非const成员函数。</u>

## 指针常量和常量指针()

如果const位于星号*的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；

如果const位于星号*的右侧，const就是修饰指针本身，即指针本身是常量。

通俗理解：
*左定值，右定向，const修饰不变量**

### 常量指针

当为常量指针时，不可以通过修改所指向的变量的值，但是指针可以指向别的变量。

```c++
int a = 5;
const int *p =&a;
*p = 20;   //error  不可以通过修改所指向的变量的值

int b =20;
p = &b; //right  指针可以指向别的变量

```

### 注意事项

1. 如果<u>一个变量已被声明为常变量，只能用指向常变量的指针变量指向它</u>，而不能用一般的(指向非const型变量的)指针变量去指向它。指向常量的常指针变量，是指针变量指向一个固定的对象，该对象的值不能改变。

   ```c++
   const char c[] =“boy”;
   const char *p1;
   p1 = c;
   char *p2 = c; // 错误
   ```

  2.如果函数的形参是指向非const型变量的指针，实参只能用指向非const型变量的指针

```c++
const char str[] =“boy”;
void fun(char *ptr);
fun(str); // 错误
```

#### 指向常对象的指针变量

1.如果一个对象已被声明为常对象，只能用指向常对象的指针变量指向它，而不能用一般的(指向非const型对象的)指针变量去指向它。

2.如果定义了一个指向常对象的指针变量，并     使它指向一个非const的对象，<u>则其指向的对象是不能通过指针来改变的。</u>如果希望在任何情况下t1的值都不能改变，则应把它定义为const型。指向常对象的指针只能调用常成员函数。

```c++
Time t1(10, 12, 15);
const Time *p = &t1;
t1.hour = 18;   // 正确
(*p).hour = 18; // 错误   指向的对象是不能通过指针来改变的。
```

3.指向常对象的指针最常用于函数的形参，目的是在保护形参指针所指向的对象，使它在函数执行过程中不被修改。

#### 指向常对象的指针只能调用常成员函数

![image-20240517200941630](https://cdn.jsdelivr.net/gh/Github/hongjianMa/picture/master/202405172009952.png)



### 指针常量

当为指针常量时，指针常量的值不可以修改，就是不能指向别的变量，但是可以通过指针修改它所指向的变量的值。

```c++
int a = 5;
int *const p = &a;//左定值
*p = 20;     //right 可以修改所指向变量的值

int b = 10;
p = &b;      //error 不可以指向别的变量

```

## 对象的常引用

![image-20240517202203805](https://cdn.jsdelivr.net/gh/Github/hongjianMa/picture/master/202405172022101.png)

强制修改const限定的值

```c++
#include <iostream>
using namespace std;
int main()
{
	const int a = 10;
	int* b = (int*)&a;
	*b = 20;
	cout << *b;
	return 0;
}
```

```c++
#include <iostream>
using namespace std;
int main()
{
	const int a = 10;
	int* b = const_cast<int*>(&a);
	*b = 20;
	cout << *b << endl;
	return 0;
}
```

这段代码尝试通过指针间接修改一个`const`变量的值。虽然这种做法违反了C++中`const`关键字的语义，并且会导致未定义行为，但我们可以从内存和编译器模型的角度讨论其背后的原理。

1. **声明与初始化**:
   - 首先，`const int a = 10;`声明了一个常量`a`，并初始化为10。这意味着按照语言规则，`a`的值不应该在后续被修改。
2. **指针赋值与类型转换**:
   - 接着，`int* b = (int *)(&a);`这一行创建了一个整型指针`b`，并通过类型强制转换`(int *)`获得了`a`的地址。这里，我们告诉编译器忽略`a`的`const`属性，允许我们用一个非`const`指针指向它。这是一个危险的操作，因为它破坏了类型系统的安全性。
3. **修改指针指向的值**:
   - 然后，`*b = 20;`尝试通过指针`b`修改其指向地址的内容，即尝试修改`a`的值为20。由于指针`b`失去了`const`信息，从指针操作的角度看，这行代码似乎可以执行对内存的写入操作。
4. **未定义行为**:
   - 尽管上述步骤在某些环境下看似能“工作”，但实际上这是未定义行为。C++标准并没有规定在这种情况下程序应该如何反应。可能的结果包括但不限于：
     - **修改成功**：在一些简单的环境或特定编译器配置下，内存中的值确实被修改了，因此`*b`现在指向的是20。
     - **修改未反映**：编译器可能已经将`a`的值内联到了所有使用它的位置，导致修改内存的实际效果无法体现，打印`*b`时可能看到预期的20，但直接打印`a`可能还是10。
     - **程序崩溃**：在更严格的安全检查或特定平台下，尝试修改只读内存可能会导致访问违规，从而引发程序崩溃。
     - **其他不可预测行为**：包括但不限于数据损坏、优化导致的意料之外的结果等。
5. **输出**:
   - 最后，`cout << *b << endl;`输出了指针`b`所指向的值，如果修改生效，这里会输出20，但这并不意味着`a`本身的值在语言逻辑层面被成功修改了。

总结来说，虽然通过指针强制类型转换和解引用来修改`const`变量的值在某些情况下看似可行，但这是一种危险且不符合语言规范的做法，可能导致各种不可预料的问题。正确的编程实践是遵循`const`的语义，确保程序的健壮性和可维护性。



## const 和 mutable

mutable就是为了突破成员函数 const的限制，可以在const函数里面来修改被mutable修饰的成员变量。

```c++
class A
{
public:
	A()
	{
		this->number = 0;
	}
private:
	mutable int number;//可改变的
public:
	int getNum()const//常量函数
	{
		this->number++;
		return this->number;
	}
};
int main()
{
	A a;
	cout << a.getNum() << endl;
	cout << a.getNum() << endl;

	return 0;
}

```

# 继承

![image-20240503110157564](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131337461.png)

## 继承中构造和析构的顺序

先构造父类，再构造子类

先析构子类，后析构父类

```c++
#include <iostream>
using namespace std;

class Base
{
public:
    Base()
    {
        cout << "Base的构造函数" << endl;
    }
    ~Base()
    {
        cout << "Base的析构函数" << endl;
    }
};

class Son : public Base
{
public:
    Son()
    {
        cout << "Son的构造函数" << endl;
    }
    ~Son()
    {
        cout << "Son的析构函数" << endl;
    }
};
void test01()
{
    Son s1;
}

int main()
{
    test01();
    return 0;
}
```

![](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338863.png)

## 同名成员的调用

```c++
#include <iostream>
using namespace std;

class base
{
public:
    int m_a;

    static int m_b;

    void func()
    {
        cout << "base的func()的调用" << endl;
    }
};
int base::m_b = 0;

class Son : public base
{
public:
    int m_a;

    static int m_b;

    void func()
    {
        cout << "Son的func()的调用" << endl;
    }
};
int Son::m_b = 10;
void test01()
{
    Son s1;
    s1.func();
    s1.base::func();
    cout << "Son的m_b: " << s1.m_b << endl;
    cout << "base的m_b: " << s1.base::m_b << endl;
}

int main()
{
    test01();
    return 0;
}
```

![image-20240503182301224](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338906.png)

## 虚基类：菱形继承

```c++
#include <iostream>
using namespace std;
class N
{
    public:
    int num;
};

class A:virtual public N{};
class B:virtual public N{};

class C:public A,public B{};

void test01()
{
    C c;
    c.A::num = 100;
    c.B::num = 200;
    cout<<"c中A作用域下的num=="<<c.A::num<<endl;
    cout<<"c中B作用域下的num=="<<c.B::num<<endl;
    cout<<"c中的num=="<<c.num<<endl;
}

int main()
{
    test01();
    return 0;
}
```

![image-20240503191559133](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338132.png)

//唯一性，解决二义性

# C++运算符重载

## 重载运算符的函数一般格式如下：

函数类型 operator 运算符名称 (形参表列)

{ 对运算符的重载处理 }

## 重载运算符的规则

![image-20240517204911869](https://cdn.jsdelivr.net/gh/Github/hongjianMa/picture/master/202405172049158.png)

## 加号+运算重载

  //类成员函数实现加号重载

```c++
#include <iostream>
using namespace std;
class A
{
public:
    int m_a;
    int m_b;
    //类成员函数实现加号重载
    A operator+(A p)
    {
        A temp;
        temp.m_a = this->m_a+p.m_a;
        temp.m_b = this->m_b+p.m_b;
        return temp;
    }
};
void test01()
{
    A p1;
    p1.m_a = 20;
    p1.m_b = 30;
    A p2;
    p2.m_a = p2.m_b = 40;
    A p3;
    p3 = p1+p2;
    cout<<"p3的m_a = "<<p3.m_a<<endl;
    cout<<"p3的m_b = "<<p3.m_b<<endl;
}
int main()
{
    test01();
    return 0;
}
```



//全局函数重载加号运算符

```c++

#include <iostream>
using namespace std;
class A
{
public:
    int m_a;
    int m_b;
    //类成员函数实现加号重载
    /* A operator+(A p)
    {
        A temp;
        temp.m_a = this->m_a+p.m_a;
        temp.m_b = this->m_b+p.m_b;
        return temp;
    } */
};

//全局函数重载加号运算符
A operator+(A a1,A a2)
{
    A temp;
    temp.m_a = a1.m_a+a2.m_a;
    temp.m_b = a1.m_b+a2.m_b;
    return temp;

}
void test01()
{
    A p1;
    p1.m_a = 20;
    p1.m_b = 30;
    A p2;
    p2.m_a = p2.m_b = 40;
    A p3;
    p3 = p1+p2;
    cout<<"p3的m_a = "<<p3.m_a<<endl;
    cout<<"p3的m_b = "<<p3.m_b<<endl;
}
int main()
{
    test01();

    return 0;
}
```

## 左移运算符<<重载

```c++
#include <iostream>
using namespace std;
class A
{
    //友元访问隐私成员
    friend ostream& operator<<(ostream &cout,A p);
public:
    A(int a,int b):m_a(a),m_b(b)
    {

    }

private:
    int m_a;
    int m_b;
};
//只能全局函数重载左移运算符
ostream& operator<<(ostream &cout,A p)//链式编程为了和endl匹配
{
    cout<<"p的m_a = "<<p.m_a<<"  p的m_b = "<<p.m_b;
    return cout;
}
void test01()
{
    A p1(20,40);
    cout<<p1<<"\n"<<"小马哥太帅啦!"<<endl;
}

int main()
{
    test01();
    return 0;
}
```

## >>和<<运算符重载

### 例题

有两个矩阵a和b，均为2行3列,求两个矩阵之和。

重载运算符“+”，使之能用于矩阵相加。如： c=a+b。

重载流插入运算符“<<”和流提取运算符“>>”，使之能用于该矩阵的输入和输出。

```C++
#include <iostream>
using namespace std;
class Matrix
{
    friend ostream &operator<<(ostream &out,const Matrix &a);//<<运算符重载
    friend istream &operator>>(istream &in,Matrix &a);//>>运算符重载
private:
    int m_a[2][3];
public:
    Matrix()
    {
        for (int i = 0; i < 2; i++)
        {
            for (int j = 0; j < 3; j++)
            {
                m_a[i][j] = 0;
            }
        }
    }
    Matrix operator+(const Matrix &b)//+号运算符重载
    {
        Matrix c;
        for (int i = 0; i < 2; i++)
        {
            for (int j = 0; j < 3; j++)
            {
                c.m_a[i][j] = this->m_a[i][j] + b.m_a[i][j];
            }
        }
        return c;
    }
};
ostream &operator<<(ostream &out,const Matrix &a)//注意引用的使用
{
    for (int i = 0; i < 2; i++)
        {
            for (int j = 0; j < 3; j++)
            {
                out<<a.m_a[i][j]<<' ';
            }
            out<<endl;
        }
        return out;
}
istream &operator>>(istream &in,Matrix &a)
{
    for (int i = 0; i < 2; i++)
        {
            for (int j = 0; j < 3; j++)
            {
                in>>a.m_a[i][j];
            }
        }
        return in;
}
int main()
{
    
    Matrix A,B;
    cout<<"请输入A矩阵："<<endl;
    cin>>A;
    cout<<"请输入B矩阵："<<endl;
    cin>>B;
    cout<<"矩阵C = A + B："<<endl;
    Matrix C = A+B;
    cout<<C;
    return 0;
}
```



## 递增运算符重载

```c++
#include <iostream>
using namespace std;

class A
{
    friend ostream &operator <<(ostream& cout,A p);//这里不引用
public:
    A(int a):m_a(a){};

    //前置++
    A &operator++()//这里一定要引用
    {
        m_a++;
        return *this;
    }

    //后置++
    A operator++(int)//这里不引用***
    {
        //记录当时结果
        A temp = *this;
        m_a++;
        return temp;
    }


private:
    int m_a;

};

ostream &operator <<(ostream& cout,A p)
{
    cout<<p.m_a;
    return cout;
}
void test01()
{
    A p1(1);
    cout<<++p1<<endl;
    cout<<p1<<endl;
}
void test02()
{
    A p2(1);
    cout<<p2++<<endl;
    cout<<p2;
}
int main()
{
    test01();
    test02();
    return 0;
}
```

![image-20240507105423210](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338981.png)

## 赋值运算符重载

```c++
#include <iostream>
using namespace std;
class A
{
    public:
    A(int a)
    {
        m_a = new int(a);
    }

    ~A()
    {
       if(m_a!=NULL)
        {
            delete m_a;
            m_a = NULL;
        }
    }
    //重载赋值运算符
    //为了防堆区内存重复释放
    A &operator=(A &p)
    {
        if(m_a!=NULL)//析构函数的思路
        {
            delete m_a;
            m_a = NULL;
        }
        m_a = new int(*p.m_a);
        return *this;
    }
    int *m_a;

};

void test01()
{
    A p1(20),p2(30),p3(40);
    p1 = p2 = p3;
    cout<<*p1.m_a<<endl;
    cout<<*p2.m_a<<endl;
    cout<<*p3.m_a<<endl;

}
int main()
{
    test01();    
    return 0;
}
```

## 关系运算符重载

```c++
#include <iostream>
using namespace std;
#include <cstring>
class A
{
public:
    A(int a,string name):m_a(a),m_name(name){};
    int m_a;
    string m_name;

    bool operator==(A p)
    {
        if(this->m_a==p.m_a&&this->m_name==p.m_name){
            return true;
        }
        return false;

    }
};

void test01()
{
    A p1(18,"马洪建");
    A p2(18,"马洪建");
    if(p1==p2){
        cout<<"相同"<<endl;
    }
    else{
        cout<<"不同"<<endl;
    }

}
int main()
{
    test01();
    return 0;
}
```

# 多态

## 多态的基本使用

```c++
#include <iostream>
using namespace std;
class Animal
{
    public:
    //虚函数，在编译阶段就不调用了
    virtual void Speak()
    {
        cout<<"动物在说话"<<endl;
    }
};
class Cat:public Animal
{
    public:
    void Speak()
    {
        cout<<"小猫在说话"<<endl;
    }
};
class Dog:public Animal
{
    public:
    void Speak()
    {
        cout<<"小狗在说话"<<endl;
    }
};

void DoSpeak(Animal &animal)
{
    animal.Speak();
}
//多态满足的基本条件：
//1,有继承关系
//2，子类重写父类中的虚函数

//多态的使用
//父类指针或引用子类对象

void test01()
{
    Cat cat;
    DoSpeak(cat);
    Dog dog;
    DoSpeak(dog);
}

int main()
{
    test01();
    return 0;
}
```

## 纯虚函数和抽象类

```c++
#include <iostream>
using namespace std;

//纯虚函数和抽象类
class Base
{
public:
    virtual func() = 0;
    //纯虚函数
    //只要有一个纯虚函数，这个类就叫做抽象类
    //抽象类特点：
    //1,无法实例化对象
    //2,抽象类的子类一定要重写父类中的纯虚函数，不然也属于抽象类

};
class Son:public Base
{
public:
    virtual func(){
        cout<<"func()函数的调用"<<endl;
    }
};

void test01()
{
    Base *base = new Son;
    base->func();

}
int main()
{
    test01();

    return 0;
}
```

## 虚析构和纯虚析构

```c++
#include <iostream>
using namespace std;
#include <cstring>
class Animal
{
    public:
    Animal()
    {
        cout<<"Animal的构造函数调用！"<<endl;
    }
    //利用虚析构可以解决父类指针在析构的时候 不会调用子类中析构函数
    /* virtual ~Animal()
    {
        cout<<"Animal的虚析构函数调用！"<<endl;
    }
 */
    //纯虚析构
    //需要声明，也需要具体实现
    virtual ~Animal() = 0;

    //虚函数，在编译阶段就不调用了
    virtual void Speak()
    {
        cout<<"动物在说话"<<endl;
    }
};

Animal::~Animal()
{
    cout<<"Animal的纯虚析构函数调用！"<<endl;
}

class Cat:public Animal
{
    public:
    Cat(string name)
    {
        cout<<"Cat的构造函数调用"<<endl;
        m_name = new string(name);
    }
    ~Cat()
    {
        if(m_name!=NULL){
            cout<<"Cat的析构函数调用！"<<endl;
            delete m_name;
            m_name = NULL;
        }
    }
    void Speak()
    {
        cout<<*m_name<<"小猫在说话"<<endl;
    }
    string *m_name;
};
class Dog:public Animal
{
    public:
    void Speak()
    {
        cout<<"小狗在说话"<<endl;
    }
};

void DoSpeak(Animal &animal)
{
    animal.Speak();
}


void test01()
{
    Animal *animal = new Cat("汤姆猫");
    animal->Speak();
    delete animal;
    //父类指针在析构的时候 不会调用子类中析构函数，导致子类如果有堆区属性，会导致内存泄露
}

int main()
{
    test01();
    return 0;
}
```

# 文件操作

![image-20240513092703392](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338790.png)

![image-20240513092735264](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338605.png)

![image-20240513092750319](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131341088.png)



![image-20240513092803677](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338350.png)

## 写文件

```c++
#include <iostream>
using namespace std;
#include <fstream>

void test01()
{
    //创建流对象
    ofstream ofs;

    //指定打开方式
    ofs.open("text.txt", ios::out);

    //写内容
    ofs << "小马哥" << endl;
    ofs << "男" << endl;
    ofs << "19岁" << endl;

    ofs.close();
}

int main()
{
    test01();
    std::cout << "Hello World!\n";
}
```

## 读文件

![image-20240513092622834](https://raw.githubusercontent.com/hongjianMa/picture/master/202405131338406.png)



```c++
#include <iostream>
using namespace std;
#include <fstream>
#include <string>

//文本文件

void test01()
{
    //1.包含头文件

    //2.创建流对象
    ifstream ifs;
    //3.打开文件 ，判断是否打开成功
    ifs.open(("E:\\学习\\编程\\vs2022\\c++\\黑马程序员\\文本操作 写文件\\文本操作 写文件\\text.txt"),ios::in);
    if (!ifs.is_open())
    {
        cout << "文件打开失败了" << endl;
        return;
    }
    //4.读数据

    //第一种
    char buf[1024] = { 0 };
    while (ifs >> buf)
    {
        cout << buf << endl;
    }

    //第二种
    char buf[1024] = { 0 };
    while (ifs.getline(buf, sizeof(buf)))
    {
        cout << buf << endl;
    }

    //第三种
    string s;
    while (getline(ifs, s))
    {
        cout << s << endl;
    }
    //第四种
    char c;
    while ((c = ifs.get()) != EOF)
    {
        cout << c;
    }
    //5.关闭
    ifs.close();
}
int main()
{
    test01();
}
```

# 模板

## 函数模板

### 函数模板的使用方法

```c++
#include <iostream>
using namespace std;
//两种方式使用函数模板
    /* 
    1.自动类型推导
     mySwap(a,b);
    2,显示指定类型
    mySwap<int>(a,b) */
//函数模板
template<typename T>
void mySwap(T &a,T &b)
{
    T temp = a;
    a = b;
    b = temp;
}


void test01()
{
    int a = 201;
    int b = 10;
    mySwap<int>(a,b);
    cout<<"a = "<<a<<endl;
    cout<<"b = "<<b<<endl;
}
int main()
{
    test01(); 
    return 0;
}
```

### 函数模板注意事项

```c++
//注意事项：
//1，函数模板必须推导出一致数据类型

//2，模板必须要给出T的数据类型，才可以使用
#include <iostream>
using namespace std;
//函数模板
template<typename T>
void mySwap(T &a,T &b)
{
    T temp = a;
    a = b;
    b = temp;
}

template<class T>
void func()
{
    cout<<"func()的调用！"<<endl;
}

void test01()
{
    int a = 201;
    int b = 10;
    //两种方式使用函数模板
    /* 
    1.自动类型推导
     mySwap(a,b);
    2,显示指定类型
    mySwap<int>(a,b) */
    mySwap<int>(a,b);
     
    //必须指定T的数据类型
    func<int>();//<int>
      
    cout<<"a = "<<a<<endl;
    cout<<"b = "<<b<<endl;
}
int main()
{
    test01();
    return 0;
}
```

### 函数模板案例-数组排序

```c++
#include <iostream>
using namespace std;
/* 实现通用对数组进行排序的函数
规则 从大到小
算法 选择
测试 char数组 int数组 */

//交换函数模板
template<class T>
void mySwap(T &a,T &b)
{
    T temp = a;
    a = b;
    b = temp;
}

//排序算法
template<class T>
void mySort(T arr[],int len)
{
    for(int i = 0;i<len;i++)
    {
        int max = i;
        for(int j = i+1;j<len;j++)
        {
            if(arr[max]<arr[j])
            {
                max = j;
            }
        }
        if(max!=i)
        {
            mySwap(arr[max],arr[i]);
        }
    }
}

//打印数组模板
template<class T>
void printArr(T arr[],int len)
{
     for(int i = 0;i<len;i++)
     {
        cout<<arr[i]<<' ';
     }
     cout<<endl;
}
void test01()
{
    //测试char数组
    char chararr[] = "abrfyhuj";
    mySort(chararr,sizeof(chararr));
    printArr(chararr,sizeof(chararr));

}
void test02()
{
    //测试int数组
    int a[] = {1,2,5,6,3,1,45,444,56};
    mySort(a,sizeof(a)/sizeof(int));
    printArr(a,sizeof(a)/sizeof(int));
}

int main()
{
    test01();
    test02();
    return 0;
}
```

### 函数模板和普通函数的区别

```c++
#include <iostream>
using namespace std;


//函数模板与普通函数的区别
/* 
1，普通函数在调用时可以发生隐式类型转换
2，函数模板，用自动类型推导，不可以发生隐式类型转换
3，函数模板 用显示指定类型，可以发生隐式类型转换 */

void test01()
{
    int a = 201;
    int b = 10;
    //两种方式使用函数模板
    /* 
    1.自动类型推导
     mySwap(a,b);
    2,显示指定类型
    mySwap<int>(a,b) */
    mySwap<int>(a,b);
   
    cout<<"a = "<<a<<endl;
    cout<<"b = "<<b<<endl;
}
int main()
{
    test01();
    
    return 0;
}
```

### 函数模板的调用规则

```c++
#include <iostream>
using namespace std;

//普通函数和函数模板的调用规则
/* 
1，如果函数模板和普通函数都可以调用，优先调用普通函数
2，可以通过空模板参数列表，强制调用函数模板
3，函数模板可以发生函数重载
4，如果函数模板可以产生更好的匹配，优先调用函数模板 */

void Myfunc(int a,int b)
{
    cout<<"普通函数调用"<<endl;
}

template<typename T>
void Myfunc(T a,T b)
{
    cout<<"函数模板调用"<<endl;
}
template<typename T>
void Myfunc(T a,T b,T c)
{
    cout<<"函数模板重载的调用"<<endl;
}

void test01()
{
    int a = 201;
    int b = 10;
    //1，如果函数模板和普通函数都可以调用，优先调用普通函数
    //2，可以通过空模板参数列表，强制调用函数模板
    //3，函数模板可以发生函数重载
    Myfunc(a,b);

    //空模板参数列表，强制调用函数模板
    Myfunc<>(a,b);
    //函数模板重载的调用
    Myfunc<>(a,b,100);
}

void test02()
{
    char a = 'a';
    char b = 'b';
    //4，如果函数模板可以产生更好的匹配，优先调用函数模板
    Myfunc(a,b);
}
int main()
{
    //test01();
    test02();    
    return 0;
}
```

### 模板的局限性

```c++
#include <iostream>
using namespace std;

//函数模板的局限性
//模板并不是万能的，有些特定的数据类型，需要具体化的方式做特殊实现
class Person
{
public:
    Person(string name,int age)
    {
        this->m_Name = name;
        this->m_Age = age;
    }
    string m_Name;
    int m_Age;
};



//对比两个数据是否相等的函数
template<class T>
bool myCmp(T &a,T &b)
{
    if(a==b)return true;
    else return false;
}

//利用具体化Person的版本代码实现Compare函数
template<>
bool myCmp(Person &a,Person &b)
{
    if(a.m_Name == b.m_Name && a.m_Age == b.m_Age)
    {
        return true;
    }
    else
    {
        return false;
    }
}

void test01()
{
    int a = 10;
    int b = 20;
    bool ret = myCmp(a,b);
    if(ret)
    {
        cout<<"a==b"<<endl;
    } 
    else
    {
        cout<<"a!=b"<<endl;
    }
}

//测试并不是万能的
void test02()
{
    Person p1("Tom",10);
    Person p2("Tom",10);
    bool ret = myCmp(p1,p2);
    if(ret)
    {
        cout<<"p1==p2"<<endl;
    } 
    else
    {
        cout<<"p1!=p2"<<endl;
    }
}
int main()
{
    test01();
    test02();
    return 0;
}
```

## 类模板

### 类模板的基本语法

```c++
#include <iostream>
using namespace std;
#include <string>
//类模板
template<class Nametype,class Agetype>
class Person
{
    public:

    Person(Nametype name,Agetype age):m_name(name),m_age(age){};
    Nametype m_name;
    Agetype m_age;

};

void test01()
{
    Person<string,int> p("Tom",20);//类模板的基础语法
    cout<<p.m_name<<endl;
    cout<<p.m_age<<endl;
}
int main()
{
    test01();
    return 0;
}
```

### 类模板和函数模板的区别

#### 类模板不能发生自动类型推导，必须显示调用类模板

```c++
//类模板和函数模板的调用区别
//1.类模板没有自动类型推导，只能用显示调用
#include <iostream>
using namespace std;
template <class NameType,class AgeType>
class Person
{
    public:
    Person(NameType name,AgeType age):m_name(name),m_age(age){};
    void Show()
    {
        cout<<this->m_name<<' '<<this->m_age<<endl;
    }
    NameType m_name;
    AgeType m_age;
};

//类模板没有自动类型推导
void test01()
{
    //Person p1("小明",20);没有自动类型推导，有的编译器会优化
    Person <string,int>p1("小明",20);
    p1.Show();
}
int main()
{
    test01();

    return 0;
}
```

#### 类模板可以有默认参数列表

```c++
//类模板和函数模板的调用区别
//2.类模板在参数列表中，可以有默认参数
#include <iostream>
using namespace std;
template <class NameType,class AgeType = int>//默认参数
class Person
{
    public:
    Person(NameType name,AgeType age):m_name(name),m_age(age){};
    void Show()
    {
        cout<<this->m_name<<' '<<this->m_age<<endl;
    }
    NameType m_name;
    AgeType m_age;
};

//类模板在参数列表中，可以有默认参数
void test01()
{
    
    Person <string>p1("小明",20);//这里可以不写int因为AgeType = int
    p1.Show();
}
int main()
{
    test01();

    return 0;
}
```

### 类模板中成员函数的调用时机

```c++
#include <iostream>
using namespace std;
//类模板中成员函数的调用时机
//类模板中的成员函数在调用的时候才去创建
class Person1
{
    public:
    ShowPerson1()
    {
        cout<<"Person1的函数调用"<<endl;
    }
};
class Person2
{
    public:
    void ShowPerson2()
    {
        cout<<"Person2的函数调用"<<endl;
    }
};


template <class T>//不调用只编译是不会报错的
class A
{
    public:
    T obj;
    
    //类模板中的成员函数
    void func1()
    {
        obj.ShowPerson1();
    }

    void func2()
    {
        obj.ShowPerson2();
    }
};

void test01()
{
    A<Person2>m;
    //m.func1();   调用1不可以，因为类型不匹配
    m.func2();
}
int main()
{
    test01();
    return 0;
}
```

### 类模板做函数的参数

```c++
//类模板对象做函数的参数
//1.指定传入类型--整体拿进来
//2.参数模板化
//3.整个类模板化
#include <iostream>
using namespace std;
#include <string>
template <class Nametype,class Agetype>
class Person
{
public:
    Person(Nametype name, Agetype age)
    {
        this->name = name;
        this->age = age;
    }
    void Showperson()
    {
        cout << "name: " << name << endl;
        cout << "age: " << age << endl;
    }
    Nametype name;
    Agetype age;
};

//1.指定传入类型--整体拿进来
void PrintPerson1(Person<string, int> &p)
{
    p.Showperson();
}
void test01()
{
    Person<string, int> p("Tom", 10);
    PrintPerson1(p);
}
//2.参数模板化
template <class T1,class T2>
void PrintPerson2(Person<T1,T2>&p)
{
    p.Showperson();
    cout<<"T1的数据类型是："<<typeid(T1).name()<<endl;
    cout<<"T2的数据类型是："<<typeid(T2).name()<<endl;
}

void test02()
{
    Person<string,int> p2("猪八戒",40);
    PrintPerson2(p2);
}

//3.整个类模板化
template<class T>
void PrintPerson3(T &p)
{
    p.Showperson();
    cout<<"T的数据类型是："<<typeid(T).name()<<endl;
}
void test03()
{
    Person <string,int>p3("孙悟空",100);
    PrintPerson3(p3);
}
int main()
{
    test01();
    test02();
    test03();
    return 0;
}
```

### 类模板和继承

```c++
#include <iostream>
using namespace std;
#include <string>
//如果父类是类模板，子类要么指定出父类模板的数据类型，要么子类也是类模板
template<class T>
class base
{
public:
    T name;
};
//指定出父类模板的数据类型
class Son1:public base<string>{
    public:
};
void test01()
{
    Son1 c;
}

//类模板继承类模板，就可以用T2指定父类中T的类型
template <class T1,class T2>
class Son2:public base<T2>
{
    public:
    Son2()
    {
        cout<<"T1的数据类型是："<<typeid(T1).name()<<endl;
        cout<<"T2的数据类型是："<<typeid(T2).name()<<endl;

    }
    T1 obj;
};
void test02()
{
    Son2<string,int> a;
}
int main()
{
    test02();

    return 0;
}
```

## 类模板外定义各成员函数

源代码

```c++
#include <iostream>
using namespace std;
template <class numtype>
class Compare
{
public:
    Compare(numtype a, numtype b)
    {
        x = a;
        y = b;
    }

    numtype max()
    {
        return (x > y) ? x : y;
    }

    numtype min()
    {
        return (x < y) ? x : y;
    }

private:
    numtype x, y;
};


int main()
{
    Compare<int> cmp1(3, 7);
    cout << cmp1.max() << endl;
    cout << cmp1.min() << endl
         << endl;
    Compare<float> cmp2(45.7, 93.6);
    cout << cmp2.max() << endl;
    cout << cmp2.min() << endl
         << endl;
    Compare<char> cmp3('a','A');
    cout << cmp3.max() << endl;
    cout << cmp3.min() << endl;
    return 0;
}
```

在类外定义各成员函数

```c++
#include <iostream>
using namespace std;

template <class numtype>
class Compare
{
public:
    Compare(numtype a, numtype b);
    numtype max();
    numtype min();

private:
    numtype x, y;
};

// 在类模板外部声明成员函数
template <class numtype>
Compare<numtype>::Compare(numtype a, numtype b)
{
    x = a;
    y = b;
}

template <class numtype>
numtype Compare<numtype>::max()
{
    return (x > y) ? x : y;
}

template <class numtype>
numtype Compare<numtype>::min()
{
    return (x < y) ? x : y;
}

int main()
{
    Compare<int> cmp1(3, 7);
    cout << cmp1.max() << endl;
    cout << cmp1.min() << endl
         << endl;
    Compare<float> cmp2(45.7, 93.6);
    cout << cmp2.max() << endl;
    cout << cmp2.min() << endl
         << endl;
    Compare<char> cmp3('a', 'A');
    cout << cmp3.max() << endl;
    cout << cmp3.min() << endl;
    return 0;
}
```

