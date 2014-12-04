C++ Class
===

C++ can put functions inside a struct;

```C++
struct student {
	int assns, mt, final;
	float grade() {}
};
```

* grade() is called method
* assns, mt, and final are called fields
* A instance of a class is called an object

A method always has a hidden parameter called "this", which is a pointer to the object on which the methoed was called.

Constructor
---

```C++
struct Student {
	int assns, mid, final;
	Student(int a, int m, int f) {
		assns = a, mid = m, final = f;
	}
};
Student billy(60, 70, 80);	//construction
Student billy = Student(60, 70, 80);	//construct object billy on the stack
Student* billyPtr = new Student(60, 70, 80);
delete billPtr;
```

Advantages of Contructor

* arbitrary computation
* default arguments
* overloading

Default constructor goes away as soon as you write a constructor of your own. As soon as you write a constructor, you loss c-style initialization. Unable to use "Vec v = {1, 2}"

Cannot initialize constants and references inside a constructor body.

Steps that occur when an object is create:

1. space is allocated (stack/heap)
2. fields are initialized, default constructors
3. constructor body runs

Hijack step 2: member initialization
```C++
struct MyStruct {
	const int myConst;
	int &myRef;
	MyStruct(int c, int &r) : myConst(c), myRef(r) {}
};
```

Fields are initialized in declaration order irrespective of the order in member initialization list

* only way to initialize constants/references
* same name for field/parameter
* can be more efficient

Copy constructor
---

```C++
struct Node {
	int data;
	Node* next;
	Node(int d, Node* n) : data(d), next(n) {}
	Node(const Node &other) : data(other.data), 
		next(other.next ? new Node(*other.next) : NULL) {}
};
```
Places where the copy constructor is called

* when we create an object is a copy of another
* when an object is passed by value to a function
* when an object is return from a function

Destructor
---

To destroy an object, a method called the destructor runs. A destructor takes no arguments, there is always only one destructor for a class.

* stack allocated object is destroyed when it goes out of scope
* heap allocated object is destroyed when we call delete on it

Note: base types (int, bool, etc.) and ptrs to object do not have destructor.

```C++
Node::~Node() { delete next; }
Node *np = new Node(1, new Node(2, new Node(3, NULL)));
```

Assignment Operator
---
Default operator= does a field for field copy. Custom operator= if there is dynamic memory involved.
```C++
struct Node {
	Node &operator=(const Node &other) {
		if(this == &other) return *this;
		delete next; //may leak memory if next is pointing something already
		if(*other.next) next = new Node(*other.next);
		else next = NULL;
		return *this;
	}
}
```
If new fails, next is not assigned, since we deleted next, next is a dangling ptr. Solution is delay the delete.
```C++
Node *tmp = next;
```

Copy and Swap Idiom
---
```C++
struct Node {
	void swap(Node &other) {
		std::swap(data, other.data);
		std::swap(next, other.next);
	}
	Node &operator=(const Node &other) {
		Node tmp = other;	//copy constructor, stack allocated, deep copy;
		swap(tmp);			//tmp has the old data, delete by stack;
		return *this;
	}
};
```

Rule of Three
---

If you need to write custom version of

1. copy constructor
2. operator=
3. destructor

then you usually need to implement all three

Implicit Conversion
---

```C++
struct Node {
	int data;
	Node *next;
	Node(int d) : data(d), next(NULL) {}
};
Node n(4);
Node n1 = 4;
```
One argument constructor creates an implicit conversion. To disable implicit conversion, use the explicit keyword.
```C++
struct Node {
	explicit Node(int data) {}
};
```

Member Funtions vs Standalone Functions

Input/output operators are always implemented as standalone funcitons. Following operatos must be implemented as methods functions.

* operator=
* operator[]
* operator()
* operator->
* operatorT()

Const method
---

```C++
struct Student {
	int assns, mid ,final;
	mutable int numcalls;
	float grade() const {
		numcalls ++;	//cannot modify other fields
		return ...;
	}
}
```

Static
---

A static field is associated with the class not any object. One field shared by all object
```C++
struct Student {
	static int numInstances;	//declaration
	student(...) : ... {numInstances ++;}
};
int Student::numInstances = 0;	//definition
```
Static member functions are associated with the class. You do not need an object to call this member function. You do not have access to "this" parameter.

Restrictions: static member functions can only call other static functions and access static fields

Encapsulation
---

* keep certain implementation details hidden
* object appears as a black box with exposed interface

Default visibility in a struct:	public

Default visibility in a class:	private

With private fields, you can provide users with the field values using getters and setters.

Hide implemetation details (include fields)

* maintain class invaraants
* change implemetation

Friend
---
```C++
class Vec {
	int x, y;
	public:
	friend std::ostream &operator << (std::ostream &out, const Vec &c);	//now can access to the private parts
};
```
Advice: have few friends as possible -- break encapsulation

Relationship 1: Composition
---
class Vec {
	int x, y, z;
	public:
	vec(int x, int y, int z) : x(x), y(y), z(z) {}
};
class Plane {
	Vec v1, v2;
	public:
	Plane() : v1(2, 3, 4), v2(1, 0, 1) {}
};
```
Relationship between Plane and Vec is a "owns a" relationship. Generally, if A "owns a" B, then

* B has no existence outside A
* if A destroyed, B is destroyed
* if A is coopied, B is copied

Car "owns a" wheels

Relationship 2: Aggregation
---
```C++
class Pond {
	Duck* ducks[20];
};
```
Pond "has a" Ducks, Ducks exist outside a Pond.

Implemented through using pointers to objects inside another object

Relationship 3: Inheritance
---
```C++
class Book {
	string title, author;
	int numPages;
	public:
	Book(string title, string author, int numPages);
};
class Textbook : public Book {
	string topic;
};
class Comic: public Book {
	string protag;
};
```

Book is a base class (super class), Textbook and Comic are derived classes (sub class).

Private fields are private to that class
```C++
Textbook::Textbook() : title(title), author(author) ... //dose not work
```
When an object is created
1. space is allocated
2. superclass part is created
3. default constructor, Member Initialization List
4. Consturct body

Virual Method
---

Book is heavy if numPages > 300, Textbook is heavy if numPages > 500
```C++
class Book {
	public:
	bool isHeavy() { return numPages > 300; }
};
class Textbook : public Book {
	public:
	bool isHeavy() { return getNumPages() > 500; }
};
Textbook b1 = Textbook(..., 400, ...);
b1.isHeavy() == false;
Book &b2 = b1;
b2.isHeavy() == true;
Book &pb1 = &b1;
pb1->isHeavy() == true; 
```
Textbook has bee coerced(forced) into a Book.

If you access an object through a superclass variable, then the object is sliced to fit the superclass. If we access an object through a superclass pointer, the superclass method runs. compiler checks the declared type of the pointer to decide which method to run.

We would like to refer to different Books using Book pointers, and still have the most "specialied" method called. Use the virtual keyword.
```C++
class Book {
	public:
	virtual bool isHeavy() { return numPages > 300; }
};
class Textbook : public Book {
	public:
	bool isHeavy() { return getNumPages() > 500; }
};
Textbook b1 = Textbook(..., 400, ...);
b1.isHeavy() == false;
Book &b2 = b1;
b2.isHeavy() == false;
Book &pb1 = &b1;
pb1->isHeavy() == false;
```
The process of finding which method to run based on the run time type is called Dynamic Dispatch -- small performance penalty.
```C++
book* collection[20];
for(int i = 0; i < 20; i ++) 
	collection[i]->isHeavy();
```
Ability to accommodate multiple types in an abstraction is called Polymorphism.

Abstract Class
---
```C++
class Studnet {
	protected:
		int numCourses;
	public:
		virtual int fee() = 0;	//method with no implementation, pure virtual
};
class Regular: public Student {
	public:
		int fee() {return 700 * numCourses;}
};
class Coop: public Student {
	public:
		int fee() {return 1300 * numCourses;}
};
```
We do not want to give an implementation for the method Student::fee().
A class with at least one pure virtual method cannot be instantiated, it is an abstract class.
A class that inherits from an abstract class is also abstract unless all inherited pure virtual methods are implemented.
A class with no pure virtual method is called a concrete class (can be instantiated)

Uses of abstract classes

* specify the functionality expected from subclass
* polymorphism
* place common fields in superclass

If you need to make a class abstract but cannot decide on a method, pick the destructor```C++
virtual ~Subject() = 0;	//pure virtual
```
Must implement the destruct, because subclass destructor call supercalss destructor. A pure virtual method must be overriden in the subclass.
