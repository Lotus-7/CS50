# Recover

## Introduction

In anticipation of this problem, we spent the past several days taking photos of people we know, all of which were saved on a digital camera as JPEGs on a memory card. (Okay, it’s possible we actually spent the past several days on Facebook instead.) Unfortunately, we somehow deleted them all! Thankfully, in the computer world, “deleted” tends not to mean “deleted” so much as “forgotten.” Even though the camera insists that the card is now blank, we’re pretty sure that’s not quite true. Indeed, we’re hoping (er, expecting!) you can write a program that recovers the photos for us!

Even though JPEGs are more complicated than BMPs, JPEGs have “signatures,” patterns of bytes that can distinguish them from other file formats. Specifically, the first three bytes of JPEGs are

```C
0xff 0xd8 0xff
```

from first byte to third byte, left to right. The fourth byte, meanwhile, is either `0xe0`, `0xe1`, `0xe2`, `0xe3`, `0xe4`, `0xe5`, `0xe6`, `0xe7`, `0xe8`, `0xe9`, `0xea`, `0xeb`, `0xec`, `0xed`, `0xee`, or `0xef`. Put another way, the fourth byte’s first four bits are `1110`.

Odds are, if you find this pattern of four bytes on media known to store photos (e.g., my memory card), they demarcate the start of a JPEG. To be fair, you might encounter these patterns on some disk purely by chance, so data recovery isn’t an exact science.

Fortunately, digital cameras tend to store photographs contiguously on memory cards, whereby each photo is stored immediately after the previously taken photo. Accordingly, the start of a JPEG usually demarks the end of another. However, digital cameras often initialize cards with a FAT file system whose “block size” is 512 bytes (B). The implication is that these cameras only write to those cards in units of 512 B. A photo that’s 1 MB (i.e., 1,048,576 B) thus takes up 1048576 ÷ 512 = 2048 “blocks” on a memory card. But so does a photo that’s, say, one byte smaller (i.e., 1,048,575 B)! The wasted space on disk is called “slack space.” Forensic investigators often look at slack space for remnants of suspicious data.

The implication of all these details is that you, the investigator, can probably write a program that iterates over a copy of my memory card, looking for JPEGs’ signatures. Each time you find a signature, you can open a new file for writing and start filling that file with bytes from my memory card, closing that file only once you encounter another signature. Moreover, rather than read my memory card’s bytes one at a time, you can read 512 of them at a time into a buffer for efficiency’s sake. Thanks to FAT, you can trust that JPEGs’ signatures will be “block-aligned.” That is, you need only look for those signatures in a block’s first four bytes.

Realize, of course, that JPEGs can span contiguous blocks. Otherwise, no JPEG could be larger than 512 B. But the last byte of a JPEG might not fall at the very end of a block. Recall the possibility of slack space. But not to worry. Because this memory card was brand-new when I started snapping photos, odds are it’d been “zeroed” (i.e., filled with 0s) by the manufacturer, in which case any slack space will be filled with 0s. It’s okay if those trailing 0s end up in the JPEGs you recover; they should still be viewable.

Now, I only have one memory card, but there are a lot of you! And so I’ve gone ahead and created a “forensic image” of the card, storing its contents, byte after byte, in a file called `card.raw`. So that you don’t waste time iterating over millions of 0s unnecessarily, I’ve only imaged the first few megabytes of the memory card. But you should ultimately find that the image contains 50 JPEGs.

Implement a program that recovers JPEGs from a forensic image, per the below.

```shell
$ ./recover card.raw
```

![4-30](https://doc.shiyanlou.com/courses/2618/1347963/487d0bb38f11cfd060ac5bb5aae750d1-0/wm)

Only part of the result is shown in the figure above, and we will recover 50 images(000.jpg-049.jpg) after running the code.

This is a simple challenge, but if you want to take on a more difficult one, try it [on the website](https://cs50.harvard.edu/x/2020/psets/4/). 

## goal

1. Execute `wget https://cdn.cs50.net/2019/fall/psets/4/recover/recover.zip` to download a (compressed) ZIP file with this problem’s distribution.
2. Execute `unzip recover.zip` to uncompress that file.
3. Execute `cd recover` to change into that directory.
4. Execute `ls`. You should see this problem’s distribution, including `card.raw` and `recover.c`.
5. Implement your program in a file called `recover.c` in a directory called `recover`.
6. Your program should accept exactly one command-line argument, the name of a forensic image from which to recover JPEGs.
7. If your program is not executed with exactly one command-line argument, it should remind the user of correct usage, and main should return 1.
8. If the forensic image cannot be opened for reading, your program should inform the user as much, and main should return 1.
9. Your program, if it uses malloc, must not leak any memory.

## Hints

Keep in mind that you can open `card.raw` programmatically with `fopen`, as with the below, provided `argv[1]` exists.

```C
FILE *file = fopen(argv[1], "r");
```

When executed, your program should recover every one of the JPEGs from `card.raw`, storing each as a separate file in your current working directory. Your program should number the files it outputs by naming each `###.jpg`, where `###` is three-digit decimal number from `000` on up. (Befriend `sprintf`.) You need not try to recover the JPEGs’ original names. To check whether the JPEGs your program spit out are correct, simply double-click and take a look! If each photo appears intact, your operation was likely a success!

If you’d like to create a new type to store a byte of data, you can do so via the below, which defines a new type called `BYTE` to be a `uint8_t` (a type defined in `stdint.h`, representing an 8-bit unsigned integer).

```C
typedef uint8_t BYTE;
```

Keep in mind, too, that you can read data from a file using `fread`, which will read data from a file into a location in memory and return the number of items successfully read from the file.

## reference code

The following content is for reference only. In order to have a better learning effect, please try to complete the exercises according to your own ideas.

<details>
<summary>reference</summary>

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <stdint.h>

typedef uint8_t  BYTE;
typedef uint32_t DWORD;
typedef int32_t  LONG;
typedef uint16_t WORD;


/**
 * JPG start
 */
typedef struct
{
BYTE first;
BYTE second;
BYTE third;
BYTE fourth;
} __attribute__((__packed__))
JPGheader;


int main(int argc, char *argv[])
{
// ensure proper usage
if (argc != 2)
{
    fprintf(stderr, "Usage: ./recover raw_file\n");
    return 1;
}

// remember filenames
char *infile = argv[1];
//printf("%s\n", infile);

// open input file 
FILE *inptr = fopen(infile, "r");
if (inptr == NULL)
{
    fprintf(stderr, "Could not open %s.\n", infile);
    return 2;
}

JPGheader jpg;
jpg.first = 0xff;
jpg.second = 0xd8;
jpg.third = 0xff;
jpg.fourth = 0xe0;

int jpg_fourth_top = (jpg.fourth >> 4) & ((1 << 4) - 1);
//printf("original: %i%i%i%i  %i\n", jpg.first, jpg.second, jpg.third, jpg.fourth, jpg_fourth_top);

int jpg_order = 0;
//char *jpg_ext = ".jpg";
char *BLOCK = malloc(sizeof(BYTE)*512);
bool JPG = false;
FILE *outptr;

//while (jpg_order<49){
while (jpg_order<51){

    JPGheader raw_read;
    fread(&raw_read, sizeof(JPGheader), 1, inptr); 
    //printf("%i%i%i%i\n", raw_read.first, raw_read.second, raw_read.third, raw_read.fourth);
    int raw_read_fourth_top = (raw_read.fourth >> 4) & ((1 << 4) - 1);

    fseek(inptr, -(sizeof(JPGheader)), SEEK_CUR);

    if(jpg.first == raw_read.first && jpg.second == raw_read.second && jpg.third == raw_read.third && jpg_fourth_top == raw_read_fourth_top){
        if (JPG){
            fclose(outptr);
            jpg_order++;
            JPG = false;
        }

        // open output file
        char fileSpec[10];
        sprintf(fileSpec, "%03i.jpg" ,jpg_order );
        printf("%s found!\n", fileSpec);

        outptr = fopen(fileSpec, "w"); //FILE *
        if (outptr == NULL){
            fclose(inptr);
            fprintf(stderr, "Could not create %s.\n", fileSpec);
            return 3;
        }
        JPG = true;

    }

    if (JPG){
        fread(&BLOCK, sizeof(BLOCK), 1, inptr);
        fwrite(&BLOCK, sizeof(BLOCK), 1, outptr);
    }
    else
        fseek(inptr, sizeof(BLOCK), SEEK_CUR);

    if(feof(inptr)){
        break;
    }
}

fclose(outptr);
fclose(inptr);

// success
return 0;
}
```

</details>