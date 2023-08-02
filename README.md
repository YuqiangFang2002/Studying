# Studying
To record my stduying.
# C++

##     1.1类的定义

1. 创建**对象**的过程也叫**类的实例化**，成员变量称为类的**属性**，将类的成员函数称为类的**方法**

2. 类是创建对象的模板，**用户自定义的复杂数据类型的声明**，类只是一个模板，编译后不占用内存空间，所以在定义类时不能对成员变量进行初始化，因为没有地方存储数据。只有在创建对象以后才会给成员变量分配内存，因为**对象**是类（数据类型）的一个变量，**占用内存空间**，这个时候就可以赋值了。

   ## 1.2创建对象的方式

   1.在栈上创建

   ```
   Student liLei;  //创建对象
   int a; //定义整型变量
   Student allStu[100];    //创建了一个 allStu 数组，它拥有100个元素，每个元素都是 Student 类型的对象
   ```

   2.在堆上使用 new 关键字创建，必须要用一个指针指向它，读者要记得 delete 掉不再使用的对象。

   ```
   Student *pStu = new Student;
   ```

   ## 1.3访问类的成员

   ```
       1.在栈上创建，通过对象名字访问成员使用点号`.`
   class Student{
   public:
       //类包含的变量
       char *name;
       int age;
       float score;
       //类包含的函数
       void say(){
           cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
       }
   };
   
   int main(){
       //创建对象
       Student stu;
       stu.name = "小明";
       stu.age = 15;
       stu.score = 92.5f;
       stu.say();
   
       return 0;
   }
   ```

   2.使用对象[指针]
   在栈上创建出来的对象都有名字，比如 stu，使用指针指向它不是必须的。
   创建的对象 stu 在栈上分配内存，需要使用`&`获取它的地址

   ```
   Student *pStu = &stu;
   ```

   在堆上创建对象使用`new`关键字，是匿名的，通过 new 创建出来的对象必须有指针指向它
   通过对象指针访问成员使用箭头`->`

   ```
   Student *pStu = new Student;
   ```

   > *在堆上分配内存，没有名字，只能得到一个指向它的指针，所以必须使用一个指针变量来接收这个指针，否则以后再也无法找到这个对象了，更没有办法使用它。也就是说，使用 new 在堆上创建出来的对象是匿名的，没法直接使用，必须要用一个指针指向它，再借助指针来访问它的成员变量或成员函数*。

   也就是说：
   new Student，得到一个指向Student类对象的指针p1，再定义一个Student类数据类型的指针pStu指向p1
   在堆上创建类对象，只能得到一个**装有类对象所在地址的地址**（指向类对象的指针），需要再**自主开辟一个内存的地址**储存**这个地址的地址**（这个新指针指向类对象的指针），这就是使用一个指针变量接受这个指针的含义。

   栈内存是程序自动管理的，不能使用 delete 删除在栈上创建的对象；堆内存由程序员管理，对象使用完毕后可以通过 delete 删除。在实际开发中，new 和 delete 往往成对出现，以保证及时删除不再使用的对象，防止无用内存堆积。
   有了对象指针后，可以通过箭头`->`来访问对象的成员变量和成员函数

   ~~~c++
   #include <iostream>
   using namespace std;
   
   class Student{
   public:
       char *name;
       int age;
       float score;
   
       void say(){
           cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
       }
   };
   
   int main(){
       Student *pStu = new Student;
       pStu -> name = "小明";
       pStu -> age = 15;
       pStu -> score = 92.5f;
       pStu -> say();
       delete pStu;  //删除对象
   
       return 0;
   }
   创建匿名对象：
   
   ```
   #include <iostream>
   using namespace std;
   
   class Student{
   public:
       Student(char *name, int age, float score);
       void show();
   private:
       static int m_total;  //静态成员变量
   private:
       char *m_name;
       int m_age;
       float m_score;
   };
   
   //初始化静态成员变量
   int Student::m_total = 0;
   
   Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){
       m_total++;  //操作静态成员变量
   }
   void Student::show(){
       cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<"（当前共有"<<m_total<<"名学生）"<<endl;
   }
   
   int main(){
       //创建匿名对象
       (new Student("小明", 15, 90)) -> show();
       (new Student("李磊", 16, 80)) -> show();
       (new Student("张华", 16, 99)) -> show();
       (new Student("王康", 14, 60)) -> show();
   
       return 0;
   }
   ```
   
   
   
   ~~~

## 类的成员函数

成员函数出现在类体中，作用范围由类来决定；
建议成员函数先在类体中作原型声明，然后在类外定义，这是一种良好的编程习惯，实际开发中大家也是这样做的。因为类体内部定义的函数默认就是内联函数（此处暂时不对内联函数有了解，遵从规则）。

## 构造函数

目的：在创建对象的同时为成员变量赋值

属性：

1. 构造函数必须是 public 属性的，否则创建对象时无法调用，创建对象时自动执行，用户不能调用，如果用户自己没有定义构造函数，那么编译器会自动生成一个默认的构造函数。

2. 构造函数的调用是**强制性**的，一旦在类中定义了构造函数，那么创建对象时就**一定要调用**，不调用是错误的。

   Student stu("小明", 15, 92.5f)；

   new Student("李华", 16, 96)；

   如果写作`Student stu`或者`new Student`就是错误的，因为类中包含了构造函数，而创建对象时却没有调用

3. 如果有多个重载的构造函数，只有一个构造函数会被调用。一个类可以有多个重载的构造函数，创建对象时根据传递的实参来判断调用哪一个构造函数。

4. 调用没有参数的构造函数也可以省略括号。对于示例2的代码，

   在栈上创建对象可以写作`Student stu()`或`Student stu`

   在堆上创建对象可以写作

   `Student *pstu = new Student()`或`Student *pstu = new Student`，它们都会调用构造函数 Student::Student()。

   

初始化列表

目的：

1. 仅仅是书写方便，成员变量较多时，这种写法简单明了。
2. 初始化 const 成员变量的唯一方法就是使用初始化列表

属性：

成员变量的赋值顺序由它们在类中的声明顺序决定,赋值顺序与声明顺序保持一致；

```c++
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){

}
//等价于
Student::Student(char *name, int age, float score): m_name(name){
	m_age = age;
	m_score = score;
}
```

## 析构函数

1. 构造函数的名字和类名相同，在类名前面加一个`~`符号，是一种特殊的成员函数，没有返回值，程序员也没法显式调用，在销毁对象时自动执行。
2. 析构函数没有参数，不能被重载，因此一个类只能有一个析构函数。如果用户没有定义，编译器会自动生成一个默认的析构函数。
3. Cpp中的 new 和 delete 分别用来分配和释放内存，它们与C语言中 malloc()、free() 最大的一个不同之处在于：用 new 分配内存时会调用构造函数，用 delete 释放内存时会调用析构函数。构造函数和析构函数对于类来说是不可或缺的，所以在C++中我们非常鼓励使用 new 和 delete。
4. 在函数内部创建的对象是局部对象，它和局部变量类似，位于栈区，函数执行结束时会调用这些对象的析构函数。
5. new 创建的对象位于堆区，通过 delete 删除时才会调用析构函数；如果没有 delete，析构函数就不会被执行。

## this 指针

1. this 是 [C++](http://c.biancheng.net/cplus/) 中的一个关键字，也是一个 const [指针](http://c.biancheng.net/c/80/)，它指向当前对象，通过它可以访问当前对象的所有成员。
2. this 只能用在类的内部，通过 this 可以访问类的所有成员，包括 private、protected、public 属性的。
3. 成员函数的参数和成员变量重名，只能通过 this 区分。

## static静态成员变量

目的：使用静态成员变量来实现多个对象共享数据的目标

1. static 成员变量属于类，不属于某个具体的对象，即使创建多个对象，也只为 m_total 分配一份内存，所有对象使用的都是这份内存中的数据。当某个对象修改了 m_total，也会影响到其他对象。

2. static 成员变量属于类，static 成员变量不占用对象的内存，而是在所有对象之外开辟内存，在内存分区中的全局数据区分配内存，到程序结束时才释放。这就意味着，static 成员变量不随对象的创建而分配内存，也不随对象的销毁而释放内存。

3. static 成员变量的内存既不是在声明类时分配，也不是在创建对象时分配，而是在（类外）初始化时分配。反过来说，没有在类外初始化的 static 成员变量不能使用。static 成员变量必须在类声明的外部初始化，具体形式为：

   type class::name = value;

   int Student::m_total = 0;

4. static 成员变量三种访问方式（但要遵循 private、protected 和 public 关键字的访问权限限制）：

```c++
//通过类类访问 static 成员变量
Student::m_total = 10;
//通过对象来访问 static 成员变量
Student stu("小明", 15, 92.5f);
stu.m_total = 20;
//通过对象指针来访问 static 成员变量
Student *pstu = new Student("李华", 16, 96);
pstu -> m_total = 20;
```

## 静态成员函数

目的:静态成员函数只能访问静态成员，普通成员函数可以访问所有成员（包括成员变量和成员函数）。

1. 静态成员函数在声明时要加 static，在定义时不能加 static。

2. 普通成员函数只能在创建对象后通过对象来调用，而静态成员函数可以通过类来直接调用，所以不管有没有创建对象，都可以调用静态成员函数。

   <!--编译器在编译一个普通成员函数时，会隐式地增加一个形参 this，并把当前对象的地址赋值给 this，所以普通成员函数只能在创建对象后通过对象来调用，因为它需要当前对象的地址。而静态成员函数可以通过类来直接调用，编译器不会为它增加形参 this，它不需要当前对象的地址，所以不管有没有创建对象，都可以调用静态成员函数。-->。

3. 静态成员函数与普通成员函数的根本区别在于：普通成员函数有 this 指针，可以访问类中的任意成员；而静态成员函数没有 this 指针，不知道指向哪个对象，无法访问对象的成员变量，只能访问静态成员（包括静态成员变量和静态成员函数）。

## const成员变量和成员函数

目的：保护某些数据不被修改

#### const成员变量

初始化 const 成员变量只有一种方法，就是通过构造函数的初始化列表

#### const成员函数（常成员函数）

1. 常成员函数需要在声明和定义的时候在**函数名字的结尾**加上 const 关键字，const 成员函数可以使用类中的所有成员变量，但是不能修改它们的值。

2. 通常将 get 函数设置为常成员函数。读取成员变量的函数的名字通常以`get`开头，后跟成员变量的名字，所以通常将它们称为 get 函数。

3. ```c++
   float getscore() const;  //声明
   char * Student::getname() const{
       return m_name;
   }
   ```

4. `char *getname() const`和`char *getname()`是两个不同的函数原型，如果只在一个地方加 const 会导致声明和定义处的函数原型冲突。

   函数开头的 const 用来修饰函数的返回值，表示返回值是 const 类型，也就是不能被修改，例如`const char * getname()`。

   函数头部的结尾加上 const 表示常成员函数，这种函数只能读取成员变量的值，而不能修改成员变量的值，例如`char * getname() const`。

## const 对象

目的：将对象定义为常对象之后，就只能调用类的 const 成员（包括 const 成员变量和 const 成员函数），是const对象，不是const类

语法：

const class  object(params);
class const object(params);

const class *p = new class(params);
class const *p = new class(params);

## 友元函数和友元类

## class和struct到底有什么区别

在C语言中，struct 只能包含成员变量，不能包含成员函数。而在C++中，struct 类似于 class，既可以包含成员变量，又可以包含成员函数。

- 使用 class 时，类中的成员默认都是 private 属性的；而使用 struct 时，结构体中的成员默认都是 public 属性的。
- class 继承默认是 private 继承，而 struct 继承默认是 public 继承（《[C++继承与派生](http://c.biancheng.net/cpp/biancheng/cpp/rumen_11/)》一章会讲解继承）。
- class 可以使用模板，而 struct 不能（《[模板、字符串和异常](http://c.biancheng.net/cpp/biancheng/cpp/rumen_14/)》一章会讲解模板）。



## string详解

使用 string 类需要包含头文件`<string>`，   #include <string>

与C风格的字符串不同，string 的结尾没有结束标志`'\0'`。

1. ​	string s1;                                变量 s1 只是定义但没有初始化，编译器会将默认值赋给 s1，默认值是`""`，也即空字符串。
2. ​    string s2 = "c plus plus";      变量 s2 在定义的同时被初始化为`"c plus plus"`。
3. ​    string s3 = s2;                          s3 的内容也是`"c plus plus"`。        
4. ​    string s4 (5, 's');                      变量 s4 被初始化为由 5 个`'s'`字符组成的字符串，也就是`"sssss"`

字符串长度：

1. string s = "http://c.biancheng.net";

2. int len = s.length();

3. cout<<len<<endl;

   string 的末尾没有`'\0'`字符，所以 length() 返回的是字符串的真实长度，而不是长度 +1

输入运算符`>>`默认会忽略空格，遇到空格就认为输入结束，

string s = "1234567890";

string 字符串的起始下标仍是从 0 开始。借助下标，除了能够访问每个字符，`s[5] = '5';`就将第6个字符修改为 '5'

使用`+`或`+=`运算符来直接拼接字符串，

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1 = "first ";
    string s2 = "second ";
    char *s3 = "third ";
    char s4[] = "fourth ";
    char ch = '@';

    string s5 = s1 + s2;
    string s6 = s1 + s3;
    string s7 = s1 + s4;
    string s8 = s1 + ch;
    
    cout<<s5<<endl<<s6<<endl<<s7<<endl<<s8<<endl;

    return 0;
}
```

增删改查

1.插入字符串

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1, s2, s3;
    s1 = s2 = "1234567890";
    s3 = "aaa";
    s1.insert(5, s3);
    cout<< s1 <<endl;
    s2.insert(5, "bbb");
    cout<< s2 <<endl;
    return 0;
}
```

2.删除字符串

erase函数

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1, s2, s3;
    s1 = s2 = s3 = "1234567890";
    s2.erase(5);
    s3.erase(5, 3);
    cout<< s1 <<endl;
    cout<< s2 <<endl;
    cout<< s3 <<endl;
    return 0;
}
```

运行结果：
1234567890
12345
1234590

3.提取字符串substr 

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1 = "first second third";
    string s2;
    s2 = s1.substr(6, 6);
    cout<< s1 <<endl;
    cout<< s2 <<endl;
    return 0;
}
```

运行结果：
first second third
second

4.字符串查找

find()函数，用于在 string 字符串中查找子字符串出现的位置

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1 = "first second third";
    string s2 = "second";
    int index = s1.find(s2,5);
    if(index < s1.length())
        cout<<"Found at index : "<< index <<endl;
    else
        cout<<"Not found"<<endl;
    return 0;
}
```

运行结果：
Found at index : 6

# 引用

原因：

参数传递是内存拷贝，对聚合类型，数组、结构体、对象等对它们进行频繁的内存拷贝会消耗很多时间，为了提高效率，直接传递指针，为了更加便捷，使用引用，直接目的就是让代码书写更方便。

> 参数的传递本质上是赋值的过程，赋值就是对内存进行拷贝,是指将一块内存上的数据复制到另一块内存上。
>
> 数组、结构体、对象是一系列数据的集合，数据的数量没有限制，可能很少，也可能成千上万，对它们进行频繁的内存拷贝可能会消耗很多时间，拖慢程序的执行效率。
>
> C/[C++](http://c.biancheng.net/cplus/) 禁止在函数调用时直接传递数组的内容，而是强制传递数组[指针](http://c.biancheng.net/c/80/)，而对于结构体和对象没有这种限制，调用函数时既可以传递指针，也可以直接传递内容；为了提高效率，我曾建议传递指针
>
> 但是在 C++ 中，我们有了一种比指针更加便捷的传递聚合类型数据的方式，那就是**引用（Reference）**。
>
> 在 C/C++ 中，我们将 char、int、float 等由语言本身支持的类型称为基本类型，将数组、结构体、类（对象）等由基本类型组合而成的类型称为聚合类型

定义：

引用可以看做是数据的一个别名，通过这个别名和原来的名字都能够找到这份数据。



引用类似于 Windows 中的快捷方式，一个可执行程序可以有多个快捷方式，通过这些快捷方式和可执行程序本身都能够运行程序；

引用的定义方式类似于指针，只是用`&`取代了`*`

type &name = data;

引用必须在定义的同时初始化，并且以后也要从一而终，不能再引用其它数据，这有点类似于常量（const 变量）。

```c++
#include <iostream>
using namespace std;

int main() {
    int a = 99;
    int &r = a;
    cout << a << ", " << r << endl;
    cout << &a << ", " << &r << endl;

    return 0;
}
```

运行结果：
99, 99
0x28ff44, 0x28ff44

变量 r 就是变量 a 的引用，它们用来指代同一份数据；也可以说变量 r 是变量 a 的另一个名字。

常引用，不通过引用来修改原始的数据

const type &name = value;

const int &a = c;

type const &name = value;



C++引用作为函数参数：

在定义或声明函数时，我们可以将函数的形参指定为引用的形式，这样在调用函数时就会将实参和形参绑定在一起，让它们都指代同一份数据。如此一来，如果在函数体中修改了形参的数据，那么实参的数据也会被修改，从而拥有“在函数内部影响函数外部数据”的效果。

按引用传参在使用形式上比指针更加直观。在以后的 C++ 编程中，我鼓励读者大量使用引用，它一般可以代替指针

C++引用作为返回值：

```c++
#include <iostream>
using namespace std;

int &plus10(int &r) {
    r += 10;
    return r;
}

int main() {
    int num1 = 10;
    int num2 = plus10(num1);
    cout << num1 << " " << num2 << endl;

    return 0;
}
```



不能返回局部数据（例如局部变量、局部对象、局部数组等）的引用，因为当函数调用完成后局部数据就会被销毁，

引用的实质：

引用只是对指针进行简单的封装，底层依然通过指针实现，和指针占用的长度一样，32位环境是4字节，64位环境是8个字节，不是不获取他的地址，是编辑器不让获取引用本身的地址

引用基于指针实现，但比指针更加的易用，指针获取数据要*，而引用不需要

引用与指针的区别：

引用必须在定义时初始化，从一而终，不能改变，指针定义不必赋值，以后也可以指向任意数据

有const指针，但没有const引用，如int & const r = a ;

指针可以多级，但引用只能一级

指针++，表示指向下一份数据；引用++表示它所指代的数据本身+1



# C++继承与派生

继承是儿子接收父亲的产业，派生是父亲把产业传承给儿子。派生类除了拥有基类的成员，还可以定义自己的新成员，以增强类的功能。

class Student: public People，表示是公有继承

三种继承方式：public（公有的）、private（私有的）和 protected（受保护的）

public 成员可以通过对象来访问，protected，private 成员不能通过对象访问。

但是当存在继承关系时，基类中的 protected 成员可以在派生类中使用，只有基类中的 private 成员不能在派生类中使用

**1) public继承方式** （不变）

- 基类中所有 public 成员在派生类中为 public 属性；
- 基类中所有 protected 成员在派生类中为 protected 属性；
- 基类中所有 private 成员在派生类中不能使用。


**2) protected继承方式**（两个protect)

- 基类中的所有 public 成员在派生类中为 protected 属性；
- 基类中的所有 protected 成员在派生类中为 protected 属性；
- 基类中的所有 private 成员在派生类中不能使用。


**3) private继承方式**(全private)

- 基类中的所有 public 成员在派生类中均为 private 属性；
- 基类中的所有 protected 成员在派生类中均为 private 属性；
- 基类中的所有 private 成员在派生类中不能使用

继承方式中的 public、protected、private 是用来指明基类成员在派生类中的最高访问权限的。

由于 private 和 protected 继承方式会改变基类成员在派生类中的访问权限，导致继承关系复杂，所以实际开发中我们一般使用 public。

在派生类中访问基类 private 成员的唯一方法就是借助基类的非 private 成员函数，如果基类没有非 private 成员函数，那么该成员在派生类中将无法访问。

改变访问权限：

使用 using 关键字可以改变基类成员在派生类中的访问权限，using 只能改变基类中 public 和 protected 成员的访问权限，不能改变 private 成员的访问权限，因为基类中 private 成员在派生类中是不可见的，根本不能使用，所以基类中的 private 成员在派生类中无论如何都不能访问。

所以为了设置protected一个重要原因就是能够还有机会提高访问权限

名字遮蔽问题：

派生类中的成员（包括成员变量和成员函数）和基类中的成员重名，派生类使用该成员实际使用的是派生类新增的成员，不是从基类继承过来的。

```c++
class People{}
class Student: public People
    Student stu("小明", 16, 90.5);
	    stu.show();  //派生类
    	stu.People::show();  //基类

```

#### 基类、派生类成员函数不构成重载

重载：函数名相同，参数不同，根据参数的类型选择执行的函数

基类成员和派生类成员的名字一样时会造成遮蔽，对于成员函数要引起注意，不管函数的参数如何，只要名字一样就会造成遮蔽。

换句话说，基类成员函数和派生类成员函数不会构成重载，如果派生类有同名函数，那么就会遮蔽基类中的所有同名函数，也就是基类函数无效，不管它们的参数是否一样。

#### C++基类和派生类的构造函数

类的构造函数不能被继承。它的名字和派生类的名字不一样，不能成为派生类的构造函数。

在派生类的构造函数中调用基类的构造函数。

```c++
class People{}
People::People(char *name, int age): m_name(name), m_age(age){}

class Student: public People{}
Student::Student(char *name, int age, float score): People(name, age), m_score(score){ }
or
Student::Student(char *name, int age, float score): m_score(score), People(name, age){ }
```

`People(name, age)`就是调用基类的构造函数，并将 name 和 age 作为实参传递给它，`m_score(score)`是派生类的参数初始化表，它们之间以逗号`,`隔开。

函数头部是对基类构造函数的调用，而不是声明，所以括号里的参数是实参，它们不但可以是派生类构造函数参数列表中的参数，还可以是局部变量、常量等，例如：

```c++
Student::Student(char *name, int age, float score): People("小明", 16), m_score(score){ }
```

#### 构造函数的调用顺序

A --> B --> C

那么创建 C 类对象时构造函数的执行顺序为：

A类构造函数 --> B类构造函数 --> C类构造函数

### 基类和派生类的析构函数

析构函数也不能被继承，在派生类的析构函数中不用显式地调用基类的析构函数，因为每个类只有一个析构函数，编译器知道如何选择，无需程序员干涉。

外析构函数的执行顺序和构造函数的执行顺序也刚好相反：

- 创建派生类对象时，构造函数的执行顺序和继承顺序相同，即先执行基类构造函数，再执行派生类构造函数。
- 而销毁派生类对象时，析构函数的执行顺序和继承顺序相反，即先执行派生类析构函数，再执行基类析构函数。

### C++多继承

多继承容易让代码逻辑复杂、思路混乱，一直备受争议，中小型项目中较少使用，后来的 [Java](http://c.biancheng.net/java/)、[C#](http://c.biancheng.net/csharp/)、[PHP](http://c.biancheng.net/php/) 等干脆取消了多继承

派生类都只有一个基类，称为**单继承（Single Inheri[tan](http://c.biancheng.net/ref/tan.html)ce）**。除此之外，[C++](http://c.biancheng.net/cplus/)也支持**多继承（Multiple Inheritance）**，即一个派生类可以有两个或多个基类。

### 虚继承和虚基类详解

解决多继承时的命名冲突和冗余数据问题，[C++](http://c.biancheng.net/cplus/) 提出了虚继承，使得在派生类中只保留一份间接基类的成员。

```c++
class A{}
class B: virtual public A{  }//虚继承

```

### 将派生类赋值给基类（向上转型）

数据类型转换

```c
int a = 10.9;
printf("%d\n", a);  //输出结果为 10
    
float b = 10;
printf("%f\n", b);   //输出结果为 10.000000
```

类其实也是一种数据类型，也可以发生数据类型转换，不过这种转换只有在基类和派生类之间才有意义，并且只能将派生类赋值给基类，包括将派生类对象赋值给基类对象、将派生类[指针](http://c.biancheng.net/c/80/)赋值给基类指针、将派生类引用赋值给基类引用，这在 C++ 中称为向上转型（Upcasting）。相应地，将基类赋值给派生类称为向下转型（Downcasting）。

# 多态与虚函数

多态目的:

可以通过基类指针对所有派生类（包括直接派生和间接派生）的成员变量和成员函数进行“全方位”的访问，尤其是成员函数。如果没有多态，我们只能访问成员变量。

虚函数目的：

C++中虚函数的唯一用处就是构成多态



继承时，派生类可以访问基类，但基类指针只能访问派生类的成员变量，但是不能访问派生类的成员函数，因为函数不在内存里。

让基类指针能够访问派生类的成员函数，Cpp增加了**虚函数（Virtual Function）**

有了虚函数，基类指针指向基类对象时就使用基类的成员（包括成员函数和成员变量），指向派生类对象时就使用派生类的成员。换句话说，基类指针可以按照基类的方式来做事，也可以按照派生类的方式来做事，它有多种形态,我们将这种现象称为**多态**。

C++提供多态的是：指针，引用也可以，但不明显，谈及多态一般是指针。

注意事项：

只需要在虚函数的声明处加上 virtual 关键字，函数定义处可以加也可以不加。

为了方便，你可以只将基类中的函数声明为虚函数，这样所有派生类中具有遮蔽关系的同名函数都将自动成为虚函数。

只有子类与基类的函数相同才能构成多态（通过基类[指针](http://c.biancheng.net/c/80/)访问派生类函数）。这不是遮蔽吗？？？

遮蔽是子类调用父类，多态是父类调用子类。

派生类不继承基类的构造函数，将构造函数声明为虚函数没有什么意义。

什么时候声明虚函数：

首先看成员函数所在的类是否会作为基类。

然后看成员函数在类的继承后有无可能被更改功能，如果希望更改其功能的，一般应该将它声明为虚函数。？？

如果成员函数在类被继承后功能不需修改，或派生类用不到该函数，则不要把它声明为虚函数。？？？



# 运算符重载

### 运算符优先级



但结构化并发要解决的问题我感觉更多是因为异步操作引入的，不是多处理器。即使是异步的单线程程序，有了coroutine也会简洁很多。

初学者还是把简单的内存管理学起来再说。





因为抓不到重点罗里吧嗦的人太多了。我试着用最简单的关键词概括一下C++的核心思想，顺着搜一下就行了，比你看一大堆书有用。

1. [结构化编程](https://www.zhihu.com/search?q=结构化编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})（RAII，智能指针）以及进阶的结构化并发（coroutine，未来的executor）
2. 类型擦除 （[虚函数继承](https://www.zhihu.com/search?q=虚函数继承&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})，[函数指针](https://www.zhihu.com/search?q=函数指针&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})，std::function,，std::variant以及未来的多态对象）
3. [模板元编程](https://www.zhihu.com/search?q=模板元编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})：不涉及类型运算的，constexpr为主，和普通编程尽量保持一致；涉及类型运算的，模式匹配（特化偏特化）
4. 最后，基于零开销抽象原则，C++正在不间断的从函数式编程（没有任何抽象比基于数学理论的[函数式编程](https://www.zhihu.com/search?q=函数式编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})更准确简短利于优化）抄各种好东西，上面提到的不少都是这样来的，快去学

稍微再解释一下。

1. 所谓结构化编程，无非是古早时期的一个编程思想。在远古时代，当时没有结构化编程，程序员写代码，就是goto满天飞，代码的控制流连成了一个蜘蛛网，永远不知道下一行执行的代码在哪里。但有了结构化编程的思想后呢，通过if、while、call function等控制结构，代码被划分成了一个个的块，小的块被嵌套在大的块内部。**内部的块只能通过外部的块进入、执行，并回到外部的块**。有了这个思想，代码的控制流就很清晰了。现代语言全都是结构化编程语言，结构化编程就是编程的代名词。但是，如果你写过C程序的话（虽然我没写过），你会发现C程序里还是可以看到很多goto，这是因为C语言还缺少一种必要的嵌套结构：如果程序进入一个块后，做了一些事情，在退出这个块时，一定要做一些相反的事情。（比如函数参数的入栈出栈，资源的释放和申请。）C语言没有提供这个能力，导致还是不得不写很多goto；C++通过构造函数和[析构函数](https://www.zhihu.com/search?q=析构函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})的匹配，提供了这个能力。更进一步，衍生出了RAII和[智能指针](https://www.zhihu.com/search?q=智能指针&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})。到了现在，由于多处理器很常见，程序可以很有多个控制流，不同控制流之间的执行顺序可能互不相关，又回到了远古时代那个goto满天飞时的悲惨情景，大牛们又一次提出了结构化并发的思想，希望多个控制流还是能保证“小的块被嵌套在大的块内部。内部的块只能通过外部的块进入、执行，并回到外部的块”。这一波属于是老树开花。
2. 所谓[类型擦除](https://www.zhihu.com/search?q=类型擦除&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})，就是把多个不同的类型变成另一个类型（通常是更广更泛的一个类型，像std::common_type）。为什么要这么做呢？因为有些时候我们需要把不同类型的对象统一起来一起处理。对于虚函数继承，子类指针或引用天生就可以转换成父类指针或引用；对于函数指针，可以把有相同签名的不同的函数类型转换为同一个类型；对于std::function，可以把有相同签名的不同的函数对象类型转换为同一个类型；对于std::variant，则可以把任意数量有限的已知类型转换为同一个类型。
3. 模板元编程主要分两类。第一类，普通的函数，能在编译期计算的，尽量放在编译期计算。到了C++20，几乎所有的普通函数前面都可以加上[constexpr](https://www.zhihu.com/search?q=constexpr&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})。（甚至不加编译器也会帮忙优化）第二类，[泛型编程](https://www.zhihu.com/search?q=泛型编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})。通常情况，对所有类型做相同的处理，如果要对不同类型做不同的处理，依据模式匹配的写法即可。[模式匹配](https://www.zhihu.com/search?q=模式匹配&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})是[图灵完备](https://www.zhihu.com/search?q=图灵完备&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2753180251})的，表达能力相当强大，请认识到这一点，否则有些库是看不懂的。（建议看看这个例子：[Compiler Explorer (godbolt.org)](https://link.zhihu.com/?target=https%3A//godbolt.org/z/TfW6qEa46)）
4. 不多说，学学相关思想就对了。



CPP核心思想

OOP，泛型，标准库

1. 面向对象编程（Object-Oriented Programming，OOP）：Cpp支持面向对象编程，将数据和操作封装为对象。这种编程范式使得代码更加模块化、可维护和可重用。通过类和对象，可以实现数据封装、继承和多态等概念，提供了一种组织和管理代码的强大工具。
2. 泛型编程（Generic Programming）：Cpp支持泛型编程，使得代码可以在不同类型上进行抽象和重用。通过模板（template），可以编写具有通用性的算法和数据结构，使其适用于多种数据类型，而不需要为每种类型编写重复的代码。泛型编程可以提高代码的可复用性和性能。
3. 低级控制：Cpp允许直接操作底层硬件和内存，提供了对指针和引用的支持。这使得程序员可以更加精细地控制程序的执行，实现高性能的操作和底层的系统编程。然而，这也增加了程序出错的潜在风险，需要程序员对内存管理和指针操作有深入的理解和谨慎的使用。
4. 高效性：Cpp追求高效性，旨在提供接近底层语言（如C语言）的性能。它提供了对内存管理的直接控制、内联汇编和底层操作等特性，使得程序员可以更好地优化代码的性能。
5. 标准库：Cpp提供了一个强大的标准库，其中包括各种数据结构、算法、输入输出、多线程和网络编程等功能。使用标准库可以方便地实现常见任务，减少开发时间，并提高代码的可移植性。
6. 扩展性：Cpp具有很强的扩展性，允许通过编写扩展库和使用外部库来增加语言的功能和特性。这使得Cpp可以适应各种不同的应用场景和需求。



C++提供了一些基本的数据结构，用于组织和操作数据。这些数据结构是在标准库中定义的，并且可以通过包含相应的头文件来使用。以下是一些常见的基本数据结构：

1. 数组（Array）：数组是一种连续存储相同类型元素的数据结构。它的大小在创建时确定，并且可以通过索引访问和修改元素。C++提供了内置数组和`std::array`模板，后者提供了更多的功能和安全性。
2. 向量（Vector）：向量是一种动态数组，可以根据需要自动调整大小。它使用`std::vector`模板实现，并提供了在尾部插入、删除元素，以及随机访问等操作。
3. 列表（List）：列表是一种双向链表，其中每个节点都包含一个值和指向前后节点的指针。C++提供了`std::list`模板来实现列表，它支持在任意位置插入、删除元素，但随机访问较慢。
4. 队列（Queue）：队列是一种先进先出（FIFO）的数据结构，其中元素从一端（称为队尾）插入，从另一端（称为队头）删除。C++提供了`std::queue`模板，用于实现队列。
5. 栈（Stack）：栈是一种后进先出（LIFO）的数据结构，其中元素只能从栈顶插入和删除。C++提供了`std::stack`模板，用于实现栈。
6. 集合（Set）：集合是一组唯一元素的无序容器，其中每个元素只出现一次。C++提供了`std::set`模板，用于实现集合。
7. 映射（Map）：映射是一种键值对的容器，每个键与一个值相关联。C++提供了`std::map`模板，用于实现映射。
8. 散列表（Hash Table）：散列表是一种基于散列函数的数据结构，用于快速查找和插入元素。C++提供了`std::unordered_map`和`std::unordered_set`模板，用于实现基于散列表的映射和集合。



基本的数据类型

1. 整型（Integer types）：
   - `int`：整数类型，通常为32位。
   - `short`：短整数类型，通常为16位。
   - `long`：长整数类型，通常为32位。
   - `long long`：长长整数类型，通常为64位。
   - `unsigned`：无符号整数类型，只能表示非负数。
2. 浮点型（Floating-point types）：
   - `float`：单精度浮点数，通常为32位。
   - `double`：双精度浮点数，通常为64位。
   - `long double`：扩展精度浮点数，通常为80位或更多。
3. 字符型（Character types）：
   - `char`：字符类型，用于存储单个字符。可以表示ASCII字符或扩展字符集。
   - `wchar_t`：宽字符类型，用于存储宽字符。通常为16位或更多。
   - `char16_t`：用于表示UTF-16编码的字符，通常为16位。
   - `char32_t`：用于表示UTF-32编码的字符，通常为32位。
4. 布尔型（Boolean type）：
   - `bool`：布尔类型，用于表示真（true）或假（false）值。
5. 空类型（Void type）：
   - `void`：空类型，用于表示没有值的情况。通常用于函数的返回类型或函数指针的类型。

这些基本数据类型可以用于声明变量、函数参数和函数返回值等。此外，C++还提供了一些复合类型，如数组、指针、引用、结构体和类等，可以用于组合和扩展基本数据类型。

