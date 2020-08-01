---
layout: post
title:  "A quick review of Javascript"
date:  2020-07-29T11:11:00+08:00
tags: ["review_label"]
categories: ["Programming", "Javascript"]
keywords: lexical-scope closure object-oriented deep-clone
---

> It is almost impossible to put all the information about Javascript in one mere post. 
This post mainly focuses on the core concepts and critical details in Javascript that help us better understand it. 

<!--more-->


{% include toc.html %}


## Basics

### Keywords

```javascript
var let const
if(){} else if(){} else{}
while(){}
do{}while() 
for(;;){}
for(a in b){}
fot(a of b){}
break label_name;
continue label_name;
function(){}
function*(){yield 0;}
return 
switch(){ case a:{} break; default:{} }
try{}catch(){}
```
The `for...in` syntax iterates over the enumerable properties of an object, in an arbitrary order.
The `for...of` syntax is specific to collections, it iterates over values that the iterable object defines to be iterated over.
*(Ref: [for...of][21])*

### Operators
 - Arithmetic Operators

```
+ - * / % ++ -- (** Exponentiation ES2016)
```

 - Assignment Operators

```
= += -= *= /= %= **=
```

 - Comparison Operators

```
== === != !== > < >= <= ?:
```

 - Logical Operators

```javascript
&& || !
```

```javascript
var ele;(ele = document.getElementById("demo")) && ele.innerHTML="Hello";
document.getElementById("demo").innerHTML = msg || "default message";
```

 - Type Operators

```javascript
typeof instanceof
```

 - Bitwise Operators

```
& | ~ ^ << >> >>> 
```

 - String Operators

When used on strings, operator `+` and `+=` are the string concatenation operators.

 - Spread Operator & Rest Syntax

```javascript
myFunction(...iterableObj);     // spread operator in "function call"
[...iterableObj, 4, 5, 6];      // spread operator in an array literal
function foo(a, b, ...args) {}  // Rest syntax in "function definition"
```

 - Comma Operator

The comma operator evaluates each of its operands (from left to right) and **returns the value of the last operand**.

`expr1, expr2, expr3...` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*(Ref: [Comma Operator][27])*

 - Other Operators

```javascript
in
yield 
yield*
async function
await
```

### Data Types

 - Primitive Types

```javascript
  string    number    boolean    undefined
"my string"  345    true/false
```

```javascript
var a = 12.00;  // with decimals
var b = 12;     // without decimals
var c = 12e3;   // 12000
var d = 12e-3;  // 0.012 
```
Note that: 
`!1` evaluated as `false` and `!0` evaluated as `true`.
It is used to shorten the code.

 - Object Types

```javascript
Object Date Array String Number Boolean 
```

 - `typeof` Operator

```javascript
typeof "meerkat"                 // string
typeof 3.14                      // number
typeof NaN                       // number    *
typeof false                     // boolean
typeof {}                        // object
typeof []                        // object    *
typeof function(){}              // function  *
typeof null                      // object    *
typeof undefined                 // undefined *
typeof new Date()                // object
```

The returned types ares: `string number boolean object function undefined`

 - "constructor" Property

```javascript
"meerkat".constructor                 // function String() {[native code]}
(3.14).constructor                    // function Number() {[native code]}
false.constructor                     // function Boolean() {[native code]}
({}).constructor                      // function Object() {[native code]}
[].constructor                        // function Array() {[native code]}
function(){}.constructor              // function Function() {[native code]}
new Date().constructor                // function Date() {[native code]}
```

### Type Conversion

 - Explicit Type Conversion

```javascript
String(123) (123).toString()  // toExponential() toFixed() toPrecision()
String(false) false.toString()
String(Date()) Date().toString()
Number("3.14") Number(" ") Number("") Number("99 88")  // parseFloat() parseInt()
Number(false) Number(new Date()) Number(undefined)
Boolean(1) Boolean("hello") ( Boolean(" ")  // true
Boolean(0) Boolean("")                      // false
parseFloat("12 34 56")  parseInt("12 34 56")   // 12
parseFloat("  88  ") parseInt("  88  ")        // 88
parseFloat("30 days") parseInt("30 days")      // 30
parseFloat("Model 2000") parseInt("Model 2000")  // NaN
```

 - Type Conversion Table *(Source: [JavaScript Tutorial][8])*

```javascript
Original         to Number     to String      to Boolean
false 	            0 	        "false" 	  false
true 	            1 	        "true" 	          true
0 	            0 	        "0" 	          false
1 	            1 	        "1" 	          true
"0" 	            0 	        "0" 	          true*
"000" 	            0 	        "000" 	          true*
"1" 	            1 	        "1" 	          true
NaN 	           NaN 	        "NaN" 	          false
Infinity        Infinity 	"Infinity" 	  true
-Infinity      -Infinity       "-Infinity" 	  true
"" 	            0* 	        "" 	          false*
"20" 	            20 	        "20" 	          true
"twenty"            NaN 	"twenty" 	  true
[] 	            0*	        "" 	          true
[20] 	            20*	        "20" 	          true
[10,20]             NaN 	"10,20" 	  true
["twenty"]          NaN 	"twenty" 	  true
["ten","twenty"]    NaN         "ten,twenty" 	  true
function(){} 	    NaN 	"function(){}" 	  true
{} 	            NaN 	"[object Object]" true
null 	            0*	        "null" 	          false
undefined 	    NaN 	"undefined" 	  false
```

 - Automatic Type Conversion

```javascript
 6 + null    //   6            null converted to 0
"6" + null   //  "6null"       '+' is string operator
"6" + 2      //  "62"
 6 + "2"     //  "62"
"num" + 2    //  "num2"
"6" - 2      //   4            "6" converted to 6
"6" * "2"    //   12           "6" and "2" converted to 6 and 2 
"6" / "2"    //   3
+"num"       //  NaN           Unary '+' Operator is arithmetic operator
-"num"       //  NaN
+"6"         //   6
-"6"         //  -6
+"30 days"   //  NaN
+"12 34 56"  //  NaN
+"  88  "    //  88
+" "         //  0
```

Note that unlike other pure arithmetic operators such as `-`, operator `+` is both a string operator and an arithmetic operator.
If it has two operands, one of them is a string, `+` is perceived as a string operator.

`toString()` automatically called when you try to "output" an object or a variable.


### Hoisting and Strict Mode

Hoisting is JavaScript's default behavior of moving declarations to the top, 
therefore a variable can be used before it has been declared.

Variables defined with `let` or `const` are not hoisted to the top.

The `"use strict";` directive introduced in ECMAScript version 5.
*(More: [JavaScript Use Strict][11])*

Redeclaring variable with `let` or `const` in the same scope is not allowed.

<br>
*(Ref: [JavaScript Tutorial][8])*

## Function and Scope

### Scope Type

 - Global scope

Variables Declared Globally (outside any function or block) have Global Scope.
Undeclared Variables: If you assign a value to a variable that has not been declared, it will automatically become a Global variable.

 - Function scope
   
Variables declared inside a function have Function Scope. Each function creates a new scope. 

 - Block scope 

Variables declared inside a block `{}` with the `let` keyword can have Block Scope.
*(ES2015/ES6)*

 - Loop Scope

Note that when using `let` every loop defines a new `i`.

```javascript
for (var i = 0; i < 3; i++) {                  // output: 3
  setTimeout(() => console.log(i),0);          //         3
}                                              //         3 

for (let i = 0; i < 3; i++) {                  // output: 0
  setTimeout(() => console.log(i),0);          //         1
}                                              //         2
```
*(More: [Variable Binding Problem in Loops][15])*

*(Ref: [JavaScript Tutorial][8])*


### Lexical Scope / Environment

```javascript
function foo() {
  return function innerFoo() {
    console.log(foobar);
  }
}
function bar() {
  var foobar = 0;
  var innerBar= foo();
  innerBar();
}
bar(); // ReferenceError: foobar is not defined 
```
**Lexical scope(static scope)** is the scope where the function is defined, i.e., the program text of the function definition. 
when a function is executed, the name resolution depends on where it was originally defined (the lexical environment of the definition), not the environment where it is called. 
In contrast, dynamic scope means the scope is determined by where the function is called.

The nested scopes of a particular function (from most local to most global) in JavaScript, 
particularly of a closure, used as a callback, are sometimes referred to as the **scope chain**.

*(Ref: [Scope_(computer_science][14])*

### Closure & Private Variables

```javascript
var Bar = (function(){          // Immediately-Invoked Function Expression (IIFE)
             var num = 0;       // Note that "private variable" num is shared by all the Bar instances
             function Foo() {}
             Foo.prototype.getNum = function(){return num;};
             Foo.prototype.setNum = function(n){num = n;};
             return Foo;
          })();

function Foo() {
  var num = 0;            // "private variable" num would be different with each execution of "new Foo()" 
  function Bar(){}
  Bar.prototype.getNum = function(){return num;};
  Bar.prototype.setNum = function(n){num = n;};
  return new Bar();       // override the returned object of "new Foo()" 
}

function Foobar() {       // Another Example:
  var num = 0;            // "private variable" `num` is a a different variable during each function execution.
  return {                // The environment during each execution of the Foobar() function is different. 
    getnum: function() {  // something similar to recursion.
      return num;
    },
    setNUm: function(n) {
      num = n;
    }
  }
}
```

 - Runtime Implementation Detail

Regarding implementations, for storing local variables after the context is destroyed, the stack-based implementation does not fit anymore (because it contradicts the definition of stack-based structure). 
Therefore in this case captured environments(data of the parent context) are stored in the dynamic memory (on the “heap”, i.e. heap-based implementations), with using a garbage collector(GC) (and references counting). Such systems are less effective by speed than stack-based systems. However, implementations can always do different optimizations(optimize it), e.g.: not to allocated data on the heap, if this data is not closured (at parsing stage to find out, whether free variables are used in function, and depending on this decide — to place the data in the stack or in the “heap”). *([Source 1][42], [Source 2][43])*


### Arrow Function

 - Differences

1. Does not bind its own `this`, `arguments`, `super`, or `new.target`. 
2. Always anonymous. 
3. Do not have a prototype property.
3. Best suited for non-method functions, and they cannot be used as constructors.
4. Cannot be used as generators.

```javascript
var func = x => x * x;                   // concise body syntax, implied "return"
var func = (x, y) => { return x + y; };  // with block body, explicit "return" needed
```

 - `this` value in regular function

Regular function defined its own `this` value based on how the function was called:
1. A new object in the case of a constructor.
2. The base object if the function was called as an "object method".
3. undefined in strict mode function calls.

```javascript
function Person() {                // The Person() constructor defines `this` as an instance of itself.
  this.age = 0;       // In non-strict mode, the `growUp()` function defines `this` as the global object
// var that = this;
  setInterval(function growUp() {  // (e.g.,`window` object) 
    this.age++;              // becasue `growUp()` is executed in `window.setInterval`.
// that.age++;
  }, 1000);                  // which is different from the `this` defined by the Person() constructor.

  setInterval(() => {
    this.age++;            // |this| properly refers to the Person object
  }, 1000);
}
var p = new Person();
```

 - Invoked through call or apply

Since arrow functions do not have their own `this`, the methods `call()` and `apply()` can only pass in parameters. 
The first `this` argument of the methods `call()` and `apply()` is ignored.

```javascript
var adder = {
  base: 1,
  add: function(a) {
    var f = v => v + this.base;
    var b = { base: 2 };
    return f.call(b, a);
  }
};
console.log(adder.add(1)); // 2
```

 - Arrow functions do not have a prototype property.
```javascript
var Foo = () => {};
console.log(Foo.prototype); // undefined
```

 - Returning object literals

Returning object literals using the concise body syntax params => {object:literal} will not work as expected.

```javascript
var func = () => { foo: 1 };              // Calling func() returns undefined!
var func = () => { foo: function() {} };  // SyntaxError: function statement requires a name
var func = () => ({ foo: 1 });            // You must wrap the object literal in parentheses
```

Because `{}` is treated as code block separator. 
The code inside curly braces `{}` is parsed as a sequence of statements. 
(i.e. `foo` is treated like a label, not a key in an object literal).


 - Parsing Order

```javascript
callback = callback || () => {};      // SyntaxError: invalid arrow-function arguments
callback = callback || (() => {});    // ok
```

*(Ref: [Arrow Functions][16])*

### IIFE

```javascript
;(function () {})();  // IIFE: Immediately-Invoked Function Expression
!function(){}();      // `!` is a hint of IIFE
(function(){}()); 
```

*(More: [Creating Modules Using IIFE][13])*


### Currying

```javascript
// Question: How would you make this work?
add(2, 5); // 7
add(2)(5); // 7
// Answer:
function add_(a){
   return function(b){
     return a + b;
   };
}
function add(a){
   var acc = 0;
   var sub = function(b){
       if(b === undefined)
         return acc;
       else if(!isNaN(b))
         acc += b;
       return sub;
   };
   return sub(a);
}
```

### Function Expression

 - Hoisting

Function expressions in JavaScript are not hoisted, unlike function declarations. You can't use function expressions before you declare them:

```javascript
notHoisted(); // TypeError: notHoisted is not a function
var notHoisted = function() {
   console.log("bar");
};
```

 - Named function expression

If you want to refer to the current function inside the function body, you need to create a named function expression. 
This name is then local only to the function body (scope). This also avoids using the non-standard `arguments.callee` property.
```javascript
var math = {
  'factorial': function factorial(n) {
    if (n <= 1)
      return 1;
    return n * factorial(n - 1);
  }
};
```

*(Ref: [Function Expression][12])*



### "Function" Constructor

 - Instance Properties

```javascript
Function.arguments Function.caller Function.name
Function.displayName Function.length
```

 - Instance Methods

```javascript
Function.prototype.call() Function.prototype.apply() 
Function.prototype.bind() Function.prototype.toString()
```

### Recursion

There are three ways for a function to refer to itself:

(1).The function's name (2).arguments.callee (3).An in-scope variable that refers to the function

```javascript
var foo = function bar() {  // Recursion uses the function stack
   // Recursive call `bar()`, `arguments.callee()`, `foo()` all equivalent
}
```
    
*(Ref: [Functions][17])*


```javascript
function sumDigits(number) {
    var temp;
    var remainder = number % 10;
    var sum = remainder;
    if (number >= 10) {
        var rest = Math.floor(number / 10);                 // Output:
        console.log(sum + " + sumDigits(" + rest + ")");    // 5 + sumDigits(14)
        temp = sumDigits(rest);                             // 4 + sumDigits(1)
        console.log(sum + " + " + temp);                    // 4 + 1
        sum += temp;                                        // 5 + 5
    }                                                       // 10
    return sum;
}
console.log(sumDigits(145));
```


### Parameters & Returns

In Javascript, parameters and returns are passed by value. Objects are passed by their reference values.

 - Parameters

When an argument value is passed, a new local variable with the corresponding parameter name is created to hold that value. 
If the parameter is an object, the argument value is the reference of the object, 
a new local variable with the corresponding parameter name is created to hold that reference value during the parameter-passing process. 

 - Returns

Objects are not destroyed until all references to them are gone and garbage collected. When the object is returned, 
the calling code gains a reference to it, and the object is not garbage collected.

Technically, the called function's stack frame is destroyed when it returns. The object, however, is not on the stack, but on the heap. 
The function's local reference to the object is on the stack, and is therefore destroyed, but the calling code's reference isn't destroyed until some time later.

*(Ref: [Javascript Return by Value][18])*
<br>

### Function to String

For the built-in Function object, `toSource()` returns the following string indicating that the source code is not available: 
```javascript
function Function() { 
    [native code] 
}
```

```javascript
var s1 = "function a() {}"
var s2 = "(function a() {})"
var f1 = eval(s1)  // return undefined
var f2 = eval(s2)  // return a function
```
<br>
*(Ref: [Object toSource][19])*

## Object Model

### Definitions

JavaScript is an object-based language **based on prototypes**, rather than being class-based.

Class-based object-oriented languages, such as Java and C++, are founded on the concept of two distinct entities: classes and instances.

A **prototype-based language**, such as JavaScript, does not make this distinction: it simply has objects. 
A prototype-based language has the notion of a prototypical object, an object used as a template from which to get the initial properties for a new object. 
Any object can specify its own properties, either when you create it or at run time. 
In addition, any object can be associated as the prototype for another object, allowing the second object to share the first object's properties.

**Prototype-based programming** is a style that allows the creation of an object without first defining its class, 
a style of object-oriented programming in which classes are not explicitly defined, 
but rather derived by adding properties and methods to an instance of another class or, less frequently, adding them to an empty object.

<br>
*(Ref: [Details of the Object Model][2] |&nbsp;[Prototype-based Programming][5])*


### Create Objects

 - Literal Notation(Initializer Notation),

```javascript
var obj = { a: 'foo', b: 56, c: {} };
```

Note that `{}.toString()` is illegal,  use `({}).toString()` instead.

 - `Object.create()`

Creates a new object, using an existing object as the prototype of the newly created object.

`{}` is equivalent to `Object.create(Object.prototype)`, it inherit all the properties and methods from `Object.prototype`.
`Object.create(null)` creates an object that doesn't inherit any property or method, 
so if you use the methods defined in `Object.prototype`, e.g., `Object.create(null).hasOwnProperty('xxx')`, 
it will trigger an error: *"Object doesn't support property or method 'hasOwnProperty' ".*

```javascript
isNaN(Object.create(null)); // Uncaught TypeError: Cannot convert object to primitive value (Chrome)
Number.isNaN(Object.create(null));  // false
```

 - `Object.assign()`

Copy the values of all **own enumerable properties** from one or more source objects to a target object (a **shallow copy**)

Use **spread properties** to shallow-cloning (excluding prototype) or merging objects. 
```javascript
let obj1 = { foo: 'bar', a: 66 }
let obj2 = { foo: 'barr', b: 22 }
let obj3 = { ...obj1, ...obj2 }  // Result: { foo: "barr", a: 66, b: 22 }
```
 - `JSON.parse()`

```javascript
var obj = JSON.parse('{"running":true, "times":33}'); // Note that single quotes 'running' not allowed
```

 - Constructor Functions: `new Object()` / `new Foo()`

<br>
*(Ref: [Object Initializer][6] |&nbsp;[Object Class][7])*

### The Process of Object Creation

*How objects are created when using `new` keyword ?* e.g.:

 - `new` keyword

```javascript
function Foo(){
  this.bar = true;
}
var obj1 = new Foo();
```
When the code `new Foo()` is executed, the following things happen:

1. Function `Foo()` is called, since the function is called with the `new` keyword then it is treated as a constructor, an empty object is created.

2. The object is linked to the function's prototype，inheriting from `Foo.prototype`.

3. `this` bound to the newly created object. `this.bar = true` executed. 
`new Foo` is equivalent to `new Foo()`, i.e. if no argument list is specified, `Foo` is called without arguments.

4. The object returned by the constructor function becomes the result of the whole new expression. 
If the constructor function doesn't explicitly return an object, the object created in the first step will be returned instead. 
(Normally constructors don't return a value, but you can choose to do so if you want to override the normal object creation process.)

 - Shared Variable vs. Instance variable

```javascript
console.log(new Foo().hasOwnProperty("bar"));  // true
Foo.prototype.bar2 = true;
console.log(new Foo().hasOwnProperty("bar2")); // false
```
The value of `bar2` is shared by all the `Foo` instances, 
but `bar` property isn't shared by `Foo` instances, each `Foo` instance could have its own bar value.

 - The Role of `Foo.prototype.constructor` 

```javascript
Foo.prototype.constructor = 3
var obj2 = new Foo();
```
When you declare constructor `Foo`, `Foo.prototype.constructor` will automatically point to `Foo`, `console.log(Foo.prototype)` will show that, 
actually this is called *circular reference*, which should be detected when you traverse an object recursively. 
But in the case of `Foo.prototype.constructor = 3`, fortunately, the process of `new Foo()` expression doesn't involve `Foo.prototype.constructor` property, 
so it will be done correctly, but the value of `obj2.constructor` or `Foo.prototype.constructor` is still 3.

 - `Foo.prototype`

```javascript
Foo.prototype = 3;
var obj3 = new Foo();
console.log(Object.prototype === Object.getPrototypeOf(obj3)); // true
console.log(Foo.prototype); // 3
console.log(obj3.bar); // true
```

`Foo.prototype = 3`, when `new Foo()` executed, as in *Step 2*, the created object is supposed to linked to `Foo.prototype`, 
but since the value of `Foo.prototype` doesn't refer to an object, implicitly a default value was assigned, which is the `Object.prototype`.  *[(More..)][9]*

Due to `object.prototype.constructor`'s reference to the `Object`, `obj3.constructor` also refer to the `Object`, 
but `obj3`'s *de facto* constructor is still `Foo`, because of *Step 1*. 


<br>
*(Ref: [Object Initializer][6] |&nbsp;[Object Class][7] |&nbsp;[How objects are created?][10])*


### "Object" Constructor

 - Static Methods

```javascript
Object.create() Object.assign() 
Object.getPrototypeOf() Object.setPrototypeOf()
Object.keys() Object.getOwnPropertyNames() Object.values() 
Object.defineProperty() Object.defineProperties()
Object.getOwnPropertyDescriptor() Object.getOwnPropertyDescriptors()
Object.getOwnPropertySymbols() Object.is() 
Object.entries() Object.fromEntries()
Object.freeze() Object.isFrozen()
Object.seal() Object.isSealed()
Object.preventExtensions() Object.isExtensible()
```

 - Instance Properties

```javascript
Object.prototype.constructor
Object.prototype.__proto__    // Deprecated
Object.prototype.__noSuchMethod__
```

 - Instance Methods

```javascript
Object.prototype.__defineGetter__() Object.prototype.__defineSetter__()
Object.prototype.__lookupGetter__() Object.prototype.__lookupSetter__()
Object.prototype.hasOwnProperty() Object.prototype.isPrototypeOf()
Object.prototype.propertyIsEnumerable() Object.prototype.valueOf()
Object.prototype.toString() Object.prototype.toLocaleString()
Object.prototype.watch() Object.prototype.unwatch()
```


### Interitance

Different objects with the same values treated as the same, a deep unique set by extending the original `Set`.

```javascript
function DeepSet() {
    //
}
DeepSet.prototype = Object.create(Set.prototype);
DeepSet.prototype.constructor = DeepSet;
DeepSet.prototype.add = function(o) {
    for (let i of this)
        if (deepCompare(o, i))  // deepCompare() is a custom method defined by user
            throw "Already existed";
    Set.prototype.add.call(this, o);
};
```

### Getter & Setter

```javascript
var obj = {
  n: 0,
  get num() {
    return this.n;
  },
  set num(x) {
    this.n = x;
  }
};
console.log(obj.num);
obj.num = 6;
```


### Deep Compare & Deep Clone

 - `deepCompare` function

A deep compare function is needed to test deep clone function.

e.g., some code could be used in the `deepCompare` function :

```javascript
if (o1.isPrototypeOf(o2) || o2.isPrototypeOf(o1))
    return false;
if (o1.constructor !== o2.constructor)
    return false;
if (o1.prototype !== o2.prototype)
    return false;
```

 - Circular Reference & Built-in Objects Detection

Deep Cloning is a recursive process that traverses an object tree copying every node, 
built-in objects should be regarded as the leaf nodes of the deep cloning tree like primitive values such as numbers or strings
while the cloned circular reference should parallelly mirror the original circular reference by pointing to the previously cloned object(or itself) at the same position.

e.g., some built-in objects:

```javascript
console.log(Window.prototype.__proto__.__proto__.__proto__ === Object.prototype);  // true (Firefox & Chrome)
console.log(Window.prototype.__proto__ === Object.prototype);                      // true (IE11)
console.log(window instanceof Window);                  // true
console.log(Window.prototype.isPrototypeOf(window));    // true 
```

 - Problems

Some built-in objects have properties hidden in a closure(namespace), such as Date/String objects. 
So if you clone an object by just recursively copying all the enumerable or non-enumerable variables/methods into an empty object, it won't work. 
It is impossible to detect the private variables(within closure) of an object in Javascript. 

Plus, after definition, the lexical environment of a function is neither readable nor replicable through code therefore it cannot be cloned.

People who try to write a general deep clone function will eventually find it almost impossible to be perfect.

Due to the inherent limitations of deep cloning discussed above, we are not supposed to place too much confidence in general cloning functions. 
It depends on the structure of the objects to be cloned. If there is no private variables or functions, 
or the lexical scopes of functions can be ignored, in that case, a general deep clone function would be fine.
If you know the internal structure of the objects to be cloned, 
it is better to write a custom function specialized in cloning specific types of objects.


### Output Object Structure

e.g., traverse an object and output all the enumerable properties: 

```javascript
function joOutput(o, decodeUrl) {
  var txt, depth, sp, sub, isEmpty;
  if (typeof o !== "object")
    return "[Not an object]";
  isEmpty = function(e) {
    var i;
    for (i in e)
      return false;
    return true;
  };
  if (isEmpty(o))
    return "[Empty Object]";
  txt = "<b>NOTE:</b>n for attribute name, d for depth, v for value.<br>";
  txt += "-----------------------------------<br>";
  depth = 0;

  sp = function(n) {
    var s = "";
    for (var i = 0; i < n; i++) {
      s += "&nbsp&nbsp&nbsp&nbsp&nbsp.";
    }
    return s;
  };
  sub = function(obj) {
    var attr;
    for (attr in obj) {
      if ((typeof obj[attr]) !== "object") {
        if (decodeUrl)
          obj[attr] = decodeURIComponent(obj[attr]);
        txt += sp(depth) + "[n: " + attr + " - d: " + depth + " - v: <b>" + obj[attr] + "</b>]<br>";
      } else {
        txt += sp(depth) + "[n:" + attr + " - d:" + depth + "]...<br>";
        depth++;
        arguments.callee(obj[attr]);
      }
    }
    depth--;
    return txt;
  };
  return sub(o);
}

// Test:
var templateObject = {
    "addressbook": {
        "streetaddress": ["streetaddress1", "1"],
        "country": ["country", "2"]
    },
    "companyname": ["thecompanyname", "1"],
    "email": ["theemail", "1"]
};

var txt = joOutput(templateObject);
document.write(txt);
```

Results:

```
NOTE:n for attribute name, d for depth, v for value.
-----------------------------------
[n:addressbook - d:0]...
     .[n:streetaddress - d:1]...
     .     .[n: 0 - d: 2 - v: streetaddress1]
     .     .[n: 1 - d: 2 - v: 1]
     .[n:country - d:1]...
     .     .[n: 0 - d: 2 - v: country]
     .     .[n: 1 - d: 2 - v: 2]
[n:companyname - d:0]...
     .[n: 0 - d: 1 - v: thecompanyname]
     .[n: 1 - d: 1 - v: 1]
[n:email - d:0]...
     .[n: 0 - d: 1 - v: theemail]
     .[n: 1 - d: 1 - v: 1]
```

## Grobal Methods

### Global Built-in Functions

```javascript
eval() uneval()
isFinite() isNaN()
parseFloat() parseInt()
decodeURI() decodeURIComponent()
encodeURI() encodeURIComponent()
escape() unescape()
```

 - The difference between `decodeURIComponent` and `decodeURI`

The encodeURI function is intended for use on the full URI.
The encodeURIComponent function is intended to be used on URI components that is any part that lies between separators(; / ? : @ & = + $ , #).
These separators are encoded also because they are regarded as text and not special characters.

```javascript
encodeURIComponent("&") returns "%26".
decodeURIComponent("%26") returns "&".
encodeURI("&") returns "&".
decodeURI("%26") returns "%26".
```

*(Ref: [The difference between decodeuricomponent() and decodeuri()][28])*

 - `setTimeout()` & `setInterval()`

```javascript
console.log('one');          //output: one
setTimeout(function() {      //        three
  console.log('two');        //        two
}, 0);
console.log('three');
```

 - `isNaN()`

```javascript
// Number.isNaN() & Polyfill
Number.isNaN = Number.isNaN || function(value) {
    return typeof value === 'number' && isNaN(value);
}
// Or
Number.isNaN = Number.isNaN || function(value) {
    return value !== value;
}
```



## Array


 - Properties

```javascript
Array.prototype.length Array.prototype[@@unscopables]
```

 - Methods

```javascript
Array.from() Array.isArray() Array.of()
Array.prototype.forEach() Array.prototype.reverse() 
Array.prototype.join() Array.prototype.sort() Array.prototype.slice()
Array.prototype.pop() Array.prototype.push() Array.prototype.shift() Array.prototype.unshift()
Array.prototype.reduce() Array.prototype.reduceRight()
Array.prototype.every() Array.prototype.some()
Array.prototype.filter() Array.prototype.map()
Array.prototype.find() Array.prototype.findIndex() Array.prototype.includes()
Array.prototype.indexOf() Array.prototype.lastIndexOf()
Array.prototype.concat() Array.prototype.splice()
Array.prototype.keys() Array.prototype.values() Array.prototype.entries()
Array.prototype.toString() Array.prototype.toLocaleString()
Array.prototype.flat() Array.prototype.flatMap()
Array.prototype.copyWithin() Array.prototype.fill()
get Array[@@species] Array.prototype[@@iterator]()
```

e.g.:

```javascript
var a={length: 2, 0: 'zero', 1: 'one'};
Array.prototype.slice.call(a);  //  ["zero", "one"]
var a={length: 2};
Array.prototype.slice.call(a);  //  [undefined, undefined]
Array.prototype.slice.call("apple", 0);  // "apple".split('');
```

In Javascript, Arrays are objects that allow you to reference their properties with numeric instances.
Internally, all numeric keys are converted to strings. The Length property merely fetches the highest index, 
and adds 1 to it. Nothing more. When you iterate your array, the object is iterated for each key.

In most programming languages, an array is a contiguous block of memory allocation with homogeneous elements.
However, that is not the case with Javascript. In the first version of JavaScript, there were no arrays. They were later introduced as a sub-class of `Object`.
A Javascript array is simply an object with array like characteristics reflected through certain methods.
For sparse array, the `undefined` values are not there, they're merely the default return value when Javascript scans an object and the prototypes and can't find what you're looking for.
Working with largely undefined arrays doesn't affect the size of the object itself, but accessing an undefined key might be very slow, because the prototypes have to be scanned, too.

<a></a>
*(Ref: [Memory Management of Javascript Array][29] | [Common Pitfalls When Working With Javascript Arrays][30])*

<br>
e.g., search an object within an array: 

```javascript
var cars = [
    { id:23, make:'honda', color: 'green' },
    { id:36, make:'acura', color:'silver' },
    { id:18, make:'ford', color:'blue' },
    { id:62, make:'ford', color:'green' } ];
let cars_s = cars.map(x => x.id);
let i = cars_s.indexOf(18);
console.log(i); // 2
console.log(cars[i]); // { id:18, make:'ford', color:'blue' }

let index = {};
for (let i = 0; i < cars_s.length; i++) {
    index[cars_s[i]] = i;
}
console.log(index[18]); // 2
```

## String

 - Properties

`.length`

 - Methods

```javascript
String.prototype.split() String.prototype.substring() String.prototype.slice() 
String.prototype.startsWith() String.prototype.endsWith() String.prototype.includes()
String.prototype.replace() String.prototype.replaceAll()
String.prototype.match() String.prototype.matchAll() String.prototype.search() 
String.prototype.indexOf() String.prototype.lastIndexOf()
String.prototype.charAt() String.prototype.localeCompare()
String.prototype.charCodeAt() String.prototype.codePointAt()
String.prototype.padEnd() String.prototype.padStart()
String.prototype.trim() String.prototype.trimEnd() String.prototype.trimStart()
String.prototype.repeat() String.prototype.toString() String.prototype.valueOf()
String.prototype.toLocaleLowerCase() String.prototype.toLocaleUpperCase()
String.prototype.toLowerCase() String.prototype.toUpperCase()
String.prototype.normalize() String.raw()
String.fromCharCode() String.fromCodePoint()
String.prototype.concat() String.prototype[@@iterator]()
```

e.g.:

```javascript
function pad(num, size) {
   var s = num + "";
   while (s.length < size) s = "0" + s;
   return s;
}
pad(15, 4); // 0015
```

*([Padding Zeros][20])*


```javascript
function palindrome(str) {
    var temp;
    temp = str.toLowerCase();
    temp = temp.replace(/[^A-Za-z0-9]/g, '');
    console.log(temp);
    for (let a = 0, b = temp.length - 1; b > a; a++, b--) {
        if (temp.charAt(a) !== temp.charAt(b))
            return false;
    }
    return true;
}
```

## Other Built-in Objects


<br>
*(More: [Global Object Date][37] | [Global Object Math][38] | [Global Object RegExp][39] | [Global Object Error][40])*


## DOM

<br>
*(More: [JavaScript HTML DOM][31] | [Document Object][32] | [Element Object][33] | [Events][34])*


## BOM

<br>
*(More: [JS Browser BOM][35] | [JS Window][36])*

## Ajax

```javascript
 function loadDoc(url, myCallback) {
  var xhttp;
  xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      myCallback(this);
    }
  };
  xhttp.open("GET", url, true);
  xhttp.send();
}
function myFunction(xhttp) {
  document.getElementById("demo").innerHTML =
  xhttp.responseText;
}
loadDoc("url-1", myFunction);
```

The `readyState` property holds the status of the `XMLHttpRequest`.

```
0: request not initialized
1: server connection established
2: request received
3: processing request
4: request finished and response is ready
```

The `onreadystatechange` property defines a function to be executed when the `readyState` changes.

The `status` property and the `statusText` property holds the message returned from the server.

```
status 200: "OK"
403: "Forbidden"
404: "Page not found"
502: "Bad Gateway"
```

`statusText` Returns the status-text (e.g. "OK" or "Not Found")

*(Ref: [Ajax http response][24])*

  - cross domain call


## JSON

 - Methods
```javascript
JSON.parse() JSON.stringify()
```
 - number vs string
```javascript
{"a":123}     // number
{"a":"123"}   // string
```

 - toString vs. stringify

```javascript
["1,",2,3].toString();       //"1,,2,3" ... so you can't just split by comma and get original array
                             //it is in fact impossible to restore the original array from this result
JSON.stringify(["1,",2,3])   //'["1,",2,3]' //original array can be restored exactly

({ a: 'a', '1': 1 }).toString()         //  "[object Object]"
JSON.stringify({ a: 'a', '1': 1 })      // "{"1":1,"a":"a"}" 
```

*(Ref: [toString vs. stringify][23])*

*(More: [JSON][22])*



## JSONP

Requesting a file from another domain can cause problems, due to cross-domain policy.
Requesting an external script from another domain does not have this problem.
JSONP uses this advantage, and request files using the `script` tag instead of the `XMLHttpRequest` object.

Creating a dynamic script tag when needed.
The following example above will load data and execute the "myFunc" function after the script tag is created.

```javascript
function clickButton() {
  var s = document.createElement("script");
  s.src = "jsonp_demo_db.php?callback=myFunc";
  document.body.appendChild(s);
}
```

The server page offers a **callback function** as a parameter 
so that it will call the correct local function after the script is loaded.

*(Ref:  [JSONP][41])*


## Web Workers & WebSocket

*(More: [Web Workers API][25])*

*(More: [WebSockets API][26])*

<br>

## Classes
> JavaScript classes, introduced in ECMAScript 2015 (ES6), are primarily **syntactical sugar** over JavaScript's existing prototype-based inheritance. 
The `class` syntax does not introduce a new object-oriented inheritance model to JavaScript.
A class is a type of function, but instead of using the keyword function to initiate it, we use the keyword `class`.

<br>
 - `constructor()` method

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}
```

<br>
 - Hoisting

An important difference between function declarations and class declarations is that function declarations are hoisted and class declarations are not.


<br>
 - Public field declarations

```javascript
class Rectangle {
  height = 0;
  width;
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

<br>
 - Private field declarations

```javascript
class Rectangle {
  #height = 0;
  #width;
  constructor(height, width) {
    this.#height = height;
    this.#width = width;
  }
}
```

<br>
 - `static` keyword

The `static` keyword defines a static method for a class. 
Static methods are called without instantiating their class and **cannot be called through a class instance**.

Instance properties must be defined inside of class methods.
**Static (class-side) data properties and prototype data properties** must be defined outside of the ClassBody declaration:
```javascript
Rectangle.staticWidth = 20;
Rectangle.prototype.prototypeWidth = 25;
```

<br>
 - Strict Mode

The code within the class body's syntactic boundary is always executed in **strict mode**.
When a static or prototype method is called without a value for `this`, the `this` value will be `undefined` inside the method.

```javascript
class Animal { 
  static eat() {
    return this;
  }
}
let obj = new Animal();
Animal.eat() // class Animal
let eat = Animal.eat;
eat(); // undefined
```

If the above code is written using traditional non–strict mode function-based syntax, 
then **autoboxing** in method calls will happen: &nbsp; 
If the initial value is `undefined`, this will be set to the global object.

<br>
 - `super` keyword

A constructor can use the `super` keyword to call the constructor of the super class.

The `super` keyword is used to call corresponding methods of super class. This is one advantage over prototype-based inheritance.

<br>
 - `extends` keyword

The `extends` keyword is used in sub classing / inheritance.

If there is a constructor present in the subclass, it needs to first call `super()` before using `this` to assign values to properties.

One class may also extend traditional constructor function.

To inherit from a regular object, you can instead use Object.setPrototypeOf(), e.g.:
```javascript
Object.setPrototypeOf(DogCls.prototype, animalObj);
```

<br>
 - Mix-ins

Mix-ins are templates for classes.

A function with a superclass as input and a subclass extending that superclass as output:
```javascript
let calculatorMixin = Base => class extends Base {
  calc() { }
};
let randomizerMixin = Base => class extends Base {
  randomize() { }
};

class Foo { }
class Bar extends calculatorMixin(randomizerMixin(Foo)) { }
```

<br>
 - Species
  
*(See source)*

<a></a>
*(Source: [JavaScript Classes][3] |&nbsp;[ES6][4])*




## References & Resources  {#references}

*\[1\]. [MDN JavaScript Docs][1]*
{: id="ref-mdn-javaScript-docs" class="ref-item" }

*\[2\]. [Details of the Object Model][2]*
{: id="ref-mdn-details-of-the-object-model" class="ref-item" }

*\[3\]. [JavaScript Classes][3]*
{: id="ref-mdn-javaScript-classes" class="ref-item" }

*\[4\]. [ECMAScript 6 - ECMAScript 2015][4]*
{: id="ref-w3schools-es-6" class="ref-item" }

*\[5\]. [Prototype-based Programming][5]*
{: id="ref-mdn-prototype-based-programming" class="ref-item" }

*\[6\]. [Object Initializer][6]*
{: id="ref-mdn-object-initializer" class="ref-item" }

*\[7\]. [Object Class][7]*
{: id="ref-mdn-object-class" class="ref-item" }

*\[8\]. [JavaScript Tutorial][8]*
{: id="ref-w3schools-javaScript-tutorial" class="ref-item" }

*\[9\]. [How objects are created when the prototype of their constructor isnt an object?][9]*
{: id="ref-stackoverflow-constructor-isnt-an-object" class="ref-item" }

*\[10\]. [How objects are created?][10]*
{: id="ref-stackoverflow-how-do-objects-created" class="ref-item" }

*\[11\]. [JavaScript Use Strict][11]*
{: id="ref-w3schools-javajcript-use-strict" class="ref-item" }

*\[12\]. [Function Expression][12]*
{: id="ref-mdn-function-expression" class="ref-item" }

*\[13\]. [Creating Javascript Modules][13]*
{: id="ref-creating-javascript-modules" class="ref-item" }

*\[14\]. [Scope_(computer_science][14]*
{: id="ref-wikipedia-Scope-computer-science" class="ref-item" }

*\[15\]. [Creating closures in loops][15]*
{: id="ref-mdn-creating-closures-in-loops" class="ref-item" }

*\[16\]. [Arrow Functions][16]*
{: id="ref-mdn-arrow-functions" class="ref-item" }

*\[17\]. [Functions][17]*
{: id="ref-mdn-functions" class="ref-item" }

*\[18\]. [Javascript Return by Value][18]*
{: id="ref-stackoverflow-javascript-return-by-value" class="ref-item" }

*\[19\]. [Object toSource][19]*
{: id="ref-mdn-object-tosource" class="ref-item" }

*\[20\]. [Padding Zeros][20]*
{: id="ref-stackoverflow-padding-zeros" class="ref-item" }

*\[21\]. [for...of][21]*
{: id="ref-mdn-for-of" class="ref-item" }

*\[22\]. [JSON][22]*
{: id="ref-mdn-json" class="ref-item" }

*\[23\]. [toString vs. stringify][23]*
{: id="ref-stackoverflow-tostring-vs-stringify" class="ref-item" }

*\[24\]. [Ajax http response][24]*
{: id="ref-w3schools-ajax-http-response" class="ref-item" }

*\[25\]. [Web Workers API][25]*
{: id="ref-mozilla-web-workers-api" class="ref-item" }

*\[26\]. [WebSockets API][26]*
{: id="ref-mozilla-web-workers-api" class="ref-item" }

*\[27\]. [Comma Operator][27]*
{: id="ref-mozilla-comma-operator" class="ref-item" }

*\[28\]. [The difference between decodeuricomponent() and decodeuri()][28]*
{: id="ref-stackoverflow-difference-between-decodeuricomponent-and-decodeuri" class="ref-item" }

*\[29\]. [Memory Management of Javascript Array][29]*
{: id="ref-stackoverflow-memory-management-of-javascript-array" class="ref-item" }

*\[30\]. [Common Pitfalls When Working With Javascript Arrays][30]*
{: id="ref-common-pitfalls-when-working-with-javascript-arrays" class="ref-item" }

*\[31\]. [JavaScript HTML DOM][31]*
{: id="ref-w3schools-js-html-dom" class="ref-item" }

*\[32\]. [Document Object][32]*
{: id="ref-mdn-document-object" class="ref-item" }

*\[33\]. [Element Object][33]*
{: id="ref-mdn-element-object" class="ref-item" }

*\[34\]. [Events][34]*
{: id="ref-mdn-events" class="ref-item" }

*\[35\]. [JS Browser BOM][35]*
{: id="ref-w3schools-js-browser-bom" class="ref-item" }

*\[36\]. [JS Window][36]*
{: id="ref-mdn-window" class="ref-item" }

*\[37\]. [Global Object Date][37]*
{: id="ref-mdn-date" class="ref-item" }

*\[38\]. [Global Object Math][38]*
{: id="ref-mdn-math" class="ref-item" }

*\[39\]. [Global Object RegExp][39]*
{: id="ref-mdn-regexp" class="ref-item" }

*\[40\]. [Global Object Error][40]*
{: id="ref-mdn-error" class="ref-item" }

*\[41\]. [JS JSONP][41]*
{: id="ref-w3schools-jsonp" class="ref-item" }

*\[42\]. [Chapter-6-Closures][42]*
{: id="ref-chapter-6-closures" class="ref-item" }

*\[43\]. [Javascript closures on heap or stack][43]*
{: id="ref-stackoverflow-closures-on-heap-or-stack" class="ref-item" }


[1]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript>  "MDN JavaScript Docs"

[2]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model>  "Details of the Object Model"

[3]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes>  "JavaScript Classes"

[4]: <https://www.w3schools.com/js/js_es6.asp>  "ECMAScript 6 - ECMAScript 2015"

[5]: <https://developer.mozilla.org/en-US/docs/Glossary/Prototype-based_programming>  "Prototype-based Programming"

[6]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer>  "Object Initializer"

[7]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object>  "Object Class"

[8]: <https://www.w3schools.com/js/default.asp>  "JavaScript Tutorial"

[9]: <https://stackoverflow.com/questions/41846565/how-objects-are-created-when-the-prototype-of-their-constructor-isnt-an-object>  "How objects are created when the prototype of their constructor isnt an object?"

[10]: <https://stackoverflow.com/questions/41822188/how-do-objects-created>  "How objects are created?"

[11]: <https://www.w3schools.com/js/js_strict.asp>  "JavaScript Use Strict"

[12]: <https://developer.mozilla.org/en-US/docs/web/JavaScript/Reference/Operators/function>  "Function Expression"

[13]: <http://qnimate.com/creating-javascript-modules/>  "Creating Javascript Modules"

[14]: <https://en.wikipedia.org/wiki/Scope_(computer_science)/>  "Scope_(computer_science)"

[15]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#Creating_closures_in_loops_A_common_mistake>  "Creating closures in loops"

[16]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions>  "Arrow Functions"

[17]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions>  "Functions"

[18]: <https://stackoverflow.com/questions/10956970/returning-by-reference-in-javascript>  "Javascript Return by Value"

[19]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toSource>  "Object toSource"

[20]: <https://stackoverflow.com/questions/41372291/javascript-convert-number-with-leading-zero-to-string-it-change-the-decimal-numb>  "Padding Zeros"

[21]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of>  "for...of"

[22]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON>  "JSON"

[23]: <https://stackoverflow.com/questions/15834172/whats-the-difference-in-using-tostring-compared-to-json-stringify>  "toString vs. stringify"

[24]: <https://www.w3schools.com/js/js_ajax_http_response.asp>  "Ajax http response"

[25]: <https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API>  "Web Workers API"

[26]: <https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API>  "WebSockets API"

[27]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator>  "Comma Operator"

[28]: <https://stackoverflow.com/questions/747641/what-is-the-difference-between-decodeuricomponent-and-decodeuri>  "The difference between decodeuricomponent() and decodeuri()"

[29]: <https://stackoverflow.com/questions/12640216/memory-management-of-javascript-array>  "Memory Management of Javascript Array"

[30]: <https://www.thecodeship.com/web-development/common-pitfalls-when-working-with-javascript-arrays/>  "Common Pitfalls When Working With Javascript Arrays"

[31]: <https://www.w3schools.com/js/js_htmldom.asp>  "JavaScript HTML DOM"

[32]: <https://developer.mozilla.org/en-US/docs/Web/API/Document>  "Document Object"

[33]: <https://developer.mozilla.org/en-US/docs/Web/API/Element>  "Element Object"

[34]: <https://developer.mozilla.org/en-US/docs/Web/Events>  "Events"

[35]: <https://www.w3schools.com/js/js_window.asp>  "JS Browser BOM"

[36]: <https://developer.mozilla.org/en-US/docs/Web/API/Window>  "JS Window"

[37]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date>  "Global Object Date"

[38]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math>  "Global Object Math"

[39]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp>  "Global Object RegExp"

[40]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error>  "Global Object Error"

[41]: <https://www.w3schools.com/js/js_json_jsonp.asp>  "JS JSONP"

[42]: <http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/>  "Chapter-6-Closures"

[43]: <https://stackoverflow.com/questions/16959342/javascript-closures-on-heap-or-stack>  "Javascript closures on heap or stack?"









