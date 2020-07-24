# Mario

## Introduction

Let’s recreate that pyramid in C, albeit in text, using hashes (`#`) for bricks, a la the below. Each hash is a bit taller than it is wide, so the pyramid itself is also be taller than it is wide.

The program we’ll write will be called `mario`. And let’s allow the user to decide just how tall the pyramid should be by first prompting them for a positive integer between, say, 1 and 8, inclusive.

Here’s how the program might work if the user inputs 8 when prompted:

```bash
./mario
Height: 8
#
##
###
####
#####
######
#######
########
```

Here’s how the program might work if the user inputs 4 when prompted:

```bash
$ ./mario
Height: 4
#
##
###
####
```

Here’s how the program might work if the user inputs 2 when prompted:

```bash
$ ./mario
Height: 2
#
##
```

And here’s how the program might work if the user inputs 1 when prompted:

```bash
$ ./mario
Height: 1
#
```

This is a simple challenge, but if you want to take on a more difficult one, try it [on the website](https://cs50.harvard.edu/x/2020/psets/1/).

## goal

1. First, create a new file, it called `mario.c`.
2. use `scanf` to get a integer.
3. Keep in mind that a hash is just a character like any other, so you can print it with `printf`.
4. You can actually “nest” loops, iterating with one variable (e.g., `i`) in the “outer” loop and another (e.g., `j`) in the “inner” loop. For instance, here’s how you might print a square of height and width `n`, below. Of course, it’s not a square that you want to print!

## reminder

- scanf
- printf
- for loop
- positive integer

## reference code

The following content is for reference only. In order to have a better learning effect, please try to complete the exercises according to your own ideas.

<details>
<summary>reference</summary>

```C
# include <stdio.h>

void main(){
    int n;
    printf("Height:");
    scanf("%d",&n);
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < i+1; j++)
        {
            printf("#");
        }
    printf("\n");
    }
}
```

</details>