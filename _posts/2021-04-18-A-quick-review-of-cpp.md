---
layout: post
title:  "A quick review of C++"
date:  2021-04-18T10:29:00+08:00
tags: ["review_label", "hidden_label"]
categories: ["Programming", "C++"]
keywords: object-oriented
author: Yichong Zhou
hidden: true
---

> It is almost impossible to put all the information about C++ in one mere post. 
This post mainly focuses on the core concepts and critical details in C++ that help us better understand it. 
Most of the materials in this post are collected from internet. 

<!--more-->


{% include toc.html %}


## Basics

### forward declaration

Compiler compiles the contents of a code file sequentially.
```c++
int add(int x, int y); // forward declaration `add()`, so `print()` knows its existence
// or `int add(int, int);`
void print() {std::cout << add(2, 3) << std::endl;}  // 5
int add(int x, int y){return x + y;}  // actual definition after the above usage
// Forward declarations can also be used with other identifiers, such as variables and user-defined types. 
int foo; class MyClass; struct MyStruct; // classes and structs can be forward-declared
// variables that are defined in other files must have a forward declaration specified with keyword `extern`
extern int bar; 
```

All definitions also serve as declarations. 
e.g., `int x;` is a definition, it’s also a declaration. 
Those declarations that aren’t definitions are called **pure declarations**.
There is only one definition for an identifier, 
but you can have as many pure declarations for an identifier as you desire. 


### preprocessor

```c++
// replace all `MSG` in the code with `"Preprocessor"`
#define MSG "Preprocessor"  
void print(){std::cout << "Hello " << MSG << std::endl;} // Hello Preprocessor
// macros without substitution text (any occurrence of the identifier is removed and replaced by nothing) 
#define PRINT_MSG
// conditional compilation: #ifdef #ifndef #endif
#ifdef PRINT_MSG
  std::cout << "Conditional Compilation" << std::endl; // Conditional Compilation
#endif
// use `#if 0` and `#endif` to exclude a block of code from being compiled
// which provides a convenient way to "comment out" multi-line code 
// The scope: directives are only valid from the point of definition to the end of the file
```

### header files

Header files generally only contain the **pure declarations** of functions and objects (otherwise one-definition rule would be violated). 
e.g., `std::cout` is forward declared in the `iostream` header, 
but defined as part of the C++ standard library, 
which is automatically linked into your program during the linking phase. 
Sometimes header files also contain global constant variables. 


Note that including a relative path to the header file (e.g. `#include "_Basics/namespace.h"`) 
or including `.cpp` files is a bad idea; 
you can, for example, use `include_directories(_Basics/)` in cmake to set the relative path. 
If a header file is paired with a `.cpp` file, they should have the same name. 
Headers may include other headers. All header files should have header guards.

```c++
// header guards
#ifndef SOME_UNIQUE_NAME
#define SOME_UNIQUE_NAME
void some_func();  // forward declaration
#endif 
```

Note that header guards do not prevent a header from being included once into different code files. 
The best way is simply to put the definition in one of the `.cpp` files, 
so that the header just contains a forward declaration. 
<!--But sometimes it’s necessary to put non-function definitions in a header, such as user-defined types.-->

### namespace

```c++
int x=10; // global namespace. 
namespace my_ns{int x=11;} // custom namespaces 
#include<iostream>
int main() {std::cout << "The result is " << my_ns::x << std::endl;return 0;} // The result is 11 
namespace foo {namespace bar {int x = 11;}} // nested namespaces  
namespace foo::bar {int y = 12;}  // nested namespaces
namespace foobar = foo::bar; // namespace aliases
std::cout << "The result is " << foo::bar::x + foobar::y << std::endl; // The result is 23
// avoid “using” statements outside of a function (global scope) if possible.
// limit the scope of `using` statement by using the block `{}`.
{using namespace std; cout << "Hello C++" << std::endl;}  // Hello C++
{using std::cout; cout << "Hello C++" << std::endl;} // Hello C++
```

It is legal to declare `namespace` blocks in multiple locations (either across multiple files, or multiple places within the
same file). All `namespace` blocks with the same name are considered part of that `namespace`. 



### reference variables

A reference is an alias to another object or value. 
It acts like a pointer that is implicitly dereferenced when accessed 
(references are usually implemented internally using pointers).

Unlike pointers, which can hold a null value, 
**references must be initialized when created**. 
Once initialized, a reference can not be changed to reference another variable. 
References are much safer than pointers since there's no risk of dereferencing a null pointer. 
Pointers should only be used in situations where references are not sufficient (such as dynamically allocating memory).


```c++
// In this context, the `&` does not mean “take the address of”, it means “reference”.
int x = 10; int & x_1 = x;  // non-const references
const int y = 11; const int & y_1 = y;  // const references
const int & z = 12;  // r-value references (C++11).
std::cout << x_1 << " " << y_1 << " " << z << std::endl; // 10 11 12
std::cout << &x << " " << &x_1 << std::endl; // 0x7ffec4c28544 0x7ffec4c28544 // take the address
```

**l-values** are the values that can be on the left side of an assignment statement, 
e.g., `int x=5;int y=x;`, where `x` is an l-value.
Apparently, an l-value is a value that has an address.
All variables are l-values. 

**r-values** are the values that can only be on the right side of an assignment statement. 
e.g., `5 = 6;` will cause a compile error, therefore `5` is not an l-value. 
r-values are always evaluated to produce a single value. e.g., literals, variables and expressions are all r-values.

Non-const references can only be initialized with non-const l-values, 
can not be initialized with const l-values or r-values. 

```c++
// const int x = 10; int & x_1 = x;  // compile error
// int & z = 12;  // compile error
int x = 20; const int & x_1 = x;  // ok
```

By using reference parameter as an alias for the argument, 
we can **avoid making an expensive copy** of the argument. 
Const references guarantee that referenced values will not be changed, 
can be initialized with non-const l-value, const l-values, r-values. 
Literals and const variables are not allowed to passed into the function as non-const references, 
because it requires that the value can be potentially modified. 

```c++
int add_1(const int &x, const int &y){return x + y;}  // const references
int add_2(int &x, int &y){return x + y;}  // non-const references
int add_3(int x, int y){return x + y;}  // copy values
std::cout << add_1(2,3) << std::endl; // 5  // literal arguments 
const int a=3,b=4; std::cout << add_1(a,b) << std::endl; // 7  // const variable arguments
int c=4,d=5; std::cout << add_1(c,d) << std::endl; // 9  // non-const variable arguments
// std::cout << add_2(2,3) << std::endl; // compile error
// std::cout << add_2(a,b) << std::endl; // compile error
std::cout << add_2(c,d) << std::endl; // 9  // non-const references are restrictive
// Non-pointer, non-fundamental variables (such as 'structs') should be passed by (const) reference.
```

C-style arrays in most cases decay to pointers when evaluated.
However, if it is passed by reference, the decaying won't happen. 

```c++
int arr[3] = {1, 2, 3};
int c_array_len_1(int arr[3]){return sizeof(arr) / sizeof(arr[0]);} // `arr` decay to pointers when evaluated
int c_array_len_2(int (&arr)[3]){return sizeof(arr) / sizeof(arr[0]);}
std::cout << c_array_len_1(arr) << ", " << c_array_len_2(arr) << std::endl; // 2, 3
```


### dynamic memory allocation

C++ supports 3 basic types of memory allocation: 

**Static memory allocation** happens for static and global variables. 
Memory for these types of variables is allocated once and persists throughout the life of your program. 

**Automatic memory allocation** happens for function parameters and local variables. 
Memory for these types of variables is allocated when the relevant block is entered, 
and freed when the block is exited. 

For static and automatic allocation: 
Memory allocation and deallocation happens automatically. 
The size of the variable/array must be known at compile time. 
They are allocated in a portion of memory called the **stack**. 
The amount of stack memory for a program is generally quite small, 
e.g. 1MB. Stack overflow will result if exceed this number. 

**Dynamic memory allocation** is a way to request memory from a much larger memory pool called the **heap**, 
which can be gigabytes in size. Dynamically allocated memory has no scope, 
it stays allocated until it is explicitly deallocated using `delete` or until the program ends (the OS cleans it up). 

Note that the `delete` does not actually delete anything. 
It simply returns the memory to the OS for reassigning. 
The pointer variable after `delete` still has the same scope as before, 
and can be assigned a new value just like any other pointer. 

A pointer that is pointing to deallocated memory is called **dangling pointer**. 
Dereferencing or deleting a dangling pointer will lead to undefined behavior. 
Set deleted pointers to `0` (or `nullptr`) unless they are going out of scope immediately afterward.
```c++
int *ptr= new int; *ptr = 3; int *other_ptr = ptr; // `int *ptr= new int(3);` or `int *ptr= new int{3};`
std::cout << ptr << ", " << *ptr << std::endl; // 0x563d10e742c0, 3
delete ptr; ptr=nullptr; // ptr=0 // not dangling pointer anymore
// delete other_ptr; // `other_ptr` now is a dangling pointer, delete it again will lead to undefined behavior
```

By default, if `new` fails (e.g, the OS may not have enough memory), a `bad_alloc` exception is thrown. 
If this exception isn't properly handled, the program will simply terminate with an unhandled exception error. 

```c++
int *ptr = new (std::nothrow) int(6); // tell `new` to return a null pointer if memory can't be allocated 
if(!ptr){std::cout << "Could not allocate memory" << std::endl;} // check if allocated
if(!ptr){ptr = new int;} // null pointers allows us to conditionally allocate memory
delete ptr; // nothing will happen if it is null
```

**Memory leaks** happen when a program loses the address of dynamically allocated memory, 
which cannot be released for reuse because no longer knows where it is. 
It results from a pointer going out of function scope or being reassigned with another value. 

```c++
void addr_lost(){int *ptr= new int;} // memory address lost after local pointer going out of function scope
int v = 1; int *ptr= new int; ptr = &v; // `delete ptr;` before `ptr = &v;` to fix it
int *ptr = new int; ptr = new int; // double allocation
```

### pointers

Either `->` or `.` operator can be used to access a member of a struct (or class). 
When using a pointer to access the value of a member, use `->` instead of `.` operator. 


```c++
void *ptr_random; // void pointer(or generic pointer) can point at values of any data type
void *ptr_zero=nullptr; // or `void *ptr_zero=0;` prior to c++11 
std::cout << ptr_random << ", " << ptr_zero << std::endl; // 0x55f2ff557380, 0
// void pointer does not know what type of object it is pointing to, cannot be dereferenced directly
int a = 6; void *void_ptr= &a; // ultimately, it is up to you to keep track of the type.
// must first be explicitly cast to another pointer type before it is dereferenced
int * int_ptr = static_cast<int*> (void_ptr); 
// Note that there are better ways such as function overloading and templates to to handle multiple data types
```

### built-in array & c-style string


```c++
int f_arr[3];  // fixed array with specified length
// int f_arr_1[3] = {1,2,3}; // or `int f_arr_1[] = {1,2,3};` (for C++03)
int f_arr_1[]{1,2,3};  // implicit array size, initialized using 'uniform initialization'
char f_arr_2[]{"Hello"}; // initialized using a C-style string literal
// int *d_arr = new int[3]; // note that a pointer must be used to hold its address
int *d_arr = new int[3]();  // dynamically allocated array initialized with 0's // `new[]` operator is called
// int *d_arr_1 = new int[]{1,2,3};  // implicit array size is not allowed for dynamically allocated array
int *d_arr_1 = new int[3]{1,2,3};  // must use explicit array size // (C++11)
// char *d_arr_2 = new char[3]{"Hello"};  // not allowed
delete[] d_arr; // use the scalar version of `delete` on an array will result in undefined behavior
```

When an array is dynamically allocated, the `new[]` operator is called, 
even though the `[]` isn't placed next to the `new` keyword.
the `new[]` keeps track of how much memory was allocated to a array, so that `delete[]` can delete the proper amount. 
However this size/length isn't accessible to the programmer. 

Note that C arrays or pointers are neither objects nor structures, they have no size/length parameter available by default. 
Usually functions that take arrays also take a length parameter to deal with this problem. 

```c++
int a[7];
std::cout << sizeof(a) << ", " << sizeof(a)/sizeof(*a) << std::endl; // 28, 7
std::cout << sizeof(a) << ", " << sizeof(a)/sizeof(a[0]) << std::endl; // 28, 7
std::cout << sizeof(a) << ", " << sizeof(a)/sizeof(int) << std::endl; // 28, 7
int *b = new int[2]; int *c = new int[7]; int *d = new int[100]; // note that `sizeof` doesn't work on pointers
std::cout << sizeof(b) << ", " << sizeof(c) << ", " << sizeof(d) << std::endl; // 8, 8, 8
char* s1 = "hi"; char* s2 = "hello";
std::cout << sizeof(s1) << ", " << sizeof(s2) << std::endl; // 8, 8
```

Note that a **fixed array can decay into a pointer** that points to the first element of the array.
Therefore the length of the fixed array is not available and neither is the size of the array via `sizeof()`. 
A **dynamically allocated array starts as a pointer** that points to the first element of the array. 
Consequently, it has the same limitation.

C++ does not provide a built-in way to resize an array that has already been allocated. 
It is possible to work around this limitation by dynamically allocating a new array, copying the elements over and deleting the old array.



### default arguments

Default argument can be declared in either the forward declaration or the function definition, but not both. 
It can only be declared once and can not be redeclared. 
Best practice is to declare the default argument in the forward declaration, e.g. in a header file.
All default parameters must follow any non-default parameters. 


```c++
void setValues(int x){} // the conflict between default arguments and function overloading
void setValues(int x, int y=0){} // parameter signature not unique
```

### pointers to pointers & dynamic multidimensional arrays 





## OOP 


### constructor & initializer list

The purpose of constructors is to initialize objects that have just been created by the compiler. 

A class without any **public constructors** can't be instantiated. 
If your class has no other constructors, C++ will automatically generate a public default/implicit constructor for you. 

Member **initializer lists** allow us to initialize member variables directly rather than assign (copy) values to them. 
It is the only way to initialize const or reference members, 
and it can be more performant than assigning values in the body of the constructor. 

Note that variables in the initializer list are not initialized in the order that they are specified in the initializer list. 
Instead, they are initialized in the order in which they are declared in the class. 

```c++
int a = 1; // copy initialization
double b(2.5); // direct initialization
char c{'c'}; // uniform initialization (C++11 only)
struct Date_struct{int year; int month; int day;}; // public member by default
class Date_cls{ // 3 access specifier: `public`, `private`, and `protected`
   const int year; int month; int day; // private member by default // const variable must be initialized
public: // note that there is no return type for constructor
   Date_cls(): year(2021),month(1),day(1) {}//no parameters(or all have default values) => default constructor
   Date_cls(int y, int m, int d=1): year(y),month(m),day{d} {} // directly/uniform initialization
   // favor uniform initialization over direct initialization if C++11 compatible
   // constructors can be reduced by using default arguments
   void setDate(int m, int d=1){this->month=m;day=d;}
   void printDate(){std::cout << year << "-" << month << "-" << day << std::endl;}
};
Date_cls *d_1 = new Date_cls(2021,3); Date_cls *d_2 = new Date_cls{2021,3,8};
d_1->printDate(); d_2->printDate(); delete d_1; delete d_2; // 2021-3-1 // 2021-3-8 // `->` for pointers
Date_cls d_3(2021,1); Date_cls d_4{2021, 1, 8}; // direct initialization & uniform initialization
d_3.printDate(); d_4.printDate(); // 2021-1-1 // 2021-1-8 // `.` operator for regular object variables
Date_cls d_6; d_6.printDate(); // 2021-1-1  // default constructor
Date_cls d_5 = {2021,3,6}; d_5.printDate(); //2021-3-6  // copy initialization using `=` // *less efficient*
Date_cls d_7 = d_5; d_7.printDate(); // 2021-3-6  // copy initialization 
class Foo{int x; public: Foo(int a):x{a}{std::cout << "x initialized:";} int get(){return x;}};
Foo f = 7; std::cout << f.get() << std::endl;  // or `Foo f = {7};` // x initialized:7 // copy initialization
// note that the `Foo` constructor only called once 
class Bar{public: const int arr[5]; public: Bar(): arr{1, 2, 3, 4, 5} {} }; // initialized with default values
Bar b; int len = sizeof(b.arr) / sizeof(int); // calculate the size of array
for(int i=0;i<len;i++){std::cout << b.arr[i] << " ";} std::cout << "\n"; // 1 2 3 4 5
class Num{int n{9}; public: int get(){return n;}};
Num n; std::cout << n.get() << std::endl; // 9
class P{ public: int x,y; public: P(int a,int b): x{a}, y{b}{}; P(): P(0,0){} }; // delegating constructors
```

Starting with C++11, constructors are now allowed to call other constructors. 
This process is called **delegating constructors** (or constructor chaining). 
Simply **call the constructor in the initializer list** instead of in the body of the constructor. 

Note that: A constructor that delegates to another constructor is not allowed to initialize any particular member itself, 
either delegate or initialize, but not both.
It's possible for constructor delegating to form an infinite loop that will cause your program to crash. 
This can be avoided by ensuring all of your constructors resolve to a non-delegating constructor. 

```c++
// A non-constructor function that does the common initialization
class F { public: F(){init();}; F(int x){init();}; private: void init(){}}; 
```


### destructors 


Destructor is designed to clean up the resources held by an object, e.g. dynamic memory, file or database handle. 
It is automatically called and typically the last thing to happen before the object is destroyed. 
e.g. an object goes out of scope or a dynamically allocated object is deleted. 

Destructor has the same name as the class, preceded by a tilde `~`. It can not take arguments and has no return type. 
Only one destructor per class as there is no way to overload destructors. 
```c++
class A { public: ~A(){std::cout << "destructor called" << std::endl;} }; A a; // destructor called
```

Note that objects holding resources should not be dynamically allocated 
because if the user forgets to **manually** `delete` the object, the destructor will not be called. 
If your class dynamically allocates memory, 
do not dynamically allocate objects of your class. Put them in the stack, 
so that the memory allocated by your class can be **automatically cleaned up** (in the destructor). 
Note the difference between "*the class dynamically allocates memory*" and "*dynamically allocate objects of your class*". 


### class definition & member function chaining 

```c++
class V{ int v; public: V(int v=0); // contructor declaration with default value
        V& add(int x); V& sub(int x); V& mul(int x){ v*=x; return *this;} // member function declaration
        V& set(int v){ this->v=v; return *this;} int get(){return v;} }; // `this` is a pointer
V::V(int v): v{v}{} // constructor definition outside the class definition
V& V::add(int x){ v+=x; return *this;} V& V::sub(int x){ v-=x; return *this;} // member function definition
V val; std::cout << val.add(8).mul(2).sub(6).get() << std::endl;  // 10
// note that operator `<<` returns `*this`, which is the `std::cout` object, also chainable
```

Traditionally, the class definition is put in a header file of the same name as the class, 
and the member functions defined outside of the class are put in a `.cpp` file of the same name as the class. 

Types (include classes) are exempt from the one-definition rule. 
Therefore we can `#including` class definitions in the header into multiple code files. 
Member functions defined inside the class definition are considered implicitly inline. 
**Inline functions** are exempt from the one-definition rule, 
which means we can define **trivial member functions** (such as getters & setters, trivial constructors / destructors, etc) 
inside the class definition in the header.
Non-trivial member functions defined outside the class definition are 
subject to the one-definition rule and should be defined in a `.cpp` file. 

Default parameters for member functions should be **declared** in the class definition (in the header), 
where they can be seen by whomever `#includes` the header. 

Separating the class definition and member function definitions is very common for libraries. 
e.g., you can `#include <iostream>` header but no `iostream.cpp`. 
Declarations from the header files are needed for the compiler to validate your program are syntactically correct. 
The implementation (member function definitions) is contained in a precompiled file that is linked in at the link stage.


### `static` member

`static` members are shared by all objects with the same class. 

```c++ 
int getID(){static int id = 0; return ++id;} //static variables not destroyed after go out of function scope
for(int i=0; i < 3; ++i){std::cout << getID() << " ";} // 1 2 3 
//Static members must be explicitly defined outside of the class in the global scope(just like global variable)
class A{public: static int x;}; int A::x=1; // declaration & definition of a static member
// when the static member is a `const` type, it can be initialized inside the class definition 
class B{private: static const int x{9}; public: static int get();}; int B::get(){return B::x;} // or `x=1`
std::cout << A::x << ", " << ++(A::x=2) << "\n"; // 1, 3 
// static member functions are used to access static members without the need to instantiate an object
std::cout << B::get() << "\n"; // 9
```
If the class is defined in a header, the static member definition is usually placed in the associated `.cpp` file. 

`static` `constexpr` members of any type that supports `constexpr` initialization can be initialized inside the class. 

When `static` is applied to global variables, it gives them "internal linkage" 
(which restricts them from being seen/used outside of the file they are defined in). 



### nested types in classes


```c++
class Foo{ // classes act as a namespace for any nested types
public: enum Type{A,B,C}; // `enum` nested in class
private: Type t;
public: Foo(Type ft):t{ft}{} Type getType(){return t;}
};
Foo f{Foo::B}; std::cout << f.getType() << std::endl; // 1 // `B` can be accessed by `Foo::B`
// C++ does not support static constructors 
// If initializing static member variables requires executing code (e.g. a loop): 
class Bar{
static std::vector<int> d; // declaration
public: static std::vector<int>& get(){return d;}
   class _init{ public:_init(){d.push_back(1);d.push_back(2);d.push_back(3);} }; 
private: static _init initializer; // declaration // initialize static member `d` using nested class `_init`
};
std::vector<int> Bar::d; // definition // allocated in the stack
Bar::_init initializer;  // definition // allocated in the stack
for(int i=0; i<3; ++i){std::cout << Bar::get()[i] << " ";} std::cout << "\n"; // 1 2 3
```


### anonymous objects

```c++
class Num{int n; public: Num(int x=0):n{x}{} int get(){return n;}}; 
Num a(1); std::cout << a.get() << std::endl; // normal object
std::cout << Num(2).get() << std::endl; // anonymous object 
```

Anonymous objects are primarily used to pass values without creating temporary variables. 
They are treated as r-values and can only be passed by value or const reference. 
Note that dynamical Memory allocation is also done anonymously, which is why its address must be assigned to a pointer. 


### const objects and member functions 

Once a `const` object has been initialized via constructor, any attempt to modify the member variables is not allowed, 
which includes changing member variables either directly or by calling member functions. 

`const` objects can only explicitly call `const` member functions. 
A `const` member function guarantees it will not modify the object 
or call any non-const member functions (as they may modify the object). 
Constructors **cannot** be marked as `const`.

Make any member function that does not modify the state of the class object const, 
so that it can be called by const objects

```c++
class P { public: int x,y; public: P(int a=0, int b=0):x{a},y{b}{}
int getX() const {return x;} P& setX(int a){x=a;return *this;}; int getY() const; P& setY(int b); };
int P::getY() const {return y;} P& P::setY(int b){y=b;return *this;} // `const` keyword used in definition
const P p(1,2); std::cout << p.getX() << ", " << p.getY() << std::endl; // 1, 2
// p.setX(0); // compile error // p.x=0; // compile error
```

We can overload const and non-const function 
because the const-ness is considered part of the function's signature. 

```c++
class M{ std::string msg; public: M(std::string s):msg{s}{}
    std::string& get(){return msg;} const std::string& get() const {return msg;} };
M m("Hello"); m.get() += " World"; std::cout << m.get() << std::endl; // Hello World
const M m1("Hello"); std::cout << m1.get()+"_World" << std::endl; // Hello_World
```

It is common to pass objects by const reference, your classes should be **const-friendly**, 
which means any member function that does not modify the object state should be `const`. 


```c++
// `const` also works with pointers 
// where `const` is put determines whether the pointer or the variable it points to is constant 
const int * a; // a variable pointer to a constant `int`
int const * b; // a variable pointer to a constant `int` (alternative syntax)
int * const c = nullptr; // a constant pointer to a variable `int`
int const * const d = nullptr; // a constant pointer to a constant `int`
const int * const e = nullptr; // a constant pointer to a constant `int` (alternative syntax)
class A{ const int * const foo(const int * const & x) const {}; }; 
///
char * foo(){ return "some text";} // the program could crash if `foo()[1]='a';`
const char * bar() {return "some text";} // the error would be spotted by compiler if `bar()[1]='a';`
```







## Inheritance 

### constructors & destructors


When constructing a derived class, the derived class constructor will call the constructor of its direct/immediate base class at first. 
If there is no constructor of its direct base is specified in the derived constructor initialization list, 
the default constructor of its direct base will be called. 
The classes are constructed in the order from most base to most derived. 
Note that it doesn't matter where in the derived constructor initialization list the base constructor is called, 
it will always execute first. 

```c++
class A { public: int a; ~A(){std::cout << "A destructor called.\n";}
	A(int x=0): a(x) {std::cout << "A constructor called, ";} };
class B: public A { public: int b; ~B(){std::cout << "B destructor called, ";} // public inheritance
	B(int x=0, int y=0): A(x),b(y) {std::cout << "B constructor called, ";} };
class C: public B { public: int c; ~C(){std::cout << "C destructor called, ";} // public inheritance
	C(int x=0, int y=0, int z=0): B(x,y),c(z) {std::cout << "C constructor called.\n";} }; ///
C c(1,2,3); // A constructor called, B constructor called, C constructor called.
            // C destructor called, B destructor called, A destructor called.
```



When a derived class is destroyed, each destructor is called in the reverse order of construction. 
In the above example, when `C` is destroyed, the `C` destructor is called first, then the `B` destructor, then the `A` destructor. 



### public / protected / private inheritance


`protected` members can only be accessed by the class they belong to, their friends or derived classes. 
They are not accessible from outside the class. 
Note that although derived classes cannot access the private members of its base class directly, 
they can still be accessed through access functions. 

<!--( With public inheritance, public members from the base class stay public in derived class and protected members stay protected. 
Private members in the base class are inaccessible for the derived class. 
With private inheritance, all members from the base class are inherited as private, which means protected and public members become private, 
and private members are still inaccessible. 
With protected inheritance, the public and protected members become protected, and private members stay inaccessible. ) -->


| :---------------------: | :--------------------------------------------: | :--------------------------------------------: | :------------------------------------------- : |
|    Base class member    |      Derived class (private inheritance)       |       Derived class (protected inheritance)    |       Derived class (public inheritance)       |
| :---------------------: | :--------------------------------------------: | :--------------------------------------------: | :--------------------------------------------: |
|        private          |             inaccessible directly              |              inaccessible directly             |              inaccessible directly             |
|       protected         |            (inherited as) private              |             (inherited as) protected           |             (inherited as) protected           |
|        public           |            (inherited as) private              |             (inherited as) protected           |             (inherited as) public              |
{: class="info" style=""}


In practice, **private inheritance and protected inheritance are rarely used**. 
**Use public inheritance** unless you have a specific reason to do otherwise. 



### friend function in base class

```c++
class N1{ int n; public: N1(int x=0):n(x){}
    friend std::ostream& operator<<(std::ostream& o, const N1& x); };
std::ostream& operator<<(std::ostream& o, const N1& x){ o << "In N1: " << x.n; return o;}
class N2: public N1 { int n; public: N2(int x=0): N1(x){}
    friend std::ostream& operator<<(std::ostream& o, const N2& x); };
std::ostream& operator<<(std::ostream& o, const N2& x){ o << "In N2: " << static_cast<N1>(x); return o;}
/// use `static_cast` to invoke the friend function in base class 
std::cout << N2(3) << std::endl; // In N2: In N1: 3 
```

### change access specifier & hiding member function


You can only change the access specifiers of base members the derived class can directly access. 
Therefore, a base member can never be changed from `private` to `protected` or `public`. 

<!-- As of C++11, `using` declarations are the preferred way of changing access levels. 
Altough "access declaration" works identically to the `using` declaration, it is considered deprecated. -->

```c++
class N3 { public: int n; N3(int x=0): n(x){} protected: void foo(){std::cout << "foo" << std::endl;} };
class N4: public N3 { public: N4(int x=0): N3(x){} using N3::foo; private: using N3::n; }; // 
/// `using` declarations to change access level
N3 a(2); a.n; // a.foo() is inaccessible
N4 b(2); b.foo(); // b.n is inaccessible
```

In a derived class, it is possible to hide base member functions by marking base member functions as `= delete`, 
which ensures they can not be called at all through a derived object. 
However the derived object can still access those hidden base functions 
by upcasting the derived object to a base object using `static_cast`. 

### multiple inheritance 


```c++
class O { public: void foo(){std::cout << "O" << std::endl;} }; 
class A: public O { public: void bar(){std::cout << "A" << std::endl;} };
class B: public O { public: void bar(){std::cout << "A" << std::endl;} };
class C: public A, public B {};
C c; c.A::bar(); // A // solve the ambiguity problem by explicitly specifying which version to call
c.A::foo(); // O // diamond problem: inherits from two classes which each inherits from a single base class 
```

**Avoid multiple inheritance** unless alternatives lead to more complexity. 
Multiple inheritance makes the language too complex and ultimately causes more problems than it fixes. 



## Operator Overloading

### basics

When evaluating an expression containing an operator, the compiler uses the following rules: 
<br>
If all operands are fundamental data types, the compiler will call a built-in routine. 
Produce a compiler error if does not exist. 
<br>
If any of the operands are user data types, the compiler looks to see 
whether the type has a matching custom overloaded operator function. 
If none can be found, it will try to convert one or more of the user-defined type operands into fundamental data types 
so it can use a matching built-in operator (via an overloaded typecast). 
Produce a compile error if that fails. 

Almost all operators in C++ can be overloaded, 
except: conditional `?:`, sizeof, scope `::`, member selector `.`, and member pointer selector `.*`.
At least one of the operands in an overloaded operator must be a user-defined type. 
All operators keep their default precedence and associativity. 

We can overload operators using any of the follwing: member function, friend function or normal function. 

```c++
class Num{ int n;
public: Num(int x):n{x}{} int get() const {return n;}
    friend Num operator+(const Num& a, const Num& b); // friend function
    Num operator*(const Num& b); // member function
    friend Num operator+(const Num& a, const int b); // overload operators for operands of different types
    friend Num operator+(const int a, const Num& b);
};
Num operator+(const Num &a, const Num &b){return Num(a.n + b.n);} // private properties can be accessed
Num operator-(const Num &a, const Num &b){ return Num(a.get() - b.get());} // normal function
Num Num::operator*(const Num &b) {return Num(n * b.get());} // member function
Num operator+(const Num& a, const int b){return Num(a.n + b);} // operands of different types
Num operator+(const int a, const Num& b){return b + a;} // implement it by calling the above function 
///
Num a(2), b(3); std::cout << (b-a).get() << ", " << (a*b).get() << std::endl; // 1, 6
std::cout << (a+b).get() << ", " << (b+1).get() << ", " << (1+b).get() << std::endl; // 5, 4, 4
```


C++ requires assignment `=`, subscript `[]`, function call `()` and member selection `->` must be overloaded as member functions. 
If you're overloading an unary operator (e.g. `++`), do so as a member function. 
Note that we can't overload operators as member functions if the **left operand** is either not a class 
or it is a class that we can't modify (e.g. `std::ostream`). 
If you're overloading a binary operator that modifies its left operand (e.g. `+=`), do so as a member function if you can, 
otherwise (e.g. `+`), do so as a normal function or friend function.

The difference between normal function and friend function is that 
the friend function declaration inside the class is placed in the header. 
As for the normal function, you have to provide a declaration in the header separately. 
To reduce the exposure of private members, 
overloading operators as normal functions is preferred 
if it is possible to do so without adding additional functions. 

### overloading typecasts

Overloading the typecast operators allows us to convert our object into other data types, including user-defined types.

```c++
class N{ int n; public: N(int x=0):n{x}{}
    operator int(){return n;} // trivial // no return type needed
    // operator Num(){return Num(n);} // convert to other custom types
};
std::cout << 1+N(2) << std::endl; // 3 // implicitly cast to an `int`
std::cout << static_cast<int>(N(5)) << std::endl; // 5 // explicitly cast to an `int`
if(typeid(N()+0) == typeid(int)){std::cout << "int" << std::endl;} // int
```

### copy constructor & converting constructor


A copy constructor is a special type of constructor that can be implicitly called to copy an existing object. 
By default, C++ will provide a `public` copy constructor, 
each member of the copy is initialized directly from the corresponding member of the one being copied (memberwise initialization, shallow copy). 
We can prevent copies by making the copy constructor `private` or using the `delete`. 
<br>
**Copy elision** means that the compiler is allowed to opt out of calling the copy constructor 
and just do direct initialization instead. 
As of C++17, some cases of copy elision have been made mandatory. 

```c++ 
class F { int n,d; // numerator & denominator 
public: F(int x=0,int y=1):n{x},d{y}{assert(y!=0);} // default constructor 
    F(const F& f):n{f.n},d{f.d}{std::cout << "**copy constructor called\n";} // copy constructor 
    // note we can access the private member `f.n` directly, even if `f` is a different object. 
    friend std::ostream& operator<<(std::ostream&,const F& f); // overloading operator `<<` 
    friend F toNegative(F f); // parameter passed by copy (copy elision in the following example) 
};
std::ostream& operator<<(std::ostream& o,const F& f){ o << f.n << "/" << f.d; return o; }
F toNegative(F f){f.n = -f.n;return f;} // return by copy (copy constructor called here) 
/// note that all custom anonymous objects are elided by the compiler 
std::cout << F(1,3) << std::endl; // 1/3 
F f = 2; std::cout << f << std::endl; // 2/1  // implicit conversion
// the compiler will find a converting constructor to convert `2` to a `F`, which is the default constructor 
F f1(F(2,3)); // copy elision, which is equivalent to `F f1(2,3)`
toNegative(F(1,2)); // **copy constructor called 
```

This **implicit conversion** works for all kinds of initialization (direct, uniform, and copy). 
Constructors that can be used for implicit conversions are called **converting constructors** (or conversion constructors). 
With the new uniform initialization syntax in C++11, 
constructors taking multiple parameters can now be converting constructors. 

Note that making a constructor `explicit` only prevents implicit conversions.
To completely disallow a constructor from implicitly or explicitly converting, 
using the `delete` keyword (introduced in C++11) to delete the function. 
When a function is deleted, any use of that function will cause a compile error.
Note that the copy constructor and overloaded operators may also be deleted. 

```c++
class Str { int len; char* s; public: // Str(char) = delete;           // (See `S` class in next section)
    explicit Str(int l) : len{l} { s = new char[len]; } ~Str(){delete[] s;} 
    Str(const char* cs): len{static_cast<int>(strlen(cs))} { // initialize from c-style string
          if(len>0){s=new char[len]; for(int i=0; i<len; ++i){s[i]=cs[i];}} }
}; Str s1("hello"); Str s2 = "hello"; // Str(const char*)
// Str s = 'a'; // `explicit` only prevents implicit conversions
Str s3('a'); // allowed if only use `explicit` 
const Str& s4 = static_cast<Str>('a'); // explicit converting
```

Note that chars are part of the integer family, if the user tries to initialize a string with a `char`, 
the compiler will use the converting constructor `Str(int)` to implicitly convert the `char` to a `Str`. 




### overloading operator `[]` and `()` 

The subscript operator `[]` and parenthesis operator `()` must be overloaded as member functions. 
The `[]` is limited to one single parameter, 
however, the `()` can take as many parameters as we want (e.g. to index multidimensional arrays). 

```c++
class A{ const int len; int* a; public: A(int s=0):len{s}{ a = new int[s]; } ~A(){ delete[] a; }
   const int& size() const {return len;} int& operator[](int i); const int& operator[](int i) const; 
}; // define a non-const and a const version separately to deal with const objects
int& A::operator[](int i){ assert(i>=0 && i<len); return a[i]; } // returns a reference/l-value
const int& A::operator[](int i) const{ assert(i>=0 && i<len); return a[i]; } ///
A a(3); a[0]=1;a[1]=2;a[2]=3; for(int i=0; i<a.size(); ++i){std::cout << a[i] << " "; } // 1 2 3
A b(3); A* p=&b; std::cout << (typeid(b)==typeid(p[0])) << std::endl; // 1
```

Note that if you try to call `[]` on a pointer to an object, 
e.g., `p[0]` is assumed as an object of `A` and `p` is an array of `A`. 
<br>
The parameter does not need to be an integer. 
Overloaded `[]` to can be defined to take a `std::string`, or whatever you like. 


```c++
class S { int len; char* s; static int id_gen; const int id; 
public: S(char)=delete; S(int l):len{l},id{++id_gen}{s = new char[len];} // empty string initialization
    S(const S& x):len{x.len},id{++id_gen}{ // copy constructor // deep copy
        s = new char[len]; for(int i=0; i<len; ++i){ s[i]=x.s[i]; }
        std::cout << "copy constructor called @ id:" << id << std::endl; }
    S(const char* cs):len{static_cast<int>(strlen(cs))},id{++id_gen} { // initialize from c-style string
        if(len>0){s=new char[len]; for(int i=0; i<len; ++i){s[i]=cs[i];}} }
    ~S(){std::cout << "destructor called: " << s << std::endl; delete[] s;}
    S operator()(int a, int b) const; // parenthesis operator
    S& operator=(const S& x)=delete; // assignment without creating new object is not allowed
    friend std::ostream& operator<<(std::ostream& o, const S& t);
}; int S::id_gen=0; // initialize static member
S S::operator()(int a, int b) const { assert(a>=0 && b>a && b<=len); // definition
	S t(b-a); for(int i=a; i<b; ++i){ t.s[i-a]=s[i]; }
	std::cout << "** " << t << std::endl; return t; } // ** S | id:2 | len:5 | v:hello
std::ostream& operator<<(std::ostream& o, const S& t){
    o << "S | id:" << t.id << " | len:" << t.len << " | v:" << t.s; return o; }
///
S str("hello12345"); std::cout << str << std::endl; // S | id:1 | len:10 | v:hello12345
std::cout << str(0,5) << std::endl; // S | id:2 | len:5 | v:hello // copy elision
S a = str; std::cout << a << std::endl; // copy constructor called @ id:3 // S | id:3 | len:10 | v:hello12345 
```

Operator`()` is commonly overloaded to implement **functors (or function object)**, which are objects that operate like functions. 
The advantage of a functor over a normal function is that functor can store data in its member variables. 


### overloading operator `++` or `--` 

We can distinguish the prefix from the postfix operators 
by providing an `int` dummy parameter on the postfix version. 



```c++
class D{int n; public: D(int x=0):n{x}{}
    D& operator++(); // prefix
    D operator++(int); // postfix // member function
    friend std::ostream& operator<<(std::ostream& o, const D& x);
};
D& D::operator++(){++n;return *this;}
D D::operator++(int){int t=n;++n;return t;} // return value must be a non-reference for postfix
std::ostream& operator<<(std::ostream& o, const D& d) { o << d.n; return o; }
D d(6); std::cout << d++ << ", " << d << std::endl; // 6, 7
std::cout << ++d << ", " << d << std::endl; // 8, 8
```


Note the dummy parameter is not used in the function implementation, it has no name, 
which tells the compiler to treat this variable as a placeholder 
and it won't warn us that we declared a variable but never used it.

For the **postfix**, we use a temporary variable that holds the value of the object before it is incremented or decremented. 
The caller receives a copy of the object before it was incremented or decremented, 
which means its **return value must be a non-reference**. 
Because we can't return a reference to a local variable that will be destroyed when the function exits. 

### assignment operator `=`

For any object assignment, either copy constructor or assignment operator is implicitly called. 
If a new object has to be created, the copy constructor of newly created object is called, 
otherwise the assignment operator is called. 

Unlike other operators, the compiler will provide a **default public assignment operator** for your class if you do not provide one. 
This assignment operator does **memberwise assignment**, 
which is the same as the memberwise initialization (shallow) that default copy constructors do. 
You can prevent assignments by making assignment operator `private` or using the `delete`. 

Assignment operator must be overloaded as a member function. 
In most cases, a **self-assignment** doesn't has no overall impact. 
However, in cases where an assignment operator needs to dynamically allocate memory, 
self-assignment could be dangerous. 

```c++ 
class Str2 { int len; char* s; public: ~Str2(){delete[] s;}
    Str2(const char* cs): len{static_cast<int>(strlen(cs))+1} { // initialize from c-style string
        if(len>0){s=new char[len]; for(int i=0; i<len; ++i){s[i]=cs[i];}} s[len]='\0';}
    Str2& operator=(const Str2& x); // assignment operator
    friend std::ostream& operator<<(std::ostream& o, const Str2& t); };
Str2& Str2::operator=(const Str2& x){ if(this==&x){return *this;} // self-assignment guard
    if(len != x.len){ delete[] s; s=new char[x.len]; len=x.len; } for(int i=0; i<len; ++i){s[i]=x.s[i];} }
std::ostream& operator<<(std::ostream& o, const Str2& t){o << t.s; return o;}
Str2 s1 = "Hi"; Str2 s2 = "world"; Str2 s3 = "none"; // `Str2(const char*)` called
s2 = "operator="; // first `Str2(const char*)` called, then assignment operator called
s3 = s1; // no new object needed // assignment operator called
Str2 s0 = s1;    // new object has to be created // copy constructor called 
std::cout << s0 << ", " << s2 << ", " << s3 << std::endl; // Hi, operator=, Hi 
```





## Templates 

A template is not a class or a function, it is a stencil used to create classes or functions. 

### function templates

The basic idea behind function templates is to create a function without having to specify the exact types. 
Function is defined using placeholder types, called template type parameters. 
Note that there is no need to explicitly specify the **template type** in the function name 
so long as the **compiler can deduce it from the parameter types**. 

When the compiler encounters a call to a template function, 
it replicates the template function code and replaces the template type parameters with actual types. 
The function with actual types is called a function **template instance**. 
The compiler only creates one template instance per file. 
If a template function is not called, no template instance will be created. 

```c++
template<typename T> // template parameter declaration
const T& max(const T& a, const T& b) { return a > b ? a : b; } // note `std::max()` in the standard library
std::cout << max<int>(2, 3) << ", " << max(2.5, 3.5) << std::endl; // 3, 3.5 // compiler can deduce types 
class N{ int n; public: N(int x=0):n{x}{} operator int(){return n;};
    friend bool operator>(const N& x, const N& y);
    friend std::ostream& operator<<(std::ostream& o, const N& x);
    N& operator+=(const N& x){n+=x.n; return *this;};
    N operator/(const N& x){assert(x.n!=0);N t=n/x.n; return t;};
}; bool operator>(const N& x, const N& y){return x.n > y.n;} //
std::ostream& operator<<(std::ostream& o, const N& x){o << x.n; return o;}
template<class T> T average(T a[], int len){ T t=0; for(int i=0; i<len; ++i){ t+=a[i]; } return (t/N(len)); }
///
std::cout << max(N(2),N(3)) << std::endl; // 3
N arr[] = {N(1), N(2), N(3)}; std::cout << average(arr, 3) << std::endl; // 2
```

The downside of template functions: 
template functions often produce crazy-looking error messages that are much harder to decipher than those of regular functions. 
They can increase your compile time and code size, 
as a single template might be instantiated and recompiled in many files. 





### template classes

```c++
template<typename T>
class N { T n; public: N(T x=0):n{x}{}
    N& operator++(); // the same as `N1<T>& operator++()`
    template<typename R> operator R(){return R(n);} // each instance of `N` works with all typecast operators
    friend std::ostream& operator<<(std::ostream& o, const N& x){o << x.n; return o;}; 
};
template<typename T> //each templated member function defined outside the class also needs template declaration
N<T>& N<T>::operator++(){++n; return *this;} // note it is `N1<T>`, not `N1`
///
std::cout << N<int>(1) << ", " << N<double>(3.6) << ", " << ++N<int>(1) << std::endl; // 1, 3.6, 2
std::cout << static_cast<int>(N<double>(1.2)) << std::endl; // 1
```


Template classes are instantiated in the same way as template functions. 
The compiler stencils out a copy upon demand, 
with the template parameter replaced by the actual data type, and then compiles the copy. 
e.g., class `A<int>` is an instance of template class `A<T>`. 
If a template class is not used, no instance will be created. 


With non-template classes, the common procedure is to put the class definition in a header, 
and the member function definitions in a similarly named `.cpp` file. 
However, with templates, this does not work. 
Note that C++ compiles files individually. 
When a header is `#included` in a file, the template class definitions and the declarations of templated member functions are copied into the file. 
The compiler will only instantiate those included, and the definitions of templated member functions are excluded. 
Thus, there will be a linker error, because the definitions of templated member functions cannot be found. 

1. One solution is to simply put all the definitions of templated member functions in the header. 

2. An alternative is to put member function definitions into an similarly named `.inl` file (`.inl` for inline), 
and then include the `.inl` file from the bottom of the header, 
which yields the same result as putting all the codes in the header.

3. Another alternative is that in addition to the header and similarly named `.cpp` file, add a third file (e.g. `templates.cpp`), 
which includes both the header and the similarly named `.cpp` file and contains all of the instantiated classes you need, 
using `template class` command to explicitly instantiate these template classes. 
These functions compiled in the third file can be linked to from elsewhere. 
This method is more efficient, but requires maintaining the `templates.cpp` for each program. 

```c++
// templates.cpp // all the definitions of templated member functions are included
#include "Num.h"  
#include "Num.cpp" 
template class Num<int>; template class Num<double>; // explicitly instantiate `Num<int>` and `Num<double>`
```

### templated friend functions


 - All instances of template class are friends with all instances of templated non-member functions 
 
```c++ 
template <typename T> class N2{ T n; public: N2(T x=0):n{x}{} 
    template<class U> friend N2<U> foo(N2<U>& x); // note it must be `N2<U>`, not `N2` 
    // friend N2 foo(N2& x); // not allowed // compile error 
}; template<class U> N2<U> foo(N2<U>& x) { return x; } N2<double> pi(3.14);
struct dummy{}; template<> // define a random `foo` instance 
N2<dummy> foo(N2<dummy>& x){ std::cout << pi.n << std::endl; return x; } // private member access
/// every type of `foo` instance can access the private members of all types of `N2` instances
dummy d; N2<dummy> n(d); foo(n); // 3.14
```


 - Each instance of template class friends with the corresponding instance of templated non-member function 
 
```c++ 
template <typename T> class N4; // forward declare 'N4' class 
template <typename U> N4<U> foo(N4<U>& x); // forward declare 'foo' using the above `N4` declaration
template <typename T> class N4{ T n; public: N4(T x=0): n{x}{} // 'N4' class definition 
    friend N4 foo<T>(N4& x); // only friends with `foo<T>` 
}; template<typename U> N4<U> foo(const N4<U>& x) { return x; } // `foo` definition  ///
N4<int> a(5); foo(a); // This approach does not perform implicit conversions on parameters passed to them 
// int i=3; foo(i); // not allowed // no implicit conversion for templated non-member functon
// int j=3; foo(N4<int>(j)); // explicit conversion // compile error, `const` in `foo` definition is needed 
```

 - **ADL** (Argument Dependent Lookup) 
 
A **non-template friend function defined inside a template class** is unusual 
since the function is neither in the global scope nor a class member (not in the "standard" places). 
C++ has a feature called ADL that can search through functions that aren't in the current scope 
but exist in a class or namespace that is **suggested by the type of the arguments** passed to the function call. 

```c++
template<typename T> // approach that performs implicit conversions on parameters passed to them 
class N5{T n; public: N5(T x=0):n(x){} 
    friend N5 foo(N5& x){return x;} // non-template non-member function definition included 
    friend N5 operator+(const N5& a, const N5& b){return a.n+b.n;} // neither in the global scope nor a member 
    friend std::ostream& operator<<(std::ostream& o, const N5& x){o << x.n; return o;} 
}; N5<int> a{2}, b{3}; int i = 2; foo(a); std::cout << a+b << std::endl; // 5 
std::cout << i+b << std::endl; // 5 // implicitly convert `i` to `N5<int>` because of the 2nd argument 
//foo(i); // failed because no argument hints that it needs ADL to search functions not in the current scope 
```

Note that `foo` and `operator+` do not exist until a template class instance is generated. 

This approach generates a corresponding non-template friend function associated with each instance of a template class. 
However, it depends on ADL to find that function. 
When it fails to perform implicit conversions because of lacking argument hint, 
the compile errors may be odd (e.g., "function not declared in this scope" rather than "cannot convert argument"). 


### non-type template parameters

A non-type template parameter is a special kind of template parameter that is replaced by a value instead of a type. 


```c++
template<typename T, int len> // similar to `std::array`, e.g., `std::array<int, 5>`
class A{ T a[len]; // not dynamically allocated array
public: T& operator[](int i){assert(i>=0&&i<len); return a[i];} T* getArr();
}; template<typename T, int len> T* A<T,len>::getArr(){return a;}
///
A<int, 3> a; for(int i=0; i<3 ; ++i){ a[i]=i+1; }
for(int i=0; i<3 ; ++i){ std::cout << a[i] << " "; } std::cout << "\n"; // 1 2 3
for(int i=0; i<3 ; ++i){ std::cout << a.getArr()[i] << " "; } std::cout << "\n"; // 1 2 3 
```

A non-type parameter can be any of the following:
<br>
1. A value that has an integral type or enumeration
2. A pointer or reference to a class object
3. A pointer or reference to a function
4. A pointer or reference to a class member function
5. std::nullptr_t



### template specialization

 - function template specialization

```c++ 
template<typename T> class N{ T n; public: N(T x=0): n(x){} N foo(N& x); }; 
template<typename T> N<T> N<T>::foo(N<T>& x){std::cout << "T" << std::endl; return x;} // general definition
template<> // template function without template parameters 
N<double> N<double>::foo(N<double>& x){std::cout << "double" << std::endl; return x;} // specialized definition
///
N<int> i; i.foo(i); N<double> d; d.foo(d); // T // double
/// fix 'shallow copy' using template specialization
template<typename T>
class N1{ public: T n; N1(T x=0): n(x){} ~N1(){}; // default constructor & destructor
    N1& operator=(const N1&)=delete; N1(const N1& x): n(x.n){}; }; // copy constructor
template<> // specialized destructor
N1<char*>::~N1(){ if(n){delete[] n; n=nullptr;} } // there must be a destructor in the class 
template<> // specialized default constructor // deep copy 
N1<char*>::N1(char* x){ int len=strlen(x)+1; n=new char[len]; for(int i=0; i<len; ++i){n[i]=x[i];} }
template<> N1<char*>::N1(const N1<char*>& x):N1(x.n){} // specialized copy constructor // deep copy 
///
N1<char*> s1("hello"); std::cout << s1.n << std::endl; // hello // specialized default constructor called
N1<char*> s2 = "world"; std::cout << s2.n << std::endl; // world // specialized default constructor called
N1<char*> s3 = s1; std::cout << s3.n << std::endl; // hello // specialized copy constructor called
// s3 = s2; // compile error // shallow copy `operator=` deleted
N1<int> a(2); std::cout << a.n << std::endl; // 2
```

 - class template specialization

Class template specialization allows us to specialize a template class for particular data types. 

Specialized template classes are treated as completely independent classes and need to be completely rewritten, 
we can make any change inside the specialized classes, just like it is a different class. 
However, it is better to keep a consistent interface so they can be used in exactly the same manner. 


```c++
template<typename T> class N2{ public: T n; N2(int x=0):n(x){} N2& foo(); };
template<typename T> N2<T>& N2<T>::foo(){ std::cout << "foo" << std::endl; return *this; }
template<> // member variables and functions in general definition are not available for specialized class
class N2<double>{ public: N2& bar(); }; // specialized classes have to be completely rewritten
N2<double>& N2<double>::bar(){ std::cout << "bar" << std::endl; return *this; } 
///
N2<int> b; b.n; b.foo(); // foo
N2<double> a; a.bar();   // bar // note that `a.foo()` and `a.n` are not available 
```

 - partial template specialization


Note that as of C++14, partial template specialization **can only be used with template classes**, 
not template functions (functions must be fully specialized).

```c++
template<typename T, int len=1> class A { T a[len]; public: T& operator[](const int i); };
template<typename T, int len> T& A<T,len>::operator[](const int i){assert(i>=0&&i<len);return a[i];}
template<int len=1> // used with class A
void print_s(A<char, len>& a){ for(int i=0; i<len; ++i){std::cout << a[i];}std::cout << "\n"; }
///
A<char, 3> a; a[0]='a';a[1]='b';a[2]='c'; print_s<3>(a); // abc
```

To partially specialize member functions, we need to partially specialize the entire class. 
A workaround is to create a sub-class of the template class and use `override` to do partial specialization. 

A template class can be partially specialized if we only use the pointer type, 
e.g. `T*` of `template <typename T>`, even though the underlying type is not specified. 

```c++
template<typename T>
class N3{ public: T n; N3(T x=0): n(x){} ~N3(){}; // default constructor & destructor
    N3& operator=(const N3&)=delete; N3(const N3& x): n(x.n){}; }; // copy constructor
template<typename T> // partial template specialization // a specialized class that works for pointers
class N3<T*>{ public: T* n; int len; N3& operator=(const N3&)=delete;
    N3(T* x=nullptr, int s=0):len(s){ // default constructor
        assert(x && s>0); if(x){ n=new T[len]; for(int i=0; i<len; ++i){n[i]=x[i];} } }
    N3(const N3& x): N3(x.n, x.len){}; // copy constructor that calls the default constructor
    ~N3(){if(n){delete[] n; n=nullptr;}}; // default  destructor
};
int x[] = {1 ,2 ,3}; N3<int*> a{x, 3}; // default constructor called
N3<int*> b = a; // copy constructor called, then it calls the default constructor 
std::cout << b.n[0] << ", " << b.len << std::endl; // 1, 3
N3<int> c(9); std::cout << c.n << std::endl; // 9 
```


### STL 


`std::vector` is actually a template class, 
and int is the type parameter to the template! 
The standard library is full of predefined template classes available for your use

## std 

Classes in the standard library that deal with dynamic memory, such as `std::string` and `std::vector`, 
handle all of their memory management, and have overloaded copy constructors and assignment operators that do proper deep copying. 


### std::size_t 


The standard C protocol says that a `long` should occupy at least 32 bits. 
When moving from `unsigned int` to `unsigned long`, the performance starts to degrade on some systems. 
Because running a 32-bit operator requires two or more instructions on most platforms, 
compared with one instruction for the 16 bits. 

<!-- `size_t` is an unsigned integer that can express the size of any memory range supported on a machine, 
and big enough to contain the size of the biggest object the system can handle. e.g., an array of 4 GB. --> 

`size_t` is actually an alias for some unsigned integer type, 
which can be `unsigned int`, `unsigned long`, or `unsigned long long`. 
The standard C implementation is free to choose the unsigned integer that is big enough (but not too much)
<!--big enough for our needs, but not bigger than what's needed,--> 
to represent the size of the largest possible object on our platform. 



### std::vector

### std::array

### std::string

### std::string_view 

Unlike `std::string`, which keeps its own copy of the string, 
`std::string_view` provides a view of a existing string. 
When the underlying string is modified, the `std::string_view` version appears to be changing as well. 

`std::string_view` contains methods that manipulate the view without modifying the viewed string. 

```c++
char* s = "hello"; std::string_view sv{s}; std::cout << sv << '\n'; // hello
sv.remove_prefix(1); std::cout << sv << '\n'; // ello // remove the 1st character from the view
sv.remove_suffix(2); std::cout << sv << '\n'; // el // remove the last 2 characters from the view
std::cout << s << '\n'; // hello 
```

Unlike c-style strings and `std::string`, `std::string_view` doesn't use `null` terminators to mark the end of the string. 
Instead, it keeps track of the length of a string. 

```c++
char s[]{ 'a', 'b', 'c' }; // not null-terminated
std::string_view str{ s, std::size(s) }; std::cout << str << '\n'; // need to pass the length manually
```

If **the viewed string goes out of scope**, `std::string_view` has nothing to observe and accessing it causes undefined behavior. 

A `std::string_view` can be explicitly converted a `std::string`, 
which can be converted to a c-style string using `.c_str()`. 
However, creating a `std::string` every time we want to pass a `std::string_view` as a c-style string is expensive. 

The string being viewed by a `std::string_view` can be accessed by using the `.data()`, which returns a c-style string. 
Only use `std::string_view::data()` if the view hasn't been modified and the string being viewed is null-terminated, 
otherwise it may lead to undefined behavior. 

Note that `std::string_view::npos` or `std::string::npos` is a constant defined with a value of `-1`, 
which is the greatest possible value of type `size_t`, representing a non-position or indicating no matches(until the end of a string). 



### smart pointer


## lambda expression (anonymous functions)

### Intro

**Lambdas are functor objects** containing overloaded `operator()` that make them callable like a function.


```c++
// [ capture-clause ] ( parameters ) -> returnType { statements; }
[]() {}; // a lambda with no captures, no parameters, and no return type
```
The **capture-clause** and **parameters** can both be empty if they are not needed. 
The **return-type** is optional, and if omitted, `auto` will be assumed 
(thus using type inference used to determine the return-type). 

```c++ 
// static means internal linkage in this context 
static bool contains_c(std::string_view str) { return (str.find("c") != std::string_view::npos); }
constexpr std::array<std::string_view, 4> arr { "a", "b", "c", "d" };
const auto r1 { std::find_if(arr.begin(), arr.end(), contains_c) }; // using regular function `contains_c`
const auto r2 { std::find_if(arr.begin(), arr.end(),                // using lambda expression
                         [](std::string_view str) { return (str.find("c") != std::string_view::npos);}) };
if (r2 == arr.end()) { std::cout << "No 'c'\n"; } else { std::cout << "Found '" << *r2 << "'\n"; } //Found 'c' 
///
constexpr std::array<int, 4> arr { 1, 2, 3, 4 };
auto isEven{ [](int i) { return ((i % 2) == 0); } }; // store the lambda in a named variable
auto r{ std::all_of(arr.begin(), arr.end(), isEven) }; 
/// there are several ways of storing a lambda
// A regular function pointer. Only works with an empty capture clause.
double (*add1)(double, double){ [](double a, double b){return (a + b);} }; std::cout << add1(1, 2) << "\n"; //3
// Using std::function. The lambda could have a non-empty capture clause.
std::function add2{ [](double a, double b){return (a + b);} }; 
std::cout << add2(3, 4) << "\n"; // 7 // note: pre-C++17, use `std::function<double(double, double)>` instead
auto add3{[](double a, double b){return (a + b);} }; // Using auto. Stores the lambda with its real type.
std::cout << add3(5, 6) << "\n"; // 11
```

The only way of using the lambda's actual type is by means of `auto`. 
`auto` also has the benefit of having no overhead compared to `std::function`. 
In cases where the actual lambda is unknown, 
e.g. when passing a lambda to a function as a parameter, `std::function` should be used rather than `auto`. 

```c++
void foo(int n, const std::function<void(int)>& fn){ for(int i{0}; i < n; ++i){fn(i);} }
foo(3, [](int i){std::cout << i << ' ';}); std::cout << '\n'; // 0 1 2
```


Since C++14 `auto` are allowed to use for lambda parameters, which is just a shorthand for a template parameter, 
a unique lambda will be generated for each different type. 
(in C++20, regular functions are allowed to use `auto` for parameters too). 
<!--
When a lambda has one or more `auto` parameter, 
the compiler will infer what parameter types are needed from the calls to the lambda. -->
Lambdas with one or more `auto` parameter can potentially work with a wide variety of types, 
they are called **generic lambdas**. 

However, `auto` isn't always the best choice for lambda parameters, 
e.g., using `auto` could infer a string type of `const char*` instead of `std::string`. 

Note that **static variables** used by generic lambda are not shared between the generated lambdas of different types. 

```c++
auto print{ [](auto v){static int c{0}; std::cout << c++ << ": " << v << ", ";} };
print("a"); print("b"); print(1); print(2); // 0: a, 1: b, 0: 1, 1: 2, 
```

The return type of a lambda is deduced from the `return` statements inside the lambda. 
All return statements in the lambda must return the same type. 
When the return type is explicitly specified for a lambda, the compiler will do implicit conversions. 

Standard library function objects
For common operations (e.g. addition, negation, or comparison) you don’t need to write your own lambdas,
because the standard library comes with many basic callable objects that can be used instead. These are defined
in the <functional> header. 

The standard library comes with many basic callable objects defined in the `<functional>` header. 

```c++
std::array arr{ 3, 0, 2, 1 };
std::sort(arr.begin(), arr.end(), std::greater{}); // curly braces are needed to instantiate object
for (int i : arr){ std::cout << i << ' '; } std::cout << '\n'; // 3 2 1 0 
```

### lambda captures 

Unlike nested blocks, where any identifier defined in an outer block is accessible in the
scope of the inner block, lambdas can only access: global identifiers, entities that are
known at compile time, and entities with static storage duration. 

We need to list the entities we want to access from within the lambda as part of the capture clause. 
The captured variables of a lambda are clones of the outer scope variables, not the actual variables. 
Lambdas are functor objects that can be called like functions. 
When the compiler encounters a lambda definition, it creates a custom object definition for the lambda, 
each captured variable becomes a member variable. 
At runtime, where the lambda definition is encountered, the lambda object is instantiated, 
and these member variables are initialized from the outer scope variables of the same name at this point. 
Therefore, for each variable that the lambda captures, 
a clone is made (with an identical name) inside the lambda object. 


```c++
[o = std::ref(std::cout << "Hello ")](){ o.get() << "World" << '\n';}(); // Hello World
int c{9}; // note that the clone `c` variable in lambda object is shared across calls to the lambda
auto cd{ [c]() mutable {std::cout << "cloned `c`: "  << c-- << '\n';} }; // add `mutable` to modify clone `c`
cd(); cd(); std::cout << "old `c`: " << c << '\n'; // cloned `c`: 9  // cloned `c`: 8  // old `c`: 9 
/// capture by reference should be preferred to affect the actual variable
int a{1}; int b{2}; std::vector<int> d{}; [a, b, &d](){}; // capture `a` and `b` by value, and `d` by reference
[a, b, &c](){}; // capture `a` and `b` by value, and `c` by reference
[=, &c](){}; // capture `c` by reference and everything else by value
[&, b](){}; // Capture `b` by value and everything else by reference
// [&, &b](){}; // illegal, already said we want to capture everything by reference
// [=, b](){}; // illegal, already said we want to capture everything by value
// [b, &a, &b](){}; // // illegal, `b` appears twice.
// [b, &](){}; // illegal, the default capture has to be the first element in the capture group 
```


By default, when the lambda object is created, the lambda captures a constant copy of the outer scope variable, 
which means the lambda is not allowed to modify them. 
To allow modifications, mark the lambda as `mutable`, which will remove the `const` from all variables captured. 
Much like function parameters, we can also **capture variables by reference** to affect the actual argument. 

A **default capture** automatically captures all variables that are mentioned in the lambda. 
To capture all used variables by value, use `=`. 
To capture all used variables by reference, use `&`. 

We can define a new variable that's visible only to the lambda in the lambda-capture `[]` **without specifying its type**. 

If a outer scope variable captured by reference dies before the lambda, 
the lambda will be left holding a **dangling reference**. 
If we want it to be valid when the lambda is used, capture it by value instead. 


```c++
// `s` dies when `foo` returns // capture `s` by reference and return the lambda // undefined behavior
auto foo(const std::string& s) { return [&](){std::cout << s << '\n';}; } 
// auto say{ foo("Hello") }; say(); // note there is no closure in c++
```

Lambdas are objects that can be copied. 

```c++
void invoke(const std::function<void(void)>& fn) { fn(); } ///
int i{0}; auto count{ [i]() mutable {std::cout << ++i << '\n';} };
count(); auto otherCount{count}; // 1 // create a copy of count
count(); otherCount(); // 2 // 2
invoke(count); invoke(count); // 3 // 3
invoke(std::ref(count)); invoke(std::ref(count)); count();  // 3 // 4 // 5
```

When `std::function` is created with a lambda, 
the `std::function` internally makes a copy of that lambda object. 
`std::ref()` creates a `std::reference_wrapper`. 
By using `std::ref`, whenever anybody tries to make a copy of the lambda, 
it will make a copy of the reference rather than the actual object. 

Try to avoid lambdas with states altogether. 
**Stateless lambdas** are easier to understand and don't suffer from the above issues. 



## performance

### constexpr 

The idea of using `constexpr` is to **compute expressions at compile** time so that time can be saved when code is run. 

`const` can only be used with non-static member function 
whereas `constexpr` can be used with member and non-member functions, 
even with constructors but with condition that argument and return type must be of literal types.

<!-- Unlike `const`, `constexpr` **can also be applied to functions and class constructors**. 
`constexpr` indicates that the value or return value is constant 
and, where possible, is **computed at compile time**. 

 A `constexpr` integer can be used wherever a `const` integer is required, such as in template arguments and array size declarations. 
And when a value is computed at compile time instead of run time, it runs faster and uses less memory. -->

- `constexpr` functions 

<!-- A `constexpr` function is one whose return value is computable at compile time when 'consuming code' requires it. 
'Consuming code' requires the return value at compile time to initialize a constexpr variable, or to provide a non-type template argument. -->

When the arguments are `constexpr` values, a `constexpr` function is executed at compile time and produces a compile-time constant. 
When called with non-`constexpr` arguments, or when its value isn't required at compile time, it behaves like a regular function. 

```c++
// int j = 0; constexpr int k = j + 1; // compile error! `j` is not a constexpr 
constexpr int add(int x, int y){ return x+y; } 
constexpr int a = add(1,2); // compile-time evaluation 
```

### inline functions


When the compiler compiles the code, all `inline` functions are expanded in-place, 
i.e., the function call is replaced with a copy of the contents of the function itself, 
which removes the function call overhead. 
The downside is that because the `inline` function is expanded in-place for every function call, 
this can make your compiled code larger, 
especially when the `inline` function is long or there are many calls to the `inline` function. 

```c++
inline int max(int x, int y) { return x > y ? x : y; } // 
```

### timer


## virtual functions 


### Intro 

```c++ 
class A { public: void foo(){std::cout << "A" << std::endl;} }; 
class B: public A { public: void foo() override {std::cout << "B" << std::endl;} }; // polymorphism
B b; A& a = b; a.foo(); // A // use pointers or references to the base class to call the base version function
class A { public: virtual void foo(){std::cout << "A" << std::endl;} };
class B: public A { public: virtual void foo() override {std::cout << "B" << std::endl;} };
B b; A& a = b; a.foo(); // B // marked as `virtual` to call the more-derived-version function
```


If a function is marked as `virtual`, all matching overrides are also considered `virtual`, 
even if they are not explicitly marked as such. 
However, having the `virtual` keyword on the overrides serves as a reminder that the function is `virtual`. 
If a user tries to override a function or class that has been marked as `final`, the compiler will give a compile error. 

 - Never call virtual functions from constructors or destructors 
 
When a derived class is created, the base portion is constructed first. 
If you were to call a virtual function from the base constructor, and derived portion hadn't even been created yet, 
it would be unable to call the derived version of the virtual function. Consequently, the base version will be called instead. 
If a virtual function is called in a base class destructor, 
it will always resolve to the base class version of the function, 
because the derived portion will already have been destroyed. 
<!--Note that base class destructor can free memory allocated in the derived class by using virtual destructor. -->

 - Virtual function is inefficient 
 
Resolving a virtual function call takes longer than resolving a regular one. 
Furthermore, the compiler also has to allocate an extra pointer for each object that has one or more virtual functions. 

 - covariant return types

If the return type of a virtual function is a pointer or a reference to a base class, 
override functions can return a pointer or a reference to a derived class. 
If the virtual function is called with an derived object that is typed as a base object, 
it will return a base pointer or reference. 




### virtual destructors 

A base class destructor should be either `virtual` and `public`, or non-virtual and `protected`. 
If a base destructor is `virtual`, then all derived overrides will be considered
`virtual` regardless of whether they are specified as `virtual`.

Note that a class with a `protected` destructor cannot be deleted via a pointer, 
thus preventing the accidental deleting of a derived class through a base pointer when the base class has a non-virtual destructor. 
However, this also means the base class can't be deleted through a base class pointer, 
which essentially means the class can't be dynamically allocated or deleted except by a derived class. 
This also precludes using smart pointers (such as `std::unique_ptr` and `std::shared_ptr`) for such classes. 
It also means the base class can't be allocated on the stack. 

There is a performance penalty for making all destructors `virtual`. 
If your class will be inherited, use the `virtual` destructor . 
Because when a derived object held by a base pointer or reference is deleted, 
the most derived version of the virtual destructor will be invoked, 
so that the resources allocated in the derived classes can be freed. 
If you want a base pointer to a derived object to call the base version of the virtual function, 
simply use the scope resolution operator `::`. 






### function pointer & dynamic binding  

A function pointer is a type of pointer that points to a function instead of a variable. 

```c++
int add(int x, int y){ return x+y; }
int (*f_ptr)(int,int) = add; 
std::cout << f_ptr(1,2) << std::endl; // 3 // use the call operator `()` on the pointer
```

All functions have a unique address. 
**Static binding** (early binding) means the compiler (or linker) is able to 
directly associate the identifier (function or variable name) with a machine address. 
So when the compiler (or linker) encounters a function call, 
it replaces the function call with a machine language instruction that jumps to the address of the function. 

In C++, one way to get **dynamic binding** (late binding) is to use function pointers, 
which means it can not tell which function will be called until runtime. 

Calling a function via a function pointer is also known as an indirect function call. 
Late binding is slightly less efficient since it involves an extra level of indirection. 
With early binding, the CPU can jump directly to the function’s address. 
With late binding, the program has to read the address held in the pointer and then jump to that address, 
which involves one extra step, making it slightly slower. 
However, the advantage of late binding is that it is more flexible than early binding, 
because decisions about what function to call do not need to be made until run time. 


### virtual table

The virtual table is a lookup table of virtual functions used to resolve virtual function calls in a dynamic binding manner.
Every class with virtual functions (or derived from a class with virtual functions) owns a virtual table, 
which is a static array the compiler sets up at compile time. 
A virtual table contains one entry for each virtual function that can be called by objects of the class. 
Each entry in this table is a function pointer that points to the most-derived virtual function.

The compiler adds a hidden pointer `*__vptr` to the class with virtual functions. 
When an object is created, `*__vptr` is set to point to its virtual table. 
Unlike the `*this` pointer, which is actually a function parameter, 
`*__vptr` is a real pointer that makes each class object allocated bigger by the size of one pointer. 
It also means that `*__vptr` is inherited by derived classes, 
and it is in the base portion of a derived class. 

Calling a virtual function is slower than calling a non-virtual function because: 
First, we have to use the `*__vptr` to get to the appropriate virtual table; 
Second, we have to index the virtual table to find the correct function to call. 
However, with modern computers, this added time is usually fairly insignificant. 




### pure virtual function & abstract base class & interface class


**Pure virtual function** (abstract function) acts as a placeholder that is meant to be redefined by derived classes. 
Any class with one or more pure virtual functions becomes an **abstract base class**, which means that it can not be instantiated. 

```c++ 
class P { public: virtual void foo() = 0; virtual void bar() = 0;}; // abstract base class
void P::bar() {std::cout << "bar" << std::endl;} // default definition for `bar()` 
class D: public P { void foo(){std::cout << "bar" << std::endl;} 
	virtual void bar(){P::bar();} }; // use the default implementation in `P`
// P p; // abstract class cannot be instantiated
D d; 
```

Even though pure virtual function may have default definition, 
It is still considered as an abstract base class because of the `=0`, 
and all its derived classes are forced to provide their own implementations. 
Note that the default definition body must be provided separately. 

An **interface class** is a class that has no member variables and all the member functions are pure virtual. 
Interface classes are often named beginning with an 'I'. 

Abstract classes still have virtual tables, 
which can still be used if you have a pointer or reference of the abstract class. 
The virtual table entry (in abstract class) for a pure virtual function will either be `nullptr` 
or points to a generic function that prints an error. 



### virtual base classes

(Note that "virtual base classes" is not "abstract base classes".)

 - for multiple inheritance & diamond problem
 
To share a base class, simply insert the `virtual` keyword in the inheritance list of the derived class. 
This creates what is called a virtual base class, which means there is only one base object that is shared. 



### object slicing 

The derived object has a base part and a derived part. 
When we assign a derived object to a base object, only the base portion of the derived object is copied. 
The derived portion is not. 
Consequently, the assigning of a derived object to a base object is called object slicing. 
In practice, object slicing is much more likely to occur accidentally with functions. 

```c++
class Base{}; class Derived: public Base{};
void print_base1(const Base b){} // pass by value 
void print_base2(const Base& b){} // object slicing can be avoided through reference
Derived d; Base b = d; // object slicing 
print_base1(d); // object slicing is much more likely to occur with functions
// Derived d1, d2; Base& b = d1; b = d2; // Franken-object // only the base portion of `d2` is copied into `d1`
```

Frankenobject, composed of parts of multiple objects. 
The default `operator=` for classes isn't `virtual` by default. 
e.g., as a result, `d1` has the base portion of `d2` and the derived portion of `d1`.
There's no easy way to prevent this from happening (other than avoiding assignments like this as much as possible). 


 - slicing vectors

Assume the `std::vector` is declared to be a vector of base type, 
when the derived object is added to the vector, it will be sliced. 
Fixing this is a little more difficult. A `std::vector` of references to an object won't compile. 
Because the elements of `std::vector` must be assignable, whereas references can't be reassigned (only initialized). 
We can fix this by making a vector of pointers or `std::reference_wrapper`. 
The standard library provides a workaround: 
`std::reference_wrapper` is a class that acts like a reference, 
but also allows assignment and copying, so it's compatible with `std::vector`. 
Note that the object wrapped by `std::reference_wrapper` can't be an anonymous object 
(since the expression scope of anonymous objects would leave the reference dangling). 
To get the object back out of `std::reference_wrapper`, use `get()`. 




### Dynamic casting


The process of converting a derived pointer into a base pointer is called upcasting. 
The casting operator `dynamic_cast` is commonly used to convert base-class pointers into derived-class pointers (downcasting). 
In general, virtual function should be preferred over downcasting. 
When downcasting is the better choice, `dynamic_cast` should be used. 

`dynamic_cast` can be used with both pointer and references. 
If the `dynamic_cast` of a pointer fails, it will return a null pointer. 
If the `dynamic_cast` of a reference fails, an exception of `std::bad_cast` will be thrown. 
Note that because `dynamic_cast` does some consistency checking at runtime, 
use of `dynamic_cast` does incur a performance penalty. 

Also note that there are several cases where downcasting using `dynamic_cast` will not work:
1. with `protected` or `private` inheritance. 
2. for classes that do not declare or inherit any virtual functions (and thus don't have a virtual table). 
3. certain cases involving virtual base classes. 

Downcasting can also be done with `static_cast`. 
The difference is that `static_cast` **does no runtime type checking** to ensure that what you're doing makes sense, 
which makes using `static_cast` **faster, but more dangerous**. 
If you cast a base pointer to a derived pointer, 
`static_cast` will "succeed" even if the base pointer isn't pointing to a derived object. 
This will result in undefined behavior when you try to access the resulting derived pointer 
(that is actually pointing to a base object). 


### Printing inherited classes using `<<`

.....

## Exceptions 

## IO



## Build & Compile


### CMake

### Make



## Libraries

### Boost  








## References & Resources  {#references}


*\[1\]. [Learn Cpp Tutorial][1]*
{: id="ref-1-learn-cpp-tutorial" class="ref-item" }

*\[2\]. [The C++ 'const' Declaration][2]*
{: id="ref-2-const-declaration" class="ref-item" }

*\[3\]. [Template Classes and Friend Function][3]*
{: id="ref-3-template-classes-and-friend-function" class="ref-item" }

*\[4\]. [How to find the length of an array][4]* 
{: id="ref-4-how-to-find-the-length-of-an-array" class="ref-item" }

*\[5\]. [Why Do We Need size_t][5]* 
{: id="ref-5-why-do-we-need-size_t" class="ref-item" }




[1]: <https://www.learncpp.com/>  "Learn Cpp Tutorial"

[2]: <http://duramecho.com/ComputerInformation/WhyHowCppConst.html>  "The C++ 'const' Declaration"

[3]: <https://web.mst.edu/~nmjxv3/articles/templates.html>  "Template Classes and Friend Function"

[4]: <https://stackoverflow.com/questions/4108313/how-do-i-find-the-length-of-an-array>  "How to find the length of an array"

[5]: <https://prateekvjoshi.com/2015/01/03/why-do-we-need-size_t/>  "Why Do We Need size_t"





















