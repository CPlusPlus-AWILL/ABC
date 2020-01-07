# typedef与define

[github仓库](https://github.com/CPlusPlus-AWILL/typedefVsDefine)

## typedef关键字

[参考链接](https://www.cnblogs.com/rednodel/p/9287851.html)

* 用法一：定义类型别名。

    ```c++
    char* pa, pb;	// pa是char*类型，pb是char类型；可能并不符合我们的意图。
    ```

    使用`typedef`定义类型别名：

    ```c++
    typedef char* PCHAR;
    PCHAR pa, pb;	// pa是char*类型，pb也是char*类型。
    ```

* 用法二：辅助声明struct。

    做为了解就行，方便读比较老的源代码。
    在旧的C代码中，会有这样的代码：

    ```c
    struct PointTag
    {
    	int x;
        int y;
    };
    struct PointTag pt;
    ```

    ```c++
    typedef struct PointTag
    {
        int x;
        int y;
    }Point;
    Point pt;
    ```

    现在的c++中可以这样：

    ```c++
    typedef struct
    {
        int x;
        int y;
    }Point;
    ```

    也可以这样：

    ```c++
    typedef struct PointTag
    {
    	int x;
        int y;
    }Point;
    ```

    同样也可以这样：

    ```c++
    typedef struct Point
    {
    	int x;
        int y;
    }Point;
    ```

    至于这种形式的为什么可以——

    ```c++
    typedef int int;	// 非法。
    typedef Point Point;	// 合法，但是没什么用。
    ```

* 用法三：定义与平台无关的类型（比如`size_t`）。

* 用法四：简化代码

    * type (*)(...)函数指针
    * type (*)[]数组指针

* 用法五：解释性编程

    ```c++
    typedef double weight;
    typedef double accelerate;
    typedef double force;
    static accelerate Newton2ndLaw(weight m, force f)
    {
        return f/(m*1.0);
    }
    ```


## #define预处理指令

[参考链接](https://blog.csdn.net/king110108/article/details/80728010)

## 两者区别

[参考链接](https://blog.csdn.net/summer00072/article/details/80918483)

* 原理

    * `#define`是C语言中定义的语法，是`预处理`指令，在*编译阶段*之前就完成了。
        在预处理时*只进行简单机械的字符串替换*，不做任何安全性检查。
    * `typedef`是C++语言中的关键字，因此是在编译阶段进行处理的，具备类型检查功能。
        它在==自己的作用域内==给一个已经存在的类型起一个别名，但是，<u>不能够在函数定义里面使用</u>`typedef`关键字。
        用typedef定义**数组、指针、结构**等类型确实很方便且易读。

* 功能

    * `#define`的功能比较强大，甚至说是比较具有破坏力。
        它不仅仅可以为类型起别名，还可以定义常量、变量、编译开关、函数等。
    * `typedef`用来定义类型别名。

* 作用域

    * `#define`没有作用域的限制。（`namespace`对它不起作用。）
        只要是之前预定义过的宏，在以后的程序内都可以使用。
    * `typedef`有自己的作用域——就是它所在的作用域。

* 指针操作

    * `#define`记住一点——它只进行字符串替换。
    * `typedef`是类型别名。
    
    >指针与`const`结合：
    >
    >Tip1：遇到`p`就翻译为`p is a`，遇到`*`就翻译为`pointer to`。英文翻译时记住加上`变量`。
    >
    >Tip2：常量指针——指向常量的指针；指针常量——指针类型的常量。*其实区分它的汉语翻译是什么反而没有太大意义。如果实在要记，那就记住<u>一个规则</u>——const在星号左边，就是常量指针，否则，就是指针常量。*
    
    ```c++
    #include<iostream>
    
    #define INTPTR_1 int*
    typedef int* INTPTR_2;
    
    int main()
    {
        //
        INTPTR_1 p1, p2;    // >p1 int*     >p2 int
        INTPTR_2 p3, p4;    // >p3 int*     >p4 int*
    
        // 与const结合
        const INTPTR_1 p5 = nullptr;    // >p5 const int*
        const INTPTR_2 p6 = nullptr;    // >p6 int* const
        INTPTR_1 const p7 = nullptr;    // >p7 int* const;
        INTPTR_2 const p8 = nullptr;    // >p8 int* const;
    }
    ```

