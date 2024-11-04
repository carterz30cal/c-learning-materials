The absolute minimum you need for a C program is a function called main. 
That's it. Every C program calls this function when the executable runs, and the code will all ultimately lead back to the **main**() function.

```c
int main(void) {
	
}
```
*This is a perfectly valid C program*

Most C programs want to return a code when they're done. Most of the time, this will be 0, which indicates success, but you can return any other number arbitrarily.

```c
int main(void) {
	return 0; // Success!
}

int main(void) {
	return -50; // not a success, and can mean whatever you want really.
}
```
*There are a few pre-defined exit codes with special meanings, which are usually OS specific.*

All modern C compilers will implicitly return 0, if you do not write a return statement at the end of your **main**() function, but for best practice you should write this out explicitly. 

These next couple pages are all about the simple building blocks for creating a C program:
- [[Variables]]
- [[Defining Functions]]

