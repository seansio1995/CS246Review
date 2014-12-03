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

To read an entire line, use getline(istream, s) to read from the first unread char in the input stream until we hit a newline.

```C++
cout << hex << i;
```

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
