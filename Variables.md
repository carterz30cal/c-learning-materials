If you want to store values, you need variables.
C is **not** an object-oriented programming language, so you'll find little in the way of complex objects such as lists or trees, at least defined within the language itself.

Similarly, C does not do pass-by-reference, so when you set one variable's value equal to another, you are essentially copying the data across, not retaining a reference.
```c
int main(void) {
	// defining a variable goes like this
	// <TYPE> <NAME> then optionally = <VALUE>
	int x = 0;

	// I would recommend avoiding declaring multiple variables per line
	// e.g.
	int y = 6, z = 5;
	// Whilst valid, it's not as obvious as just using one line per variable
	// and there's no harm in using more lines.
	return 0;
}
```

After declaration, you are free to use variables in place of values whenever you see fit.
Anywhere where you could put a number, it is valid to replace it with an integer variable.
```c 
#include <stdio.h>
int main(void) {
	int x = 0;
	int y = x;
	
	y = 6;
	printf(x) // this will print 0 to the console, as x is unchanged by y's reassignment.
	return 0;
}
```

Using temporary variables like this is called *automatic memory allocation*, and these variables will be stored on the stack. 
This is the easiest form of memory management, and poses no risk for memory leaks at this level. It is however, rather basic, and offers no solution for persistent storage of information at runtime, which is useful in nearly every program. This is where pointers come in.

## Pointers
A basic use-case for a pointer is to create an array. 
You can make an automatic-scope array using the following code:
```c
int main(void) {
	int x[10];

	return 0;
}
```
But, this has a problem. That 10 inside the square brackets is one of the few cases in which you cannot replace the number with a variable. This is because this type of array must have a fixed length at compile time, so cannot have a dynamic length. This can be a particularly frustrating restriction coming from higher-level languages, but there is a solution - **pointers**.
*Note: Some implementations of C do allow variable-length arrays, but this is not guaranteed to be supported.*

To use pointers in C, you must first be aware that each variable uses a different number of bytes, and account for that. Luckily, there is a function which tells us the amount of bytes a type uses.
```c
int main(void) {
	// The little asterisk tells us that this is a pointer to a variable of type int.
	int* x = malloc(1 * sizeof(int));
	// This has made a pointer, x, which points towards a memory block with one undefined integer in it.

	int length = 90;
	int* y = malloc(length * sizeof(int));
	// We can make an *array* of any length, memory providing. 

	free(x);
	free(y);
	return 0;
}
```
*Technically malloc returns a void\* pointer, but C implicitly casts/converts to the preferred type upon assignment. If you are writing a C++ program, you would need explicit casts, but they are redundant for C*

It is highly recommended you check the pointer after assignment, to make sure malloc succeeded, as it is allowed to return `NULL`, which you may not be expecting.
```c title:"NULL checking"
int main(void) {
	int* x = malloc(10 * sizeof(int));
	if (x == NULL) {
		// scream, or something.
	}
	else {
		// normal program function.
		free(x);
	}
	return 0;
}
```

To access pointers like an array, you can use either array-like syntax or pointer-style syntax, as they are generally interchangeable. 
```c title:'using pointer-style syntax'
#include <stdio.h>
int main(void) {
	int* x = malloc(100 * sizeof(int));
	if (x != NULL) {
		// array-style
		x[0] = 10;
		printf(x[0]); // prints '10'

		// pointer-type
		int index = 1;
		*(x + index) = 10;
		printf(*(x + index));

		free(x);
	}
	return 0;
}
```

