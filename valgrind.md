# Understanding Valgrind Error Messages

## Background

We use Valgrind to obtain information about two categories of incorrect memory
management: memory leaks and memory errors.  Memory leaks occur when heap memory
is not freed, and generally do not cause any kind of misbehavior (unless the
process runs out of allocatable memory).  Memory errors, in constrast, are a
common source of undefined behavior---and are the focus of this guide.

There are four error messages that you should understand:

1. Invalid reads
2. Invalid writes
3. Conditional jumps and moves that depend on uninitialized value(s)
4. Segmentation faults (colloquially, "segfaults")

This guide will provide an explanation and example for each.

## Invalid read

An invalid read occurs when you attempt to read a value from a location in
memory that is not available to the program (e.g. heap memory outside of
`malloc()`ed blocks and memory which overruns the top of the stack).  The error
message always includes the number of bytes your program attempted to read,
which is useful for debugging the error.  On CLAC, a size of 1 indicates a
`char`, 4 indicates an `int`, and 8 usually indicates a pointer.

Here's a simple program that will lead to an invalid read of size 4 by reading
4 bytes (one extra `int`) beyond a heap-allocated array of integers.  The
program allocates 20 bytes while it attempts to dereference and read from 24.

```c
int main(void) {
    int *p = malloc(5 * sizeof(int));

    if (p == NULL)
        exit(1);

    for (int i = 0; i < 5; i++)
        p[i] = i;

    for (int i = 0; i < 6; i++)
        printf("%d\n", p[i]);

    free(p);
}
```

## Invalid write

An invalid write is similar to an invalid read, but it occurs when you attempt
to write to an illegal memory location.

Here's a simple program that causes an invalid write of size 4 by writing one
extra `int` outside of a heap-allocated array.  This program allocates 20 bytes
(5 `int`s), but tries to write 24 bytes worth of values (6 `int`s).

```c
int main(void) {
    int *p = malloc(5 * sizeof(int));

    if (p == NULL)
        exit(1);

    for (int i = 0; i < 6; i++)
        p[i] = i;


    for (int i = 0; i < 5; i++) {
        printf("%d\n", p[i]);
    }

    free(p);
}
```

## Conditional jump or move depends on uninitialized value(s)

The error message "conditional jump or move depends on uninitialized value(s)"
indicates that the outcome of some operation depends on a variable that has not
been initialized.  Because the value of an uninitialized variable is not
defined, a program whose outcome is dependent on an uninitialized variable will
have undetermined behavior.

A conditional jump refers to a statement that determines control flow, such as
an `if` statement, a `while` loop, or a `for` loop.  A move refers to any other
kind of read from memory.  Note that conditional jumps and moves may take place
deep within library code if you pass pointers to uninitialized memory to
library functions like `printf()`.

In the following program, `h()` passes a pointer to the uninitialized variable
`x` to `g()`, which dereferences that pointer.  The value that `g()` reads is
usually unpredictable; in this case, `g()` may pick up a value of 42 left
behind by `f()`'s stack frame.

```c
#include <stdio.h>

void f(void) {
    int x = 42;
    printf("f: %p -> %d\n", &x, x);
}

void g(int *i) {
    int j = *i + 10;
    printf("g: %d\n", j);
}

void h(void) {
    int x; // Uninitialized
    g(&x);
}

int main(void) {
    f();
    h();
}
```

This error often occurs when you forget to null-terminate a string.  Here's
a sample program which populates and prints out a `char` array `a`.  However,
it forgets to null-terminate `a` before passing it to `printf()`, leading to
a memory error:

```c
int main(void) {
    char *s = "hello";
    char a[6];

    for(int i = 0; i < 5; i++)
        a[i] = s[i];

    printf("%s\n", a);
}
```

## Segmentation fault

A segmentation fault occurs when the operating system intervenes after an
invalid read or write, causing your program to crash.  The most common source
of segmentation faults is an attempt to dereference a null pointer, indicated
by an invalid read at `0x0`.  Segmentation faults can be observed without using
Valgrind, but Valgrind will provide more detailed information about where and
why the error occurred.

Here's a program which attempts to write to read-only memory. `char *s` points
to the read-only code section of memory, but the `char` arrays `s1` and `s2`
are on the stack, which can be read from and written to.  The first call to
`strcpy()` reads from `s` and writes to `s1`, which is fine.  However, the
second call attempts to write to `s`, which leads to a segmentation fault.

```c
int main(void) {
    char *s = "hi";
    char s1[3] = "no";
    char s2[3] = "ok";

    printf("%s %s %s\n", s, s1, s2);

    strcpy(s1, s);  // This is OK.

    printf("%s %s %s\n", s, s1, s2);

    strcpy(s, s2);  // This is not.

    printf("%s %s %s \n", s, s1, s2);
}
```

----

### Acknowledgements

This guide was originally written for the listserv by Brennan McManus in Fall 2019.
