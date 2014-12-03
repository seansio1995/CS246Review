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
