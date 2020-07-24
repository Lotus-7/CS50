# Mario for Python

## Introduction

Implement a program that prints out a half-pyramid of a specified height, per the below.

```Python
$ python mario.py
Height: 4
   #
  ##
 ###
####
```

No `check50` for this problem, but be sure to test your code for each of the following.

- Run your program as `python mario.py` and wait for a prompt for input. If you input is not a positive integer or integer greater than 8, your program will print `please give input between 1 and 8`.
- Run your program as `python mario.py` and wait for a prompt for input. Type in `1` and press enter. Your program should generate the below output. Be sure that the pyramid is aligned to the bottom-left corner of your terminal, and that there are no extra spaces at the end of each line.

```shell
#
```

- Run your program as `python mario.py` and wait for a prompt for input. Type in `2` and press enter. Your program should generate the below output. Be sure that the pyramid is aligned to the bottom-left corner of your terminal, and that there are no extra spaces at the end of each line.

```shell
 #
##
```

- Run your program as `python mario.py` and wait for a prompt for input. Type in `8` and press enter. Your program should generate the below output. Be sure that the pyramid is aligned to the bottom-left corner of your terminal, and that there are no extra spaces at the end of each line.

```shell
       #
      ##
     ###
    ####
   #####
  ######
 #######
########
```

This is a simple challenge, but if you want to take on a more difficult one, try it [on the website](https://cs50.harvard.edu/x/2020/psets/6/)

## goal

- Write, in a file called `mario.py` in `/home/project`, a program that recreates the half-pyramid using hashes (#) for blocks.
- To make things more interesting, first prompt the user with `input` for the half-pyramid’s height, a positive integer between `1` and `8`, inclusive.
- If the user fails to provide a positive integer no greater than `8`, you should re-prompt for the same again.
- Then, generate (with the help of `print` and one or more loops) the desired half-pyramid.
- Take care to align the bottom-left corner of your half-pyramid with the left-hand edge of your terminal window.

## reminder

- input
- for loop
- if...else

## reference code

The following content is for reference only. In order to have a better learning effect, please try to complete the exercises according to your own ideas.

<details>
<summary>reference</summary>

```Python
height = int(input("Height: "))


if height > 0 and height < 9:
    for i in range(height):
        print(" "*(height-i)+"#"*(i+1))
else:
    print("please give input between 1 and 8")
```

</details>