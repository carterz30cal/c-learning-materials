Structs are useful if you want to aggregate several variables into one package, which can then be assigned to a variable. This allows some object-oriented flexibility into an otherwise quite rigid language, but does have some caveats. 
The main thing to keep in mind is these do not pass-by-reference. They work exactly like any other variable would, so don't go passing them around and expecting changes to be reflected. You cannot run away from the pointers.

Basic struct declaration looks like this:
```c title:'struct declaration'
struct person {
	unsigned char age;
	char* firstname;
	char* surname;
};
```

You can then create a variable of type `person` in your code.
```c title:'using structs'
#include <stdio.h>
struct person {
	unsigned char age;
	char* firstname;
	char* surname;
};

int main(void) {
	person bob;

	// you may then use a period to access/assign the 'inner' variables of the structure
	bob.age = (unsigned char)28u;
	bob.firstname = "Bob";
	bob.surname = "Bobingson";

	printf(bob.firstname);

	return 0;
}
```

If you want to use pointers with your structs, which you probably do, then you'll need to use slightly different syntax. There are two options, both of which I'll show down below.
```c title:"structs and pointers: a dummy's guide"
struct person {
	unsigned char age;
	char* firstname;
	char* surname;
};

int main(void) {
	person bob;
	bob.age = (unsigned char)28u;
	bob.firstname = "Bob";
	bob.surname = "Bobingson";

	person* bobs_phone_number = &bob;

	// now if you want to change bob's age through his phone number, you can either do it by de-referencing the pointer, like this
	(*bobs_phone_number).age = (unsigned char)29u;
	// or, you can use some syntactic sugar and do it like this
	bobs_phone_number->age = (unsigned char)29u;
	return 0;
}
```
Either of these methods work, but I would advise the latter as it makes it clear that `bobs_phone_number` is a pointer to a struct. 

For further reading, please see [[Bit Fields]] and [[Unions]]. 