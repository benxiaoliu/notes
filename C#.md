C# VS .NET
===
C# is a programminng language
.NET is a framework for buildinng applications on Windows, .NET is not limited to C#

.NET
CLR (common language runtime)
Class library

CLR
===
C/C++ :compiler translated out code into the native code for the machinne onn which it was running, which means if I wrote an application
in C++ on a Windows machine with 8086 processor architecture, the compiler will translate my code into the native code for that machine
that is a Windows machine with an 8086 processor. Now we only have different hardwares ad 
we have different operating systems so if I took the application that compile the the applicationn on
the computer with adifferent architecture that would not run. so Microsoft was designing C# language and the .NET
framework they came up with an idea that they borrowed from Java community. In Java when you compile your code it's not translated
directly into the machine code. it's translated into an intermediate language called bytecode and we have the ame conncepy in C#
when you compile your c# code, the result is waht we call IL or intermediate language code is independent of the computer on which
it's running now we need something that translate that il into the ative code on the machine that is running the application
and that is the job of CLR or commonn language runtime. so CLR is essentially an application that is sitting in the memory 
whose job is to translate the il code into the machine code and this process is called just-in-time compilation or JIT
so with this architecture you can write an application in C# and you don't have to worry about compiling that into the native code
for different machines as long as a machine has CLR that can run your application

- namespace is a container for related classes
- assembly (DLL or EXE): a container for related namespaces. phically, it's a file on the disk, which can either be an executable or a DDL
stands for dynamically linked library so when you compile an application the compiler builds one or more assemblies depending on how you 
partition your code

- when we write code in a namespace, we have access to any class defined innn this namespace, when you want to a class that is defined in a different
namespace we need to import it in our code file

- main is the entry point of the application. when you run your application, CLR execute the code inside main method

- C# is a case sensitive language

- {} for methods, classes and namespaces

- statements in C# terminate with a semicolon 

- run the application with ctrl + F5
- compile: Ctrl + shift + B

Variables and Constant
===
Variables: a name given to storage location in memory

Constants: an immutable value, use it for safety, like pi, we don't want it to be changed. const float Pi = 3.14f;
int number, which is a number between minus 2 billion and plus 2 billion

- double is the default type for real numbers, when you want to use float, add f after the number, float number = 1.2f;

overflow
===
exceed the boundary of the assigned data type
```
byte number = 255;
unmber = number + 1; // get 0
```
C# by default we don't have overflow checking which means we can modify the value of a variable at runtime and if we go beyong the boundary
of its unnderlying data type we will get overflow. to stop overflowing, used checked keyword
```
checked
{
  byte number = 255;
  number = number + 1;
}
```
Scope
===
scope is where a variable or a constant has meaning and is accessible
```
{
  byte a = 1;
  {
    byte b = 2;
    {
      byte c = 3;
    }
  }
}
```
- a block is indicated by curly braces
a is accessible anywhere inside this block or 
any of these child. if I go out of this block and try to access a, the program will not compile
最外层的块都所以地方都能访问到
if an exception happens in the try block, this catch block will be executed. this prevents your 
application from crashing. the reason our application crashed was because we did not handle the 
exceptionn so if you don't handle exception, the exception will be propagated to the .NET runtime
and that the run times mechanism is to stop your application and display the error

```C#
try{
}
catch{
}
```

operator
===
- use == to test equality
- b = ++a is different from b == a++
- logical operators are used in boolean expressions which are often used in conditional statements; the not operator is indicated by an exclamation mark
- im the real world, keep comments to be minimum and explain whys, hows and constrains, etc. not thhe whats.

public bool Equals( string value ) 
判断当前的 string 对象是否与指定的 string 对象具有相同的值。

类的 构造函数 是类的一个特殊的成员函数，当创建类的新对象时执行。

构造函数的名称与类的名称完全相同，它没有任何返回类型。

类的 析构函数 是类的一个特殊的成员函数，当类的对象超出范围时执行。

析构函数的名称是在类的名称前加上一个波浪形（~）作为前缀，它不返回值，也不带任何参数。

析构函数用于在结束程序（比如关闭文件、释放内存等）之前释放资源。

我们可以使用 static 关键字把类成员定义为静态的。当我们声明一个类成员为静态时，意味着无论有多少个类的对象被创建，只会有一个该静态成员的副本。你也可以把一个成员函数声明为 static。这样的函数只能访问静态变量。静态函数在对象被创建之前就已经存在。
