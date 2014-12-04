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
* assns, mt, and final are called feilds
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


