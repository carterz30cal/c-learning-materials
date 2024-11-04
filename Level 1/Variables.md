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
```c title:'automatic arrays'
int main(void) {
	int x[10];

	return 0;
}
```
But, this has a problem. That 10 inside the square brackets is one of the few cases in which you cannot replace the number with a variable. This is because this type of array must have a fixed length at compile time, so cannot have a dynamic length. This can be a particularly frustrating restriction coming from higher-level languages, but there is a solution - **pointers**.
*Note: Some implementations of C do allow variable-length arrays, but this is not guaranteed to be supported.*

To use pointers in C, you must first be aware that each variable uses a different number of bytes, and account for that. Luckily, there is a function which tells us the amount of bytes a type uses.
```c title:pointers
#include <stdlib.h>
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
#include <stdlib.h>
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
#include <stdlib.h>
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
		printf(*(x + index)); // also prints '10'

		free(x);
	}
	return 0;
}
```
*If you want to learn about multi-dimensional arrays, read [[Multi-Dimensional Arrays]]*

Pointer syntax works because when you allocate a block of heap memory using `malloc`, it's created in sequence, so to access elements after the first you simply just add the index to your address value.
Generally though, array-style works fine. If you can, use that. 

### Memory leaks
Memory leaks occur when you don't free up heap-allocated memory after you're done using it. It's like washing your dishes. More modern languages such as C#, Java, JavaScript and Python use what's known as a garbage collector, which you can think of as a dishwasher. In C, you're doing that by hand. 
You may have noticed in the previous snippets `free(x)`, which is the function you call to free up that memory you previously allocated using `malloc`. 
If you remember to wash your dishes, you won't get any memory leaks.

To call `free`, all you need is to pass in the variable you wish to free. That's it really.
Some programmers choose to also assign the freed variable to NULL, which ensures that further use of the pointer will crash the program, instead of causing undefined behaviour, which can be tricky to debug. Whilst good practice, you are free to choose not to do this, if you so desire.
```c title:'good practice example'
#include <stdlib.h>
int main(void) {
	int* x = malloc(1 * sizeof(int));
	*x = 10;
	free(x);
	x = NULL; // this is the 'good practice' line. feel free to leave it out.
	return 0;
}
```

### Referencing and De-referencing
When you declare a pointer, you're pointing towards a bit of memory with no defined value. To use pointers effectively, you must *point* the pointer towards a value stored. 
```c title:referencing
#include <stdlib.h>
int main(void) {
	int* x;
	int a = 88;
	// we want x to reference 88.
	x = &a;
	// & is the reference operator. the variable that follows it will then produce it's memory address, which we can use in the pointer.
	free(x);
	return 0;
}
```
To get the value out of the pointer, you'll want to *de-reference* the pointer. 
```c title:de-referencing
#include <stdlib.h>
int main(void) {
	int* x;
	int a = 88;
	x = &a;

	int b;
	b = *x; // essentially the opposite of &, it produces the value at an address.
	free(x);
	return 0;
}
```
*You may notice this looks very similar to pointer-style array syntax, and that's because it's exactly the same thing.*


# Continuations
- [[Data Types]]
- 
