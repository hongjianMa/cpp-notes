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

## 转换构造函数（隐式类型转换 ）

### 使用转换构造函数的方法

使用转换构造函数将一个指定的数据转换为类对象的方法如下： 

（1） 先声明一个类。

（2） 在这个类中定义一个只有一个参数的构造函数，参数的类型是需要转换的类型，在函数体中指定转换的方法。

（3） 在该类的作用域内可以用以下形式进行类型转换：    类名(指定类型的数据)就可以将指定类型的数据转换为此类的对象。

例：转换构造函数1

```c++
#include<iostream>
#include<cstring>
using namespace std ;
class Student  
{	public :
	   int num ;
	   string name ;
	   char sex1; 
	   Student (int number,string nam,char sex )
	   {
	   	     num=number;
	   	     name=nam ;
			 sex1=sex;
	   }
};

 class Teacher
 {
	public :
	   void display( )
	   {
	   	  cout<<"the number is:"<<num1<<endl ;
		  cout<<"the name is :"<<name1<<endl ;
	   	  cout<<"the sex is:"<<sex2<<endl ;
	   	  cout<<"the phone is :"<<phone<<endl ;
	   	  cout<<"the addr is:"<<addr<<endl ;
	   }
	    
	   
	   Teacher(Student s,long phon,string add)
	   {  num1=s.num ;//转换构造函数
	      name1=s.name ;
		  sex2=s.sex1 ;
		  phone=phon;
	   	   addr=add;
	   }

	private :
	   long phone ;
	   string addr ;
	   int num1 ;
	   string name1 ;
	   char  sex2 ;
 }; 
int main(){
	Student st(201401,"xusong", 'm' );
	Teacher Te(st,120,"wuhan") ;
	Te.display() ;
	return 0 ;	
}
```

例2：

```c++
#include <iostream>
#include <string>

using namespace std;

class Test
{
private:
    
    int mValue;

public:
    Test()
    {
        mValue = 0;
    }
    //转换构造函数
    Test(int i)//类名(指定类型的数据)就可以将指定类型的数据转换为此类的对象。
    {
        mValue = i;
    }

    Test operator+(const Test &p)
    {
        Test ret(mValue + p.mValue);

        return ret;
    }

    int value()
    {
        return mValue;
    }
};

int main()
{
    Test t;

    t = 5; // t = Test(5);

    Test r;

    r = t + 10; // r = t + Test(10);  可能是手误写错，编译却通过

    cout << r.value() << endl;

    return 0;
}
```

工程中通过**explicit**关键字杜绝编译器的转换尝试 

转换构造函数被**explicit修饰时只能进行显示转换**



例如

```c++ 
explicit Test(int i)  
{  
    mValue = i;  
}  
```

 此时直接编译，报错，编译器不会进行转换尝试

![image-20240530072427712](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405300724870.png)



－三种显式转换方式

```c++
static_cast<ClassName>(value); 
 
ClassName(value); // 不推荐 
(ClassName)value; // 不推荐 
```



main.cpp

```c++
int main()  
{     
    Test t;  
  
    //显式类型转换
    t = static_cast<Test>(5);    // t = Test(5);  
    //t = (Test)5;   // t = Test(5);  
 
    Test r;  
      
    r = t + static_cast<Test>(10);   // r = t + Test(10);  
    //r = t + (Test)10;   // r = t + Test(10);  
      
    cout << r.value() << endl;  
      
    return 0;  
}  
```

![image-20240530072842040](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405300728132.png)



## 例3:转换构造函数

```c++
#include <iostream>
using namespace std;

// 定义一个表示复数的类
class Complex
{
public:
    // 默认构造函数，初始化复数为0+0i
    Complex()
    {
        real = 0;
        imag = 0;
    }

    // 转换构造函数，允许从一个double直接创建复数，虚部默认为0
    Complex(double r)
    {
        real = r;
        imag = 0;
    }

    // 带参数的构造函数，初始化复数的实部和虚部
    Complex(double r, double i)
    {
        real = r;
        imag = i;
    }

    // 声明友元函数来重载加法运算符，以便直接操作私有成员
    friend Complex operator+(Complex c1, Complex c2);

    // 成员函数，用于显示复数的实部和虚部
    void display();

private:
    double real;  // 复数的实部
    double imag;  // 复数的虚部
};

// 重载加法运算符，用于计算两个复数的和
Complex operator+(Complex c1, Complex c2)
{
    return Complex(c1.real + c2.real, c1.imag + c2.imag);
}

// 实现display方法，格式化输出复数
void Complex::  display()
{
    cout << "(" << real << "+" << imag << "i)" << endl;
}

int main()
{
    // 创建两个复数对象和一个默认复数对象
    Complex c1(3, 4), c2(5, -10), c3;

    // 将复数c1与一个double值2.5相加，这里会调用Complex(double)将2.5转换为复数
    c3 = c1 + 2.5;

    // 显示相加后的复数
    c3.display();
    return 0;
}

```

#### (1) 如果程序中没有Complex(double r ){real=r;imag=0;} ，程序编译时会如何？

程序编译会报错。由于已重载了“+”，在处理表达式c1+2.5时，编译系统把它解释为operator+(c1,2.5)，

由于2.5不是Complex类对象，系统先调用转换构造函数Complex(2.5)，建立一个临时的Complex类对象，其值为(2.5+0i)。

上面的函数调用相当于operator+(c1,Complex(2.5))将c1与(2.5+0i) 相加，赋给c3。运行结果为(5.5+4i)

### (2) 如果把“c3=c1+2.5;”改为c3=2.5+c1; 程序可以通过编译吗？ 

###  可以。

### (3) 如果将运算符函数重载为成员函数，complex operator+(double  d);        这个+重载函数可以被 c3=c1+2.5;调用当遇到第一个操作数不是类对象时，怎么办？

再重载一个运算符“+”函数，其第一个参数为double型。

当然此函数只能是友元函数，函数原型为 friend complex   operator+(double  d, Complex c);

显然这样做不太方便，还是将双目运算符函数重载为友元函数方便些。

### （4）

![202405292056713](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405300709724.png)

(4) 如果增加类型转换函数： ;程序在编译时出错，为什么？

```c++
#include<iostream>
using namespace std;
class complex
{
public:
 complex(   ){real=0;imag=0;}
 complex(double a){real=a;imag=0;}//这里出现问题
 complex(double a,double b){real=a;imag=b;}
 operator double(){return real;}//这里出现问题
  friend complex operator + (complex c1,complex c2);
 void display();
 private:
 double real;
 double imag;
};
void complex::display(  )
{
 cout<<"("<<real<<"+"<<imag<<"i"<<")"<<"\n";
}
complex operator + (complex c1,complex c2)
{
 return complex(c1.real+c2.real,c1.imag+c2.imag);
}
int main()
{
 complex c1(2,3),c3;
   c3=c1+2.5;                //出现二义性
 c3.display();
 return 0;
}
```

解决方案：

一、删除语句  operator double( ){return real;}

二、删除语句 friend complex operator + (complex c1,complex c2);

### （6）

![image-20240529215221266](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292152587.png)

![image-20240529215235382](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292152567.png)

![image-20240529215247745](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292152943.png)

## (重点）类型转换函数（类类型转换到普通类型）

类型转换函数的作用是**将一个类的对象转换成另一类型的数据。**

类型转换函数的一般形式为     **operator 类型名( ){实现转换的语句}**

```C++
operator Type ()  
{  
    Type ret;  
      
    //...  
      
    return ret;  
}  
```



在函数名前面不能指定函数类型。其返回值的类型是由函数名中指定的类型名来确定的。

类型转换函数只能作为成员函数，因为转换的主体是本类的对象。不能作为友元函数或普通函数.

**转换构造函数和类型转换运算符有一个共同的功能：当需要的时候，编译系统会自动调用这些函数，建立一个无名的临时对象(或临时变量)。**



**类型转换函数** 

​      －与**转换构造函数**具有**同等的地位** 

​      －使得编译器有能力将对象转化为其它类型 

​      －编译器能够**隐式**的使用类型转换函数 



代码说明

```C++

#include <iostream>  
#include <string>  
  
using namespace std;  
  
class Test  
{  
    int mValue;  
public:  
    Test(int i = 0)  
    {  
        mValue = i;  
    }  
    int value()  
    {  
        return mValue;  
    }  
    operator int ()  
    {  
        return mValue;  
    }  
};  
  
int main()  
{     
    Test t(100);  
    int i = t;  //t.operator int()
      
    cout << "t.value() = " << t.value() << endl;  
    cout << "i = " << i << endl;  
      
    return 0;  
} 
```

![image-20240530073437862](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405300734011.png)



编译器会尽力尝试让源码通过编译 ，如下

```c++
Test t(100);  
int i = t;  //t.operator int()
```

t这个对象为Test类型，怎么可能用于初始化int类型的变量呢！现在就报错吗？

不急，看看有没有类型转换函数! Ok, 发现Test类中定义了operator int () , 可以进行转换



类类型之间的转换

```c++
#include <iostream>  
 
using namespace std;  
  
class Test;  
  
class Value  
{  
public:  
    Value()  
    {  
    }  
    explicit Value(Test& t) //若不加explicit，则会报错，两个转换函数都会隐式调用不知调用哪个，所以指明该转换构造函数必须显示调用
    {  
    }  
};  
  
class Test  
{  
    int mValue;  
public:  
    Test(int i = 0)  
    {  
        mValue = i;  
    }  
    int value()  
    {  
        return mValue;  
    }  
    operator Value()  
    {  
        Value ret;  
        cout << "operator Value()" << endl;  
        return ret;  
    }  
};  
  
int main()  
{     
    Test t(100);  
    Value v = t;  
      
    return 0;  
}  
```

![屏幕截图 2024-05-30 073933](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405300753645.png)



无法抑制隐式的类型转换函数调用 

类型转换函数可能与转换构造函数冲突 

工程中以Type toType()的公有成员代替类型转换函数 

```cpp
#include <iostream>  
  
using namespace std;  
  
class Test;  
  
class Value  
{  
public:  
    Value()  
    {  
    }  
    Value(Test& t)  
    {  
    }  
};  
  
class Test  
{  
    int mValue;  
public:  
    Test(int i = 0)  
    {  
        mValue = i;  
    }  
    int value()  
    {  
        return mValue;  
    }  
    Value toValue()  //工程中以Type toType()的公有成员代替类型转换函数
    {  
        Value ret;  
        cout<<"toValue!"<<endl;
        return ret;  
    }  
};  
  
int main()  
{     
    Test t(100);  
    Value v = t.toValue();  
      
    return 0;  
} 
```





例：类型转换函数

```c++
#include <iostream>
using namespace std;

// 定义一个表示复数的类
class Complex
{
public:
    // 默认构造函数，初始化复数为0+0i
    Complex()
    {
        real = 0;
        imag = 0;
    }

    // 带参数的构造函数，初始化复数的实部和虚部
    Complex(double r, double i)
    {
        real = r;
        imag = i;
    }

    // 类型转换运算符重载，允许将Complex对象转换为double类型，返回实部
    operator double() { return real; }

private:
    double real;  // 复数的实部
    double imag;  // 复数的虚部
};

int main()
{
    // 创建两个Complex对象c1和c2，并初始化它们的值
    Complex c1(3, 4), c2(5, -10), c3;

    // 定义一个double类型的变量d
    double d;

    // 将c1转换为double类型（即获取其实部），并与2.5相加后赋值给d
    d = 2.5 + c1;

    // 输出计算结果
    cout << d << endl;

    return 0;
}

```



# 常见基础知识点

## 常对象

### 常对象的定义方式

类名  const  对象名 [(实参表列)];

const   类名 对象名[(实参表列)];

### 如果一个对象被声明为常对象，则不能调用该对象的非const型的成员函数。

![202405171915199](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292154428.png)

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

![202405172009952](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292154018.png)

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

![202405172022101](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292155462.png)

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

![image-20240503110157564](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292155550.png)

## 派生类的构造函数

派生类构造函数一般形式为

**派生类构造函数名（总参数表列）:基类构造函数名（参数表列）  {派生类中新增数据成员初始化语句}**

```c++
Student1(int n,string nam,char s,int a,string ad) : Student(n,nam,s) 
{    age=a;                        
     addr=ad;
}
```

初始化列表

```c++
Student1(int n, string nam,char s,int a, string ad):Student(n,nam,s),age(a),addr(ad){   }

```

### 派生类的构造函数的任务

（1） 对基类数据成员初始化；

（2） 对子对象数据成员初始化；

（3） 对派生类数据成员初始化。

### 执行派生类构造函数的顺序  

基类-->子对象-->派生



① 调用基类构造函数，对基类数据成员初始化；

② 调用子对象构造函数，对子对象数据成员初始化；

③ 再执行派生类构造函数本身，对派生类数据成员初始化。

### 注意点

如果在**基类或子对象类型的声明中定义了带参数的构造函数**，那么就必须显式地定义派生类构造函数，并在派生类构造函数中写出基类或子对象类型的构造函数及其参数表

 如果在基类中既定义了无参的构造函数，又定义了有参的构造函数(构造函数重载)，则在定义派生类构造函数时，既可以包含基类构造函数及其参数，也可以不包含基类构造函数。在调用派生类构造函数时，根据构造函数的内容决定调用基类的有参的构造函数还是无参的构造函数。编程者可以根据派生类的需要决定采用哪一种方式。

## 派生类的析构函数

1.在派生时，派生类是不能继承基类的析构函数的，需要通过派生类的析构函数去调用基类的析构函数。在派生类中可以根据需要定义自己的析构函数，用来对派生类中所增加的成员进行清理工作。基类的清理工作仍然由基类的析构函数负责。



2.在执行派生类的析构函数时，系统会自动调用基类的析构函数和子对象的析构函数，对基类和子对象进行清理。



3.**调用的顺序与构造函数正好相反**:            派生->子对象->基类

先执行派生类自己的析构函数，对派生类新增加的成员进行清理，然后调用子对象的析构函数，对子对象进行清理，最后调用基类的析构函数，对基类进行清理。



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

![](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292155628.png)

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

![image-20240503182301224](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292155023.png)

## 多重继承派生类的构造函数

### 构造函数形式

 派生类构造函数名(总参数表列): 基类1构造函数(参数表列), 基类2构造函数(参数表列), 基类3构造函数 (参数表列) {派生类中新增数据成员初始化语句}

### 





## 虚基类

### 菱形继承

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

![image-20240503191559133](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156760.png)

//唯一性，解决二义性

### 虚基类的作用

  如果一个派生类有多个直接基类，而这些直接基类又有一个共同的基类，则在最终的派生类中会保留该间接共同基类数据成员的多份同名成员。在引用这些同名的成员时，必须在派生类对象名后增加直接基类名，以避免产生二义性，使其惟一地标识一个成员，c1. A::display( )。

  C++提供虚基类的方法，使得在继**承间接共同基类时只保留一份成员**。

![image-20240615101645356](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151016723.png)

### 虚基类声明方式

class 派生类名: virtual  继承方式   基类名

```c++
class A:virtual public N{};
class B:virtual public N{};
```

经过这样的声明后，当基类通过多条派生路径被一个派生类继承时，该**派生类只继承该基类一次**。

### 虚基类的初始化

![image-20240615102446503](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151024877.png)



**C++编译系统只执行最后的派生类对虚基类的构造函数的调用**，而忽略虚基类的其他派生类(如类B和类C) 对虚基类的构造函数的调用，这就保证了虚基类的数据成员不会被多次初始化。



## 基类与派生类的转换

### 派生类对象可以向基类对象赋值

可以用子类B(即公用派生类)对象对其基类A对象赋值。

A  a1;              

B   b1;                

a1=b1;               

在赋值时舍弃派生类自己的成员。**赋值只是对数据成员赋值**，**对成员函数不存在赋值问题。**  

![image-20240615104448266](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151044585.png)

![image-20240615104613841](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151046217.png)

###  派生类对象可以替代基类对象向基类对象的引用进行赋值或初始化。

![image-20240615105005437](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151050816.png)

![image-20240615105113212](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151051570.png)

![image-20240615105241619](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151052007.png)

### 如果函数的参数是基类对象或基类对象的引用，相应的实参可以用子类对象

![image-20240615105424854](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151054210.png)

### 指向基类对象的指针变量也可以指向派生类对象

```c++
#include <iostream>
#include <string>
using namespace std;
class Student
 {public:
   Student(int,string,float);
   void display();
  private:
   int num;
   string name;
   float score;
 };

Student::Student(int n,string nam,float s)
 {num=n;
  name=nam;
  score=s;
 }

void Student::display()
 {cout<<endl<<"num:"<<num<<endl;
  cout<<"name:"<<name<<endl;
  cout<<"score:"<<score<<endl;
 }
 
class Graduate:public Student
 {public:
   Graduate(int,string,float,float);
   void display();
 private:
  float pay;
};

void Graduate::display()
 {Student::display();
  cout<<"pay="<<pay<<endl;
 }

Graduate::Graduate(int n,string nam,float s,float p):Student(n,nam,s),pay(p){}

int main()
 {Student stud1(1001,"Li",87.5);
  Graduate grad1(2001,"Wang",98.5,563.5);
  Student *pt=&stud1;
  pt->display();
  pt=&grad1;
  pt->display();
  return 0;
 }
  

```

![image-20240615105802829](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151058057.png)

![image-20240615105815320](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151058668.png)







# C++运算符重载

## 不能重载的运算符

1.类属关系运算符“.“

2.成员指针运算符".*"

3.作用域运算符”::“

4.测试运算符”sizeof“

5.三目运算符"?:"

## 重载运算符的函数一般格式如下：

函数类型 operator 运算符名称 (形参表列)

{ 对运算符的重载处理 }

## 重载运算符的规则

![202405172049158](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151019649.png)

(1)C++不允许用户自己定义新的运算符，只能对已有的C++运算符进行重载。

(2) C++允许重载的运算符C++中绝大部分的运算符允许重载。不能重载的运算符只有5个

1.类属关系运算符“.“

2.成员指针运算符".*"

3.作用域运算符”::“

4.测试运算符”sizeof“

5.三目运算符"?:"

前两个运算符不能重载是为了保证访问成员的功能不能被改变，域运算符和sizeof运算符的运算对象是类型而不是变量或一般表达式，不具有重载的特征。

(3) 重载不能改变运算符运算对象(即操作数)的个数。

(4) 重载不能改变运算符的优先级别。

(5) 重载不能改变运算符的结合性。

(6) 重载运算符的函数<u>不能有默认的参数</u>，否则就改变了运算符参数的个数，与前面第(3)点矛盾。

(7) 重载的运算符必须和用户定义的自定义类型的对象一起使用，其参数至少应有一个是类对象(或类对象的引用)。也就是说，参数不能全部是C++的标准类型，以防止用户修改用于标准类型数据的运算符的性质。

(8) 用于类对象的运算符一般必须重载，但有两个例外，运算符“=”和“&”不必用户重载。① 赋值运算符(=)可以用于每一个类对象，可以利用它在同类对象之间相互赋值。② 地址运算符&也不必重载，它能返回类对象在内存中的起始地址。

(9) 应当使重载运算符的功能类似于该运算符作用于标准类型数据时所实现的功能。

(10) 运算符重载函数可以是类的成员函数，也可以是类的友元函数，还可以是既非类的成员函数也不是友元函数的普通函数。

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

## 左移运算符<<重载和>>运算符重载

对“<<”和“>>”重载的函数形式如下：

 istream & operator >> (istream &,自定义类 &);

ostream & operator << (ostream &,自定义类 &);

即重载运算符“>>”的函数的第一个参数和函数的类型都必须是istream &类型，第二个参数是要进行输入操作的类。

重载“<<”的函数的第一个参数和函数的类型都必须是ostream&类型，第二个参数是要进行输出操作的类。

只能将重载“>>”和“<<”的函数作为友元函数或普通的函数，而不能将它们定义为成员函数。

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

### >>和<<运算符重载

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
    friend ostream &operator <<(ostream& cout,const A &p);//这里一点更要加const
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

ostream &operator <<(ostream& cout,const A &p)
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

![image-20240507105423210](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156141.png)

#### 为什么重载前置++要返回引用，后置++不返回引用

在 C++ 中，重载前置和后置自增运算符时，使用不同的方法来实现这两个运算符有其特定的理由。这涉及到返回值的类型和方法，以及期望的行为。

1. **前置自增（`++operator`）**:
   - **返回引用（`A&`）**：前置自增运算符的目的是增加对象的值，并返回增加后的对象。返回引用是合理的选择，因为我们希望直接修改原始对象，而不是其副本。这也使得前置自增可以用在更复杂的表达式中，如 `++a = b`，并确保操作是在原对象上进行，提高了效率（避免了不必要的对象复制）。
2. **后置自增（`operator++(int)`）**:
   - **返回值（不是引用）**：后置自增的行为与前置自增不同。后置自增应该首先保存对象当前的状态，然后增加对象的值，最后返回原始状态的一个副本。因此，它返回的是对象的副本，而不是引用。如果你尝试返回引用，那么返回的将是已经被修改的对象，这不符合后置自增的预期行为（应返回增加之前的状态）。

总结一下，这些差异归根结底是为了确保前置和后置自增符合其预期的语义：

- **前置自增**直接修改原对象并返回修改后的对象，因此返回引用。
- **后置自增**应返回操作前的状态，因此需要返回一个新的对象副本，以反映自增前的状态。

这样设计可以确保每种运算符的使用都符合直觉和C++中的常规操作语义。

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

重载> < ==关系运算符，学习strcmp的用法，1表示大于，0表示相等，-1表示小于

```c++

#include <iostream>
#include <cstring>
using namespace std;
class String
{
public:
    String() { p = NULL; }
    String(char *str);
    //双目重载关系运算符用友元函数
    friend bool operator>(String &string1, String &string2);
    friend bool operator<(String &string1, String &string2);
    friend bool operator==(String &string1, String &string2);
    void display();

private:
    char *p;
};

String::String(char *str)
{
    p = str;
}

void String::display()
{
    cout << p;
}

bool operator>(String &string1, String &string2)
{
    if (strcmp(string1.p, string2.p) > 0)
        return true;
    else
        return false;
}

bool operator<(String &string1, String &string2)
{
    if (strcmp(string1.p, string2.p) < 0)
        return true;
    else
        return false;
}

bool operator==(String &string1, String &string2)
{
    if (strcmp(string1.p, string2.p) == 0)
        return true;
    else
        return false;
}

void compare(String &string1, String &string2)
{
    if (operator>(string1, string2))
    {
        string1.display();
        cout << ">";
        string2.display();
    }
    else if (operator<(string1, string2))
    {
        string1.display();
        cout << "<";
        string2.display();
    }
    else if (operator==(string1, string2))
    {
        string1.display();
        cout << "=";
        string2.display();
    }
    cout << endl;
}

int main()
{
    String string1("Hello"), string2("Book"), string3("Computer"), string4("Hello");
    compare(string1, string2);
    compare(string2, string3);
    compare(string1, string4);
    return 0;
}

```



# 多态

## 多态概念

多态性是指具有**不同功能的函数可以用同一个函数名**，这样就可以用一个函数名调用不同内容的函数。

多态性分为两类: **静态多态性**和**动态多态性**。

**静态多态性**：函数重载和运算符重载实现的多态性属于静态多态性。在程序编译时系统就能决定调用的是哪个函数，因此静态多态性又称编译时的多态性，静态多态性是通过**函数的重载**实现的(运算符重载实质上也是函数重载)。

动态多态性：在程序运行过程中才动态地确定操作所针对的对象。它又称运行时的多态性。动态多态性是通过虚函数实现的。



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

## 虚函数

![image-20240615135547427](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151355837.png)

### 一个例子

```c++
#include <iostream>
#include <string>
using namespace std;
class Student
 {public:
   Student(int,string,float);
   void display();            
  protected:
   int num;
   string name;
   float score;
 };

Student::Student(int n,string nam,float s)
 {num=n;name=nam;score=s;}

void Student::display()
 {cout<<"num:"<<num<<"\nname:"<<name<<"\nscore:"<<score<<"\n\n";}
 
class Graduate:public Student
 {public:
   Graduate(int,string,float,float);
   void display();
 private:
  float pay;
};

void Graduate::display()
 {cout<<"num:"<<num<<"\nname:"<<name<<"\nscore:"<<score<<"\npay="<<pay<<endl;}

Graduate::Graduate(int n,string nam,float s,float p):Student(n,nam,s),pay(p){}

int main()
 {Student stud1(1001,"Li",87.5);
  Graduate grad1(2001,"Wang",98.5,1200);
  Student *pt=&stud1;
  pt->display();
  pt=&grad1;
  pt->display();
  return 0;
 }
```

![image-20240615140153759](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151401947.png)

//注意这里没有输出grad1的pay，因为没有使用虚函数



下面是使用了虚函数的程序

```c++
#include <iostream>
#include <string>
using namespace std;
class Student
 {public:
   Student(int,string,float);
   virtual void display();//这里使用了虚函数
  protected:
   int num;
   string name;
   float score;
 };

Student::Student(int n,string nam,float s)
 {num=n;name=nam;score=s;}

void Student::display()//在外面具体实现虚函数不需要加virtual关键字
 {cout<<"num:"<<num<<"\nname:"<<name<<"\nscore:"<<score<<"\n\n";}
 
class Graduate:public Student
 {public:
   Graduate(int,string,float,float);
   void display();
 private:
  float pay;
};

void Graduate::display()
 {cout<<"num:"<<num<<"\nname:"<<name<<"\nscore:"<<score<<"\npay="<<pay<<endl;}

Graduate::Graduate(int n,string nam,float s,float p):Student(n,nam,s),pay(p){}

int main()
 {Student stud1(1001,"Li",87.5);
  Graduate grad1(2001,"Wang",98.5,1200);
  Student *pt=&stud1;
  pt->display();
  pt=&grad1;
  pt->display();
  return 0;
 }
  

```

![image-20240615140344005](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151403210.png)

这里就正常输出了pay了

![image-20240615141407070](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151414436.png)

![image-20240615141543968](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151415307.png)





## 要使用虚函数的情况

![image-20240615142148565](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151421933.png)

![image-20240615142313926](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151423276.png)

![image-20240615142354811](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151423178.png)







## 纯虚函数和抽象类

![image-20240615143328693](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151433074.png)

![image-20240615143405062](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151434447.png)

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



### 虚析构函数

![image-20240615142550184](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151425543.png)

#### 一个虚析构的例子

下面是没有使用虚析构

```c++
#include <iostream>
using namespace std;
class Point
{public:
  Point(){}
  ~Point(){cout<<"executing Point destructor"<<endl;}
};

class Circle:public Point
{public:
  Circle(){}
  ~Circle(){cout<<"executing Circle destructor"<<endl;}
 private:
  int radus;
};

int main()
{Point *p=new Circle;
 delete p;
 return 0;
}

```

![image-20240615142841974](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151428201.png)

可以看到当new创建的对象delete时只执行了基类的析构函数



下面是使用了虚析构的情况

```c++
#include <iostream>
using namespace std;
class Point
{public:
  Point(){}
  virtual ~Point(){cout<<"executing Point destructor"<<endl;}
};

class Circle:public Point
{public:
  Circle(){}
  ~Circle(){cout<<"executing Circle destructor"<<endl;}
 private:
  int radus;
};

int main()
{Point *p=new Circle;
 delete p;
 return 0;
}

```

![image-20240615143042758](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202406151430980.png)

可以看到派生类的析构函数也调用了



最好把**基类的析构函数声明为虚函数**。这将使所有派生类的析构函数自动成为虚函数。这样，如果程序中显式地用了delete运算符准备删除一个对象，而delete运算符的操作对象用了指向派生类对象的基类指针，则系统会调用相应类的析构函数。





# 文件操作

![image-20240513092703392](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156238.png)

![image-20240513092735264](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156944.png)

![image-20240513092750319](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156149.png)



![image-20240513092803677](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156483.png)

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

![image-20240513092622834](https://gitee.com/hongjian-ma/tuchuang/raw/master/picture/202405292156586.png)



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

