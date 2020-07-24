# Caesar’s cipher

## Introduction

Implement a program that encrypts messages using Caesar’s cipher, per the below.

```shell
$ ./caesar 
13
plaintext:  HELLO
ciphertext: URYYB
```

Supposedly, Caesar (yes, that Caesar) used to “encrypt” (i.e., conceal in a reversible way) confidential messages by shifting each letter therein by some number of places. For instance, he might write `A` as `B`, `B` as `C`, `C` as `D`, …, and, wrapping around alphabetically, `Z` as `A`. And so, to say `HELLO` to someone, Caesar might write `IFMMP`. Upon receiving such messages from Caesar, recipients would have to “decrypt” them by shifting letters in the opposite direction by the same number of places.

The secrecy of this “cryptosystem” relied on only Caesar and the recipients knowing a secret, the number of places by which Caesar had shifted his letters (e.g., 1). Not particularly secure by modern standards, but, hey, if you’re perhaps the first in the world to do it, pretty secure!

Unencrypted text is generally called plaintext. Encrypted text is generally called ciphertext. And the secret used is called a key.

To be clear, then, here’s how encrypting `HELLO` with a key of 1 yields `IFMMP`:

![2-10](https://doc.shiyanlou.com/courses/2618/1347963/f2be7834719558271748260bab068bdb-0/wm)

More formally, Caesar’s algorithm (i.e., cipher) encrypts messages by “rotating” each letter by k positions. More formally, if p is some plaintext (i.e., an unencrypted message), pi is the ith character in p, and k is a secret key (i.e., a non-negative integer), then each letter, ci, in the ciphertext, c, is computed as

ci = (pi + k) % 26

wherein `% 26` here means “remainder when dividing by 26.” This formula perhaps makes the cipher seem more complicated than it is, but it’s really just a concise way of expressing the algorithm precisely. Indeed, for the sake of discussion, think of A (or a) as 0, B (or b) as 1, …, H (or h) as 7, I (or i) as 8, …, and Z (or z) as 25. Suppose that Caesar just wants to say Hi to someone confidentially using, this time, a key, k, of 3. And so his plaintext, p, is Hi, in which case his plaintext’s first character, p0, is H (aka 7), and his plaintext’s second character, p1, is i (aka 8). His ciphertext’s first character, c0, is thus K, and his ciphertext’s second character, c1, is thus L. Can you see why?

Let’s write a program called `caesar` that enables you to encrypt messages using Caesar’s cipher. At the time the user executes the program, they should decide, by providing a command-line argument, on what the key should be in the secret message they’ll provide at runtime. We shouldn’t necessarily assume that the user’s key is going to be a number; though you may assume that, if it is a number, it will be a positive integer.

Here are a few examples of how the program might work. For example, if the user inputs a key of 1 and a plaintext of `HELLO`:

```shell
$ ./caesar 
1
plaintext:  HELLO
ciphertext: IFMMP
```

Here’s how the program might work if the user provides a key of 13 and a plaintext of hello, world:

```shell
$ ./caesar 
13
plaintext:  hello, world
ciphertext: uryyb, jbeyq
```

Notice that neither the comma nor the space were “shifted” by the cipher. Only rotate alphabetical characters!

This is a simple challenge, but if you want to take on a more difficult one, try it [on the website](https://cs50.harvard.edu/x/2020/psets/2/).

## goal

1. First, create a new file, it called `Caesar.c`.
2. You need to define an integer variable to count the number of letters moved.
3. You need to define two one-dimensional arrays,one for plaintext and one for ciphertext.
4. Use a `for` loop to iterate an array, then convert each letter in turn to another array.
5. Use `if...else` to determine the current letter.
6. Finally, you should output the converted string.

## reminder

- string
- array
- for loop
- if else
- command-line argument

## reference code

The following content is for reference only. In order to have a better learning effect, please try to complete the exercises according to your own ideas.

<details>
<summary>reference</summary>

```C
# include<stdio.h>
# include<string.h>

int main()
{
    int n; // 定义一个整数，用于计算字母移动的个数
    char s[100]; // 定义一个一维数组，用于存放明文
    char e[100]; // 用于存放密文
    int i = 0; // 定义一个整型变量，用于遍历数组

    scanf("%d",&n); 
    printf("plaintext:  "); 
    scanf("%s",s); // 输入明文
    
    // 遍历数组
    // 用 strlen() 来计算数组长度
    for(i; i < strlen(s); i++){
        // 如果为大写字母，大写字母对应 ASCLL 码为 65-90
        if ((int)s[i] > 64 && (int)s[i] < 91){
            e[i] = (char)((((int)s[i] - 65 + n) % 26) + 65);
        }
        // 如果为小写字母，小写字母对应 ASCLL 码为 97-122
        else if ((int)s[i] > 96 && (int)s[i] < 123){
            e[i] = (char)((((int)s[i] - 97 + n) % 26) + 97);
        }
        else{
            e[i] = s[i];
        }
    }

    printf("ciphertext:  ");
    printf("%s\n",e);
    
}

```

</details>