# Cpp

## String 

```
string str = "fuck" + " yeah"; //throws error 
string st1 = "fuck";
string str = st1 + " yeah"; //this is fine because 
//One of the first two strings being concatenated must be a std::string object:
// adding two string literals is adding two character arrays which won't work in cpp

// you can although concatenate string literals by simply placing them next to each other
string temp = "Hello" 
  " bitches";
  p(temp); //prints `Hello bitches`
```


#### [Functor](https://stackoverflow.com/questions/356950/what-are-c-functors-and-their-uses)

#### useful for changing long ass names 
`auto &shorty = longAssName;`  
(doesn't matter even if it is a vector)

#### finding number of elements in a range when all are sorted 
with set you can find lower_bound but it can't be subtracted so kind of overcomplicated 
use vector which is sorted like so   
```
int main(){
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  vi v = {1,2,3,4,5,6,7,9};
  set <int> s(v.begin(), v.end());
  auto high = lower_bound(v.begin(), v.end(), 8);
  auto low = lower_bound(v.begin(), v.end(), 5);
  cout << (high-low);
}
```

## Constructor 

```
class X {
    int a, b, i, j;
public:
    const int& r;
    X(int i)
      : r(a) // initializes X::r to refer to X::a
      , b{i} // initializes X::b to the value of the parameter i
      , i(i) // initializes X::i to the value of the parameter i
      , j(this->i) // initializes X::j to the value of X::i
    { }
};
```

```
struct S {
    int n = 42;   // default member initializer
    S() : n(7) {} // will set n to 7, not 42
};
```

## Inheritance 

syntax `class A: public B {}`

```
class A{
    public:
     void fuck(){
        cout << "fuck A\n";
    }
    A(){}
};

class B : public A{
    public:
    void fuck(){
        cout << "fuck B\n";
    }
    B(){}
};


int main(){

  A *obj1 = new B();
  obj1->fuck(); //prints fuck A
}
```

- however you can not do the opposite you can not create a child type object using constructor of parent 
- [diamond problem](https://en.wikipedia.org/wiki/Multiple_inheritance)
- [virtual base class](https://www.geeksforgeeks.org/virtual-base-class-in-c/) basically add virtual to class that both provide same shit, so no ambiguity

```
class A{
    public:
     virtual void fuck(){
        cout << "fuck A\n";
    }
    A(){}
};

class B : public A{
    public:
    void fuck(){
        cout << "fuck B\n";
    }
    B(){}
};


int main(){
  A *obj = new A();
  obj->fuck();//prints fuck A
  A *obj1 = new B();
  obj1->fuck(); //prints fuck B
}
```

- A pure virtual function is a function declared in the base class that has no definition relative to the base class.
- A class containing the pure virtual function cannot be used to declare the objects of its own, such classes are known as abstract base classes.
- The main objective of the base class is to provide the traits to the derived classes and to create the base pointer used for achieving the runtime polymorphism. 
- Pure virtual function can be defined as: `virtual void display() = 0;`

- When a base class is specified as a virtual base, it can act as an indirect base more than once without duplication of its data members. A single copy of its data members is shared by all the base classes that use virtual base.
- simple enoguh if there will be ambiguity it will throw error even if u inherit virtually but define same stuff in both B and C and then expect D to just guess
- virtual inheritance, virtual keyword makes such that only one copy of base class stuff goes down to everyone inheriting virtually but if u obviously redefine stuff then its gonna be ambigous 
- same way if u have int nigga in a class and char nigga in another class and then you derive from both of these classes then ambiguous shit will happen (it will compile if you don't use it though) (The ambiguity can be removed by using the scope resolution operator inside the class such as B::nigga.)

### inhertiance in terms of constructor 

- Order of Constructor Call : Base class constructors are always called in the derived class constructors. Whenever you create derived class object, first the base class default constructor is executed and then the derived class's constructor finishes execution.
- reverse order for destructor 
- you can not inherit constructor unless you do this [stuff](https://stackoverflow.com/questions/347358/inheriting-constructors)

```
class D: public A, public B{
    public: 
   
    D(){
        cout << "D constructor\n";
    }
};
```

- here B inherits from A, so first A constructor called then to call B constructor we have to call A again so order becomes A -> A -> B -> D

```
#include <iostream>
using namespace std;
class Base
{
    int arr[5];
};
class Child1: public Base
{
};
class Child2: public Base
{
};
class GrandChild: public Child1, public Child2
{
};
int main(void)
{
    cout << sizeof(GrandChild); //answer will be 40 bytes if int size taken to be 4 bytes 
    return 0;
}
```

## [static](https://www.tutorialspoint.com/static-keyword-in-cplusplus)
## [scope resolution operator](https://www.geeksforgeeks.org/scope-resolution-operator-in-c/)

- a default constructor is one which takes no arguments or has default values for all its arguments 
- if you call constructor of derived class and do not mention base class constructor then it will call its default constructor (of base class) if that isn't present then error 
- to call base class parameterized constructor one can do this , (trying to call it some other way such as in constructor body throws error)

```
Derived(int a, int b) : Base(a, b){
    }
```

- A base class pointer can point to a derived class object, but we can only access base class members or virtual functions using the base class pointer because object slicing happens.
- When a derived class object is assigned to a base class object. Additional attributes of a derived class object are sliced off to form the base class object.
- if it is a pointer or a reference from a child object to base class then on calling virtual functions child class function will be called 
- however if you simply cast it to a Base object then even the functions will be sliced off (like `Base obj = d` where d is a child object)
- you can't have virtual data members only virtual functions 

### Copy constructors 

- A copy constructor is a member function that initializes an object using another object of the same class
- in the diamond problem when B and C virtually inherit from A, and you construct D object, constructor calls will be A B C D not A B A C D 
- If you use virtual inheritance, you only have a single instance of ClassA in your object. As such, neither the constructor of ClassB nor the constructor of ClassC will construct the shared base, ClassA would be constructed twice if they did. Instead, ClassA is constructed directly from the constructor of ClassD before it calls the other two base constructors

### function overload inheritance 

- When a child class writes its own method, then all functions of the base class with the same name become hidden, even if signatures of base class functions are different. to overcome this use [using](https://stackoverflow.com/questions/14212190/c-issue-with-function-overloading-in-an-inherited-class)
- however if you don't rewrite it then you can overload just fine 
- The derived classes do not have to implement all virtual functions themselves. They only need to implement the pure ones.
- if the virtual function is not implemented in derived class then base class function will run 
- . If a derived class is not instantiated directly, but only exists as a base class of more derived classes, then it's those classes that are responsible for having all their pure virtual methods implemented. The "middle" class in the hierarchy is allowed to leave some pure virtual methods unimplemented, just like the base class. If the "middle" class does implement a pure virtual method, then its descendants will inherit that implementation, so they don't have to re-implement it themselves.
- you can call base methods like this through object (obviously also through scope resolution inside class) `d.Base::print();`

## [oops questions](https://www.interviewbit.com/oops-interview-questions/)

- interfaces and abstract classes only describe the behaviour or capabilites of class without any implementation 
- interfaces and abstract classes are almost the same in cpp although ( interface = a C++ class with only pure virtual methods) ( an abstract class in C++ is a class that has at least one pure virtual function, rest it can have anything.  The classes inheriting the abstract class must provide a definition for the pure virtual function; otherwise, the subclass would become an abstract class itself..)
- they shouldn't have constructors but can have virtual destructors 
- in cpp pure virtual functions and abstract methods are the same thing

## Conversion constructor 

- In C++, if a class has a constructor which can be called with a single argument, then this constructor becomes conversion constructor because such a constructor allows automatic conversion to the class being constructed. `ClassName obj = 69`
- you can also do as if you would do in pair pushing

```
#include <iostream>

class MyClass {
	int a, b;

public:
	MyClass(int i, int y)
	{
		a = i;
		b = y;
	}
	void display()
	{
		std::cout << " a = " << a << " b = " << b << "\n";
	}
};

int main()
{
	MyClass object(10, 20);
	object.display();

	// Multiple parameterized conversion constructor is invoked.
	object = { 30, 40 };
	object.display();
	return 0;
}
```

## [coversion operators](https://www.geeksforgeeks.org/advanced-c-conversion-operators/)

```
#include<iostream> 
using namespace std; 

class ClassA {  
public: 
   ClassA(int ii = 0) : i(ii) {} 
   void show() { cout << "i = " << i << endl;} 
private: 
   int i; 
}; 

class ClassB { 
public: 
   ClassB(int xx) : x(xx) {} 
   operator ClassA() const { return ClassA(x); } 
private: 
   int x; 
}; 

void g(ClassA a) 
{  a.show(); } 

int main() { 
 ClassB b(10); 
 g(b); 
 g(20); 
 getchar(); 
 return 0; 
}  
```

## Facts 

- in cpp strings are compared lexicographically through < > etc operators 

## Regex 

- quite the same as js with few diff 
- regex pattern is a string "pattern" like this
- escaping is done by two backslashes `\\[`
- `[]` use to specify range and any nigga in that range, () to specify group which could be repeated 




















======================================================================================================================================================================================





## References

[lvalue and rvalue](https://www.tutorialspoint.com/What-are-Lvalues-and-Rvalues-in-Cplusplus)  
[different types of values](https://en.cppreference.com/w/cpp/language/value_category)

References are not objects; they do not necessarily occupy storage, although the compiler may allocate storage if it is necessary to implement the desired semantics (e.g. a non-static data member of reference type usually increases the size of the class by the amount necessary to store a memory address).

Because references are not objects, there are no arrays of references, no pointers to references, and no references to references: 

```
int& a[3]; // error
int&* p;   // error
int& &r;   // error
```

#### Lvalue reference 

```
int a = 10;
const int &b = a;
```

When a function's return type is lvalue reference, the function call expression becomes an lvalue expression

```
#include <iostream>
#include <string>
 
char& char_number(std::string& s, std::size_t n) {
    return s.at(n); // string::at() returns a reference to char
}
 
int main() {
    std::string str = "Test";
    char_number(str, 1) = 'a'; // the function call is lvalue, can be assigned to
    std::cout << str << '\n'; //outputs tast
}
```

#### Rvalue references 

Rvalue references can be used to extend the lifetimes of temporary objects (note, lvalue references to const can extend the lifetimes of temporary objects too, but they are not modifiable through them)

```
string s = "test";
string &ref = s + s; //this will throw error because you can't assign a lvalue reference to an rvalue
const string &ref = s + s; //this will work though, obviously you can't modify it through ref

string &&ref = "Hello "
  "bitches";
  ref += " what's up"; //can modify, no problem
  p(ref); //prints `hello bitches what's up`
  //rvalue references can also be used as parameters to functions, no problem

```

[the reason for allowing lvalue reference to rvalues if it is constant](https://stackoverflow.com/questions/2088259/literal-initialization-for-const-references)

##### extend the lifetime 
Whenever a reference is bound to a temporary object or to a subobject thereof, the lifetime of the temporary object is extended to match the lifetime of the reference

### reference overloads

```
#include <iostream>
#include <utility>
 
void f(int& x) {
    std::cout << "lvalue reference overload f(" << x << ")\n";
}
 
void f(const int& x) {
    std::cout << "lvalue reference to const overload f(" << x << ")\n";
}
 
void f(int&& x) {
    std::cout << "rvalue reference overload f(" << x << ")\n";
}
 
int main() {
    int i = 1;
    const int ci = 2;
    f(i);  // calls f(int&)
    f(ci); // calls f(const int&)
    f(3);  // calls f(int&&)
           // would call f(const int&) if f(int&&) overload wasn't provided
    f(std::move(i)); // calls f(int&&)
 
    // rvalue reference variables are lvalues when used in expressions
    int&& x = 1;
    f(x);            // calls f(int& x)
    f(std::move(x)); // calls f(int&& x)
}
```

#### Because rvalue references can bind to xvalues, they can refer to non-temporary objects: 

```
int i2 = 42;
int &&ref = i2;//fails 
int&& rri = std::move(i2); // binds directly to i2
```

[move](https://en.cppreference.com/w/cpp/utility/move)

### Chain referencing

```
int main(){
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  int a = 10;
  int &b = a;
  b = 20;
  int &c = b;
  c = 30;
  p(a);
}
```

you cannot pass by reference in constructor initializer list like below, it'll throw error 

```
struct node{
	int a;
	node(int &aa): &a(aa){};
};
```

rather do this 

```
struct node{
	int &a;
	node(int &aa): a(aa){}; //you can also replace aa with a, and it will work fine (a bit of warnings will be // generated, look below in the Constructor heading to see the differences in struct member and //parameter
};
```

also the above thing can't be done in normal circumstances, References appear without initializers only in function parameter declaration, in function return type declaration, in the declaration of a class member, and with the extern specifier. 

```
int &ref; //this will throw error
 int val = 5;
ref = val;
```

[Different ways of reference intialization](https://en.cppreference.com/w/cpp/language/reference_initialization)
#### Important note : 
Once initialized, a reference cannot be changed to refer to another object.  

[returning values by reference](https://www.tutorialspoint.com/cplusplus/returning_values_by_reference.htm)
When a function returns a reference, it returns an implicit pointer to its return value.

```
int main(){
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  int val = 5;
  const int &ref = val; //you can't change ref hereon because it is constant 
  val = 6;
  p(ref); //although here it prints 6
}
```

```
int main(){
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  const int val = 5;
  int &ref = val; //this will throw error, reason below, however a way to do this is through const_cast
  // int &ref = const_cast <int&> (val)   
  // the above line will work 
  ref = 6;
  p(val);
}
```

the above code throws error because **you cannot bind a non-const reference to a const-object** 

### various referencing 

```
void (&rf)(int) = foo; //lvalue reference to a function

int (&ra)[3] = ar; //lvalue reference to array, you absolutely have to specify the size exactly equal to //the size of original array, otherwise it throws error
```

### Dangling reference 

```
string& f()
{
    std::string s = "Example";
    return s; // exits the scope of s:
              // its destructor is called and its storage deallocated
}

 int main(){
	 string& r = f(); // dangling reference
	 //cout << r;       // undefined behavior: reads from a dangling reference
	 //string s = f();  // undefined behavior: copy-initializes from a dangling reference

 }
 ```
