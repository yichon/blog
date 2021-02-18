---
layout: post
title:  "A quick review of Python"
date:  2021-02-17T20:00:00+08:00
tags: ["review_label", "hidden_label"]
categories: ["Programming", "Python"]
keywords: object-oriented
author: Yichong Zhou
hidden: true
---

> It is almost impossible to put all the information about Python in one mere post. 
This post mainly focuses on the core concepts and critical details in Python that help us better understand it. 
Most of the materials in this post are collected from internet. 

<!--more-->


{% include toc.html %}


## Basics


Python aims to write less code to achieve the same thing as in Java. 

Python combines compiling and interpreting. 
The default implementation of the Python is CPython which is written in the C language. 
CPython compiles source code into a platform-independent intermediate format called **bytecode**, 
which is interpreted by the CPython virtual machine line by line. 
The bytecode is stored in the `.pyc` file.

### Keywords


```python
print("Hello", "World") # Hello World
help(class_name) # e.g. help(int)
a, b, c = 10, 20, 30 # simultaneous assignment 
name = input("Enter name: ") # receive input from console
type(a) # <type 'int'> # built-in function # type(x=12) doesn't work
myvar = "Hello Python"; myvar = 1 # dynamically typed
1 + "2" # strongly typed # will produce an error
print(not False) # True
###
if statement is True: pass # 'is' keyword to test if two variables refer to the same object 
elif another_statement == True: pass # (else if) # '==' operator to test if two variables are equal
else: pass
for _ in range(5): print(_, end=',') # 0,1,2,3,4
for x in range(3, 6): print(x,end=',') # 3,4,5
for x in range(3, 8, 2): print(x,end=',') # 3,5,7
count=0 
while(count<5): print(count,end=','); count +=1 # first print '0,1,2,3,4'
else: print(f"count value reached {count}") # then print 'count value reached 5'
try: pass  # 
except: pass # or 'except IOError:'
finally: pass
if not type(x) is int: raise TypeError("blah") # Exception("blah")
a = 1; b = 2 
print(1 if a > b else -1)  # -1  # ternary operator  # <expression 1> if <condition> else <expression 2>
print(1 if a > b else -1 if a < b else 0)  # -1 
# assert <expression> # for debugging 
x = ["a", "b", "c"]; y = [1, 2, 3]; z = [True,True,False] 
print(zip(x,y,z)) # <zip object at 0x7f7d9223d2d0>
for t in zip(x,y,z): print(t) # ('a', 1, True) \\ ('b', 2, True) \\ ('c', 3, False)
for c in zip("Py"): print(c) # ('P',) \\ ('y',)
z = [("a", 1), ("b", 2), ("c", 3)]; x, y = list(zip(*z)); print(x, y, sep="/")  # ('a', 'b', 'c')/(1, 2, 3)
x = "abc"; y = [1, 2, 3]; print(dict(zip(x, y))) # {'a': 1, 'b': 2, 'c': 3} 
if __name__ == "__main__": print("Executing as main program")
```
The difference between `range()` and `xrange()` function in Python 2 is that the `range` function returns a new list with numbers of that specified range, 
whereas `xrange` returns an iterator. 
`xrange()` has to create the values when asked for them, 
whereas some of the `range`'s elements may never be used.
(Python 3 uses the `range` function that acts like `xrange`).

Unlike languages like C, C++, we can use `else` for loops.
After the loop condition of `for` or `while` statement fails, the code in `else` is executed. 
If a `break` statement is executed inside the `for` loop then the `else` part is skipped. 
Note that the `else` part is executed even if there is a `continue` statement.


Python has 5 standard **data types**.
```python
# Numbers # String   # List     # Tuple    # Dictionary   # Boolean 
  123      "hello"   [1,2,3]    (1,2,3)    {"a":1,"b":2}     True
```
`True` and `False` are boolean literals. The following values are also **considered as false**: \\
`0, 0.0, [], (), {}, '', None`.

Literals can be of different types e.g. `11` are of type int, `3.14` is a float and `"hello"` is a string. 

Python supports 3 different **numerical types**. \\
**int** `100`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  **float** `3.14`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **complex** `-2+2.3j`

```python
+ - * / // ** %  += -= *= /= //= **= %=  # operators
###
float(10) int(3.14) int("123") str(100) round(23.97312, 2)  # Datatype conversion
# 10.0      # 3       # 123    # '100'       # 23.97
```
<br/>


### Data Structures


#### List/Tuple


```python
["abc",3.14,True,40] ("abc",3.14,True,40) # tuples are immutable
[] () # empty list/tuple
[2,] (2,) # single item list/tuple 
tuple([1,2,3,4,4]) # tuple from list 
# list[(1,2,3,4,4)] # raise an error 
list("abc") tuple("abc") # list/tuple from string 
###
t = [11,22,33,44,55] / t = (11,22,33,44,55)
for i in t: print(i, end=" ") # 11 22 33 44 55 # iterating
t[0:2] # [11, 22] / (11, 22) # slicing
t[3:] # [44, 55] / (44, 55) 
t[-1] # -1 refers to the last item 
t[1:3] = [2, 3] # t => [11,2,3,44,55] 
t[2:3] = [33, 4] # t => [11,2,33,4,44,55] 
tt = t[:] # Make a copy by doing a full slice 
# the 'stepsize' for slicing (the default stepsize is '1')
print(t[::1])   # [11, 22, 33, 44, 55] / (11, 22, 33, 44, 55)  # '1' is the step size
print(t[::2])   # [11, 33, 55] / (11, 33, 55)
print(t[::3])   # [11, 44] / (11, 44)
print(t[::-1])  # [55, 44, 33, 22, 11] / (55, 44, 33, 22, 11)
for i, e in enumerate(t[::-1]): print(i, e, end=", ") # 0 55, 1 44, 2 33, 3 22, 4 11,
22 not in t # True
###
z = [1, 2, 3] / z = (1, 2, 3)
print(f'Number of elements in z is {len(z)}') # 3
y = [4, 5, z] / y = 4, 5, z # parentheses not required for tuple
print('Number of elements in y is', len(y)) # 3
print(y) # [4, 5, [1, 2, 3]] / (4, 5, (1, 2, 3))
print(y[2]) # [1, 2, 3] / (1, 2, 3)
print(y[2][2]) # 3
print(len(y)-1+len(y[2])) # 5
###
v = [1,2,3] / v = (1,2,3)
a,b,c = v / (a,b,c) = v / [a,b,c] = v # unpack a list/tuple
print(a,b,c) # 1 2 3
v = [1,2,3,4,5] / v = (1,2,3,4,5)
(a,*b,c) = v  # * =>  a list returned, not tuple
print(a, b, c, sep=", ") # 1, [2, 3, 4], 5
[1,2,3] + [4,5,6]  # [1, 2, 3, 4, 5, 6] ## (1,2,3) + (4,5,6)  # (1, 2, 3, 4, 5, 6)
['Hi!',] * 3  # ['Hi!', 'Hi!', 'Hi!'] ## ('Hi!',) * 3  # ('Hi!', 'Hi!', 'Hi!')
for x in (1, 2, 3): print(x, end=' ') # 1 2 3 
### common functions
min() max() sum() len()
count(v) index(pos) 
### List Functions 
append(v) extend(v) insert(pos, value) pop() pop(index) remove(v) reverse() copy() clear()
sort() sort(reverse=True) sort(key=myfunc) # 'myfunc' return a number to sort 
sort(key=str.lower) # case-insensitive sort
del mylist[0] # 
list2 = list(list1) # copy
### List Comprehension 
# newlist = [expression for item in iterable if condition] 
# newlist = [expression if condition else alt-item for item in iterable] 
[ x for x in range(10) ] # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[ x + 1 for x in range(10) ] # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[ x for x in range(10) if x % 2 == 0 ] # [0, 2, 4, 6, 8]
[ x *2 for x in range(10) if x % 2 == 0 ] # [0, 4, 8, 12, 16]
###
a, (b, c) = [1, [2, 3]] # Works
# a, b, c = [1, [2, 3]] # Does not work
odd, even = [(1, 3, 5), (2, 4, 6)]
(x, y, z) = ('a', 'b', 'c') 
print(*[1,2,3])  # => print(1,2,3) # 1 2 3 # unpacking a list or tuple
```
Always using parentheses to indicate start and end of tuple,
even though parentheses are optional. \\
Explicit is better than implicit.
<br/><br/>

#### String 


```python
s1 = 'Hello' + " World"  # strings are immutable
s2 = """create a multiline string 
       using three double quotes."""
'Hello'*2 # 'HelloHello'
s1[1] # e # Square brackets are used to access characters in string
len(s1) # 11
"hello" not in s1 # False 
s1[2:5]     s1[-5:-2]     s1[2:]     s1[:5]       s1[:]       # slice a string
 # llo        # Wor    # llo World  # Hello   # Hello World
for x in "banana": print(x, end=",") # b,a,n,a,n,a,
print(','.join(['a', 'b', 'c'])) # a,b,c
###
txt = "Hello, {}! I'm {}"  # string format
print(txt.format("A", "B")) # Hello, A! I'm B
txt = "Hello, {1}! I'm {0}"
print(txt.format("A", "B")) # Hello, B! I'm A
s = 'world'
print(f'Hello, {s}!') # Hello, world! 
# Note that the 'str()' function returns the string version of a given object.
### String Methods
strip() strip() lstrip() rstrip()	
upper() lower() title() swapcase()
count()	index()	rindex() replace() find() rfind() 
join() split() rsplit() splitlines() format()
endswith() startswith() islower() isupper() istitle() 
isdigit() isalnum() isalpha() isdecimal() isnumeric() isspace()
encode() decode() center() capitalize() isidentifier() isprintable() translate() zfill()
casefold() format_map() ljust()	rjust()	expandtabs() maketrans() partition() rpartition()
###
print('C:\\nowhere') # C:\nowhere
print(r'C:\\nowhere') # C:\\nowhere # raw string
print(u'Hello world!') # Unicode string
str(b'Hello world!', 'utf-8') 
print(b'Hello'.decode('ascii')) # Hello
print('Hello'.encode('ascii')) # b'Hello' # 
print(len("你好")) # 2 
print(len(("你好").encode("utf-8"))) # 6
print("你好".encode("utf-8")) # b'\xe4\xbd\xa0\xe5\xa5\xbd'
print(b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode('utf-8')) # 你好
```
Strings in Python are arrays of bytes representing unicode characters.
Python does not have a character data type, a single character is simply a string with a length of `1`. 
Note that the default encoding in Python 2 is ASCII.
The default encoding in Python 3 is utf-8, 
we can use `# coding=ascii` statement at the top to switch back to ASCII. 
Note that prefixes such as `r` `b` can only be used on string literals, not variables.
<br/><br/>

#### Dictionary

```python
d = {} # empty dictionary
d= {'a': 1}
d['b'] = 2 # {'a': 1}
# Note that d.get('key') will return None while d['key'] will raise an error if 'key' doesn't exist
get() update() keys() values() items() 
pop() popitem()	copy() fromkeys() 
clear()	setdefault() 
del dict_name["key_name"]
for i, (k, v) in enumerate(d.items()): print(i, k, v, end=", ")   # 0 a 1, 1 b 2,
dict = {"a":1, "b":2, "c":3}
print({**dict}) # {'a': 1, 'b': 2, 'c': 3}  # unpack a dictionary
def func(**args): print(args)
func(**dict) # {'a': 1, 'b': 2, 'c': 3}  # unpack a dictionary
```
`in` and `not in` operators are used to check keys. 
<br/><br/>

#### Set 

```python
s = set() # empty set # there's no literal syntax for the empty set
{"abc", 34, True, 40, "male"} 
set(["a", "b", "c"])
del s # delete the set completely, 's' no longer exists
len() type()
###
add() remove() discard() pop() 
difference() intersection() union() update()
difference_update() intersection_update()
isdisjoint() issubset() issuperset()
clear() copy()
symmetric_difference() symmetric_difference_update() 
```
Set items can appear in a different order every time you use them. 
Duplicate values will be ignored. 
<br/><br/>


### Functions 


#### regular function
```python
def empty_func(): pass
x = 3
def func(a, b=-1, c=-2): # docstring
    '''This is a function with 3 parameters,
       2 of them have default arguments.'''
    global x # global variable # assignment (when declaring global) will raise an error 
    print('a =', a, ', b =', b, ', c =', c)
    return -3, x # return a tuple
func(0, 1) # a = 0 , b = 1 , c = -2
func(0, c=2) # a = 0 , b = 1 , c = -2
func(c=2, a=0) # a = 0 , b = -1 , c = 2
print(func(0)) # (-3, 3)
print(func.__doc__) # output: see docstring 
print(func.__name__, func.__class__, sep=', ')  # func, <class 'function'>  # func.__annotations__ ...etc.
###
def param(a=5, *args, **kwargs): print('a =',a,end=", ");print('args =',args,end=", ");print('kwargs =',kwargs)
param(0, 1, 2, 3, b=4, c=5, d=6) # a = 0, args = (1, 2, 3), kwargs = {'b': 4, 'c': 5, 'd': 6}
def f(a, L=[]):  L.append(a); return L  # default value is evaluated only once
print(f(1));print(f(2));print(f(3)) # [1] // [1, 2] // [1, 2, 3]
###
def power(p, b=2): # recursion
    if p == 0: return 1
    if p == 1: return b
    else: return power(p-1, b) * b
### function annotations are completely optional meta-information
def f(param1: str, param2: str = 'default_val') -> str: 
    print("Annotations:", f.__annotations__)
    print("Arguments:", param1, param2)
    return 'a string'
f('abc') # Annotations: {'param1': <class 'str'>, 'param2': <class 'str'>, 'return': <class 'str'>}
         # Arguments: abc default_val
###
def kwd_only_arg(*, arg): print(arg) # keyword-only arguments
# def pos_only_arg(arg, /): print(arg) # positional-only parameters # Python 3.8+
# def combined(pos_only, /, std, *, kwd_only): print(pos_only, std, kwd_only)  # Python 3.8+
# def f(name, /, **kwds): return 'name' in kwds  # Python 3.8+
# f(1, **{'name': 2}) # it allows 'name' as a positional argument and 'name' as a key in the keyword arguments
```
<br/>

#### Lambda, map, filter and reduce
```python
func = lambda a,b: a+b  # lambda expressions
print(func(11,22))  # 33 
map(func, seq) # 'map' and 'filter' returns an 'iterator' in Py3 and a 'list' in Py2
[f(pi) for f in funcs]  # mapping a list of functions
odd = list(filter(lambda x: x % 2, fibonacci))   # filter(func, seq)  # fibonacci = [0,1,1,2,3,5,8,13,21,34,55]
[x for x in fibonacci if (lambda x: x % 2)(x)!=0]   # [1, 1, 3, 5, 13, 21, 55]
reduce(lambda x,y: x+y, range(1,101))   # 5050  # from functools import reduce
```
<br/>

#### Built-in Functions

```python
print() input() help() dir() locals() globals() vars() 
super() isinstance() issubclass() type()
abs() pow() sum() min() max() len() round() divmod()
str() list() dict() tuple() set() bool() float() int() object()
complex() bytes() bytearray() hex() oct() bin() frozenset()
iter() next() enumerate() range() zip() filter() sorted() reversed() map() all() any()
format() slice() chr() ord()
delattr() getattr() setattr() hasattr() property() 
callable() classmethod() staticmethod() id() repr() ascii() hash()
open() compile() eval() exec() breakpoint() __import__() memoryview()
```
<br/>

### OOP 

Python is object-oriented. 
Almost everything in python is object, including int, str, bool, modules, functions etc. 
<br/><br/>

#### Classes/Objects

```python 
class EmptyClass: pass 
o = EmptyClass()
o.attr1 = "attr1"  # properties can be dynamically created # print(o.attr1) # attr1
class MyClass1:
   def __init__(self, attr1, attr2=0):   # constructor
         self.attr1 = attr1    # instance variables
         self.__attr2 = attr2  # pseudo-private instance variables
   def getAttr2(self):
         return self.__attr2
obj = MyClass1('attr1') 
isinstance(obj, MyClass1) # True
print(obj.__attr2) # error
print(obj._MyClass1__attr2)  # 0 # private variable '__attr2' is textually replaced with '_MyClass1__attr2'
del obj.attr1 # properties can be deleted 
del obj # delete object 
class MyClass2(MyClass1): # inheritance
   def __init__(self, attr0, attr1, attr2=0):
        MyClass1.__init__(attr1, attr2) # super().__init__(attr1, attr2) 
        self.attr0 = attr0
issubclass(MyClass2, MyClass1) # True
class DerivedClass(Base1, Base2, Base3): pass # multiple inheritance
# If an attribute or method cannot be found in `DerivedClass`, it is searched for in `Base1` (recursively), 
# if not found, then `Base2`, and so on. 
print(DerivedClass.__mro__)  # MRO => Method Resolution Order 
# (<class '__main__.DerivedClass'>, <class '__main__.Base1'>,
# <class '__main__.Base2'>, <class '__main__.Base3'>, <class 'object'>)
```

All methods including constructors have first parameter `self`. 
This parameter refers to the current instance. 
When creating a new object, the `self` parameter in the `__init__` method is set to the reference of the object just created. 
When calling a method, no argument needed to be passed to the `self` parameter, 
it is done automatically behind the scenes. 

Any identifier of the form `__attr` is textually replaced with `_Classname__attr`. 
There is a convention: a name prefixed with one underscore (e.g. _attr2) should also be treated as a non-public part of the API 
(whether it is a function, a method or a property). 
<br/><br/>

#### super() & MixIn


**MixIns** (like **Interfaces**) do not contain internal state and should not be instantiated, 
they can have non-abstract methods unlike interfaces. 

```python
class A:
    def say(self): print('A')
class B(A):
    def say(self): print('B')
class C(B):
    def say(self): super().say()  # equivalent to 'super(C, self)'
class D(B):
    def say(self): super(B, self).say() # select ancestor class
C().say() # B
D().say() # A
###
class Foo:
    def foo(self): print('foo')
class myMixin:
    def say(self): self.foo() # note that it has no knowledge of the implementation of 'foo()' 
class Bar(myMixin, Foo): pass
Bar().say()  # foo 
```  
<br/>

#### \_\_init_subclass__

```python
class Foo:
    def __init_subclass__(cls, *args, **kwargs):
        cls.name = "foo"  # the default value of attribute 'name'
        return super().__init_subclass__(*args, **kwargs) 
class Bar(Foo): name = "bar"; print(name) # bar # the default value will be reset to 'foo'
print(Bar().name) # foo 
```


## Advanced


### Iterators and Generators


Python iterator is a strategy of lazy evaluation, a way to deal with infinity. 

```python
class Compute:
    def __init__(self): self.num = 0
    def __iter__(self): return self
    def __next__(self):
        rv = self.num; self.num += 1
        if self.num > 5: raise StopIteration()
        return rv
c = Compute()
while True:
    try: print(next(c))  # 0 \\ 1 \\ 2 \\ 3 \\ 4 
    except StopIteration: break
###
ab = ["a", "b", "c", "d"] # list is not an iterator, but it can be used like an iterator 
ab_iterator = iter(ab) # the call 'iter(o)' will return an iterator if 'o' is iterable
for c in ab_iterator: print(c, end=" ")  # a b c d  # or using 'for c in ab' directly
print(); ab_iterator = iter(ab)  # reset 'ab_iterator' 
while ab_iterator:  # or using 'while True'
    try: c = next(ab_iterator); print(c, end="  ")  # a  b  c  d
    except StopIteration: break  # when 'next(it_o)' is exhausted, it will throw a 'StopIteration' exception
###
def fibonacci():  # Generates an infinite sequence of Fibonacci numbers on demand
    a, b = 0, 1
    while True: yield a; a, b = b, a + b
counter = 0; fibo = fibonacci()  # create an iterator by calling the generator function
for x in fibo:  # 0 1 1 2 3 5 8 13 21 34 55
    print(x, end=" "); counter += 1
    if (counter > 10): break 
def gen(): yield 1;return -1;yield 2  # 'return -1' => 'raise StopIteration(-1)' (since Python 3.3)
###
class StateOfGenerator(Exception):
    def __init__(self, message=None): self.message = message
def count(pos=0, step=1):
    while True:
        try:
          reset_v = yield pos  # 'yield' call also receives from '.send' or 'next'
          if reset_v is None: pos += step  # 'next' call always sends a 'None' object
          else: pos = reset_v - step
        except Exception: yield round(pos, 1)  # return the state of the "count" iterator.
counter = count(2.1, 0.3)
for i in range(5): new_v = next(counter); print(f"{new_v:.2f}", end=", ") # 2.10, 2.40, 2.70, 3.00, 3.30,
print(); print(counter.send(5.2))  # 4.9
for i in range(5): new_v = next(counter); print(f"{new_v:.1f}", end="  ") # 5.2  5.5  5.8  6.1  6.4
print(); print(counter.throw(StateOfGenerator)) # 6.4
# 'throw()' method raises an exception at the point where the generator was paused
# and returns the next value yielded by the generator
def gen(): yield from "Py"; yield from range(3); yield from iter(ab) 
g = gen() 
for x in g: print(x, end=" ") # P y 0 1 2 a b c d 
def sub_gen(): yield 1; return -1  # the 'return' value can be the 'yield from' value in 'delegating_gen'
def delegating_gen(): x = yield from sub_gen(); print(x)
for x in delegating_gen(): print(x, end=" ")  # 1 -1
###
def permutations(items):  # recursive generator
    if len(items)==0: yield []
    else:
        for i in range(len(items)):
            for x in permutations(items[:i]+items[i+1:]): yield [items[i]]+x  
for p in permutations(['r','e','d']): print(''.join(p), end=" ")  #  red rde erd edr dre der
###
def firstn(gen, n):  # A Generator of Generators
    g = gen()
    for i in range(n): yield next(g)
```
<br/>

### Magic Methods & Operator Overloading

Magic Methods are implicitly invoked. 

e.g. creating an instance `x = A()` will call `__new__` and `__init__` behind the scenes. 

```python
## Binary Operators
+ object.__add__(self, other)  - object.__sub__(self, other)  * object.__mul__(self, other)
// object.__floordiv__(self, other)  / object.__truediv__(self, other)  % object.__mod__(self, other)
** object.__pow__(self, other[, modulo])  << object.__lshift__(self, other)  >> object.__rshift__(self, other)
& object.__and__(self, other)  ^ object.__xor__(self, other)  | object.__or__(self, other) 
# object(3) + 5 vs. 5 + object(3) # object.__radd__(self, other)
def __radd__(self, other): return Length.__add__(self,other) 
## Extended Assignments
+= object.__iadd__(self, other)  -= object.__isub__(self, other)  *= object.__imul__(self, other)
/= object.__idiv__(self, other)  //= object.__ifloordiv__(self, other)  %= object.__imod__(self, other)
**= object.__ipow__(self, other[, modulo])
<<= object.__ilshift__(self, other)  >>= object.__irshift__(self, other)
&= object.__iand__(self, other)  ^= object.__ixor__(self, other)  |= object.__ior__(self, other) 
## Unary Operators 
- object.__neg__(self)  + object.__pos__(self)  ~ object.__invert__(self)
abs() object.__abs__(self)  complex() object.__complex__(self)
int() object.__int__(self)  long() object.__long__(self)  float() object.__float__(self)
oct() object.__oct__(self)  hex() object.__hex__(self)
str() object.__str__(self)  repr() object.__repr__(self)
## Comparison Operators 
< object.__lt__(self, other)  <= object.__le__(self, other)  == object.__eq__(self, other)
!= object.__ne__(self, other)  >= object.__ge__(self, other)  > object.__gt__(self, other)
## callable
class MyClass:
    def __call__(self): print(str(self))
obj = MyClass(); obj()  # <__main__.MyClass object at 0x7efd5e593fd0>
```
The `__call__` method turns the instances of the class into callables. Functions are callable objects. 
A callable object may behave like a function but not a function. 
<br/><br/>

### Decorators

```python
def time_decorator(func):  # elapsed time  ## import time
    def func_wrapper(x): start=time.time(); func(x); end=time.time(); print('Time lapse:', end-start)
    return func_wrapper
def f(x): print("Hello, " + x)
f = time_decorator(f); f('Decorator')  # Hello, Decorator // Time lapse: 2.6226043701171875e-06
@time_decorator  # usual syntax for decorators
def foo(x): print("Hello, " + x);
foo('Decorator Syntax')  # Hello, Decorator Syntax  // Time lapse: 2.1457672119140625e-06
### arguments can be checked with a decorator 
def call_counting_decorator(func):  #  counting function Calls
    def func_wrapper(*args, **kwargs): func_wrapper.calls += 1; return func(*args, **kwargs)
    func_wrapper.calls = 0; return func_wrapper
@call_counting_decorator
def square(x): return x*x
print(square.calls)  # 0
for i in range(10): square(i)
print(square.calls)  # 10
### decorator with parameters
def target(t):  # parameter for decorator
    def the_decorator(func):
        def function_wrapper(x): func(x);print(t)
        return function_wrapper
    return the_decorator  #  return a customized decorator
@target("World") 
def say(x): print(x, end=' ')
say('Hello')   # Hello World 
# function attributes wrapping
def append_decorator(func):
    def function_wrapper(x):
        """ the docstring of function_wrapper """
        func(x);print("Python")
    function_wrapper.__name__ = func.__name__  # function attributes wrapping
    function_wrapper.__doc__ = func.__doc__
    function_wrapper.__module__ = func.__module__
    return function_wrapper
@append_decorator
def foo(x):
    """ the docstring of original function """
    print(x, end=' ')
## or wrap function attributes using the decorator "wraps" from functools instead
def some_decorator(func):
    @wraps(func)  ## from functools import wraps
    def function_wrapper(x):
        """ the docstring of function_wrapper """
        func(x);print("Python")
    return function_wrapper
## class as a decorator
class cls_decorator:
    def __init__(self, f):
        self.f = f
    def __call__(self, *args, **kwargs):
        self.f(*args, **kwargs);print('World')
@cls_decorator
def foo(x): print(x, end=' ')
foo('Hello')  # Hello World
```
<br/>

### Context Manager

```python
with open('hello.txt', 'w') as f: f.write("Hello World")  # file-opening context manager
with open('hello.txt', 'r') as f: print(f.read())  # Hello World
###
def enter_exit_gen(): print('entering the context: '); yield; print('exit the context.'); yield
class MyContext:                # here we use the 2nd 'yield' to prevent 'StopIteration' exception
    def __init__(self): self.gen = enter_exit_gen()
    def __enter__(self): next(self.gen)
    def __exit__(self, exc_type, exc_val, exc_tb): #  exception type, value and traceback as parameters
        next(self.gen)
        if exc_type: print(f'exc_type: {exc_type}') # handle exceptions
        return True
with MyContext() as my:
    print('Hello World') # entering the context: // Hello World // exit the context.
    raise Exception('exception raised').with_traceback(None)  # exc_type: <class 'Exception'>
###
class my_contextmanager:  # from contextlib import contextmanager
    def __init__(self, gen_func): self.gen_func = gen_func
    def __call__(self, *args, **kwargs): self.args, self.kwargs = args, kwargs; return self
    def __enter__(self): self.gen = self.gen_func(); next(self.gen)
    def __exit__(self, exc_type, exc_val, exc_tb):
        next(self.gen, None) #  'None' is a default value to return instead of throwing 'StopIteration'
@my_contextmanager  # '@contextmanager' has already been defined in 'contextlib' 
def MyContext2():  # note that 'MyContext2' is the argument of '__init__' method
    print('entering the context: ')
    try: yield
    finally: print('exit the context.')
with MyContext2() as my: print('Hello World')  # entering the context: // Hello World // exit the context.
```
<br/>

### Metaclass & \_\_build_class__

A **metaclass** is a class whose runtime instances are classes. 
The built-in function `__build_class__` is called when a class is built with the `class` statement. 

```python 
class MyMetaClass(type):  # metaclasses are classes that inherit from 'type'.
    def __new__(cls, clsname, superclasses, attributedict):
        print("clsname: ", clsname)
        print("superclasses: ", superclasses)
        print("attributedict: ", attributedict)
        return type.__new__(cls, clsname, superclasses, attributedict)
class S: pass 
class A(S, metaclass=MyMetaClass): pass  # note that metaclass is not superclass
a = A()  # clsname:  A  //  superclasses:  (<class '__main__.S'>,)
         # attributedict:  {'__module__': '__main__', '__qualname__': 'A'}
class BaseMeta(type):
    def __new__(cls, clsname, superclasses, attributedict):
        if 'bar' not in attributedict: raise TypeError("...")  # check if 'bar()' is defined in subclasses
        return super().__new__(cls, clsname, superclasses, attributedict)
# class Base(metaclass=BaseMeta):
#     def foo(self): return self.bar()
###
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs): # In Python 3, 'super(Singleton, cls)' is equivalent to 'super()'
        if cls not in cls._instances: cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]
class S: pass
class SingletonClass(S, metaclass=Singleton): pass
print(SingletonClass() == SingletonClass())  # True
##### note that 'class', 'def' or general definitions are executable code in Python
# class C(A, B, *more_bases, metaclass=M, other=2, **more_kwds): pass  
# would translate into this:
# C = __build_class__(<func>, 'C', A, B, *more_bases, metaclass=M, other=2, **more_kwds)
# where <func> is a function object for the class body.
old_bc = builtins.__build_class__  # import builtins
class Base:  # class in library
    def foo(self): return self.bar()  # \'bar()\' is supposed to be defined in subclass
class Base2: pass
def my_bc(func, cls_name, *args, **kwargs):
    print(args)  # (<class '__main__.Base'>, <class '__main__.Base2'>) # print(kwargs) # {}
    if Base in args: print('check if \'bar()\' is defined');return old_bc(func, cls_name, *args, **kwargs)
    else: return old_bc(func, cls_name, *args, **kwargs)
builtins.__build_class__ = my_bc
class Derived(Base, Base2): # check if 'bar()' is defined
    def bar(self): print('hello')
Derived().foo() # hello
#### get a list of methods in a class
method_list = [func for func in dir(Derived) if callable(getattr(Derived, func))] 
print('bar' in method_list) # check if 'bar()' is defined 
print(callable(getattr(Derived, 'bar'))) 
# dir() returns a list of valid attributes of the object
# getattr() returns the value of the named attribute of an object  ## throw an AttributeError if not defined
# callable() returns True if the object passed appears callable 
```


## Other

### Module

```python
import math, random, os, inspect
print(math.pi)
print(math.e)
os.getcwd() # prints the current working directory. 
# def add(x, y): return x + y
print(inspect.getsource(add))  # def add(x, y): return x + y
print(inspect.getfile(add))  # 
from time import time
print(time()) # output current time
# A module has a private symbol table,
# which is used as the global symbol table by all functions defined in the module.
# This is a way to prevent that a global variable of a module accidentally clashes with
# a user's global variable with the same name.
# Global variables of a module can be accessed with the same notation as functions, i.e. modname.name
# A module can import other modules.
from importlib import reload
import Examples._1_ModuleExample._Module.one_time_module as one_time
reload(one_time)  # reloads a previously imported module
import sys
print("sys.builtin_module_names: \n", sys.builtin_module_names)
# There are different kind of modules:
# (1). Those written in Python They have the suffix:.py
# (2). Dynamically linked C modules Suffixes are:.dll,.pyd,.so,.sl, ...
###
# If you import a module, let's say "import xyz",
# the interpreter searches for this module in the following locations and in the order given:
#     The directory of the top-level file, i.e. the file being executed.
#     The directories of PYTHONPATH, if this global environment variable of your operating system is set.
#     standard installation path Linux/Unix e.g. in /usr/lib/python3.5.
###
import math
print("math.__file__: \n", math.__file__)  # find out where a module is located
print("dir(math): \n", dir(math))  # list all valid attributes and methods in the 'math' module
print("dir(): \n", dir())  # return a list with the names in the current local scope
import builtins
print("dir(builtins): \n", dir(builtins)) # list of the Built-in functions, exceptions, and other objects
```

### Package

A package is basically a directory with Python files and a file with the name `__init__.py`.
This means that every directory inside of the Python path, which contains a file named `__init__.py`,
will be treated as a package by Python.
It's possible to put several modules into a Package.
Packages are a way of structuring Python's module namespace by using "dotted module names".
`A.B` stands for a submodule named `B` in a package named `A`.

```python
# pkg
# |-- __init__.py
# |-- a.py
# |-- b.py
from pkg import *
```



## Libraries

### Numpy  


```python
import numpy as np
my_arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]]); print(my_arr.ndim)  # 2  # number of dimensions
print(my_arr.size)  # 8  # number of elements
print(my_arr.flags) # information of memory layout
# C_CONTIGUOUS: True // F_CONTIGUOUS: False  // OWNDATA: True // WRITEABLE: True //
# ALIGNED: True // WRITEBACKIFCOPY: False // UPDATEIFCOPY: False
print(my_arr.itemsize)  # 8  # the length of one element in bytes
print(my_arr.nbytes)  # 64  # the total bytes consumed by all elements
print(my_arr.shape)  # (2, 4)
print(len(my_arr))  # 2  # the length of array
arr = my_arr.astype(float)  # Change the data type to `float`
print(my_arr.dtype)  # int64
print(arr.dtype)  # float64 
# `np.array()` => make a copy, `np.asarray()` => using the original array
arr = [[1, 2], [3, 4]]; print(list(np.asarray(arr))) # [array([1, 2]), array([3, 4])]
print(np.mean(np.arange(3))) # 1.0
print(np.shape(7))  # ()  # shape of scalar
print(np.ndim(7))  # 0  # dimension of scalar
###
print(np.zeros((2, 3, 4), dtype=np.int16))  # create an array of `0`s 
print(np.empty((3, 2)))  # an empty array filled with `0.`s
f = np.full((2, 2), 7) # an array filled with `7`s
z = np.zeros_like(f)  # an array that has the same shape as `f` except filled with `0`s
print(np.arange(10, 25, 5)) # [10 15 20] # all the numbers from 10 to 25. "step size" is 5. # `25` excluded.
print(np.linspace(0, 2, 5)) # [0. 0.5 1. 1.5 2.] # "5 numbers" from 0 to 2  # an array of evenly-spaced values
a = np.linspace(-np.pi, np.pi, 100); b = np.sin(a); c = np.cos(a)
print(np.eye(3)) # or print(np.identity(3))
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]
print(np.eye(3, k=1))
# [[0. 1. 0.]
#  [0. 0. 1.]
#  [0. 0. 0.]]
a = np.asarray([1.0, 2.0, 3.0]); print(a * 2) #  [2. 4. 6.] # scalar `2` is broadcasted to the same size as `a`
x = np.ones((3, 4))  #  a 3x4 matrix filled with `1`s
print(x.shape)  # (3, 4)
y = np.random.random((3, 4))  # a 3x4 matrix filled with random floats # print(x + y) # the same shape
y = np.arange(4); print(y.shape)  # (4,) # print(y)  # [0 1 2 3]
print(x - y)  # subtract y from every row of x  # broadcast 
y = np.random.random((5, 1, 4)); print((x + y).shape) # (5, 3, 4)
### explicitly convert an 1D array to either a row vector or a column vector using `np.newaxis`
arr = np.arange(4)  # (4,)  # 1-D array  ## like Python's range, but returns an array
row_vec = arr[np.newaxis, :]  # (1, 4)
col_vec = arr[:, np.newaxis]  # (4, 1)
arr = np.arange(3 * 3).reshape(3, 3)  # (9,) -> (3, 3)
arr_5D = arr[np.newaxis, ..., np.newaxis, np.newaxis]  # (1, 3, 3, 1, 1)
print(np.newaxis is None) # True
# Np.newaxis uses the slicing operator to recreate the array,
# while np.reshape reshapes the array to the desired layout 
A = np.ones((3, 4, 5, 6)); B = np.ones((4, 6)) 
# print(A + B) # report error
print(B[:, np.newaxis, :].shape)  # (4, 6) -> (4, 1, 6)
print((A + B[:, np.newaxis, :]).shape)  # (3, 4, 5, 6) # broadcast: (4, 1, 6) -> (4, 5, 6)
###
arr = np.arange(12)
arr_2d = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]); arr_3d = np.random.random((3, 4, 5))
print(arr[0:2])  # select items at index 0 and 1
print(arr_2d[0:2, 1]) # select items at row 0 and 1, column 1
print(arr_3d[1, ...]) # select items at row 1  ## the same as `arr_3d[1,:,:]
print(arr[arr < 2])  # [0 1]
bigger_than_3 = (arr_3d >= .5)  # matrix of booleans  # specify a condition
print(arr_3d[bigger_than_3])  # use the condition to index `arr_3d` 
print(arr_2d[[1, 0, 1, 0], [0, 1, 2, 0]]) # select elements at (1,0), (0,1), (1,2) and (0,0)  ##  [5 2 7 1]
print(arr_2d[[1, 0, 1, 0]][:, [0, 1, 2, 0]])  # select a subset of the rows and columns
# [[5 6 7 5]
#  [1 2 3 1]
#  [5 6 7 5]
#  [1 2 3 1]]
# the second part [:,[0,1,2,0]] keeps all the rows but change the order of the columns around a bit.
# display the columns 0, 1, 2, 0, the last column repeats column 0 instead of column 3.
print(arr_2d[:, [0, 1, 2, 0]])
# [[1  2   3   1]
#  [5  6   7   5]
#  [9  10  11  9]]
print(arr_2d[[1, 0, 1, 0]])
# [[5 6 7 8]
#  [1 2 3 4]
#  [5 6 7 8]
#  [1 2 3 4]]
print(arr_2d[[0, 1]][:, [0, 1]])
# [[1 2]
#  [5 6]]
print(arr_2d[[0]][:, [1, 2, 3, 0]]) # [[2 3 4 1]]
###
from numpy.random import rand
from numpy.linalg import solve, inv
A = np.array([[1, 2, 3], [3, 4, 6.7], [5, 9.0, 5]]); temp = A.transpose()
b = np.array([3, 2, 1]); x = solve(A, b) # solve the equation Ax = b
C = rand(3, 3) * 20  # create a 3x3 random matrix of values within [0,1] scaled by 20
np.dot(A, C)  # matrix multiplication
A @ C  # starting with Python 3.5 and NumPy 1.10
###
points = np.array([[9, 2, 8], [4, 7, 2], [3, 4, 4], [5, 6, 9], [5, 0, 7],
                   [8, 2, 7], [0, 3, 2], [7, 3, 0], [6, 1, 1], [2, 9, 6]])
qPoint = np.array([4, 5, 3])
minIdx = np.argmin(np.linalg.norm(points - qPoint, ord=2, axis=1)) # compute all euclidean distances at once
print('Nearest point to q: {0}'.format(points[minIdx])) # Nearest point to q: [3 4 4]
# from numpy import linalg as LA
a = np.arange(9) - 4  # array([-4, -3, -2, -1,  0,  1,  2,  3,  4])
b = a.reshape((3, 3)); c = np.array([[1, 2, 3], [-1, 1, 4]])
print(LA.norm(c, axis=0))  # array([ 1.41421356,  2.23606798,  5.]) // horizontal
print(LA.norm(c, axis=1))  # array([ 3.74165739,  4.24264069])      // vertical
print(LA.norm(c, ord=1, axis=1))  # array([6. 6.])                  // |..| & **1
print(LA.norm(c, ord=2, axis=1))  # array([3.74165739 4.24264069])  // **2 & **.5
# i.e., np.sqrt(1**2 + 2**2 + 3**2) -> 3.74165739 ## np.sqrt(1**2 + 1**2 + 4**2) -> 4.24264069
# print(LA.norm(a))  # 7.745966692414834
# print(LA.norm(b))  # 7.745966692414834
# print(LA.norm(b, 'fro'))  # 7.745966692414834
# print(LA.norm(a, np.inf))  # 4.0  # Max Norm
# print(LA.norm(b, np.inf))  # 9.0
# print(LA.norm(a, -np.inf))  # 0.0
# print( LA.norm(b, -np.inf))  # 2.0
r = np.reshape(np.arange(256*256) % 256, (256, 256)); b = r.T; z = np.zeros_like(r)  # 256x256 pixels 
cv2.imwrite('grad.png', np.dstack([b, temp, temp])) # import cv2  ## output an image
```


### Pandas



### Matplotlib 



### SciPy 



### TensorFlow 



### PyTorch



## References & Resources  {#references}


*\[1\]. [The Python Standard Library][1]*
{: id="ref-1-python-standard-library" class="ref-item" }

*\[2\]. [The Python Tutorial][2]*
{: id="ref-2-the-python-tutorial" class="ref-item" } 

*\[3\]. [The Python Language Reference][3]*
{: id="ref-3-python-language-reference" class="ref-item" }

*\[4\]. [Python tutorials for beginners][4]*
{: id="ref-4-python-tutorials-for-beginners" class="ref-item" }

*\[5\]. [Python tutorial][5]*
{: id="ref-5-python-tutorial" class="ref-item" }

*\[6\]. [W3schools Python Tutorial][6]*
{: id="ref-6-w3schools-python-tutorial" class="ref-item" }

*\[7\]. [Python Course][7]*
{: id="ref-7-python-course" class="ref-item" }

*\[8\]. [Python Expert][8]*
{: id="ref-8-python-expert" class="ref-item" }

*\[9\]. [An introduction to Python bytecode][9]*
{: id="ref-9-an-introduction-to-python-bytecode" class="ref-item" }

*\[10\]. [Understanding Python Bytecode][10]*
{: id="ref-10-understanding-python-bytecode" class="ref-item" }



[1]: <https://docs.python.org/3/library/index.html>  "The Python Standard Library"

[2]: <https://docs.python.org/3/tutorial/>  "The Python Tutorial" 

[3]: <https://docs.python.org/3/reference/>  "The Python Language Reference" 

[4]: <https://thepythonguru.com>  "Python tutorials for beginners"

[5]: <https://www.learnpython.org/>  "Python tutorial"

[6]: <https://www.w3schools.com/python/default.asp>  "W3schools Python Tutorial" 

[7]: <https://www.python-course.eu>  "Python Course"

[8]: <https://www.youtube.com/watch?v=7lmCu8wz8ro>  "What Does It Take To Be An Expert At Python"

[9]: <https://opensource.com/article/18/4/introduction-python-bytecode>  "An introduction to Python bytecode"

[10]: <https://towardsdatascience.com/understanding-python-bytecode-e7edaae8734d>  "Understanding Python Bytecode"



