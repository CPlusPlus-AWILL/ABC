# explicit关键字

[参考链接](https://blog.csdn.net/caroline_wendy/article/details/22727823)

[参考链接](https://stackoverflow.com/questions/121162/what-does-the-explicit-keyword-mean)

[github仓库](https://github.com/CPlusPlus-AWILL/ExplicitKeyword)

* explicit的主要用法：
    1. 用于单参数的`构造函数`中，防止==隐式转换==，避免函数入口参数歧义。

比如：

```c++
class A
{
	...
};

class B
{
public:
    B(const A& a);
};

void FunctionForB(const B& b)
{
	...
}
```

未指定`explicit`关键字的构造函数，它可能被隐式调用：

```c++
A a;
B b = a;	// a隐式转换，调用构造函数。
FunctionForB(b);
FunctionForB(a);	// 对象a也可以作为函数的入口参数（发生了隐式转换），引起歧义。
```