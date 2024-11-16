# C++学习笔记

1. main函数执行前会初始化系统资源，设置栈指针，初始化静态和全局变量，全局对象，将main函数的参数argc和argv传递给main函数。

        main执行完会调用析构函数

2. 结构体内存对齐问题

        结构体内成员按照声明顺序存储，按结构体中size最大的成员对齐

    3.指针和引用的区别

- 指针是一个变量，存储的是一个地址，引用是变量的别名

- 指针可以有很多级，引用只有一级

- 指针可以为空，引用不能为NULL，在定义时必须先初始化

- 把指针当作参数传递时，也是将实参的一个拷贝传递给形参，两者指向地址不同，是不同的变量。引用是同一个变量

- 引用一旦初始化就不能再改变
4. 堆和栈的区别

    栈是由系统自动分配的，效率会高；堆是由程序员自己申请的，效率低。

5. ```cpp
   int *p[10]//指针数组
   int (*p)[10]//数组指针
   int *p(int)//函数声明
   int (*p)(int)//函数指针
   ```

6.new/delete和malloc/free

new自动计算要分配的空间，malloc需要手动计算;new封装了malloc，所以直接free不会报错

7. 宏定义和函数的区别

    宏在预编译阶段完成替换，函数调用在运行时会跳转到具体的函数上。

8. strlen和sizeof的区别

    sizeof是运算符，不是函数，结果在编译中获得而不是运行中获得；strlen是字符处理的库函数。

9. static关键字

属于类，不属于对象。static修饰全局变量时，作用域只在当前文件中。

1. static成员变量
   
   所有对象共享同一份数据,类内声明，类外初始化.

```cpp
class Persion{
  public:
 static int m_a;  
};
int Persion::m_a=10;
int main(){
 //通过对象访问
 Persion p1;
 p1.m_a=100;
 Persion p2;
 p2.m_a=200;
 cout<<p1.m_a<<p2.m_a;//两个值都为200.
 //通过类名访问
 cout<<Persion::m_a<<endl;
}
```

        2. static成员函数

            不具有this指针，无法访问类对象的非static成员变量和成员函数；不能被声明为const、虚函数和volatile；

10. const关键字

        const常量在定义时必须初始化，之后无法更改；const形参可以接受const和非const类型的实参。

        const成员变量，不能在类定义外部初始化，只能通过构造函数初始化列表进行初始化。

11. 数组名和指针

        数组名不是真正意义上的指针，没有自增自减等操作。当数组名当作形参传递给调用函数后，就退化成了一般指针，多了自增、自减操作。

12. 浅拷贝和深拷贝的区别

        浅拷贝只是拷贝一个指针，拷贝的指针和原来的指针指向同一块地址，如果原来的指针所指向的资源释放了，那么再释放浅拷贝的指针就会出现错误(<mark>浅拷贝的缺点</mark>)

13. 内联函数和宏定义
    
    宏只是在编译前进行字符串替换，内联函数可以进行参数类型检查，且具有返回值。
    
    内敛函数在编译时直接将函数代码嵌入到目标代码中，省去了函数调用的开销。
    
    使用 场景：作为类成员接口函数来读写类的私有成员，会提高效率。

14. 如何判断大小端序
    
    使用强制类型转换
    
    ```cpp
    int a=0x1234;
    char c=(char)a;
    //由于int转为char型，只会留下低地址的部分
    ```

15. volatile

        是一种类型修饰符，系统总是重新从他所在的内存读取数据，而不是寄存器的备份。因此<mark>多线程</mark> 中被几个任务共享的变量要定义为volatile类型。

16. explicit

        用来修饰类的构造函数，不能发生相应的<mark>隐式类型转换</mark>，只能以显示的方式进行类型转换

17. C++中的几种类型的new(plain new,nothrow new和placement new) 
- plain  new:普通的new,空间分配失败时，会抛出异常std::bad_alloc而不是NULL。

- nothrow new:在空间分配失败的情况下是不会抛出异常的，而是返回NULL。使用：new(nothrow)。

- placement new:允许在一块已经分配成功的内存上重新构造对象或对象数组，不用担心内存分配失败，因为它根本不分配内存，而是调用对象的构造函数

        注意：使用placement new构造起来的对象数组，要显式的调用他们的析构函数来销毁，不能使用delete

        使用：ptr=(Type*)new(prt)TypeName;ptr是一个指向已分配内存的指针

18. try,throw和catch关键字

        try包裹的语块中，如果发生了异常，使用throw进行异常抛出，再由catch进行捕获

19. malloc、realloc、calloc的区别
- ```cpp
  int *p=malloc(20*sizeof(int));//申请20个int类型的空间。每个int值是随机的
  ```

- ```cpp
  int *p=calloc(20,sizeof(int));//calloc申请的空间值是初始化为0的；
  ```

- ```cpp
  void realloc(void *p,size_t new_size);//给动态分配的空间分配额外的空间。
  ```
20. 类成员初始化

    1.赋值初始化，通过在函数体内进行赋值初始化；

    2.列表初始化，在冒号后使用初始化列表进行初始化；

    对于在函数体中初始化，是在所有数据成员被分配内存空间后才进行的，而列表初始化是在给数据成员分配内存空间时就进行初始化。

21. 句柄
    
    句柄的本质是对底层硬件实例的指针的引用

22. 避免内存泄露的几种方法
- 计数法：使用new或malloc时，让该数+1，delete或free时，该数-1.

- 一定要将基类的析构函数声明为虚函数(确保在删除指向派生类对象的基类指针时，会根据指针实际指向的对象类型来调用对应的析构函数)

- 对象数组的释放一定要用delete[]
23. std::move移动语义函数

        允许资源(如动态分配的内存、文件句柄等)从一个对象转移到另一个对象，而不是复制。std::move并不是真的移动对象，他只是将其转为右值引用。     

24. 右值引用(右值引用的本质是左值)(区分左右值的方法，看能不能被取地址-有没有实体)

```cpp
string s1="213";//字符串常量是特殊例子，他虽然是个常量，但是是个左值，具有地址
string test(){
    return string("abc");
}
1.string &&s2=std::move(s1); //将亡值
2.string &&s3=test();//右值引用绑定纯右值--允许将资源从临时对象移动到目标对象       
```

右值引用在传递时会变为左值

```cpp
void fun(T&&t){
    cout<<t<<endl;
}
int getInt(){
    return 5;
}
int main(){
    int a=10;
    int &b=a;//b是左值引用
    int &c=10;//错误，c是左值，不能使用右值初始化
    int &&d=10;//正确，右值引用用右值初始化
    int &&e=a;//错误，e是右值，不能使用左值初始化
    const int& f=a;//正确，左值常引用相当于万能型，可以用左值或者右值初始化
    const int& g=10;//正确，同上
    const int&& h=10;//正确，右值常引用
    const int& aa=h;//h相当于10的别名
    int& i=getInt();//错误，i是左值引用不能使用临时变量初始化
    int&& j=getInt();//正确，函数返回值是右值
    fun(10);
}
```

25. 静态绑定和动态绑定

        静态绑定发生在编译期，动态绑定发生在运行期，要想实现多态，必须使用动态绑定。

26. cout和printf的区别

        cout<<是类std::ostream的全局对象，cout<<后可以跟不同的类型是因为它已存在针对各种类型数据的重载，会自动识别数据的类型。

        输出过程会首先将输入字符放入缓冲区，然后输出到屏幕，是**有缓冲输出**，printf是**行缓冲输出**

26. const char*和string的区别

```cpp
//a)  string转const char* 
string s = “abc”; 
const char* c_s = s.c_str(); 

//b)  const char* 转string，直接赋值即可 
const char* c_s = “abc”; 
 string s(c_s); 

//c)  string 转char* 
 string s = “abc”; 
 char* c; 
 const int len = s.length(); 
 c = new char[len+1]; 
 strcpy(c,s.c_str()); 

//d)  char* 转string 
 char* c = “abc”; 
 string s(c); 
//e)  const char* 转char* 
 const char* cpc = “abc”; 
 char* pc = new char[strlen(cpc)+1]; 
 strcpy(pc,cpc);

//f)  char* 转const char*，直接赋值即可 
 char* pc = “abc”; 
 const char* cpc = pc;
```

27. 回调函数

    当发生某种事件时，系统或其他函数将会自动调用你定义的一段函数；回调函数相当于一个中断处理函数，由系统在符合设定的条件时自动调用。

    **回调函数就是通过函数指针调用的函数**

28. **一致性哈希-没太懂**

29. c++代码到可执行程序的步骤
- 预编译：主要处理源代码文件中以”#“开头的预编译指令。

- 编译：把预编译之后生成的.i或.ii文件进行分析、优化，生成相应的汇编代码。

- 汇编：将汇编代码转为机器码。产生.o文件(Linux)或.obj文件(Windows)。

- 链接：将不同源文件产生的目标文件进行链接。
30. 友元函数和友元类
- 友元函数：定义在类外，不属于任何类，可以访问其他类的私有成员。但是需要在类的定义中声明所有可以访问它的友元函数。

- 友元类：友元类的所有成员函数都是另一个类的友元函数，都可以访问另一个类中的所有隐藏信息。
31. C++中类的成员数据和成员函数内存分布情况

    一个类对象的地址就是类所包含的这片内存空间的首地址，这个首地址也就对应具体某一个成员变量的地址。

    对象的大小和对象中数据成员的大小是一致的。**成员函数是存放在代码区的。** 

    **静态成员函数与一般成员函数的区别就是没有this指针** 因此，不能访问非静态数据成员。

32. this指针
- this指针是类的指针，指向对象的首地址。

- this指针只能在成员函数中使用，在全局函数、静态成员函数中都不可以使用。

- this指针只有在成员函数中才被定义。

- this指针的存储位置会因编译器不同而不同。
33. C++11有哪些新特性
    
    - nullptr替代NULL
    
    - 引入了auto和decltype这两个关键字实现了类型推导
    
    - 基于范围的for循环  for(auto&i:res){}
    
    - 类和结构体的初始化列表
    
    - Lambda表达式(匿名函数)
    
    - std::forward_list(单向链表)
    
    - 右值引用和move语义

34. auto
    
    让编译器通过初始化值来进行类型推演，从而获得变量的类型，因此**auto定义的变量必须有初始值** 
    
    ```cpp
    //const类型
    const int i=5;
    auto j=i;//变量i的顶层是const，会被auto忽略，j的类型为int
    //引用类型
    int x=2;
    int& y=x;
    auto z=y;//z是int类型而不是int&类型
    ```

35. decltype

        选择并返回操作数的数据类型

```cpp
decltype(func())sum=5;//sum的类型是函数func()的返回值类型
//const类型
const int c=3;
decltype(c)d=c;//d的类型为const int
//引用类型
const int i=3,&j=i;
decltype(j)k=5;//k的类型为const int&
```

36. NULL和nullptr

    NULL来自C语言，被定义为(void*)0,而在c++中，NULL被定义为整数0。**在传入NULL参数时，会把NULL当作整数0来看**。nullptr在c++11中被引入用于解决这一问题，nullptr可以明确区分整型和指针类型，能够根据环境自动转换成相应的指针类型。

37. 智能指针
    
    - shared_ptr
    
    采用引用计数器的方法，允许多个智能指针指向同一个对象，每当多一个指针指向该对象时，计数器+1；每当减少一个智能指针指向对象时，计数器-1，当计数器为0时释放资源
    
    - unique_ptr
    
    一个非空的unique_ptr总是拥有它所指向的资源。转移一个unique_ptr将会把所有权全部从源指针转移给目标指针。
    
    - weak_ptr
    
    - auto_ptr

38. lambda表达式:不用定义

```cpp
[](){

}
//[&]以引用的方式获取到参数列表。
```

39. **迭代器**：++it、it++的区别

```cpp
//++i的实现代码
int& operator++(){
    *this +=1;
    return *this;
}
//i++实现代码
int operator++(int){
    int temp=*this;
    ++*this;
    return temp;
}        
```

++i返回一个引用，i++返回一个对象(会产生一个临时对象)，效率低。

40. STL中hashtable
    
    使用开链法进行冲突避免

41. traits技法

42. STL的两级空间配置器
    
    当开辟内存<=128bytes时，调用二级空间配置器(维护16条链表，最小8字节，最大128字节)，否则调用一级空间配置器。

43. vector和list
-  vector:和数组类似，只是不需要考虑大小，vector实现了容量的动态增长。

- list:list是双向链表实现的，涉及额外指针的维护，开销比较大。

- vector中的迭代器在使用后就会失效，而list的迭代器在使用之后还可以继续使用

- vector释放空间：vector的内存占用空间只增不减，clear()可以清楚vector中的元素，但是无法做到内存回收
  
  ```cpp
  vector<int>vec(100,100);
  //清除掉多余的空间
  vector<int>(vec).swap(vec);//创建一个vec的副本，并交换
  //清楚掉全部的空间
  vector<int>().swap(vec);
  ```

- 顺序容器中(vector,deque)，erase迭代器会使被删除元素之后的所有迭代器失效，所以不能使用erase(it++)的方式，但其返回值是下一个有效的迭代器。It=erase(it);而关联容器则可以使用erase(it++)的方式
44. map和set关联容器
- 底层都是以红黑树的结构实现的，插入删除操作的时间复杂度为O(logn)

- 实现map红黑树的节点数据类型是key+value，而实现set的节点数据是value

- map的插入方式：
  
  ```cpp
  //用insert函数插入pair数据，insert会创建临时对象
  mapStudent.insert(pair<int,string>(1,"student_1"));
  //用insert函数插入value_type数据
  mapStudent.insert(map<int,string>::value_type(1,"student_1"));
  //用insert函数中使用make_pair()函数
  mapStudent.insert(make_pair(1,"student_1"));
  //用数组方式插入数据,当key存在时，将值更新为目标值；key不存在时，新建元素
  mapStudent[1]="student_1";
  
  //emplace:直接调用构造函数，不会产生临时对象，效率高
  mapStudent.emplace(1,"student_1");
  ```

- unordered_map的底层实现是hash_table
45. 析构函数一般要写为虚函数

```cpp
class Parent {
public:
    Parent() {
        cout << "Parent construct function" << endl;
    }
    virtual ~Parent() {
        cout << "Parent destructor function" << endl;
    }
};
class Son : public Parent {
public:
    Son() {
        cout << "Son construct function" << endl;
    };
    ~Son() {
        cout << "Son destructor function" << endl;
    }
};
int main(){

    Parent* p = new Son();
    p->func();
    delete p;
    p = NULL;
    return 0;
}
//运行结果
//Parent construct function
//Son construct function
//Son destructor function
//Parent destructor function
```

如果构造函数为析构函数时，需要通过虚函数表(vtable)调用，可是对象还没有初始化，没有vptr指针

46. 模板和特例化

```cpp
template<typename T> //模板函数
int compare(const T &v1,const T &v2)
{
    if(v1 > v2) return -1;
    if(v2 > v1) return 1;
    return 0;
}
//模板特例化,满足针对字符串特定的比较，要提供所有实参，这里只有一个T
template<> 
int compare(const char* const &v1,const char* const &v2)
{
    return strcmp(p1,p2);
}
```

47. 虚函数

    虚函数的调用关系：this->vptr->vtable->virtual function

    纯虚函数:`virtual void fun()=0`;

48.string和其他数据类型的转换

- string转为其他数据类型时有成员函数`std::stoi`、`std::stol`、`std::stoll`

- 其他数据类型转为string类型时`to_string()`;
