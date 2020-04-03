## C 关键字 (C99) (32)

- 数据类型 (5)：`void, char, int, float, double`
- 类型修饰符 (4)：`signed, unsigned, short, long`
- 类型定义 (4)：`enum, struct, union, typedef`
- 类型限定 (2)：`const, volatile`
- 存储说明符 (4)：`auto, static, extern, register`
- 运算符 (1)：`sizeof`
- 分支结构 (5)： `if, else, switch, case, default`
- 循环控制 (3)：`for, while, do`
- 跳转控制 (4)：`break, continue, return, goto`

## C++ 关键字 (C++89/03) (63)

- 数据类型 (5+2)：`void, char, int, float, double, (bool, w_char)`
- 类型修饰符 (4)：`signed, unsigned, short, long`
- 类型定义 (4+1)：`enum, struct, union, typedef, (class)`
- 类型限定 (2)：`const, volatile`
- 存储说明符 (4)：`auto, static, extern, register`
- 分支结构 (5)：` if, else, switch, case, default`
- 循环控制 (3)：`for, while, do`
- 跳转控制 (4)：`break, continue, return, goto`
- 运算符 (1)：`sizeof`
- 常量值 (+2)：true, false
- 类型转换 (+4)：static_cast, dynamic_cast, const_cast, reinterpret_cast
- 类访问修饰符 (+3)：private, protected, public
- 访问限定符 (+6)：this, friend, virtual, explicit, operator, mutable
- 内联 (+2)：inline, asm
- 内存管理 (+2)：new, delete
- 模板 (+4)：template, typename, typeid, export
- 命名空间 (+2)：using, namespace
- 异常处理 (+3)：throw, try, catch

C++11 新增关键字 (+10)：`alignas, alignof, char16_t, char32_t, constexpr, decltype, noexcept, nullptr, static_assert, thread_local`

## const

```cpp
// const 常量，不可修改，必须初始化
const int a = 5; // int const a = 5;

// 对常量的引用
const int &r1 = 3;

// 修饰指针所指向对象的 const 称为底层 const
// 修饰指针本身的 const 称为顶层 const

// 指向常量的指针，指向内容不可变，指向可变，底层 const
const int *p = &a; // int const *p = &a;

// 常量指针，指向内容可变，指向不可变，顶层 const
int* const p = &a;

// 指向常量的常指针，指向内容不可变，指向也不可变
const int* const p = &a;


// 函数
void function1(const int var);  // 传递过来的参数在函数内不可变
void function2(const int &var); // 引用参数在函数内为常量
void function3(const int *var); // 参数指针所指内容为常量
void function4(int* const var); // 参数指针为常指针

const int i = a;
function2(i);
function3(&i);

// 函数返回值
const int function5();  // 返回一个常数
const int *function6(); // 返回一个指向常量的指针
int* const function7(); // 返回一个常指针

//常对象，成员常量，成员函数
class  A
{
public:
    A() : a(0) { };
    A(int x) : a(x) { };
    void fun1() { };
    void fun2() const { } // 常成员函数，不得修改类中的任何数据成员的值
private:
    const int a; // 常对象成员，只能在初始化列表赋值
};

const A aa;
aa.fun2(); // 正确
aa.fun1(); // 错误，const 的实例对象不能访问非 const 的函数
```

`const` 成员函数改变成员变量

- 用 mutable 修饰成员变量

    ```cpp
    class C
    {
    public:
        void func(const int &p) const
        {
            i = p;
        }
    private:
        mutable int i;
    };
    ```

- 造一个假的 this 去操作成员变量

    ```cpp
    class Class1
    {
    public:
        Class1();
        ~Class1();
        void func1() const;

        int _value;
    };

    void Class1::func1() const {
        // 声明一个指针指向 this 所指对象，并先将这个对象的常量性转型成const
        Class1* const fakeClass1 = const_cast<Class1* const>(this);
        // 使用造出来的 const 指针，去修改成员变量
        fakeClass1->_value = 1;
    }
    ```

## static

```cpp
// 静态局部变量：用于函数体内部修饰变量，这种变量的生存期长于该函数
int foo()
{
	static int i = 1; 
    int i = 1;
}

// 静态全局变量：定义在函数体外，用于修饰全局变量，表示该变量只在本文件可见
static int i = 1;
int i = 1;

int foo()
{
    i += 1;
    return i;
}

// 静态函数：静态函数跟静态全局变量的作用类似，定义该函数的文件内才能使用
static void fn()
{
	printf("this is static func in a.cpp");
}

// 静态成员变量，静态成员函数
class A
{
private:
    static int i;    
public:
    static void fun();
};

// 1.静态成员之间可以相互访问，包括静态成员函数访问静态数据成员和访问静态成员函数
// 2.非静态成员函数可以任意地访问静态成员函数和静态数据成员
// 3.静态成员函数不能访问非静态成员函数和非静态数据成员
// 4.调用静态成员函数，可以用成员访问操作符 (.) 和 (->) 为一个类的对象或指向类对象的指针调用静态成员函数，也可以用类名::函数名调用
```

## volatile

```cpp
volatile int i = 10; 
```

- volatile 关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素（操作系统、硬件、其它线程等）更改。所以使用 volatile 告诉编译器不应对这样的对象进行优化
- volatile 关键字声明的变量，每次访问时都必须从内存中取出值（没有被 volatile 修饰的变量，可能由于编译器的优化，从 CPU 寄存器中取值）
- const 可以是 volatile （如只读的状态寄存器）
- 指针可以是 volatile

## inline 内联函数

特征

- 相当于把内联函数里面的内容写在调用内联函数处
- 相当于不用执行进入函数的步骤，直接执行函数体
- 相当于宏，却比宏多了类型检查，真正具有函数特性
- 编译器一般不内联包含循环、递归、switch 等复杂操作的内联函数
- 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数

使用

```cpp
// 声明
inline int functionName(int first, int second,...);

// 定义
inline int functionName(int first, int second,...) {/****/};

// 类内定义，隐式内联
class A {
    int doA() { return 0; }         // 隐式内联
};

// 类外定义，需要显式内联
class A {
    int doA();
};
inline int A::doA() { return 0; }   // 需要显式内联
```

编译器对 inline 函数的处理步骤

1. 将 inline 函数体复制到 inline 函数调用点处
2. 为所用 inline 函数中的局部变量分配内存空间
3. 将 inline 函数的的输入参数和返回值映射到调用方法的局部变量空间中
4. 如果 inline 函数有多个返回点，将其转变为 inline 函数代码块末尾的分支（使用 goto）

优点

1. 内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度
2. 内联函数相比宏函数来说，在代码展开时，会做安全检查或自动类型转换（同普通函数），而宏定义则不会
3. 在类中声明同时定义的成员函数，自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义则不能
4. 内联函数在运行时可调试，而宏定义不可以

缺点

1. 代码膨胀。内联是以代码膨胀（复制）为代价，消除函数调用带来的开销。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间


2. inline 函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不像 non-inline 可以直接链接
3. 是否内联，程序员不可控。内联函数只是对编译器的建议，是否对函数内联，决定权在于编译器

虚函数 (virtual) 可以是内联函数 (inline) 吗？

- 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性的时候不能内联
- 内联是在编译器建编译器无法知道运行期调用哪个代码，因此虚函数表现编译器内联，而虚函数的多态性在运行期，为多态性时（运行期）不可以内联
- `inline virtual` 唯一可以内联的时候是：编译器知道所调用的对象是哪个类 (`Base::who()`)，这只有在编译器具有实际对象而不是对象的指针或引用时才会发生

虚函数内联使用

```cpp
#include <iostream>  
using namespace std;
class Base
{
public:
    inline virtual void who()
    {
        cout << "I am Base\n";
    }
    virtual ~Base() {}
};

class Derived : public Base
{
public:
    inline void who()  // 不写 inline 时隐式内联
    {
        cout << "I am Derived\n";
    }
};

int main()
{
    // 此处的虚函数 who()，是通过类 (Base) 的具体对象 (b) 来调用的，编译期间就能确定了，所以它可以是内联的，但最终是否内联取决于编译器
    Base b;
    b.who();

    // 此处的虚函数是通过指针调用的，呈现多态性，需要在运行时期间才能确定，所以不能为内联
    Base *ptr = new Derived();
    ptr->who();

    // 因为Base有虚析构函数 (virtual ~Base() {})，所以 delete 时，会先调用派生类 (Derived) 析构函数，再调用基类 (Base) 析构函数，防止内存泄漏
    delete ptr;
    ptr = nullptr;

    return 0;
} 
```

## 多态分类及实现

> - 重载多态（Ad-hoc Polymorphism，编译期）：函数重载、运算符重载
> - 子类型多态（Subtype Polymorphism，运行期）：虚函数
> - 参数多态性（Parametric Polymorphism，编译期）：类模板、函数模板
> - 强制多态（Coercion Polymorphism，编译期/运行期）：基本类型转换、自定义类型转换

静态多态（编译期/早绑定）

```cpp
// 函数重载
class A
{
public:
    void do(int a);
    void do(int a, int b);
};
```

动态多态（运行期/晚绑定）

- 虚函数：用 virtual 修饰成员函数，使其成为虚函数

**注意：**

- 普通函数（非类成员函数），静态函数 (static) 不能是虚函数
- 构造函数不能是虚函数（因为在调用构造函数时，虚表指针并没有在对象的内存空间中，必须要构造函数调用完成后才会形成虚表指针），析构函数可以是虚函数
- 内联函数不能是表现多态性时的虚函数
- 定义子类对象，并调用对象中未被子类覆盖的基类函数；指向子类对象的基类指针，并调用子类中的覆盖函数

动态多态使用

```cpp
class Shape                     // 形状类
{
public:
    virtual double calcArea();
    virtual ~Shape();
};
class Circle : public Shape     // 圆形类
{
public:
    virtual double calcArea();
};
class Rect : public Shape       // 矩形类
{
public:
    virtual double calcArea();
};
int main()
{
    Shape * shape1 = new Circle(4.0);
    Shape * shape2 = new Rect(5.0, 6.0);
    shape1->calcArea();         // 调用圆形类里面的方法
    shape2->calcArea();         // 调用矩形类里面的方法
    delete shape1;
    shape1 = nullptr;
    delete shape2;
    shape2 = nullptr;
    return 0;
}
```

## 虚函数

```cpp
virtual int A();     // 虚函数
virtual int A() = 0; // 纯虚函数，基类中不能有具体的实现，只能继承
```

区别

- 类里如果声明了虚函数，这个函数是实现的，哪怕是空实现，它的作用就是为了能让这个函数在它的子类里面可以被覆盖 (override)，这样的话，编译器就可以使用后期绑定来达到多态了；纯虚函数只是一个接口，是个函数的声明而已，它要留到子类里去实现
- 虚函数在子类里面可以不重写；但纯虚函数必须在子类实现才可以实例化子类
- 虚函数的类用于 “实作继承”，继承接口的同时也继承了父类的实现；纯虚函数关注的是接口的统一性，实现由子类完成
- 带纯虚函数的类叫抽象类，这种类不能直接生成对象，而只有被继承，并重写其虚函数后，才能使用。抽象类被继承后，子类可以继续是抽象类，也可以是普通类

虚函数指针、虚函数表

- 虚函数指针：在含有虚函数类的对象中，指向虚函数表，在运行时确定
- 虚函数表：在程序只读数据段，存放虚函数指针，如果派生类实现了基类的某个虚函数，则在虚表中覆盖原本基类的那个虚函数指针，在编译时根据类的声明创建

    相关链接：[虚函数的实现的基本原理](https://www.cnblogs.com/malecrab/p/5572730.html)

虚继承、虚基类

虚继承是解决 C++ 多重继承（菱形继承）问题的一种手段，从不同途径继承来的同一基类，会在子类中存在多份拷贝。这将存在两个问题：第一，浪费存储空间；第二，存在二义性问题。针对这种情况，C++ 提供 (virtual base class) 的方法，使得在继承间接共同基类时只保留一份成员。

```cpp
// 间接基类 A
class A
{
protected:
	int m_a;
};
// 直接基类 B
class B: virtual public A
{ // 虚继承
protected:
	int m_b;
};
// 直接基类 C
class C: virtual public A
{ // 虚继承
protected:
	int m_c;
};
// 派生类 D
class D: public B, public C
{
public:
    void seta(int a){ m_a = a; } // 正确，不加 virtual 则错误
    void setb(int b){ m_b = b; } // 正确
    void setc(int c){ m_c = c; } // 正确
    void setd(int d){ m_d = d; } // 正确
private:
	int m_d;
};
int main(){
    D d;
    return 0;
}
```

## 覆盖与隐藏

- 覆盖指的是子类覆盖父类函数（被覆盖），特征：
  - 分别位于子类和父类中
  - 函数名字与参数都相同
  - 父类的函数是虚函数 (virtual)

- 隐藏指的是子类隐藏了父类的函数（还存在），特征：
  - 子类的函数与父类的名称相同，但是参数不同，父类函数被隐藏
  - 子类函数与父类函数的名称相同，参数也相同，但是父类函数没有 virtual，父类函数被隐藏

```cpp
class father
{
public:
    void show1()
    {
        cout << "father::show1" << endl;
    }

    virtual void show2()
    {
        cout << "father::show2" << endl;
    }
};

class son:public father
{
public:
    void show1()
    {
        cout << "son::show1" << endl;
    }

    virtual void show2()
    {
        cout << "son::show2" << endl;
    }
};

int main()
{　
    father f;　
    son s;　
    father *pf=&s;　
    son *ps=&s;　　
    pf->show1();　// father::show1
    pf->show2();　// son::show2　
    return 0;
}
```

## 模板

```cpp
// 函数模板
template <class T>
void swap(T& a, T& b){}

// 类模板
template<class T>
class A
{
public: 
	T a;
    T b; 
    T hy(T c, T &d);
};
```

## 强制类型转换

1. `static_cast`

    ```cpp
    // 数值类型转型，char<->int
    char a = 'a';
    int b = static_cast<char>(a);

    // 指针转 void* 
    double *c = new double;
    void *d = static_cast<void*>(c);

    // const转换，int->const int
    int e = 10;
    const int f = static_cast<const int>(e);

    // 除了底层const都可以
    const int g = 20;
    int *h = static_cast<int*>(&g); // 编译错误，static_cast 不能转换掉 g 的 const 属性

    // 类继承转换
    class Base{};

    class Derived : public Base{};

    Base* pB = new Base();
    Derived* pD = static_cast<Derived*>(pB) // 下行转换是不安全的

    Derived* pD = new Derived();
    Base* pB = static_cast<Base*>(pD) // 上行转换是安全的
        
    // 不执行运行时类型检查（转换安全性不如dynamic_cast）
    ```

2. `dynamic_cast`

    ```cpp
    ...
    Derived* pD = dynamic_cast<Derived *>(pB);

    void f(const Base &b){
        try{
            const Derived &d = dynamic_cast<const Base &>(b);  
            // 使用 b 引用的 Derived 对象
        }
        catch(std::bad_cast){
            // 处理类型转换失败的情况
        }
    }
    // 用于多态类型的转换，可以在整个类层次结构中移动指针，包括向上转换、向下转换
    // 执行行运行时类型检查
    // 只适用于指针或引用
    // 对不明确的指针的转换将失败（返回 nullptr ），但不引发异常
    ```

3. `const_cast`

    ```cpp
    // 去除const限定
    #include <iostream>
    using namespace std; 
    void Printer (int* val,string seperator = "\n")
    {   
        cout << val<< seperator;
    } 
    int main(void) {     
        const int consatant = 20; 
        // Printer(consatant); // Error: invalid conversion from 'int' to 'int*'
        Printer(const_cast<int *>(&consatant)); 
        return 0;
    }

    #include <iostream>
    using namespace std; 
    int main(void) 
    {    
        int variable = 21;
        const int *const_p = &variable;
        int *modifier = const_cast<int *>(const_p);
        *modifier = 7;
        cout << "variable:" << variable << endl; // variable:7
        return 0;
    }
    ```

4. `reinterpret_cast`

   几乎什么都可以转，比如将`int`转指针，可能会出问题，尽量少用

5. 为什么不使用 C 的强制转换？

   C 的强制转换表面上看起来功能强大什么都能转，但是转化不够明确，不能进行错误检查，容易出错。

## 智能指针

1. `shared_ptr`

   多个智能指针可以共享同一个对象，对象的最末一个拥有着有责任销毁对象，并清理与该对象相关的所有资源。用对象去管理了一个资源指针，同时用一个计数器去计算当前指针引用对象的个数；当管理指针的对象增加或者减少时，计算器当前值也同步加 1 或减 1 ，当最后一个指针对象被销毁时，计算器为1；此时，最后的指针对象被销毁（计算器抵达 0 ）的同时也把指针管理对象的指针进行 delete 操作

    ```cpp
    template <typename T>
    class Shared_ptr
    {
    private:
        size_t* m_count;
        T* m_ptr;

    public:
        // 构造函数
        Shared_ptr() : m_ptr(nullptr), m_count(new size_t) {}

        Shared_ptr(T *ptr) : m_ptr(ptr), m_count(new size_t)
        {
            cout << "空间申请：" << ptr << endl;
            *m_count = 1;
        }

        // 析构函数
        ~Shared_ptr()
        {
            --(*m_count);
            if (*m_count == 0)
            {
            cout << "空间释放：" << m_ptr << endl;
            delete m_ptr;
            delete m_count;
            m_ptr = nullptr;
            m_count = nullptr;
            }
        }

        // 拷贝构造函数
        Shared_ptr(const Shared_ptr &ptr)
        {
            m_count = ptr.m_count;
            m_ptr = ptr.m_ptr;
            ++(*m_count);
        }

        // 拷贝赋值运算符
        void operator=(const Shared_ptr &ptr)
        {
            Shared_ptr(std::move(ptr));
        }

        // 移动构造函数
        Shared_ptr(Shared_ptr &&ptr) : m_ptr(ptr.m_ptr), m_count(ptr.m_count)
        {
            ++(*m_count);
        }

        // 移动赋值运算符
        void operator=(Shared_ptr &&ptr)
        {
            Shared_ptr(std::move(ptr));
        }

        // 解引用运算符
        T &operator*()
        {
            return *m_ptr;
        }

        // 箭头运算符
        T *operator->()
        {
            return m_ptr;
        }

        // 重载布尔值操作
        operator bool()
        {
            return m_ptr == nullptr;
        }

        // 返回存储的指针
        T *get()
        {
            return m_ptr;
        }
        // 返回 shared_ptr 所指对象的引用计数
        size_t use_count()
        {
            return *m_count;
        }
        // 检查所管理对象是否仅由当前 shared_ptr 的实例管理
        bool unique()
        {
            return *m_count == 1;
        }
        // 交换所管理的对象
        void swap(Shared_ptr &ptr)
        {
            std::swap(*this, ptr);
        }
    };
    ```

    ```cpp
    #include <iostream>
    #include <memory>

    class Test
    {
    public:
        Test()
        {
            std::cout << "Test()" << std::endl;
        }

        ~Test()
        {
            std::cout << "~Test()" << std::endl;
        }
    };

    int main()
    {
        std::shared_ptr<Test> p1 = std::make_shared<Test>();
        std::cout << "1 ref:" << p1.use_count() << std::endl;
        {

            std::shared_ptr<Test> p2 = p1;

            std::cout << "2 ref:" << p1.use_count() << std::endl;

            std::shared_ptr<Test> p3 = p1;

            std::cout << "3 ref:" << p1.use_count() << std::endl;
        }

        std::cout << "4 ref:" << p1.use_count() << std::endl;
        return 0;
    }

    // 1 ref:1
    // 2 ref:2
    // 3 ref:3
    // 4 ref:1 
    ```

    ```cpp
    #include <iostream>
    #include <memory>
    class B;

    class A
    {
    public:
        A()
        {
            std::cout << "class A : constructor" << std::endl;
        }

        ~A()
        {
            std::cout << "class A : destructor" << std::endl;
        }

        void referB(std::shared_ptr<B> test_ptr)
        {
            _B_Ptr = test_ptr;
        }

    private:
        std::shared_ptr<B> _B_Ptr;
    };

    class B
    {
    public:
        B()
        {
            std::cout << "class B : constructor" << std::endl;
        }

        ~B()
        {
            std::cout << "class B : destructor" << std::endl;
        }

        void referA(std::shared_ptr<A> test_ptr)
        {
            _A_Ptr = test_ptr;
        }
        std::shared_ptr<A> _A_Ptr;
    };

    int main()
    {
        // test
        {
            std::shared_ptr<A> ptr_a = std::make_shared<A>(); // A 引用计算器为1
            std::shared_ptr<B> ptr_b = std::make_shared<B>(); // B 引用计算器为1

            ptr_a->referB(ptr_b); // B 引用计算器加1
            ptr_b->referA(ptr_a); // A 引用计算器加1
        }
        
        return 0;
    }

    // class A : constructor
    // class B : constructor
    // 没有调用 A,B 析构函数，相互引用导致了相互释放冲突的问题，最终导致内存泄露发生
    // 利用 std::weak_ptr 就可以解决以上问题
    ```

    ```cpp
    #include <iostream>
    #include <memory>

    class B;

    class A
    {
    public:
        A()
        {
            std::cout << "class A : constructor" << std::endl;
        }

        ~A()
        {
            std::cout << "class A : destructor" << std::endl;
        }

        void referB(std::shared_ptr<B> test_ptr)
        {
            _B_Ptr = test_ptr;
        }

        void print_refer()
        {
            std::cout << "refer B count : " << _B_Ptr.use_count() << std::endl;
        }

        void test_refer()
        {
            std::shared_ptr<B> tem_p = _B_Ptr.lock();
            std::cout << "refer B : " << tem_p.use_count() << std::endl;
        }

    private:
        std::weak_ptr<B> _B_Ptr;
    };

    class B
    {
    public:
        B()
        {
            std::cout << "class B : constructor" << std::endl;
        }

        ~B()
        {
            std::cout << "class B : destructor" << std::endl;
        }

        void referA(std::shared_ptr<A> test_ptr)
        {
            _A_Ptr = test_ptr;
        }

        void print_refer()
        {
            std::cout << "refer A count : " << _A_Ptr.use_count() << std::endl;
        }

        void test_refer()
        {
            std::shared_ptr<A> tem_p = _A_Ptr.lock();
            std::cout << "refer A : " << tem_p.use_count() << std::endl;
        }

        std::weak_ptr<A> _A_Ptr;
    };

    int main()
    {
        // test
        {
            std::shared_ptr<A> ptr_a = std::make_shared<A>(); // A 引用计算器为1
            std::shared_ptr<B> ptr_b = std::make_shared<B>(); // B 引用计算器为1

            ptr_a->referB(ptr_b);
            ptr_b->referA(ptr_a);
            ptr_a->test_refer();
            ptr_b->test_refer();
            ptr_a->print_refer();
            ptr_b->print_refer();
        }

        return 0;
    }
    // class A : constructor
    // class B : constructor
    // refer B : 2
    // refer A : 2
    // refer B count : 1
    // refer A count : 1
    // class B : destructor
    // class A : destructor
    // ptr_a 和 ptr_b 在 main 退出前，引用计算均为1，也就是说在 A 和 B 中对 std::weak_ptr 的引用不会引起引用计算加1，ptr_a 和 ptr_b 可以正常释放，不会引起内存泄露

    // std::weak_ptr有以下特性：
    // 1. 只是拥有一个没有拥有的被 std::shared_ptr 托管的对象
    // 2. 只有调用 lock() 创建 std::shared_ptr 时才会引用对象
    // 3. 在 lock() 时会递增一个引用计算
    // 4. 在 std::shared_ptr 主指针结束后，如果 std::weak_ptr 的 lock 成功对象还存在，那么此时还有代码调用 lock 的话，也会引起引用计算加 1
    ```

2. `weak_ptr`

   `weak_ptr` 允许你共享但不拥有某对象，一旦最末一个拥有该对象的智能指针失去了所有权，任何 `weak_ptr` 都会自动成空。因此，在 default 和 copy 构造函数之外，`weak_ptr` 只提供 “接受一个 `shared_ptr` 的构造函数。

3. `unique_ptr`

   `unique_ptr` 是一种在异常时可以帮助避免资源泄漏的智能指针。采用独占式拥有，意味着可以确保一个对象和其相应的资源同一时间只被一个 pointer 拥有。一旦被销毁或 empty，或开始拥有另一个对象，先前拥有的那个对象就会被销毁，其任何相应资源亦会被释放。

   `unique_ptr` 用于取代 `auto_ptr`

    ```cpp
    int main(int argc, char *argv[])
    {
        std::unique_ptr<int> u1(new int(1));
        std::cout << "u1 value : " << *u1 << '\n'
                << "addredd : " << u1.get() << std::endl;
        std::unique_ptr<int> u2 = move(u1);
        std::cout << "u2 value : " << *u2 << '\n'
                << "addredd : " << u2.get() << std::endl;
        std::cout << "u1 value : " << *u1 << '\n'
                << "addredd : " << u1.get() << std::endl;
        return 0;
    }

    // u1 value : 1
    // addredd : 0xea1620
    // u2 value : 1
    // addredd : 0xea1620
    // u1 value : error
    ```

4. `auto_ptr`

   实现对动态分配对象的自动释放
   
   被 C++11 弃用，原因是缺乏语言特性如 “针对构造和赋值” 的 `std::move` 语义，以及其他瑕疵。

## 数组和指针的区别

|                             指针                             |                 数组                 |
| :----------------------------------------------------------: | :----------------------------------: |
|                        保存数据的地址                        |               保存数据               |
| 间接访问数据，首先获得指针的内容，然后将其作为地址，从该地址中提取数据 |             直接访问数据             |
|                    通常用于动态的数据结构                    | 通常用于固定数目且数据类型相同的元素 |
|               通过malloc分配内存，free释放内存               |           隐式的分配和删除           |
|                通常指向匿名数据，操作匿名函数                |            自身即为数据名            |

## 字符串常量和字符串变量

```cpp
string s1 = "abc"; // 字符串变量 
char* s2 = "abc";  // 字符串常量，先在常量区存储 "abc"，s2 直接指向常量区的 "abc" 
char s3[] = "abc"; // 字符数组，先在常量区存储 "abc"，然后在栈区申请内存空间，将 "abc" 复制过来，s3 指向栈区的 "abc"

s1[0] = 'q';       // 正确，
s2[0] = 'q';       // 错误，s2 所指向内容是常量，不能修改 
s3[0] = 'q';       // 正确 

s1 = "abcd";       // 正确 
s2 = "abcd";       // 正确，s2 指向另外一个字符串常量 
s3 = "abcd";       // 错误，s3 是数组名，相当于指针常量，指向不能改变 
```

## 指针和引用的区别

1. 指针有自己的一块空间，而引用只是一个别名
2. 使用 `sizeof` 看一个指针的大小是4，而引用则是被引用对象的大小
3. 指针可以被初始化为NULL，而引用必须被初始化且必须是一个已有对象的引用
4. 作为参数传递时，指针需要被解引用才可以对对象进行操作，而直接对引用的修改都会改变引用所指向的对象
5. 可以有 `const` 指针，但是没有 `const` 引用
6. 指针在使用中可以指向其它对象，但是引用只能是一个对象的引用，不能被改变
7. 指针可以有多级指针 `(**p)` ，而引用至于一级
8. 指针和引用使用 ++ 运算符的意义不一样
9. 如果返回动态内存分配的对象或者内存，必须使用指针，引用可能引起内存泄露

    ```cpp
    int i = 1 ;
    int &ref = i;  // 引用必须初始化，初始值为对象
    int *p = &i;
    ```

## 类成员的访问权限

水平权限控制

|            | private | protected | public |
| :--------: | :-----: | :-------: | :----: |
| 内部可见性 |  可见   |   可见    |  可见  |
| 外部可见性 | 不可见  |  不可见   |  可见  |

垂直访问控制

| 基类属性   | private | protected | public | private | protected | public | private | protected | public |
| :--------: | :-----: | :-------: | :----: | :-----: | :-------: | :----: | :-----: | :-------: | :----: |
| 继承方式   | private | private | private | protected | protected | protected | public |public|public|
| 内部可见性 | 不可见 | private | private | 不可见 | protected | protected | 不可见 | protected | public |
| 外部可见性 | 不可见 | 不可见 | 不可见 | 不可见 | 不可见 | 不可见 | 不可见 | 不可见 | 可见 |

## struct 和 class 的区别

在 C 中，`struct` 不能包含任何函数

在 C++ 中，`struct` 可以包括成员函数、可以实现继承、可以实现多态

- 默认的继承访问权。`class` 默认的是 `private`，`struct` 默认的是 `public`
- 默认访问权限。`struct` 作为数据结构的实现体，它默认的数据访问控制是 `public`；而 `class` 作为对象的实现体，它默认的成员变量访问控制是 `private` 
- `class` 这个关键字还用于定义模板参数，就像 `typename`。但关键字 `struct` 不用于定义模板参数
- `class` 和 `struct` 在使用大括号`{ }`上的区别

  关于使用大括号初始化

  - `class` 和 `struct` 如果定义了构造函数的话，都不能用大括号进行初始化
  - 如果没有定义构造函数，`struct` 可以用大括号初始化
  - 如果没有定义构造函数，且所有成员变量全是 `public` 的话，`class` 可以用大括号初始化

## extern "C" 的作用

extern "C" 的主要作用就是为了能够正确实现 C++ 代码调用其他 C 语言代码。加上extern "C" 后，会指示编译器这部分代码按 C 语言（而不是 C++ ）的方式进行编译。由于 C++ 支持函数重载，因此编译器编译函数的过程中会将函数的参数类型也加到编译后的代码中，而不仅仅是函数名；而 C 语言并不支持函数重载，因此编译 C 语言代码的函数时不会带上函数的参数类型，一般只包括函数名。

## C++ 从源文件到可执行文件的过程

- 预处理阶段：对源代码文件中文件包含关系（头文件）、预编译语句（宏定义）进行分析和替换，生成预编译文件
- 编译阶段：将经过预处理后的预编译文件转换成特定汇编代码，生成汇编文件
- 汇编阶段：将编译阶段生成的汇编文件转化成机器码，生成可重定位目标文件
- 链接阶段：将多个目标文件及所需要的库连接成最终的可执行目标文件

<img src=".\img\编译系统.png" style="zoom: 50%;" />

<img src=".\img\cpp编译过程.jpg" style="zoom: 50%;" />

目标文件 (.o) 存储结构

| 段                      | 功能                                                         |
| ----------------------- | ------------------------------------------------------------ |
| File Header             | 文件头，描述整个文件的文件属性（包括文件是否可执行、是静态链接或动态连接及入口地址、目标硬件、目标操作系统等） |
| .text section           | 代码段，执行语句编译成的机器代码                             |
| .data section           | 数据段，已初始化的全局变量和局部静态变量                     |
| .bss section            | BSS 段(Block Started by Symbol)，未初始化的全局变量和局部静态变量（因为默认值为 0，所以只是在此预留位置，不占空间） |
| .rodata section         | 只读数据段，存放只读数据，一般是程序里面的只读变量（如 const 修饰的变量）和字符串常量 |
| .comment section        | 注释信息段，存放编译器版本信息                               |
| .note.GNU-stack section | 堆栈提示段                                                   |

## 内存管理和分配

在 C++ 中，虚拟内存分为代码段、数据段、BSS 段、堆区、文件映射区以及栈区六部分

- 静态区域：
  - 代码段：包括只读存储区和文本区，其中只读存储区存储字符串常量，文本区存储程序的机器代码
  - 数据段：存储程序中已初始化的全局变量和静态变量
  - bss 段：存储未初始化的全局变量和静态变量（局部+全局），以及所有被初始化为 0 的全局变量和静态变量
- 动态区域：
  - 堆区：调用 `new/malloc` 函数时在堆区动态分配内存，同时需要调用 `delete/free` 来手动释放申请的内存
  - 映射区：存储动态链接库以及调用 mmap 函数进行的文件映射
  - 栈：使用栈空间存储函数的返回地址、参数、局部变量、返回值

## include 头文件的顺序以及双引号 "" 和尖括号 <> 的区别

- include头 文件的顺序：对于 include 的头文件来说，如果在文件 `a.h` 中声明一个在文件 `b.h` 中定义的变量，而不引用 `b.h`。那么要在 `a.c` 文件中引用 `b.h` 文件，并且要先引用 `b.h`，后引用 `a.h`，否则汇报变量类型未声明错误。

- 双引号和尖括号的区别：编译器预处理阶段查找头文件的路径不一样。
  - 对于使用双引号包含的头文件，查找头文件路径的顺序为：
    1. 当前头文件目录
    2. 编译器设置的头文件路径（编译器可使用-I显式指定搜索路径）
    3. 系统变量 CPLUS_INCLUDE_PATH/C_INCLUDE_PATH 指定的头文件路径
  - 对于使用尖括号包含的头文件，查找头文件的路径顺序为：
    1. 编译器设置的头文件路径（编译器可使用-I显式指定搜索路径）
    2. 系统变量 CPLUS_INCLUDE_PATH/C_INCLUDE_PATH 指定的头文件路径

## new 和 malloc 的区别

- new 分配内存按照数据类型进行分配，malloc 分配内存按照指定的大小分配
- new 返回的是指定对象的指针，而 malloc 返回的是 void*，因此 malloc 的返回值一般都需要进行类型转化
- new 不仅分配一段内存，而且会调用构造函数，malloc 不会
- new 分配的内存要用 delete 销毁，malloc 要用 free 来销毁；delete 销毁的时候会调用对象的析构函数，而 free 则不会
- new 是一个操作符可以重载，malloc 是一个库函数
- malloc 分配的内存不够的时候，可以用 `realloc` 扩容。new 没用这样操作
- new 如果分配失败了会抛出 bad_malloc 的异常，而 malloc 失败了会返回 NULL
- 申请数组时：new[ ] 一次分配所有内存，多次调用构造函数，搭配使用 delete[ ]，delete[ ] 多次调用析构函数，销毁数组中的每个对象。而 malloc 则只能 `sizeof(int) * n`

## C++11 新特性

- 类型推导

  - `auto`关键字，编译器可以根据初始值自动推导出类型，但是不能用于函数传参以及数组类型的推导

    ```cpp
    // 不使用auto需要写很长的迭代器的类型
    map<string, string> m;
    map<string, string>::iterator it1 = m.begin();
    // 使用auto就很简单
    auto it2 = m.begin();
    ```

  - `decltype` 关键字是为了解决 `auto` 关键字只能对变量进行类型推导的缺陷而出现的。它的用法和 `sizeof` 很相似

    ```cpp
    // 推演表达式作为变量的定义类型
    int a = 1, b = 2;
    decltype(a + b) c;
    cout << typeid(c).name() << endl;

    // 推演函数的返回值类型
    void GetMemory(size_t size)
    {
        return malloc(size);
    }
    cout << typeid(decltype(GetMemory)).name() << endl;
    ```

- `nullptr`：`nullptr` 是一种特殊类型的字面值，它可以被转换成任意其它的指针类型；而 NULL 一般被宏定义为 0，在遇到重载时可能会出现问题

- 区间迭代

    ```cpp
    // & 启用了引用
    for(auto &i : arr) {    
        cout << i << endl;
    }
    ```
  
- 智能指针

- 初始化列表：使用初始化列表来对类进行初始化

    ```cpp
    // 内置类型
    int x1 = {10};
    int x2 { 10 }
    // 数组
    int arr1[5] { 1, 2, 3, 4, 5 }
    int arr2[]{1, 2, 3, 4, 5};
    // 标准容器
    vector<int> v{1, 2, 3} map<int, int> m
    {
        {1, 1}, { 2, 2 }
    }
    // 自定义类型
    class Point
    {
        int x;
        int y;
    } Power p{1, 2};
    ```

- 右值引用：基于右值引用可以实现移动语义和完美转发，消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率

- override 和 final：override 确保在派生类中声明的重载函数跟基类的虚函数有相同的签名；final 阻止类的进一步派生和虚函数的进一步重载

    ```cpp
    struct A
    {
        virtual void func() const;
    };
    struct B : A
    {
        void func() const override final; //OK
    };
    struct C : B
    {
        void func() const; //error, B::func is final
    };
    ```

- 新增 STL 顺序容器 array, forward_list，无序关联容器 unordered_map, unordered_multimap, unordered_set, unordered_multiset，元组 tuple

- Lambda 表达式

- 线程库支持

## 右值引用

- 左值：能对表达式取地址、或具名对象/变量。一般指表达式结束后依然存在的持久对象。

- 右值：不能对表达式取地址，或匿名对象。一般指表达式结束就不再存在的临时对象。

当一个对象被用作右值的时候，用的是对象的值（内容）；当对象被用作左值的时候，用的是对象的身份（在内存中的位置）。左值有持久的状态，而右值要么是字面常量，要么是在表达式求值过程中创建的对象，即左值持久，右值短暂

```cpp
int add()
{
	return 0;
}										// 返回常量 0

int fun(int&& f)						// 形参为右值引用
{
	cout << f << endl;
	return 1;
}										// 返回常量 1
int main(void)
{
	'a';								// 常量 'a'
	3;									// 常量  3
	1 + 3;								// 寄存器量 1+3 结果 4 

	//使用右值引用

	char && Char_a = 'a';
	cout << "'a':" << Char_a << endl;

	int && Int_a = 3;
	cout << "3:" << Int_a << endl;

	int && Int_b = 1 + 3;
	cout << "1+3:" << Int_b << endl;

	int &&Int_c = add();
	cout << "add()" << Int_c << endl;

	int && Int_d = fun(3);    // 输出 3
	cout << "fun()" << Int_d << endl;
	
	return 0；
}
```

区别：

- 左值可以寻址，而右值不可以
- 左值可以被赋值，右值不可以被赋值，可以用来给左值赋值
- 左值可变，右值不可变（仅对基础类型适用，用户自定义类型右值引用可以通过成员函数改变）

C++11 中，`std::move()` 函数位于头文件中，它可以把一个左值强制转化为右值引用，通过右值引用使用该值，实现移动语义。该转化不会对左值产生影响。

注意：其更多用在生命周期即将结束的对象上。

## lambda 表达式

```cpp
[capture list] (params list) mutable exception-> return type { function body }

auto f = [] (int a, int b) -> int
{
        return a + b + 42;
};
```

## STL基本组成

- 容器
- 算法
- 迭代器
- 函数对象

## STL 容器

|     类别     |        容器        |   底层数据结构    |                         时间复杂度                         | 有无序 | 可不可重复 |                             其他                             |
| :----------: | :----------------: | :---------------: | :--------------------------------------------------------: | :----: | :--------: | :----------------------------------------------------------: |
|   顺序容器   |       array        |       数组        |                       随机读改 O(1)                        |  无序  |   可重复   |                         支持随机访问                         |
|              |       vector       |       数组        | 随机读改、尾部插入、尾部删除 O(1)，头部插入、头部删除 O(n) |  无序  |   可重复   |                         支持随机访问                         |
|              |       deque        |     双端队列      |                  头尾插入、头尾删除 O(1)                   |  无序  |   可重复   | 一个中央控制器 + 多个缓冲区，支持首尾快速增删，支持随机访问  |
|              |    forward_list    |     单向链表      |                      插入、删除 O(1)                       |  无序  |   可重复   |                        不支持随机访问                        |
|              |        list        |     双向链表      |                      插入、删除 O(1)                       |  无序  |   可重复   |                        不支持随机访问                        |
|  容器适配器  |       stack        |   deque / list    |                  顶部插入、顶部删除 O(1)                   |  无序  |   可重复   | deque 或 list 封闭头端开口，不用 vector 的原因应该是容量大小有限制，扩容耗时 |
|              |       queue        |   deque / list    |                  尾部插入、头部删除 O(1)                   |  无序  |   可重复   | deque 或 list 封闭头端开口，不用 vector 的原因应该是容量大小有限制，扩容耗时 |
|              |   priority_queue   | vector + max-heap |                   插入、删除 O(logn)                   |  有序  |   可重复   |                   vector容器+heap处理规则                    |
|   关联容器   |        set         |      红黑树       |                插入、删除、查找 O(logn)                |  有序  |  不可重复  |                                                              |
|              |      multiset      |      红黑树       |                插入、删除、查找 O(logn)                |  有序  |   可重复   |                                                              |
|              |        map         |      红黑树       |                插入、删除、查找 O(logn)                |  有序  |  不可重复  |                                                              |
|              |      multimap      |      红黑树       |                插入、删除、查找 O(logn)                |  有序  |   可重复   |                                                              |
| 无序关联容器 |   unordered_set    |      哈希表       |             插入、删除、查找 O(1) ，最差 O(n)              |  无序  |  不可重复  |                                                              |
|              | unordered_multiset |      哈希表       |             插入、删除、查找 O(1)， 最差 O(n)              |  无序  |   可重复   |                                                              |
|              |   unordered_map    |      哈希表       |             插入、删除、查找 O(1) ，最差 O(n)              |  无序  |  不可重复  |                                                              |
|              | unordered_multimap |      哈希表       |             插入、删除、查找 O(1) ，最差 O(n)              |  无序  |   可重复   |                                                              |

## vector和list的区别

1. 概念：

   - vector

     连续存储的容器，动态数组，在堆上分配空间

     底层实现：数组

     两倍容量增长：vector 增加（插入）新元素时，如果未超过当时的容量，则还有剩余空间，那么直接添加到最后（插入指定位置），然后调整迭代器。如果没有剩余空间了，则会重新配置原有元素个数的两倍空间，然后将原空间元素通过复制的方式初始化新空间，再向新空间增加元素，最后析构并释放原空间，之前的迭代器会失效。

     性能：

     - 访问：随机读改、尾部插入、尾部删除 O(1)，头部插入、头部删除 O(n)，支持随机访问

     - 插入：
       - 在最后插入（空间够）：很快

       - 在最后插入（空间不够）：需要内存申请和释放，以及对之前数据进行拷贝。
       - 在中间插入（空间够）：内存拷贝
       - 在中间插入（空间不够）：需要内存申请和释放，以及对之前数据进行拷贝。

     - 删除：
       - 在最后删除：很快
       - 在中间删除：内存拷贝

     适用场景：经常随机访问，且不经常对非尾节点进行插入删除。

   - list

     动态链表，在堆上分配空间，每插入一个元数都会分配空间，每删除一个元素都会释放空间。

     底层：双向链表

     性能：

     - 访问：插入、删除 O(1)，随机访问性能很差，只能快速访问头尾节点
     - 插入：很快，一般是常数开销
     - 删除：很快，一般是常数开销

     适用场景：经常插入删除大量数据

2. 区别：

   - vector 底层实现是数组；list 是双向链表
   - vector 支持随机访问，list 不支持
   - vector 是顺序内存，list 不是
   - vector 在中间节点进行插入删除会导致内存拷贝，list 不会
   - vector 一次性分配好内存，不够时才进行 2 倍扩容；list 每次插入新节点都会进行内存申请
   - vector 随机访问性能好，插入删除性能差；list 随机访问性能差，插入删除性能好

3. 应用
   - vector 拥有一段连续的内存空间，因此支持随机访问，如果需要高效的随即访问，而不在乎插入和删除的效率，使用 vector
   - list 拥有一段不连续的内存空间，如果需要高效的插入和删除，而不关心随机访问，则应使用 list

## vector 扩容原理

- 新增元素：vector 通过一个连续的数组存放元素，如果集合已满，在新增数据的时候，就要分配一块更大的内存，将原来的数据复制过来，释放之前的内存，在插入新增的元素
- 对 vector 的任何操作，一旦引起空间重新配置，指向原 vector 的所有迭代器就都失效了
- 初始时刻 vector 的 capacity 为 0，塞入第一个元素后 capacity 增加为 1
- 不同的编译器实现的扩容方式不一样，VS2015 中以 1.5 倍扩容，GCC 以 2 倍扩容。

`~vector`

销毁 `vector`。调用元素的析构函数，然后解分配所用的存储。若元素是指针，则不销毁所指向的对象。

## map 和 set 区别

map 和 set 都是 C++ 的关联容器，其底层实现都是红黑树 (RB-Tree)。由于 map 和 set 所开放的各种操作接口，RB-tree 也都提供了，所以几乎所有的 map 和 set 的操作行为，都只是转调 RB-tree 的操作行为。

区别：

- map 中的元素是 key-value 键值对：键起到索引的作用，值则表示与索引相关联的数据；set 与之相对就是键的简单集合，set 中每个元素只包含一个键
- set 的迭代器是 `const` 的，不允许修改元素的值；map 允许修改 value，但不允许修改 key。其原因是因为 map 和 set 是根据关键字排序来保证其有序性的，如果允许修改 key 的话，那么首先需要删除该键，然后调节平衡，再插入修改后的键值，调节平衡，如此一来，严重破坏了 map 和 set 的结构，导致 iterator 失效，不知道应该指向改变前的位置，还是指向改变后的位置。所以 STL 中将 set 的迭代器设置成 `const`，不允许修改迭代器的值；而 map 的迭代器则不允许修改 key 值，允许修改 value 值。
- map 支持下标操作，set 不支持下标操作。map 可以用 key 做下标，map 的下标运算符 [ ] 将关键码作为下标去执行查找，如果关键码不存在，则插入一个具有该关键码和 mapped_type 类型默认值的元素至map中，因此下标运算符 [ ] 在 map 应用中需要慎用，`const_map` 不能用，只希望确定某一个关键值是否存在而不希望插入元素时也不应该使用，mapped_type 类型没有默认值也不应该使用。如果 find 能解决需要，尽可能用 find。

## 哈希结构

哈希结构

- 定义哈希函数 hashfunc 使元素的存储位置与它的关键码之间建立一对一映射的关系
- 在插入元素时，根据待插入元素的关键码，以 hashfunc 计算出该元素的存储位置，在结构中按此位置进行存放
- 在搜索元素时，对关键码进行同样的计算，把求得的函数值当做元素的存储位置，在结构中按此位置去元素比较，若关键码相等，则搜索成功

哈希冲突

​	不同关键字通过哈希函数计算出相同的哈希地址

哈希函数

- 直接定制法
  
取关键字的某个线性函数为散列地址: hash (key) = A * Key + B
  
优点: 简单,均匀,缺点: 需要提前知道关键字的分布情况
  
使用场景:适合查找小且连续的场景
  
- 除留余数法

  设散列表中允许的地址数为 m，取一个不大于 m，但最接近或者等于 m 的质数 p（尽量选择一个素数）作为除数，按照哈希函数：Hash(key) = key % p (p <= m)，将关键码转换成哈希地址

哈希冲突解决

- 闭散列

  开放定址法，当哈希表未满，在插入同义字时。可以把 key 值存放在下一个空位置(线性探测)
  线性探测：从发生冲突的位置开始，依次向后探测，直到寻找下一个空位置为止。

- 开散列

  开散列法又叫链地址法(开链法)，首先对关键码集合用散列函数计算散列地址，具有相同地址的关键码归于同一子集合，每一个子集合称为一个桶，各个桶中的元素通过一个单链表链接起来，各链表的头结点存储在哈希表中。

## allocator

STL的分配器用于封装STL容器在内存管理上的底层细节

在 C++ 中，其内存配置和释放如下：

- new运算分两个阶段：
    1. 调用 ::operator new 配置内存
    2. 调用对象构造函数构造对象内容

- delete运算分两个阶段：
    1. 调用对象析构函数
    2. 调用 ::operator delete 释放内存

为了精密分工，STL allocator 将两个阶段操作区分开来：

- 内存配置有 alloc::allocate() 负责，内存释放由 alloc::deallocate() 负责
- 对象构造由 ::construct() 负责，对象析构由 ::destroy() 负责

同时为了提升内存管理的效率，减少申请小内存造成的内存碎片问题，SGI STL 采用了两级配置器，当分配的空间大小超过 128B 时，会使用第一级空间配置器；当分配的空间大小小于 128B 时，将使用第二级空间配置器。第一级空间配置器直接使用 malloc()、realloc()、free() 函数进行内存空间的分配和释放，而第二级空间配置器采用了内存池技术，通过空闲链表来管理内存。

## 迭代器和指针的区别

1. 迭代器

   iterator（迭代器）模式又称 Cursor（游标）模式，用于提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。或者这样说可能更容易理解：iterator 模式是运用于聚合对象的一种模式，通过运用该模式，使得我们可以在不知道对象内部表示的情况下，按照一定顺序（由 iterator 提供的方法）访问聚合对象中的各个元素。

   由于 iterator 模式的以上特性：与聚合对象耦合，在一定程度上限制了它的广泛运用，一般仅用于底层聚合支持类，如 STL 的 list、vector、stack 等容器类及 ostream_iterator 等扩展 iterator。

2. 区别

   迭代器不是指针，是类模板，表现的像指针。它只是模拟了指针的一些功能，通过重载了指针的一些操作符、->、*、++、--等。迭代器封装了指针，是一个“可遍历 STL 容器内全部或部分元素”的对象， 本质是封装了原生指针，是指针概念的一种提升，提供了比指针更高级的行为，相当于一种智能指针，他可以根据不同类型的数据结构来实现不同的 ++, -- 等操作。

   迭代器返回的是对象引用而不是对象的值，所以 cout 只能输出迭代器使用 * 取值后的值而不能直接输出其自身。

3. 迭代器产生原因

   iterator 类的访问方式就是把不同集合类的访问逻辑抽象出来，使得不用暴露集合内部的结构而达到循环遍历集合的效果。

## 迭代器的具体实现

以 `vector<int>` 为例

```cpp
template <typename T>
class vector
{
public:
    typedef T *Iterator;
    // 构造函数
    vector()
        : _start(0), _finish(0), _endOfstorage(0)
    {
    }
    vector(const T *str, size_t size)          // 构造 size 个元素
        : _start(new T[size]), _finish(_start) //（没放空间时）// _finish(_start+size)（放了空间）
          ,
          _endOfstorage(_start + size)
    {
        // memcpy(_start,str,sizeof(T)*size);
        for (size_t i = 0; i < size; ++i)
        {
            *_finish++ = str[i]; // _start[i]=str[i];
        }
    }
    // 拷贝构造函数
    vector(const vector<T> &v)
    {
        size_t size = Size();
        _start = new T[size];
        for (size_t i = 0; i < size; i++)
        {
            _start[i] = v._start[i];
        }
        _finish = _start + size;
        _endOfstroage = _finish;
    }
    // 赋值运算符重载
    vector &operator=(const vector<T> &v)
    {
        size_t size = v.Size();
        if (this != &v)
        {
            T *tmp = new T[size];
            for (size_t i = 0; i < size; i++) //深拷贝
            {
                tmp[i] = _start[i];
            }
            delete[] _start;
            _start = tmp;
            _finish = _start + size;
            _endOfstorge = _finish;
        }
        return *this;
    }
    // 析构函数
    ~vector()
    {
        if (_start)
        {
            delete[] _start;
            _start = NULL;
        }
    }
    //////////////////////Iterator////////////////////////////
    Iterator Begin() // 迭代器
    {
        return _start; // Begin 和 _start 类型一致
    }
    Iterator End()
    {
        return _finish;
    }
    ///////////////////Modify////////////////////////////////
    void PushBack(const T &data)
    {
        CheckCapacity();
        *_finish = data;
        ++_finish;
    }
    void PopBack()
    {
        --_finish;
    }
    void Insert(size_t pos, const T &data)
    {
        size_t size = Size();
        CheckCapacity();
        assert(pos < size);
        for (size_t i = size; i > pos; --i)
        {
            _start[i] = _start[i - 1];
        }
        _start[pos] = data;
        ++_finish;
    }
    void Erase(size_t pos)
    {
        size_t size = Size();
        assert(pos < size) for (size_t i = pos; i < size; i++)
        {
            _start[i] = start[i + 1];
        }
        --_finish;
    }
    void Resize(size_t newSize, const T &data = T()) // 改变大小
    {
        size_t size = Size();         // 原来元素大小
        size_t capacity = CapaCity(); // 原来容量
        // 1.newSize 比 size 小
        if (newSize < size)
        {
            _finish == _start + newSize;
        }
        // 2.newSize 比 size 大，但比 capacity 小
        else if (newSize > size && newSize < capacity)
        {
            for (size_t i = size; i < newSize; i++)
            {
                // *finish++=data;
                _start[i] = data;
            }
            _finish = _start + newSize;
        }
        // newSize 比 capacity 大
        else
        {
            T *tmp = new T[newSize];          // 开辟新空间
            for (size_t i = 0; i < size; i++) // 搬移原空间的元素
            {
                tmp[i] = _start[i];
            }
            for (size_t i = size; i < newSize; i++) // 把新增加的元素加进来
            {
                tmp[i] = data;
            }
            delete[] _start; // 释放旧空间
            _start = tmp;
            _finish = _start + newSize;
            _endOfstorage = _finish;
        }
    }
    //////////////////capacity////////////////////////////
    size_t Size() const
    {
        return _finish - _start;
    }
    size_t CapaCity() const
    {
        return _endOfstorage - _start;
    }
    bool Empty() const // 判空
    {
        if (_atart == _finish)
        {
            return true;
        }
        return false;
    }
    //////////////Element Acess(元素访问)///////////////////////////
    T &operator[](size_t index) // 随机访问（下标）
    {
        size_t capacity = CapaCity();
        assert(index <= capacity);
        return _start[index];
    }
    const T &operator[](size_t index) const
    {
        size_t capacity = CapaCity();
        assert(index <= capacity);
        return _start[index];
    }
    T &Front()
    {
        return *_start;
    }
    const T &Front() const
    {
        return *_start;
    }
    T &Back()
    {
        return *(_finish - 1);
    }
    const T &Back() const
    {
        return *(_finish - 1);
    }
    void Clear()
    {
        _finish = _start;
    }

    friend ostream &operator<<(ostream &os, const vector<T> &v)
    {
        for (size_t i = 0; i < v.Size(); i++)
        {
            os << v[i] << " ";
        }
        os << endl;
        return os;
    }
    /*friend ostream& operator<<(ostream& os,  Vector<T>* v) // 通过迭代器重载
    {
        Vector<int>::Iterator it = v.Begin();
            while(it!=v.End())
            {
                cout<<*it<<" ";
                ++it;
            }
            os << endl;
            return os;
    }*/
private:
    void CheckCapacity()
    {
        size_t size = Size();
        size_t capacity = CapaCity();
        size_t newcapacity = 2 * capacity + 2;
        if (size >= capacity)
        {
            // 增容
            T *tmp = new T[newcapacity];
            // 拷贝元素
            // if(_start)
            // memcpy(tmp,_start,size*sizeof(T));//浅拷贝（导致两个字符串公用同一块空间）但是效率高
            // 出了函数作用域，要销毁v，销毁旧空间时出现问题
            for (size_t i = 0; i < size; i++)
            {
                tmp[i] = _start[i];
            }
            delete[] _start; // 释放旧空间
            _start = tmp;
            _finish = _start + size;
            _endOfstorage = _start + newcapacity;
        }
    }

private:
    T *_start;
    T *_finish;
    T *_endOfstorage;
};
```

