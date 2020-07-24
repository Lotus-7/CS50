---
show: step
version: 1.0
enable_checker: true
---

# Knowledge of memory

## Introduction

In the previous lecture, we talked about memory and how each byte has an address, or identifier, so we can refer to where our variables are actually stored.

In this lesson, we will learn new knowledge, including Hexadecimal, Pointers, Memory layout and so on.

#### knowledge point

- Hexadecimal
- Pointers
- string
- Compare and copy
- Swap
- Memory layout
- get_int
- Files
- JPEG

## Hexadecimal

It turns out that, by convention, the addresses for memory use the counting system hexadecimal, where there are 16 digits, 0-9 and A-F.

Recall that, in binary, each digit stood for a power of 2:

```C
128 64 32 16  8  4  2  1
  1  1  1  1  1  1  1  1
```

- With 8 bits, we can count up to 255.

It turns out that, in hexadecimal, we can perfectly count up to 8 binary bits with just 2 digits:

```C
16^1 16^0
   F    F
```

- Here, the `F` is a value of 15 in decimal, and each place is a power of 16, so the first `F` is 16^1 * 15 = 240, plus the second `F` with the value of 16^0 * 15 = 15, for a total of 255.

And `0A` is the same as 10 in decimal, and `0F` the same as 15. `10` in hexadecimal would be 16, and we would say it as “one zero in hexadecimal” instead of “ten”, if we wanted to avoid confusion.

The RGB color system also conventionally uses hexadecimal to describe the amount of each color. For example, `000000` in hexadecimal means 0 of each red, green, and blue, for a color of black. And `FF0000` would be 255, or the highest possible, amount of red. With different values for each color, we can represent millions of different colors.

In writing, we can also indicate a value is in hexadecimal by prefixing it with `0x`, as in `0x10`, where the value is equal to 16 in decimal, as opposed to 10.

## Pointers

Create a file `memory.c`. We might create a value n, and print it out:

```C
#include <stdio.h>

int main(void)
{
    int n = 50;
    printf("%i\n", n);
}
```

In our computer’s memory, there are now 4 bytes somewhere that have the binary value of 50, labeled `n`:

![4-1](https://doc.shiyanlou.com/courses/2618/1347963/e76abd131175a8965b4b3efe5bf34449-0/wm)

It turns out that, with the billions of bytes in memory, those bytes for the variable `n` starts at some unique address that might look like `0x12345678`.

In C, we can actually see the address with the `&` operator, which means “get the address of this variable”:

```C
#include <stdio.h>

int main(void)
{
    int n = 50;
    printf("%p\n", &n);
}
```

![4-9](https://doc.shiyanlou.com/courses/2618/1347963/3237df6dd4a7c1d1e4a7ce1721c504cd-0/wm)

- And in the CS50 IDE, we might see an address like `0x7ffe00b3adbc`, where this is a specific location in the server’s memory.

The address of a variable is called a **pointer**, which we can think of as a value that “points” to a location in memory. The `*` operator lets us “go to” the location that a pointer is pointing to.

For example, we can print `*&n`, where we “go to” the address of `n`, and that will print out the value of `n`, `50`, since that’s the value at the address of `n`:

```C
#include <stdio.h>

int main(void)
{
    int n = 50;
    printf("%i\n", *&n);
}
```

![4-10](https://doc.shiyanlou.com/courses/2618/1347963/d056a1e28c3fcff1f3751ff189af42ed-0/wm)

We also have to use the `*` operator (in an unfortunately confusing way) to declare a variable that we want to be a pointer:

```C
#include <stdio.h>

int main(void)
{
   int n = 50;
   int *p = &n;
   printf("%p\n", p);
}
```

![4-11](https://doc.shiyanlou.com/courses/2618/1347963/6579961ea208a3b644f9fc5228bd090e-0/wm)

- Here, we use `int *p` to declare a variable, `p`, that has the type of `*`, a pointer, to a value of type `int`, an integer. Then, we can print its value (something like `0x12345678`), or print the value at its location with `printf("%i\n", *p);`.

In our computer’s memory, the variables might look like this:

![4-2](https://doc.shiyanlou.com/courses/2618/1347963/6c0fb6bb9b12e360d5fd11cbf3025a07-0/wm)

- We have a pointer, `p`, with the address of some variable.

We can abstract away the actual value of the addresses now, since they’ll be different as we declare variables in our programs, and simply think of `p` as “pointing at” some value:

![4-3](https://doc.shiyanlou.com/courses/2618/1347963/d7c40342940328093d3accf6451df6a3-0/wm)

Let’s say we have a mailbox labeled “123”, with the number “50” inside it. The mailbox would be `int n`, since it stores an integer. We might have another mailbox with the address “456”, inside of which is the value “123”, which is the address of our other mailbox. This would be `int *p`, since it’s a pointer to an integer.

With the ability to use pointers, we can create different data structures, or different ways to organize data in memory that we’ll see next week.

Many modern computer systems are “64-bit”, meaning that they use 64 bits to address memory, so a pointer will be 8 bytes, twice as big as an integer of 4 bytes.

```checker
- name: check memory.c
  script: |
    #!/bin/bash
    ls /home/project/memory.c
  error: /home/project/memory.c 目录不存在
```

## string

We might have a variable string s for a name like EMMA, and be able to access each character with s[0] and so on:

![4-4](https://doc.shiyanlou.com/courses/2618/1347963/b22f957ff76a2e635a7aac47a5bad17e-0/wm)

But it turns out that each character is stored in memory at a byte with some address, and `s` is actually just a pointer with the address of the first character:

![4-5](https://doc.shiyanlou.com/courses/2618/1347963/e1c07ac5ff617a74d6d70214c6608eaa-0/wm)

And since `s` is just a pointer to the beginning, only the `\0` indicates the end of the string.

In fact, the CS50 Library defines a `string` with `typedef char *string`, which just says that we want to name a new type, `string`, as a `char *`, or a pointer to a character.

Let’s print out a string:

```C
// create a file 'string1.c'
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string s = "EMMA";
    printf("%s\n", s);
}
```

This is familiar, but we can just say:

```C
// create a file 'string2.c'
#include <stdio.h>

int main(void)
{
    char *s = "EMMA";
    printf("%s\n", s);
}
```

- This will also print `EMMA`.

With `printf("%p\n", s);`, we can print `s` as its value as a pointer, like `0x42ab52`. (`printf` knows to go to the address and print the entire string when we use `%s` and pass in `s`, even though `s` only points to the first character.)

We can also try `printf("%p\n", &s[0]);`, which is the address of the first character of `s`, and it’s exactly the same as printing `s`. And printing `&s[1]`, `&s[2]`, and `&s[3]` gets us the addresses that are the next characters in memory after `&s[0]`, like `0x42ab53`, `0x42ab54`, and `0x42ab55`, exactly one byte after another.

And finally, if we try to `printf("%c\n", *s);`, we get a single character `E`, since we’re going to the address contained in `s`, which has the first character in the string.

In fact, `s[0]`, `s[1]`, and `s[2]` actually map directly to `*s`, `*(s+1)`, and `*(s+2)`, since each of the next characters are just at the address of the next byte.

```checker
- name: check string1.c
  script: |
    #!/bin/bash
    ls /home/project/string1.c
  error: /home/project/string1.c 目录不存在
```

## Compare and copy

Let’s look at `compare0.c`:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Get two integers
    int i = get_int("i: ");
    int j = get_int("j: ");

    // Compare integers
    if (i == j)
    {
        printf("Same\n");
    }
    else
    {
        printf("Different\n");
    }
}
```

![4-12](https://doc.shiyanlou.com/courses/2618/1347963/c2bd2a0ac10797cabde046d24428bba0-0/wm)

- We can compile and run this, and our program works as we’d expect, with the same values of the two integers giving us “Same” and different values “Different”.

In `compare1.c`, we see that the same string values are causing our program to print “Different”:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Get two strings
    string s = get_string("s: ");
    string t = get_string("t: ");

    // Compare strings' addresses
    if (s == t)
    {
        printf("Same\n");
    }
    else
    {
        printf("Different\n");
    }
}
```

![4-13](https://doc.shiyanlou.com/courses/2618/1347963/6a6f2cac73fef388f0f31395c20e2b2c-0/wm)

- Given what we now know about strings, this makes sense because each “string” variable is pointing to a different location in memory, where the first character of each string is stored. So even if the values of the strings are the same, this will always print “Different”.

- For example, our first string might be at address 0x123, our second might be at 0x456, and `s` will be `0x123` and `t` will be `0x456`, so those values will be different.

- And `get_string`, this whole time, has been returning just a `char *`, or a pointer to the first character of a string from the user.

Now let’s try to copy a string in `copy.c`:

```C
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>

int main(void)
{
    string s = get_string("s: ");

    string t = s;

    t[0] = toupper(t[0]);

    // Print string twice
    printf("s: %s\n", s);
    printf("t: %s\n", t);
}
```

![4-14](https://doc.shiyanlou.com/courses/2618/1347963/de351110461e3602b4349ecde0e72851-0/wm)

- We get a string `s`, and copy the value of s into `t`. Then, we capitalize the first letter in `t`.
- But when we run our program, we see that both `s` and `t` are now capitalized.
- Since we set `s` and `t` to the same values, they’re actually pointers to the same character, and so we capitalized the same character!

To actually make a copy of a string, we have to do a little more work:

```C
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(void)
{
    char *s = get_string("s: ");

    char *t = malloc(strlen(s) + 1);

    for (int i = 0, n = strlen(s); i < n + 1; i++)
    {
        t[i] = s[i];
    }

    t[0] = toupper(t[0]);

    printf("s: %s\n", s);
    printf("t: %s\n", t);
}
```

![4-15](https://doc.shiyanlou.com/courses/2618/1347963/97f89d998787437f01a401d639500eed-0/wm)

- We create a new variable, `t`, of the type `char *`, with `char *t`. Now, we want to point it to a new chunk of memory that’s large enough to store the copy of the string. With `malloc`, we can allocate some number of bytes in memory (that aren’t already used to store other values), and we pass in the number of bytes we’d like. We already know the length of `s`, so we add 1 to that for the terminating null character. So, our final line of code is `char *t = malloc(strlen(s) + 1);`.

- Then, we copy each character, one at a time, and now we can capitalize just the first letter of `t`. And we use `i < n + 1`, since we actually want to go up to `n`, to ensure we copy the terminating character in the string.

- We can actually also use the `strcpy` library function with `strcpy(t, s)` instead of our loop, to copy the string `s` into `t`. To be clear, the concept of a “string” is from the C language and well-supported; the only training wheels from CS50 are the type `string` instead of `char *`, and the `get_string` function.

If we didn’t copy the null terminating character, `\0`, and tried to print out our string `t`, `printf` will continue and print out the unknown, or garbage, values that we have in memory, until it happens to reach a `\0`, or crashes entirely, since our program might end up trying to read memory that doesn’t belong to it!

```checker
- name: check copy.c
  script: |
    #!/bin/bash
    ls /home/project/copy.c
  error: /home/project/copy.c 目录不存在
```

## Swap

We have two colored drinks, purple and green, each of which is in a cup. We want to swap the drinks between the two cups, but we can’t do that without a third cup to pour one of the drink into first.

Now, let’s say we wanted to swap the values of two integers.

```C
void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

- With a third variable to use as temporary storage space, we can do this pretty easily, by putting `a` into `tmp`, and then `b` to `a`, and finally the original value of `a`, now in `tmp`, into `b`.

But, if we tried to use that function in a program, we don’t see any changes:

```C
// create a file 'swap.c'
#include <stdio.h>

void swap(int a, int b);

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(x, y);
    printf("x is %i, y is %i\n", x, y);
}

void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

![4-16](https://doc.shiyanlou.com/courses/2618/1347963/86a4ecfa912f7988ae05d9b84cdc4cc3-0/wm)

- It turns out that the `swap` function gets its own variables, `a` and `b` when they are passed in, that are copies of `x` and `y`, and so changing those values don’t change `x` and `y` in the `main` function.

```checker
- name: check swap.c
  script: |
    #!/bin/bash
    ls /home/project/swap.c
  error: /home/project/swap.c 目录不存在
```

## Memory layout

Within our computer’s memory, the different types of data that need to be stored for our program are organized into different sections:

![4-6](https://doc.shiyanlou.com/courses/2618/1347963/8637eacdf87f57027dcd69bb65647d46-0/wm)

- The machine code section is our compiled program’s binary code. When we run our program, that code is loaded into the “top” of memory.
- Globals are global variables we declare in our program or other shared variables that our entire program can access.
- The heap section is an empty area where `malloc` can get free memory from, for our program to use.
- The stack section is used by functions in our program as they are called. For example, our `main` function is at the very bottom of the stack, and has the local variables `x` and `y`. The `swap` function, when it’s called, has its own frame, or slice, of memory that’s on top of `main`’s, with the local variables `a`, `b`, and `tmp`:

![4-7](https://doc.shiyanlou.com/courses/2618/1347963/23174e36e37b68cf057c23c88e4a4538-0/wm)

- Once the function `swap` returns, the memory it was using is freed for the next function call, and we lose anything we did, other than the return values, and our program goes back to the function that called `swap`.

- So by passing in the addresses of `x` and `y` from `main` to `swap`, we can actually change the values of `x` and `y`:

![4-8](https://doc.shiyanlou.com/courses/2618/1347963/9f98ae6d1390983b6a048abd31962075-0/wm)

By passing in the address of `x` and `y`, our `swap` function can actually work:

```C
#include <stdio.h>

void swap(int *a, int *b);

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(&x, &y);
    printf("x is %i, y is %i\n", x, y);
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
```

![4-17](https://doc.shiyanlou.com/courses/2618/1347963/d989d6c5cea1ff341d5ae550b2e02b5e-0/wm)

- The addresses of `x` and `y` are passed in from `main` to `swap`, and we use the `int *a` syntax to declare that our `swap` function takes in pointers. We save the value of `x` to `tmp` by following the pointer `a`, and then take the value of `y` by following the pointer `b`, and store that to the location `a` is pointing to (`x`). Finally, we store the value of `tmp` to the location pointed to by `b` (`y`), and we’re done.

If we call `malloc` too many times, we will have a **heap overflow**, where we end up going past our heap. Or, if we have too many functions being called, we will have a **stack overflow**, where our stack has too many frames of memory allocated as well. And these two types of overflow are generally known as buffer overflows, after which our program (or entire computer) might crash.

## get_int

We can implement `get_int` ourselves with a C library function, `scanf`:

```C
// create a file 'get_int.c'
#include <stdio.h>

int main(void)
{
    int x;
    printf("x: ");
    scanf("%i", &x);
    printf("x: %i\n", x);
}
```

![4-18](https://doc.shiyanlou.com/courses/2618/1347963/cfe0e0487199af3a8f6072d022c029bf-0/wm)

- `scanf` takes a format, `%i`, so the input is “scanned” for that format, and the address in memory where we want that input to go. But `scanf` doesn’t have much error checking, so we might not get an integer.

We can try to get a string the same way:

```C
// create a file 'get_str.c'
#include <stdio.h>

int main(void)
{
    char *s = NULL;
    printf("s: ");
    scanf("%s", s);
    printf("s: %s\n", s);
}
```

![4-19](https://doc.shiyanlou.com/courses/2618/1347963/b097ac375fa96ff8d9c107be33c9c369-0/wm)

- But we haven’t actually allocated any memory for `s` (`s` is `NULL`, or not pointing to anything), so we might want to call `char s[5]` to allocate an array of 5 characters for our string. Then, `s` will be treated as a pointer in `scanf` and `printf`.
- Now, if the user types in a string of length 4 or less, our program will work safely. But if the user types in a longer string, `scanf` might be trying to write past the end of our array into unknown memory, causing our program to crash.

```checker
- name: check get_str.c
  script: |
    #!/bin/bash
    ls /home/project/get_str.c
  error: /home/project/get_str.c 目录不存在
```

## Files

With the ability to use pointers, we can also open files:

```C
// create a file 'file.c'
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    // Open file
    FILE *file = fopen("phonebook.csv", "a");

    // Get strings from user
    char *name = get_string("Name: ");
    char *number = get_string("Number: ");

    // Print (write) strings to file
    fprintf(file, "%s,%s\n", name, number);

    // Close file
    fclose(file);
}
```

![4-20](https://doc.shiyanlou.com/courses/2618/1347963/b124666ab5f6bf373ab53d7dd2a7e62e-0/wm)

![4-21](https://doc.shiyanlou.com/courses/2618/1347963/819ffa4ffdc6ff9143fbe5c99c933dfa-0/wm)

- `fopen` is a new function we can use to open a file. It will return a pointer to a new type, `FILE`, that we can read from and write to. The first argument is the name of the file, and the second argument is the mode we want to open the file in (`r` for read, `w` for write, and `a` for append, or adding to).
- After we get some strings, we can use `fprintf` to print to a file.
- Finally, we close the file with `fclose`.

Now we can create our own CSV files, files of comma-separated values (like a mini-spreadsheet), programmatically.

```checker
- name: check file.c
  script: |
    #!/bin/bash
    ls /home/project/file.c
  error: 没有发现 file.c 文件
  timeout: 1
```

## JPEG

We can also write a program that opens a file and tells us if it’s a JPEG (image) file:

```C
// create a file 'jpeg.c'
#include <stdio.h>

int main(int argc, char *argv[])
{
    // Check usage
    if (argc != 2)
    {
        return 1;
    }

    // Open file
    FILE *file = fopen(argv[1], "r");
    if (!file)
    {
        return 1;
    }

    // Read first three bytes
    unsigned char bytes[3];
    fread(bytes, 3, 1, file);

    // Check first three bytes
    if (bytes[0] == 0xff && bytes[1] == 0xd8 && bytes[2] == 0xff)
    {
        printf("Maybe\n");
    }
    else
    {
        printf("No\n");
    }

    // Close file
    fclose(file);
}
```

- Now, if we run this program with `./jpeg brian.jpg`, our program will try to open the file we specify (checking that we indeed get a non-NULL file back), and read the first three bytes from the file with `fread`.
- We can compare the first three bytes (in hexadecimal) to the three bytes required to begin a JPEG file. If they’re the same, then our file is likely to be a JPEG file (though, other types of files may still begin with those bytes). But if they’re not the same, we know it’s definitely not a JPEG file.

```checker
- name: check jpeg.c
  script: |
    #!/bin/bash
    ls /home/project/jpeg.c
  error: 未通过检测
  timeout: 1
```

## Experimental summary 

In this lesson, we introduced some new knowledge about memory, including Hexadecimal, Pointers, Compare and copy, Memory layout and so on.

Now, we can use these abilities to read and write files, in particular images, and modify them by changing the bytes in them.
