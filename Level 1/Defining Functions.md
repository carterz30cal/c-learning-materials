You should first notice that your `main()` function is a function declaration in-of-itself.
The most basic `main()` function is one with `int` return type and `void` parameters, and an empty body. That is valid C code, but if we want to include executable arguments, then we'll need to adjust the parameters of `main()` a little.

```c title:"most basic valid main()"
int main(void) {
	
}
```
*You may also exclude* `void`*, as it is optional.*

A more reasonable `main()` function would look like this:
```c title:"more useful main()"
#include <stdio.h>
int main(int argc, char** argv) {
	return 0;
}
```
`argc` stands for 'argument count', and it is simply the number of arguments provided.
`argv` stands for 'argument vector', and it is an array of 'strings' which are the arguments provided.
By convention, the first argument will be the name of the executable, and that `argv[argc]` will be `NULL`. 
There are more optional `main()` parameters, but those are (implementation and OS)-specific. 

Other, arbitrary functions can be defined using the same technique, but these have no restrictions on return type nor parameters. If you really wanted you could return nothing and have 101 parameters, but it would be very clunky.
Here are a few examples
```c title:"function declaration examples"
#include <stdio.h>
#include <stdlib.h>
int main(void) {
	puhprint();
	int* y = arrayyyy(7);
	free(y);
	return 0;
}
void puhprint() {
	printf("noooo");
}
int* arrayyyy(int size) {
	int* x = malloc(sizeof(int) * size);
	for (int i = 0; i < size; i++) {
		*(x + i) = 6;
	}
	return x;
}
```
*Declaration inside for loop only possible since C99, but basically a moot point nowadays*

