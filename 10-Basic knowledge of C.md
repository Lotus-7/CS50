---
show: step
version: 1.0
enable_checker: true
---

# Basic knowledge of C

##  Introduction

Today we’ll learn a new language, C: a programming language. In this experiment, we will learn the C about compiler, string, types, formats, etc.

#### knowledge point

- hello, world
- Compilers
- string
- Types, formats, operators
- Memory, imprecision, and overflow

## hello, world 

In C, the first line for the same is `int main(void)`, which we’ll learn more about over the coming weeks, followed by an open curly brace `{`, and a closed curly brace `}`, wrapping everything that should be in our program.

```C
int main(void)
{

}
```

The “say (hello, world)” block is a function, and maps to `printf("hello, world");`. In C, the function to print something to the screen is `printf`, where `f` stands for “format”, meaning we can format the printed string in different ways. Then, we use parentheses to pass in what we want to print. We have to use double quotes to surround our text so it’s understood as text, and finally, we add a semicolon `;` to end this line of code.

To make our program work, we also need another line at the top, a header line `#include <stdio.h>` that defines the `printf` function that we want to use. Somewhere there is a file on our computer, `stdio.h`, that includes the code that allows us to access the `printf` function, and the `#include` line tells the computer to include that file with our program.

To write our first program in WebIDE, we opened WebIDE on the right. At the top, there is a simple code editor, where we can type text. Below, we have a terminal window, into which we can type commands:

![1-1](https://doc.shiyanlou.com/courses/2618/1347963/a5fc30232a08c9c9c748f0507b37fafb-0/wm)

We’ll type our code from earlier into the top, after using the + sign to create a new file called `hello.c`:

We should click `File` on the left, then click `New File`, and a window will appear on your screen, you can create a new file called `hello.c`.

![1-2](https://doc.shiyanlou.com/courses/2618/1347963/42389c273037d69acd2b3b965d1c92cb-0/wm)

![1-3](https://doc.shiyanlou.com/courses/2618/1347963/138968c65f51147cf2cb42897d7efa4f-0/wm)

Then, we can type our code from earlier into the top.

![1-4](https://doc.shiyanlou.com/courses/2618/1347963/1d275c2e3f7a257e80196c52462129da-0/wm)

We end our program’s file with `.c` by convention, to indicate that it’s intended as a C program. Notice that our code is colorized, so that certain things are more visible.

```checker
- name: check hello.c
  script: |
    #!/bin/bash
    ls /home/project/hello.c
  error: /home/project/hello.c 目录不存在
```

## Compilers

Once we save the code that we wrote, which is called **source code**, we need to convert it to **machine code**, binary instructions that the computer understands directly.

We use a program called a **compiler** to compile our source code into machine code.

To do this, we use the **Terminal** panel, which has a **command prompt**. The `$` at the left is a prompt, after which we can type commands.

We type terminal command `gcc -o hello hello.c`. The command calls compiler GCC to compile file `hello.c` completing "preprocessing - compile - assembler - link" and generating executable `hello`. 

Then we need to type command `./hello`. This command will run your code.

![1-6](https://doc.shiyanlou.com/courses/2618/1347963/1943725113805d2460f2ced311ec53bb-0/wm)

We just wrote, compiled, and ran our first program!

## String

But after we run our program, we see `hello, world%`, with the new prompt on the same line as our output. It turns out that we need to specify precisely that we need a new line after our program, so we can update our code to include a special newline character, `\n`:

```C
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}

```

![1-5](https://doc.shiyanlou.com/courses/2618/1347963/7ffb1548ae96c69e98d43540938c3cf6-0/wm)

>Now we need to remember to recompile our program with `gcc -o hello hello.c` before we can run this new version.

Line 2 of our program is intentionally blank since we want to start a new section of code, much like starting new paragraphs in essays. It’s not strictly necessary for our program to run correctly, but it helps humans read longer programs more easily.

If we made a mistake, like writing printf("hello, world"\n); with the \n outside of the double quotes for our string, we’ll see an errors from our compiler:

![1-7](https://doc.shiyanlou.com/courses/2618/1347963/382af97af19ff3cd643640076645eeb0-0/wm)

The first line of the error tells us to look at hello.c, line 5, column 5, where the compiler expected a closing parentheses, instead of a backslash.

To simplify things (at least for the beginning), We’ll include a library, or set of code, from CS50. The library provides us with the `string` variable type, the `get_string` function, and more. We just have to write a line at the top to `include` the file `cs50.h`:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, name\n");
}
```

So let’s make a new file, `string.c`, with this code:

```C
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

Now, if we try to compile that code, we get a lot of lines of errors. Sometimes, one mistake means that the compiler then starts interpreting correct code incorrectly, generating more errors than there actually are. So we start with our first error:

![1-8](https://doc.shiyanlou.com/courses/2618/1347963/7ce51d8bbd3d842cdefa02a46c37c050-0/wm)

In fact, we need to import another file that defines the type string (actually a training wheel from CS50, as we’ll find out in the coming weeks).

So we can include another file, cs50.h, which also includes the function get_string, among others.

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

Now, when we try to compile our program, we have just one error:

![1-9](https://doc.shiyanlou.com/courses/2618/1347963/56dda2c660538ba0e0482007c2219ed8-0/wm)

It turns out that we also have to tell our compiler to add our special CS50 library file.

You can install it for [Github](https://github.com/cs50/libcs50). In our IDE,
we should use the following command to install.

```shell
curl -s https://packagecloud.io/install/repositories/cs50/repo/script.deb.sh | sudo bash
sudo apt-get install libcs50
```

Our default installation path is `/usr/include/cs50.h`. You don't need fix path. we should use the following command to compile.

```shell
gcc -o string string.c -lcs50

./string
```

Now, you can run your code successfully.

![1-10](https://doc.shiyanlou.com/courses/2618/1347963/b85381beb093953ed7c2e25b298b0aa0-0/wm)

```checker
- name: check string.c
  script: |
    #!/bin/bash
    grep hello
  error: 运行 string.c 发生错误
  timeout: 1

```

## Types, formats, operators

There are other types we can use for our variables.

- `bool`, a Boolean expression of either `true` or `false`.
- `char`, a single character like `a` or `2`.
- `double`, a floating-point value with even more digits.
- `float`, a floating-point value, or real number with a decimal value.
- `int`, integers up to a certain size, or number of bits.
- `long`, integers with more bits, so they can count higher.
- `string`, a string of characters.

And the CS50 library has corresponding functions to get input of various types:

- `get_char`
- `get_double`
- `get_float`
- `get_int`
- `get_long`
- `get_string`

For printf, too, there are different placeholders for each type:

- `%c` for chars.
- `%f` for floats, doubles.
- `%i` for ints.
- `%li` for longs.
- `%s` for strings.

And there are some mathematical operators we can use:

- `+` for addition.
- `-` for subtraction.
- `*` for multiplication.
- `/` for division.
- `%` for remainder.

## More examples

For each of these examples, you can click on the [sandbox links](https://cs50.harvard.edu/x/2020/weeks/1/) to run and edit your own copies of them.

### int

In `int.c`, we get and print an integer:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int age = get_int("What's your age?\n");
    int days = age * 365;
    printf("You are at least %i days old.\n", days);
}
```

![1-11](https://doc.shiyanlou.com/courses/2618/1347963/45c6b3e98bbdddfb37d09318b1c01c93-0/wm)

- Notice that we use `%i` to print an integer.
- We can now run `make int` and run our program with `./int`.
- We can combine lines and remove the days variable with:

```C
int age = get_int("What's your age?\n");
printf("You are at least %i days old.\n", age * 365);
```

- Or even combine everything in one line:

```C
printf("You are at least %i days old.\n", get_int("What's your age?\n") * 365);
```

- Though, once a line is too long or complicated, it may be better to keep two or even three lines for readability.

```checker
- name: check int.c
  script: |
    #!/bin/bash
    ls /home/project/int
  error: 运行 int.c 发生错误
  timeout: 1
```

### float

In `float.c`, we can get decimal numbers (called floating-point values in computers, because the decimal point can “float” between the digits, depending on the number):

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    float price = get_float("What's the price?\n");
    printf("Your total is %f.\n", price * 1.0625);
}
```

![1-12](https://doc.shiyanlou.com/courses/2618/1347963/e00e79d8d1af6dded30e1bd31999e3ac-0/wm)

- Now, if we compile and run our program, we’ll see a price printed out with tax.

- We can specify the number of digits printed after the decimal with a placeholder like `%.2f` for two digits after the decimal point.

```checker
- name: check float.c
  script: |
    #!/bin/bash
    ls /home/project/float
  error: 运行 float.c 发生错误
  timeout: 1
```

### if...else

With `parity.c`, we can check if a number is even or odd:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int n = get_int("n: ");

    if (n % 2 == 0)
    {
        printf("even\n");
    }
    else
    {
        printf("odd\n");
    }
}
```

![1-13](https://doc.shiyanlou.com/courses/2618/1347963/9f12536281c8fa27e8528c5afc681cb7-0/wm)

- With the `%` (modulo) operator, we can get the remainder of n after it’s divided by 2. If the remainder is 0, we know that `n` is even. Otherwise, we know `n` is odd.

- And functions like `get_int` from the CS50 library do error-checking, where only inputs from the user that matches the type we want is accepted.

In `conditions.c`, we turn the condition snippets from before into a program:

```C
// Conditions and relational operators

#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Prompt user for x
    int x = get_int("x: ");

    // Prompt user for y
    int y = get_int("y: ");

    // Compare x and y
    if (x < y)
    {
        printf("x is less than y\n");
    }
    else if (x > y)
    {
        printf("x is greater than y\n");
    }
    else
    {
        printf("x is equal to y\n");
    }
}
```

![1-14](https://doc.shiyanlou.com/courses/2618/1347963/bf35d4530c0ea343b8b13ef66b869f57-0/wm)

- Lines that start with `//` are comments, or note for humans that the compiler will ignore.

In `agree.c`, we can ask the user to confirm or deny something:

```C
// Logical operators

#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Prompt user to agree
    char c = get_char("Do you agree?\n");

    // Check whether agreed
    if (c == 'Y' || c == 'y')
    {
        printf("Agreed.\n");
    }
    else if (c == 'N' || c == 'n')
    {
        printf("Not agreed.\n");
    }
}
```

![1-15](https://doc.shiyanlou.com/courses/2618/1347963/1296e3799098b0c4d1b7478ed718a42d-0/wm)

- We use two vertical bars, `||`, to indicate a logical “or”, whether either expression can be true for the condition to be followed.
- And if none of the expressions are true, nothing will happen since our program doesn’t have a loop.

```checker
- name: check parity.c
  script: |
    #!/bin/bash
    ls /home/project/parity
  error: 运行 parity.c 发生错误
  timeout: 1
```

### for loop

Now, let’s implement the coughing program in `cough.c`.

```C
#include <stdio.h>

int main(void)
{
    printf("cough\n");
    printf("cough\n");
    printf("cough\n");
}
```

We could use a for loop:

```C
#include <stdio.h>

int main(void)
{
    for (int i = 0; i < 3; i++)
    {
        printf("cough\n");
    }
}
```

- By convention, programmers tend to start counting at 0, and so `i` will have the values of `0`, `1`, and `2` before stopping, for a total of three iterations. We could also write `for (int i = 1, i <= 3, i++)` for the same final effect.

We can move the printf line to its own function:

```C
#include <stdio.h>

void cough(void);

int main(void)
{
    for (int i = 0; i < 3; i++)
    {
        cough();
    }
}

void cough(void)
{
    printf("cough\n");
}
```

- We declared a new function with `void cough(void)`;, before our main function calls it. The C compiler reads our code from top to bottom, so we need to tell it that the `cough` function exists, before we use it. Then, after our `main` function, we can implement the `cough` function. This way, the compiler knows the function exists, and we can keep our `main` function close to the top.
- And our `cough` function doesn’t take any inputs, so we have `cough(void)`.

We can abstract cough further:

```C
#include <stdio.h>

void cough(int n);

int main(void)
{
    cough(3);
}

void cough(int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("cough\n");
    }
}
```

- Now, when we want to print “cough” any number of times, we can just call the same function. Notice that, with `void cough(int n)`, we indicate that the `cough` function takes as input an `int`, which we refer to as `n`. And inside `cough`, we use `n` in our `for` loop to print “cough” the right number of times.

![1-16](https://doc.shiyanlou.com/courses/2618/1347963/8be823ea033893dc5c7b0c78009a0fa1-0/wm)

```checker
- name: check cough.c
  script: |
    #!/bin/bash
    grep cough
  error: 运行错误
  timeout: 1
```

### positive int

Let’s look at `positive.c`:

```C
#include <cs50.h>
#include <stdio.h>

int get_positive_int(void);

int main(void)
{
    int i = get_positive_int();
    printf("%i\n", i);
}

// Prompt user for positive integer
int get_positive_int(void)
{
    int n;
    do
    {
        n = get_int("%s", "Positive Integer: ");
    }
    while (n < 1);
    return n;
}
```

- The CS50 library doesn’t have a `get_positive_int` function, but we can write one ourselves. Our function `int get_positive_int(void)` will prompt the user for an `int` and return that `int`, which our main function stores as `i`. In `get_positive_int`, we initialize a variable, `int n`, without assigning a value to it yet. Then, we have a new construct, `do ... while`, which does something first, then checks a condition, and repeats until the condition is no longer true.
- Once the loop ends because we have an `n` that is not `< 1`, we can return it with the `return` keyword. And back in our `main` function, we can set `int i` to that value.

```checker
- name: check positive.c
  script: |
    #!/bin/bash
    ls /home/project/positive
  error: 运行 positive.c 错误
  timeout: 1
```

### Screens

We might want a program that prints part of a screen from a video game like Super Mario Bros. In `mario0.c`, we have:

```C
// Prints a row of 4 question marks

#include <stdio.h>

int main(void)
{
    printf("????\n");
}
```

We can ask the user for a number of question marks, and then print them, with `mario1.c`:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int n;
    do
    {
        n = get_int("Width: ");
    }
    while (n < 1);
    for (int i = 0; i < n; i++)
    {
        printf("?");
    }
    printf("\n");
}
```

And we can print a two-dimensional set of blocks with `mario2.c`:

```C
// Prints an n-by-n grid of bricks with a loop

#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int n;
    do
    {
        n = get_int("Size: ");
    }
    while (n < 1);
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

- Notice we have two nested loops, where the outer loop uses `i` to do everything inside `n` times, and the inner loop uses `j`, a different variable, to do something `n` times for each of those times. In other words, the outer loop prints `n` “rows”, or lines, and the inner loop prints `n` “columns”, or `#` characters, in each line.

```checker
- name: check mario0.c
  script: |
    #!/bin/bash
    grep ????
  error: 运行错误
  timeout: 1
```

## Memory, imprecision, and overflow

Our computer has memory, in hardware chips called RAM, random-access memory. Our programs use that RAM to store data as they run, but that memory is finite. So with a finite number of bits, we can’t represent all possible numbers (of which there are an infinite number of). So our computer has a certain number of bits for each float and int, and has to round to the nearest decimal value at a certain point.

With `floats.c`, we can see what happens when we use floats:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Prompt user for x
    float x = get_float("x: ");

    // Prompt user for y
    float y = get_float("y: ");

    // Perform division
    printf("x / y = %.50f\n", x / y);
}
```

- With `%50f`, we can specify the number of decimal places displayed. 

```C
x: 1
y: 10
x / y = 0.10000000149011611938476562500000000000000000000000
```

- It turns out that this is called **floating-point imprecision**, where we don’t have enough bits to store all possible values, so the computer has to store the closest value it can to 1 divided by 10.

We can see a similar problem in `overflow.c`:

```C
#include <stdio.h>
#include <unistd.h>

int main(void)
{
    for (int i = 1; ; i *= 2)
    {
        printf("%i\n", i);
        sleep(1);
    }
}
```

- In our `for` loop, we set `i` to `1`, and double it with `*= 2`. (And we’ll keep doing this forever, so there’s no condition we check.)
- We also use the `sleep` function from `unistd.h` to let our program pause each time.
- Now, when we run this program, we see the number getting bigger and bigger, until:

```C
1073741824
overflow.c:6:25: runtime error: signed integer overflow: 1073741824 * 2 cannot be represented in type 'int'
-2147483648
0
0
...
```

- It turns out, our program recognized that a signed integer (an integer with a positive or negative sign) couldn’t store that next value, and printed an error. Then, since it tried to double it anyways, i became a negative number, and then 0.
- This problem is called **integer overflow**, where an integer can only be so big before it runs out of bits and “rolls over”. We can picture adding 1 to 999 in decimal. The last digit becomes 0, we carry the 1 so the next digit becomes 0, and we get 1000. But if we only had three digits, we would end up with 000 since there’s no place to put the final 1!

## Experimental summary 

The Y2K problem arose because many programs stored the calendar year with just two digits, like 98 for 1998, and 99 for 1999. But when the year 2000 approached, the programs would have stored 00, leading to confusion between the years 1900 and 2000.

A Boeing 787 airplane also had a bug where a counter in the generator overflows after a certain number of days of continuous operation, since the number of seconds it has been running could no longer be stored in that counter.

So, we’ve seen a few problems that can happen, but now understand why, and how to prevent them.

With this experiment, we’ll use the CS50 to write some programs with walkthroughs to guide us.