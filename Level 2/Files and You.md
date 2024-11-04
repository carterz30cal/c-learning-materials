We'll be using the `FILE*` data type for this. 
In order to open a file in the working directory (where the executable is stored), you'll want to call
`fopen(FILE_NAME, MODE)`
- FILE_NAME is our file path, which should look like "file.txt"
- MODE is the open mode we're going to use. There are a couple different options here.
You can view file modes [here.](https://en.cppreference.com/w/cpp/io/c/fopen) *(Whilst this is a C++ reference, they're the same)*
This function returns a `FILE*` pointer, which we can then read and write to, assuming we're in the appropriate file mode.
Instead of calling`free()` on our `FILE*` objects, we'll be calling `fclose()`.
```c title:"basic file handling"
#include <stdio.h>
int main(void) {
	FILE* opening = fopen("jimbob.txt", "r+");
	if (opening == NULL) exit(1);
	// you essentially treat files like a console input/output
	int x = 60;
	fprintf(opening, "%d", x); // writes 60 to the file
	int y;
	fscanf(opening, "%d", &y); // to read one int we create a pointer to y using the reference operator and pass it into fscanf.
	

	fclose(opening);
	return 0;
}
```

If you try to read beyond the end of the file, you'll cause an error. To eliminate this problem, use the function `feof(FILE*)`, which returns zero before you hit the end of the file then an implementation specific non-zero value afterwards. 