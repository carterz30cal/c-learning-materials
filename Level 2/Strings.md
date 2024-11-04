Strings are really quite annoying in C. 
If you're raw-dogging it like we're forced to for assignments then you get essentially no functions to actually manipulate them.
Storing strings as variables requires the `char*` data type. 
```c title:"basic strings"
int main(void) {
	char* str = "Hello, dystopian world.";
	return 0;
}
```
*Note: " and ' are **not** interchangeable. " is for string literals, when you want a null terminating character at the end, and ' is for singular character values.*#
