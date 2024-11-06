You'll need to `#include <pthread.h>` in order to use the pthreads library. It is only available on UNIX systems - which is why the module relies on you using a Linux virtual machine.

The most important function to utilise pthreads is 
`pthread_create(THREAD_POINTER, ARGUMENTS, THREAD_FUNCTION, THREAD_ARGUMENTS)`
- THREAD_POINTER: You'll want to pass in a reference to a pthread_t, so that you may refer back to the thread created later in your code.
- ARGUMENTS: May be left `NULL`. 
- THREAD_FUNCTION: Write the name of your function in here, without the `()`.
- THREAD_ARGUMENTS: This is where you'll pass in your thread parameters. This must be a `void*`, which can be a pointer to anything - see [[Void Pointers]]
This will then create a thread which executes your passed-in function.

Example:
```c title:"threading example"
#include <stdlib.h>
#include <pthread.h>
struct weaver {
	int speed;
	unsigned float salary;
	char* name;
}
void* weaving(void* args) {
	weaver employee = *((weaver*)args);
	// do something
	return 0;
}

int main(void) {
	weaver jim = {10, 1000, "Jim Jimson"};

	pthread_t thr;
	pthread_create(&thr, NULL, weaving, (void*)&jim);
	return 0;
}
```

To ensure that the thread finishes before we end the program, we'll want to use `pthread_join(THREAD, RETURN_VALUE)`
- THREAD: Pass in the pthread_t that you want to wait for.
- RETURN_VALUE: You can use this to get the exit code of the thread. You can use NULL (and probably will)

Let's add this to our example
```c title:"adding joins"
#include <stdlib.h>
#include <pthread.h>
struct weaver {
	int speed;
	unsigned float salary;
	char* name;
}
void* weaving(void* args) {
	weaver employee = *((weaver*)args);
	// do something
	return 0;
}

int main(void) {
	weaver jim = {10, 1000, "Jim Jimson"};

	pthread_t thr;
	pthread_create(&thr, NULL, weaving, (void*)&jim);

	pthread_join(thr, NULL);
	return 0;
}
```
Now the program won't end until the `weaving()` thread finishes.

If you're not modifying shared variables, this is essentially as tough as threading gets.

## Race Conditions
If you're accessing data that is shared between threads, you'll inevitably run into race conditions. This is when you don't know which thread will execute first, which creates uncertainty as to how the program will run. You can either design your program to not need to share data, which is difficult at the best of times and sometimes entirely impossible, or, you can use mutexes to 'lock' parts of code, ensuring that data will be accessed by one thread at a time. 

To use a mutex, we'll want to make a `pthread_mutex_t` variable. To initialise it, we'll want to set it to `PTHREAD_MUTEX_INITIALISER`. 
To then activate the mutex, we'll want to use the functions `pthread_mutex_lock(pthread_mutex_t* MUTEX_POINTER)` and `pthread_mutex_unlock(MUTEX_POINTER)`. Between locking and unlocking the code will be exclusive to one thread whilst executing.
```c title:"mutex init"
#include <pthread.h>
#include <stdlib.h>

// make sure this is not in local scope (i.e. inside a function)
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALISER;

struct weaver {
	int speed;
	unsigned float salary;
	char* name;
}
void* weaving(void* args) {
	weaver employee = *((weaver*)args);

	// here's something
	// could make a race condition depending on if the compiler is optimising or not but we will assume it does.
	// to avoid that, we lock it.
	pthread_mutex_lock(&mutex);
	int x = employee.speed;
	int y = 77;
	employee.speed = x + y;
	pthread_mutex_unlock(&mutex);
	return 0;
}
weaver jim = {10, 1000, "Jim Jimson"};
int main(void) {
	
	pthread_t thr;
	pthread_create(&thr, NULL, weaving, (void*)&jim);

	pthread_join(thr, NULL);
	return 0;
}

```




