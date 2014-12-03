C++
===

Input/Output
---

```
<< input operator
>> output operator
```
```C++
cin.fail();
cin.eof();
```
```C++
while(true) {
	if(!(cin >> i)) {
		if(cin.eof()) break;
		else {
			cin.clear(); //lowers the flag
			cin.ignore();
		}
	}
	else cout << i << endl;
}
```

Streams
---

```C++
cout << hex << i;
```

To read an entire line, use getline(istream, s) to read from the first unread char in the input stream until we hit a newline.

```C++
#include <fstream>
#include <iostream>
#include <sstream>
using namespace std;
int main() {
	ifstream f("suite.txt");
	string s;
	while(f >> s) cout << s << endl;
	istringstream str(s);
	int tmp;
	while(str >> tmp) cout << tmp << endl;
	return 0;
}
```

| In C | In C++
--- | --- | ---
equality | strcmp | s1 == s2
inequality | strcmp | s1 != s2
comparisson | strcmp | s1 < s2
length | strlen | s1.length()
concatenation | strcat | s1 + s2

```C++
void printSuiteFile(string fileName="suite.txt") {
	ifstream myfile(name.c_str()); //name must be a C style string
	string s;
	while(myfile >> s) cout << s << endl;
}
```

1. optional parameters must appear last
2. not allowed to follow a default argument with a non-default argument

Overloading
---
```C++
int nagate(int a) { return -a}
int negate(bool a) { return !a; }
```
It is not enough for two function to only differ on the return type

Declaration and Definition
---

* Declaration: assertion that something exists
* Definition: implementation
An entity can be declared as many times as you want.

Pointers
---

```C++
int n = 5;
int *p = &n;	//p contains the address of n
cout << p;		//print out the address
cout << *p;		//print the contents of thing p points to
int **p;		//pointer to a pointer to an int;
pp = &p;
**p = 10;
```

Arrays
---

An array is not a pointer! The name of an array is shorthand for the address of the first element of the array.

```C++
int a[] = {1,2,4,8};
*a == a[0];
*(a + 1) == a[1];
```

Structure
---
```C++
struct Node {
	int n;
	Node next;	//wrong, the compliler dose not know the size of the structure
	Node *next;
};	//Do not forget the ;

Constants
---
```C++
const int maxgrade = 100;
```

A constant definition must be initialized

```C++
const int* p = &n;
```
p is a pointer to a constant int
```c++
p = &m;	\\valid
*p = 7;	\\invalid
```

```C++
int* const p = &n;
```
p is a constant pointer to an int
```c++
p = &m;	\\invalid
*p = 7;	\\valid
```

```C++
const int* const p = &n;
```
p is a constant pointer to a constant int

Passing Parameter
---

```C++
void inc1(int n) { n ++; }
void inc2(int *n) { *n ++; }
void inc3(int &n) { n ++; }
```

Reference
---
```C++
int y =10;
int &z = y;	//z is a reference to y
z = 12;
```

A reference is like a constant pointer with automatic dereferencing. Reference has no identity of its own. z mimics y.

1. cannot leave a reference uninitialized.
2. a reference must be initialized to something that has an address.
3. cannot create a pointer to a reference. (int &*x=)
4. cannot create a reference to a reference. (int &&x=)
5. cannot create a array of reference. (int &x[3] = {y, y, y})

```C++
struct ReallyBig{};
int f(ReallyBig &rb){};
```

1. no copy
2. changes visible to caller
3. automatic dereferencing

Advice: Prefer to pass by const reference for anything that is larger than an int.

Dynamic Memory
---

C++ allocates using new, deallocates using delete

```C++
int *n = new int;
cin >> *n;
int *arr = new int[*n];
delete [] arr;
delele n;
```

Local variable allotted space on the stack, space is automatically reclaimed when variables go out of scope.

```C++
Node getMeANode() {
	Node;
	return n;
}
```
inefficient, returns value is copied.
```C++
Node* getMeANode() {
	Node n;
	return &n;
}
```
Really bad: returing a pointer to a stack allocated variable
```C++
Node* getMeANode() {
	Node *np = new Node;
	return np;
}
Good: Node is allocated in heap

Operator Overloading
---

We can give meaning to C++ operators for any new type we create

```C++
struct vec {int x,y;}
vec operator + (const vec &a, const vec &b);
```

Overloading input and output operator
```C++
ostream &operator << (ostream &out, const grade &g) {
	out << g.theGrade << "%";
	return out;
}
istream &operator >> (istream &in, const grade &g) {
	in >> g.theGrade;
	if(g.theGrade > 100) g.theGrade = 100;
	if(g.theGrade < 0) g.theGrade = 0;
	return in;
}
```
