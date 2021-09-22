# Javascript

## [Front end dev documentation](https://developer.mozilla.org/en-US/docs/Learn/Front-end_web_developer)
## [javascript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)

JavaScript (JS) is a lightweight, interpreted, or [just-in-time compiled](https://en.wikipedia.org/wiki/Just-in-time_compilation) programming language with [first-class functions.](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)  JavaScript is a [prototype-based](https://developer.mozilla.org/en-US/docs/Glossary/Prototype-based_programming), multi-paradigm, single-threaded, dynamic language, 

### Basics 
A semicolon is not necessary after a statement if it is written on its own line. But if more than one statement on a line is desired, then they must be separated by semicolons.

[Hashbang comments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#hashbang_comments)

var is not block scoped(it leaks out of any blocks if {} whatever), but function scoped
let and const are block scoped
A JavaScript identifier must start with a letter, underscore (_), or dollar sign ($). Subsequent characters can also be digits (0–9).
when you declare a variable without any keyword it becomes a global variable, basically a property of the global object

#### Global variables
Global variables are in fact properties of the global object.
In web pages, the global object is window, so you can set and access global variables using the window.variable syntax.
you can access global variables declared in one window or frame from another window or frame by specifying the window or frame name. For example, if a variable called phoneNumber is declared in a document, you can refer to this variable from an iframe as parent.phoneNumber.

#### const
must be initialized to a value when declaring it  (even if it is undefined that's fine)
You cannot declare a constant with the same name as a function or variable in the same scope.

the properties of objects assigned to constants are not protected,
```
(function(){
  const obj = {1: "fuck"};
  obj[1] = "bitch";
  console.log(obj[1]);// bitch
//   obj = {2: "bithc"}; // this one gives error obviously
})();

```
the contents of an array are not protected, so the following statement is executed without problems.

```
const MY_ARRAY = ['HTML','CSS'];
MY_ARRAY.push('JAVASCRIPT');
console.log(MY_ARRAY); //logs ['HTML','CSS','JAVASCRIPT'];
```

[Array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#array_destructuring)  
[Object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#object_destructuring)

#### Variable hoisting 
Variables declared are lifted to the top of function or statement, but only the declaration is hoisted not the initialization

```
(function(){
  var myvar = 'my value';

(function() {
  console.log(myvar); // undefined
  var myvar = 'local value';
})();
  
})();
```

for `var` the variables become `undefined` until they are initialized 
for `let` and `const` they throw `ReferenceError` until they are initialized / delcared(in which case they become undefined)
(Var can be redeclared unlike let and const)

#### [Horror of implicit globals](http://blog.niftysnippets.org/2008/03/horror-of-implicit-globals.html) 

```
(function(){
  a = 3;
  console.log(window.a); //variable with no keywords, gets attached to window (or global) object
})();
var b = 4;//internally added to the window(or global) object
console.log(window.b); //also 4
let c = 5;
console.log(window.c);//undefined 
//let, const, and class statements at global scope create globals that aren't properties of the global object
```

#### Function hoisting
In the case of functions, only function declarations are hoisted—but not the function expressions.

```
/* Function declaration */

foo(); // "bar"

function foo() {
  console.log('bar');
}

/* Function expression */

baz(); // TypeError: baz is not a function

var baz = function() {
  console.log('bar2');
};

```
### Data structures and types 

#### [Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#data_types)

for + js converts numeric value to strings 
for other operators like - it does not 

```
"20" - 7 //13   
"fuck" - 5 //NaN
```

##### parseInt and parseFloat

```
parseInt('101',2) //5 because base 2 
parseInt('1010') // 1010, no second parameter means base 10 

parseInt('123a'); //123
parseInt('a123'); //NaN
parseInt('123abcdef69'); //123
parseInt('    456df'); //456 (leading whitespace is ignored )

parseFloat('1.2344abcdef'); //1.2344
parseFloat('a1.2344abcdef'); //NaN
parseFloat('     1.2344abcdef'); //1.2344 
```

you can also make number from string by unary operator +   
`var a = (+"1.1") + (+"2.2"); // 3.3`  

#### Array literal 
`[5,,3] //creates an array of size 3 with one empty slot in between, whose value is undefined`  
trailing comma at the end of array literal lke [1,] is ignored  

``` 
[1,2,] //array of length 2 
[,1,2] //array of length 3
```

#### Numeric literals 
- Number and BigInt types can be written in decimal (base 10), hexadecimal (base 16), octal (base 8) and binary (base 2).
-  A decimal number doesn't have a leading zero 
- A leading 0 (zero) on a numeric literal, or a leading 0o (or 0O) indicates it is in octal.
- A leading 0x (or 0X) indicates a hexadecimal numeric type (The case of a character does not change its value. Therefore: 0xa = 0xA = 10 and 0xf = 0xF = 15) 
- A leading 0b (or 0B) indicates a binary numeric literal.

```
var a = 0123;
console.log(a); //83
```

#### [Floating point literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#floating-point_literals)

#### Object literal 
`var obj = {fuck: 1, bitch: obj.fuck*2}; // obj is undefined error`

Property names that are not valid identifiers cannot be accessed as a dot (.) property, but can be accessed and set with the array-like notation("[]").

```
var unusualPropertyNames = {
  '': 'An empty string',
  '!': 'Bang!'
}
console.log(unusualPropertyNames.'');   // SyntaxError: Unexpected string
console.log(unusualPropertyNames['']);  // An empty string
console.log(unusualPropertyNames.!);    // SyntaxError: Unexpected token !
console.log(unusualPropertyNames['!']); // Bang!
```

```
(function(){
  var var1 = {1: 5};
  var var2 = {1: 5};
  console.log(var1 == var2); //false
  console.log(var1 === var2); //false
})();
```

```
(function(){
  var var1 = {1: 5};
  var var2 = {};
  var obj = {};
  obj[var1] = 5;
  console.log(obj[var2]); //returns 5 because only strings and symbols can be keys in objects and 
  // any object is translated to  '[object Object]'
})();
```

you can also declare objects to set prototype at construction, simply put a variable which has been previously declared without needing to specify its value, defining methods, making super calls, and computing property names with expressions 

```
(function(){
	var x = 5;
  var obj = {
    a: 4, //simply setting a value
    x,// previously declared value 
//     5, //this will give error
    func : (function(){
      console.log("fuck");
    })//delcaring a function 
    ,
    bitch(){
      console.log("bitch");
    } //simply declaring a function, using function keyword before it, gives error
    ,
    ['prop_' + 50]: 69 //dynamic property naming 
  };
  console.log(obj);
})();
```

### [template literals](https://stackoverflow.com/questions/35835362/what-does-dollar-sign-and-curly-braces-mean-in-a-string-in-javascript)  
#### [Tagged templates] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)

#### Escaping characters 

```
var str = 'this string \
is broken \
across multiple \
lines.'
console.log(str);   // this string is broken across multiple lines., 
// (actually not afterall the line break is escaped by the backsash
```

## Control Flow and Error Handling

#### Falsy values 
- false
- undefined 
- null 
- 0 
- NaN
- ""

all these values evaluate to false, everything else evaluates to true (even all the objects) 

```
var b = new Boolean(false);
if (b)         // this condition evaluates to true, because b is an object
if (b == true) // this condition evaluates to false, they are completely different thing 
//afterall 1 and 5 both evaluate to true doesn't mean they are equal
var a = new Boolean(true);
if(a == true) // this is true, because if you leave the types they have the same value 
```

if/else and switch are the same as cpp

### Exception handling

you can throw anything, numbers, strings, objects etc. 

```
(function(){
  throw {fuckyeah: 2};
})();
```

#### [Creating objects through functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#using_a_constructor_function)

```
(function(){
  function UserException(message){
    this.name = 'UserException';
    this.message = message;
  }
  UserException.prototype.toString = function(){
    return `${this.name} => "${this.message}"`;
  }
  var exception = new UserException("oh no jesus fucking christ");
  exception = exception.toString();
  throw exception;
})();
```

#### try/catch

The finally block executes after the try and catch blocks execute but before the statements following the try...catch statement.
If the finally block returns a value, this value becomes the return value of the entire try…catch…finally production, regardless of any `return` statements in the try and catch blocks: 

```
function fuck(){
  try{
    var a = 0/0;
    return 1;
  }
  catch(err){
    return 2;
  }
  finally{
    return 3;
  }
}
console.log(fuck()); // returns 3, kind of like execution moves from the return of try/catch to finally
```

Overwriting of return values by the finally block also applies to exceptions thrown or re-thrown inside of the catch block:

```
function f() {
  try {
    throw 'bogus';
  } catch(e) {
    console.log('caught inner "bogus"');
    throw e; // this throw statement is suspended until
             // finally block has completed
  } finally {
    return false; // overwrites the previous "throw"
  }
  // "return false" is executed now
}

try {
  console.log(f());
} catch(e) {
  // this is never reached!
  // while f() executes, the `finally` block returns false,
  // which overwrites the `throw` inside the above `catch`
  console.log('caught outer "bogus"');
}

// OUTPUT
// caught inner "bogus"
// false
```

For the Error Object, the `name` property provides the general class of Error (such as DOMException or Error), while `message` generally provides a more succinct message than one would get by converting the error object to a string.

```
(function(){
  try{
    throw new Error("oh fuck");
  }
  catch(error){
    console.log(error.name); //Error
    console.log(error.message); // oh fuck
  }
})();
```

### Prototype
#### Getting prototype of Object or functions 

```
(function(){
  function Person(){
    this.name = "Khushal";
    this.age = 21;
  }
  const person = new Person();
  console.log(Person.prototype);
  console.log(Object.getPrototypeOf(person));
  //or person.__proto__ also works , but it is **Deprecated**
})()
```

objects inherit properties and methods from prototype 

```
(function(){
function Person () {
    this.name = 'John',
    this.age = 23
}
const person1 = new Person();
Person.prototype.gender = 'male';
console.log(Person.prototype);
console.log(person1.gender); //male
Person.prototype.gender = "female";
console.log(person1.gender);  //female
})()
```

if prototype value itself is changed then all objects before will retain the value and new objects shall have the updated value (it's fine even if you add new properties but if you change the prototype directly using `=` then the old objects shall retain the old values)

```
(function(){
function Person () {
    this.name = 'John',
    this.age = 23
}
const person1 = new Person();
Person.prototype.gender = 'male';
console.log(Person.prototype);
console.log(person1.gender); //male
Person.prototype = {gender: "female"};
console.log(person1.gender);  //male
})()
```

If an object tries to access the same property that is in the constructor function and the prototype object, the object takes the property from the constructor function.

## Loops and Iterations 

for , do while, and while are the same as cpp

label: the same as how we use here: in cp 

```
markLoop: //this is the label
while (theMark === true) {
   doSomething();
}
```

for  break you can either simply do `break;` or use label to break a particular loop  like `break markLoop;`
continue can also be used with or without the label 

### for...in

The for...in statement iterates a specified variable over all the enumerable properties of an object. 
`for ( el in obj ) {..}`   
(el gives all the keys of the object)  
the for in loop can also return the user defined properties and methods so it is not recommended to use on arrays 

### for...of

The for...of statement creates a loop Iterating over iterable objects (including Array, Map, Set, arguments object and so on) 
`for( el of obj ) {...}`  

```
const arr = [3, 5, 7];
arr.foo = 'hello';

for (let i in arr) {
   console.log(i); // logs "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i); // logs 3, 5, 7
}
```

## Functions 

[apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

### recursion 

```
var foo = function bar() {
   // statements go here
}
```

this function can call itself using three options 
- bar()
-  argument.callee()
-  foo()

#### Nested functions 

- The inner function can be accessed only from statements in the outer function.
- The inner function forms a closure: the inner function can use the arguments and variables of the outer function, while the outer function cannot use the arguments and variables of the inner function.

the inner function forms a closure,  this means that a nested function can "inherit" the arguments and variables of its containing function. In other words, the inner function contains the scope of the outer function.

```
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give
                        // it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```

#### multiply-nested functions

```
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // logs 6 (1 + 2 + 3)
```

#### name conflicts 

```
function outside() {
  var x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

outside()(10); // returns 20 instead of 10
```

the inner-most scope takes the highest precedence, while the outer-most scope takes the lowest.The name conflict happens at the statement return x * 2 and is between inside's parameter x and outside's variable x. The scope chain here is {inside, outside, global object}. 

with closures An object containing methods for manipulating the inner variables of the outer function can be returned.

```
var createPet = function(name) {
  var sex;

  return {
    setName: function(newName) {
      name = newName;
    },

    getName: function() {
      return name;
    },

    getSex: function() {
      return sex;
    },

    setSex: function(newSex) {
      if(typeof newSex === 'string' && (newSex.toLowerCase() === 'male' ||
        newSex.toLowerCase() === 'female')) {
        sex = newSex;
      }
    }
  }
}

var pet = createPet('Vivie');
pet.getName();                  // Vivie

pet.setName('Oliver');
pet.setSex('male');
pet.getSex();                   // male
pet.getName();                  // Oliver
```

#### arguments object 
The arguments of a function are maintained in an array-like object. Within a function, you can address the arguments passed to it as follows:

```
arguments[i]
```

```
function myConcat(separator) {
   var result = ''; // initialize list
   var i;
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
```

```
// returns "red, orange, blue, "
myConcat(', ', 'red', 'orange', 'blue');
```

#### default parameters 

```
function multiply(a, b = 1) {
  return a * b;
}

multiply(5); // 5
```

#### rest parameters 

```
function multiply(multiplier, ...theArgs) {
  return theArgs.map(x => multiplier * x);
}

var arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

### Arrow functions 
An arrow function expression  has a shorter syntax compared to function expressions and does not have its own this, arguments, super, or new.target. Arrow functions are always anonymous. 

[no seperate this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#no_separate_this)

```
(function(){
  function Person() {
  // The Person() constructor defines `this` as itself.
  this.age = 0;

  setInterval(function growUp() {
    // In nonstrict mode, the growUp() function defines `this`
    // as the global object, which is different from the `this`
    // defined by the Person() constructor.
    this.age++;
  }, 1000);
}

var p = new Person();
console.log(p.age);
  setTimeout(function(){
    console.log("waiting");
    console.log(p.age); //still outputs 0
  }, 1000);
})()
```

```
(function(){
  function Person() {
  // The Person() constructor defines `this` as itself.
    var self = this;
  self.age = 0;

  setInterval(function growUp() {
    // In nonstrict mode, the growUp() function defines `this`
    // as the global object, which is different from the `this`
    // defined by the Person() constructor.
    self.age++;
  }, 1000);
}

var p = new Person();
console.log(p.age);
  setTimeout(function(){
    console.log("waiting");
    console.log(p.age); //outputs 1
  }, 1000);
})()
```
An arrow function does not have its own this; the this value of the enclosing execution context is used. Thus, in the following code, the this within the function that is passed to setInterval has the same value as this in the enclosing function:

```
function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  }, 1000);
}

var p = new Person();
```

#### [Predefined functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#predefined_functions)


## Expressions and operators 

[Exponentiation Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation_assignment)  

### Logical AND assignment 

```
(function(){
  //The logical AND assignment (x &&= y) operator only assigns if x is truthy.
  var x = 10;
  x &&= 2; 
  console.log(x); //prints 2
  x = 0;
  x &&= 2;
  console.log(x);//prints 0
  x = 10;
  x &&= 0;
  console.log(x);//prints 0
  //so it is something like the AND operator when both the operands are not zero then assignment   //occurs otherwise the assignment is always zero 
})()
```

### Logical OR assignment 

The logical OR assignment (x ||= y) operator only assigns if x is falsy, otherwise the original value remains 

```
const a = { duration: 50, title: '' };

a.duration ||= 10;
console.log(a.duration);
// expected output: 50

a.title ||= 'title is empty.';
console.log(a.title);
// expected output: "title is empty"
```

### logical Nullish Assignment 

The logical nullish assignment (x ??= y) operator only assigns if x is nullish (null or undefined).

#### Note:   
In all three operators above if the condition isn't true the expression part of y isn't evaluated at all, so if it for example contains a function call then that won't be even called 

### Return values   

In the case of logical assignments, (x &&= y), (x ||= y), and (x ??= y), the return value is that of the logical operation without the assignment, so x && y, x || y, and x ?? y, respectively.(return value is what you would get if you pass this expression to a variable)  

When chaining these expressions, each assignment is evaluated right-to-left. 
- `z += x *= y` is equivalent to `z += (x *= y)`  

### Destructuring 

```
var foo = ['one', 'two', 'three'];

// without destructuring
var one   = foo[0];
var two   = foo[1];
var three = foo[2];

// with destructuring
var [one, two, three] = foo;
```

### arithmetic operators 

```
1 / 2; // 0.5, here you see in cpp if it was integers they would have given 0 as answer 
1 / 2 == 1.0 / 2.0; // this is true
```

### binary operators 

#### [integer to binary](https://stackoverflow.com/questions/9939760/how-do-i-convert-an-integer-to-binary-in-javascript)  

#### Unsigned right shift 
The unsigned right shift operator (>>>) (zero-fill right shift) shifts the first operand the specified number of bits to the right. Excess bits shifted off to the right are discarded. Zero bits are shifted in from the left.zero-fill right shift returns an unsigned 32-bit integer.  

```
.     -9 (base 10): 11111111111111111111111111110111 (base 2)
                    --------------------------------
-9 >>> 2 (base 10): 00111111111111111111111111111101 (base 2) = 1073741821 (base 10)
```

#### Left shift 

```
(function(){
  var a = -1;
  console.log(a << 3);// outputs -8
  console.log(a << 35);// outputs -8 as well 
})()
```

#### sign propagating right shift 

This operator shifts the first operand the specified number of bits to the right. Excess bits shifted off to the right are discarded. Copies of the leftmost bit are shifted in from the left.

```
.    -9 (base 10): 11111111111111111111111111110111 (base 2)
                   --------------------------------
-9 >> 2 (base 10): 11111111111111111111111111111101 (base 2) = -3 (base 10)
```

### bitwise logical operators

- The operands are converted to thirty-two-bit integers and expressed by a series of bits. Numbers with more than 32 bits get their most significant bits discarded  

```
Before: 1110 0110 1111 1010 0000 0000 0000 0110 0000 0000 0001
After:               1010 0000 0000 0000 0110 0000 0000 0001
```

### Logical operators 

Logical operators are typically used with Boolean (logical) values; when they are, they return a Boolean value. However, the && and || operators actually return the value of one of the specified operands, so if these operators are used with non-Boolean values, they may return a non-Boolean value


- logical AND (&&): Returns expr1 if it can be converted to false; otherwise, returns expr2  

```
(function(){
  var a = 12 && 10;
  console.log(a); //prints 10
  var b = null && undefined;
  console.log(b); //prints null, not undefined 
})()
```

-  logical OR (||): Returns expr1 if it can be converted to true; otherwise, returns expr2
-  logical NOT (!): Returns false if its single operand that can be converted to true; otherwise, returns true.

### short circuit evaluation 

- false && anything is short-circuit evaluated to false. 
- true || anything is short-circuit evaluated to true.

the **anything** part of the above expressions is not evaluated

### comma operator 

The comma operator (,) evaluates both of its operands and returns the value of the last operand  
[comma operator use other than for loops](https://stackoverflow.com/questions/9579546/when-is-the-comma-operator-useful)

### Unary operators 

#### delete 

```
delete object.property;
delete object[propertyKey];
delete objectName[index];
delete property; // legal only within a with statement
```

where object is the name of an object, property is an existing property, and propertyKey is a string or symbol referring to an existing property.  
If the delete operator succeeds, it removes the property from the object. Trying to access it afterwards will yield undefined. The delete operator returns true if the operation is possible; it returns false if the operation is not possible.     

```
x = 42; // implicitly creates window.x
var y = 43;
var myobj = {h: 4}; // create object with property h

delete x;       // returns false (cannot delete if created implicitly)
delete y;       // returns false (cannot delete if declared with var)
delete Math.PI; // returns false (cannot delete non-configurable properties)
delete myobj.h; // returns true (can delete user-defined properties)
```

### typeof

 The parentheses are optional. 

 ``
 var myFun = new Function('5 + 2');
var shape = 'round';
var size = 1;
var foo = ['Apple', 'Mango', 'Orange'];
var today = new Date();

typeof myFun;       // returns "function", even if it is a method 
typeof shape;       // returns "string"
typeof size;        // returns "number"
typeof foo;         // returns "object"
typeof today;       // returns "object"
typeof doesntExist; // returns "undefined"
typeof true; // returns "boolean"
typeof null; // returns "object"

```

### void 

```
void (expression)
void expression
```

The void operator specifies an expression to be evaluated without returning a value

```
(function(){
  var a = void (5);
  console.log(a); //undefined 
  var b = 10;
  b = void (8);
  console.log(b); //undefined 
})()
```

### relational operators 

#### in 
The in operator returns true if the specified property is in the specified object.

```
// Arrays
var trees = ['redwood', 'bay', 'cedar', 'oak', 'maple'];
0 in trees;        // returns true
3 in trees;        // returns true
6 in trees;        // returns false
'bay' in trees;    // returns false (you must specify the index number,
                   // not the value at that index)
'length' in trees; // returns true (length is an Array property)

// built-in objects
'PI' in Math;          // returns true
var myString = new String('coral');
'length' in myString;  // returns true

// Custom objects
var mycar = { make: 'Honda', model: 'Accord', year: 1998 };
'make' in mycar;  // returns true
'model' in mycar; // returns true
```

#### instanceof 

The instanceof operator returns true if the specified object is of the specified object type.

```
var theDay = new Date(1995, 12, 17);
if (theDay instanceof Date) {
  // statements to execute
}
```

### Primary expressions 

#### this 

```
<p>Enter a number between 18 and 99:</p>
<input type="text" name="age" size=3 onChange="validate(this, 18, 99);">
```

```
function validate(obj, lowval, hival) {
  if ((obj.value < lowval) || (obj.value > hival))
    console.log('Invalid Value!');
}
```

#### super 
The super keyword is used to call functions on an object's parent. 

```
super([arguments]); // calls the parent constructor.
super.functionOnParent([arguments]);
```

## Numbers and Dates 

### Numbers 
numbers are implemented in double-precision 64-bit binary format IEEE 754 , about ±10^−308 to ±10^+308   
Integer values up to ±2^53 − 1 can be represented exactly.
In addition to being able to represent floating-point numbers, the number type has three symbolic values: `+Infinity`, `-Infinity`, and `NaN` (not-a-number).

you can't mix and match [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) and Number values in the same operation, and you can't use the Math object with BigInt values.

#### Decimal numbers 

```
1234567890
42

// Caution when using leading zeros:

0888 // 888 parsed as decimal 
0777 // parsed as octal in non-strict mode (511 in decimal)
```

#### Binary numbers 
Binary number syntax uses a leading zero followed by a lowercase or uppercase Latin letter "B" (0b or 0B). If the digits after the 0b are not 0 or 1, the following SyntaxError is thrown: "Missing binary digits after 0b".

#### Octal number 
Octal number syntax uses a leading zero. If the digits after the 0 are outside the range 0 through 7, the number will be interpreted as a decimal number.

Strict mode in ECMAScript 5 forbids octal syntax. Octal syntax isn't part of ECMAScript 5, but it's supported in all browsers by prefixing the octal number with a zero: 0644 === 420 and "\045" === "%". In ECMAScript 2015, octal numbers are supported if they are prefixed with 0o,

#### Hexadecimal numbers 
Hexadecimal number syntax uses a leading zero followed by a lowercase or uppercase Latin letter "X" (0x or 0X)

#### Exponentiation 

```
1E3   // 1000
2e6   // 2000000
0.1e2 // 10
```

### Number Object 
The built-in Number object has properties for numerical constants, such as maximum value, not-a-number, and infinity. You cannot change the values of these properties and you use them as follows:

```
var biggestNum = Number.MAX_VALUE;
var smallestNum = Number.MIN_VALUE;
var infiniteNum = Number.POSITIVE_INFINITY;
var negInfiniteNum = Number.NEGATIVE_INFINITY;
var notANum = Number.NaN;
```

others include Number.MAX_SAFE_INTEGER, Number.MIN_SAFE_INTEGER, Number.EPSILON (Difference between 1 and the smallest value greater than 1 that can be represented as a Number)

#### methods of number 
Number.parseFloat(), Number.parseInt(), Number.isFinite() (checks if it is not +Infinity, -Infinity or NaN) , Number.isInteger(), Number.isNaN(), Number.isSafeInteger()

#### Methods of Number.prototype 
The Number prototype provides methods for retrieving information from Number objects in various formats.
- [toExponential](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toExponential) Returns a string representing the number in exponential notation.
-  [toFixed](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) Returns a string representing the number in fixed-point notation.
-  [toPrecision](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toPrecision) Returns a string representing the number to a specified precision in fixed-point notation.

### Math Object 

```
Math.PI
Math.sin(1.56), cos, tan, asin, acos, atan, atan2, sinh, cosh, tanh, asinh, acosh, atanh
Math.abs(), pow, exp //(e^x), expm1//(e^x-1), log10, log1p, log2
//  ∀ x > - 1 , Math.log1p ( x ) = ln ( 1 + x ) 

Math.floor(), ceil 
Math.min(), max 
Math.random()
Math.round(), fround(), trunc()
Math.sqrt(), cbrt()//cube root, hypot() // returns the square root of the sum of squares of its arguments
Math.sign()
Math.clz32(), imul()
//clz32 returns Number of leading zero bits in the 32-bit binary representation.
//imul returns The result of the C-like 32-bit multiplication of the two arguments.
```

### [shallow copy and deep copy](https://www.javascripttutorial.net/3-ways-to-copy-objects-in-javascript/)




### [async/await](https://javascript.info/async-await)

async/await builds on top of promises: an async function always returns a promise. await "unwraps" a promise and either result in the value the promise was resolved with or throws an error if the promise was rejected.

### [singletons](https://www.sitepoint.com/javascript-design-patterns-singleton/) 
### [using javascript to extract values from the strings](https://stackoverflow.com/questions/41144784/using-regular-expressions-to-extract-values-in-javascript)
### [regex](https://www.w3schools.com/js/js_regexp.asp)
