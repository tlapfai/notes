# IPC144

Gayan Gamage 佳仁

- **Passing requirement**  
  Overall score > 50  
  Midterm+final > 25 score

- Workshop (8)  
  part 1 due 1 day Thursday 23:59  
  Part 2 due 5 days Monday midnight (reflect txt)

- Part-1 is due 1-day after your scheduled LAB class by the end of day 23:59 EST (UTC – 5)
- Part-2 is due 5-days after your scheduled LAB class by the end of day 23:59 EST (UTC – 5)

- Assignments (1)
- Milestone 1 build up some functions
- Milestone 2 add others
- Milestone 3

- Quizzes  
  Every week on lab day (except 1st week) on computer

- FileZilla client
- Upload c file to matrix, then compile c file in Linux

- Homework

---

Use **camel style** variable name  
int 4 bytes

- display format in printf

| Format   | Description                                     |
| -------- | ----------------------------------------------- |
| %10.3lf  | (reserve 10 spaces and align right)             |
| %010.3lf | (fill up the empty char with 0 and align right) |
| %-10.3lf | (reserve 10 spaces and align left)              |

char type use single quote, e.g. `myVar = 'A';`

Logical Operator

| Operator | Description |
| -------- | ----------- |
| &&       | and         |
| \|\|     | or          |
| !        | not         |

```c
scan("%c%*c", &variableName);
scan(" %c", &variableName);
```

Prompting user-input for a single-character value can cause unexpected behaviour.  
Use the following scanf formatting specifier (between the double-quotes) to avoid strange behaviour (notably the single-space before the percent sign)

```c
char fname[25] = {'T', 'O', 'M', '\0'}; // 它是 C-style string
char lname[25] = "TOM"; // 自動在尾加\0 字符
```

printf("Name is %s\n", fname);

用 string 在 scanf 不需要&

```c
char name[SIZE];
scanf("%s", name);
printf("Your name is %d\n", name);
```

注意：scanf 只 scan 到空格，例如"Tom Hanks"只會到 Tom

解決方法：

```c
scanf(" %20[^\n]%*c", name);
// scan 20 characters until \n 可以包含有空格的字串
// %*c 代表 scan 一個字元但不存起來
// 最開頭的空格是為了吃掉前面的空格
// 最多 scan 20 characters
```

## getchar() and putchar()

```c
// clear empties the input buffer
void clear(void)
{
    while (getchar() != '\n')
    {
        ;  // empty statement intentional
    }
    // while(getchar() != '\n'); // 也可以
}
```

getchar()也可以用作 pause，例子

```c
printf("Press Enter to continue...");
while(getchar() != '\n')
{
	;
}
```

# 隨機變數

```c
#include <stdlib.h>
rand();
RAND_MAX // 32767
```

```c
srand(1234); // 放入 seed
srand(time(0)); // 可以用 time() 作為 seed
```

## ctype.h

```c
#include <ctype.h>
isalpha();
isdigit();
isalnum();
islower();
isupper();
tolower();
toupper();
```

## Two dimensional arrays

```c
int a[3][4] = {
	{1, 2, 3, 4},
	{5, 6, 7, 8},
	{9, 10, 11, 12}
};

char names[2][21] = {
	"Tom Hanks",
	"Brad Pitt"
};

// to print "m"
printf("%c\n", names[0][2]);

// to print "Tom Hanks"
printf("%s\n", names[0]);
```

# Working with files

## File Handling

- In MS Visual Studio, put it in "Resource Files" folder in the project

- `FILE*` is the pointer to the file

| Mode | Description                                         |
| ---- | --------------------------------------------------- |
| r    | read                                                |
| w    | write                                               |
| a    | append                                              |
| r+   | read and write                                      |
| w+   | write and read                                      |
| a+   | append and read, it creates a file if no file exist |

```c
FILE* fp = NULL;
int x = 5;

// write file
fp = fopen("file.txt", "w");  // fopen return NULL if file not found, returns file pointer if found
if(fp != NULL)
{
	fprintf(fp, "Data is %d\n", x); // 寫入 file"
	fclose(fp);
}
else
{
	printf("File not found\n");
}

// read file
char str[21];
fp = fopen("file.txt", "r");
if(fp != NULL)
{
	fscanf(fp, "%20[^\n]%*c", str); // 讀取 file"
	puts(str);  // Data is 5
	fclose(fp);
}
else
{
	printf("File not found\n");
}
```

### EOF

EOF is a macro defined in stdio.h.  
It is used to indicate the end of a file.

```c
char str[21];
while(fscanf(fp, "%20[^\n]\n", str) != EOF)
// scan 20 characters until \n
{
	printf("%s\n", str);
}
```

### fscanf

fscanf is used to read formatted input from a file.  
The return value of fscanf is the number of successful conversions.

```c
FILE* fp = NULL;
fp = fopen("file.txt", "r");
int x;
int y = fscanf(fp, "%d", &x);
printf("Number of successful conversions: %d\n", y); // 1
```

When user's input has only one character, the return value of `fscanf` is 1.

Usage:

```c
FILE* fp = NULL;
fp = fopen("file.txt", "r");
while (fscanf(fp, "%20[^\n]\n", str) == 1))
// %20[^\n] scan 20 characters until \n
// and expect \n at the end
// use this instead of %s because %s stops at space
{
	printf("%s\n", str);
}
```

### Reading multiple data

When data is sepearated by comma or colon, use the following code:

`file.txt`:

```txt
Tom Hanks:1.1,180
Brad Pitt:2.2,170
```

```c
char str[21];
double gpa;
int height;
while (fscanf(fp, "%20[^:]:%lf,%d", str, &gpa, &height) == 3)
// %20[^:] is used for scaning 20 characters until :
{
	printf("%s's GPA is %lf and their height is %d\n", str, gpa, height);
}
```

### Rewind File Pointer

`rewind` is a function that moves the file pointer to the beginning of the file.

```c
int x = 5;
fprintf(fp, "Data is %d\n", x);
fprintf(fp, "Data is %d\n", x);
rewind(fp);  // pointer to the beginning of the file
scanf(fp, "%20[^\n]%*c", str); // read the first line
puts(str); // Data is 5
```

### fgetc

Get a character from a file

```c
char ch;
while ((ch = fgetc(fp)) != EOF)  // file pointer moves to the next character
{
	putchar(ch);
}
```

### feof

`feof` is a function that returns a non-zero value if the end of the file has been reached.

```c
do {
	ch = fgetc(fp);
	if (!feof(fp))
	{
		putchar(ch);
	}
} while (!feof(fp));
```

## Portability

1. K&R C (Kernighan and Ritchie)
2. C89 (ANSI C)
3. C99
4. C11

# Algorithms

## Sorting

### Selection Sort

Find the smallest element and swap it with the first element.

### Bubble Sort

Compare adjacent elements and swap them if they are in the wrong order.

## Searching

### Binary Search

1. Sort the array
2. Find the middle element
3. Compare the middle element with the target
4. If the target is greater than the middle element, search the right half
5. If the target is less than the middle element, search the left half
6. Repeat the process until the target is found
