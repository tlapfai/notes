OPS102

Saso Kocev  
Monday 17:10, Wednesday 09:50

# Basic Commands

## 日期 (cal, date)

`cal 年 月` 顯示月曆

例：

```bash
$ cal 2024 06
```

結果如下：

```bash
     June 2024
Su Mo Tu We Th Fr Sa
                   1
 2  3  4  5  6  7  8
 9 10 11 12 13 14 15
16 17 18 19 20 21 22
23 24 25 26 27 28 29
30
```

`date` 顯示日期和時間

例：

```bash
$ date
```

結果如下：

```bash
Mon Mar  8 17:10:00 EST 2021
```

## List (`ls`)

```bash
ls -l # -l=long view
ls -a # = all  -a -l OR -al OR -la 效果相同
ls {file_name} # 只印出完全符合檔名的檔案
ls {file_name}* # 用wildcard character for any number of characters
ls {file_name}? # match one character
ls [AbC] # match any one inside the bracket
ls [C-Y] # provide a range of alphabets，通常是a-z
ls [0-9]
ls [a-zA-Z0-9]
ls [!ABC] # exclude A B or C
```

## Manual (man)

user manual

```sh
man 指令名稱
```

```bash
who # to show who logged in
w # like who with more information
```

Result: WHAT -bash means not running any program

## Text Editors

| Text Editor      | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| nano             | (text editor) exit -> ctrl+X                                  |
| pico             | old version of nano                                           |
| cat              | `cat -n file_name` 印出內容 (-n shows line number)            |
| `more file_name` | (to show the content page by page, press space for next page) |
| `less file_name` | (better than more, able to scroll up and down)                |
| `vi file_name`   |                                                               |
| `vim file_name`  |                                                               |

## File Operations

| Command                           | Description                            |
| --------------------------------- | -------------------------------------- |
| `cp source_file destination_file` | (copy file)                            |
| `cp -r`                           | copy recursively a nonempty directory  |
| `mv source_file destination_file` | move file                              |
| `tree -C`                         | draw directory tree with colour        |
| `mkdir 目錄名`                    |
| `rmdir 目錄名`                    | remove empty directory                 |
| `rm -r 目錄名`                    | remove nonempty directory, r=recursive |
| `pwd`                             | 查詢 present working directory         |

use slash to access absolute path, e.g. cd /home/users/

## `find` 用檔名尋找目錄下的檔案(包含子目錄)

檔名要符合 regular expression

```bash
$ find 路徑 - name "要尋找的檔名"
$ find 路徑 - iname "要尋找的檔名" # case insensitive
```

例：

```bash
$ find /home -name "file.txt"
```

## 印出在 CLI 或在檔案

```bash
echo "something I want to print"
echo "something I want to save" > file.txt # overwrite the whole file
echo "another thing I want to save" >> file.txt # append in the file bottom
```

用`2>`把錯誤訊息存到檔案  
用`2>>`把錯誤訊息附加到檔案底部

```bash
diff -y file1.txt file2.txt # compare files
```

## `grep`

```bash
grep "mark" data.txt
```

- 在 data.txt 尋找 mark，印出有該文字的行
- 參數 -i 代表不分大小寫
- quotes are optional when searching for a single word

例：

```bash
grep -i "mark" data.txt
```

## `cat`

- 印出檔案內容
- -n 參數印出行號

```sh
$ cat data.txt
```

## head 和 tail

```sh
$ head -5 data.txt # 印出前 5 行
$ tail -5 data.txt # 印出後 5 行
```

## pipe

- 用`|`(pipe)連接兩個指令，把第一個指令的結果送給第二個指令

例：

```sh
grep "mark" data.txt | wc -l
```

# 文字處理

## `wc`

- word count

例：

**`data.txt`:**

```txt
toyota, 20000
honda, 22000
nissan, 25000
```

```sh
$ wc data.txt
```

- 結果如下：3 個數字，分別代表 lines、words、characters
- q: 為什麼是 41 個字元？ a: 有 3 個換行符號

```sh
3 6 41 data.txt
```

- 參數：`-l` (這是小寫 L) 代表 count line

```sh
$ wc -l data.txt
```

結果如下：

```sh
3 data.txt
```

## `tr` 代表 translate

- 用來取代文字 (不會儲存到檔案)
- 文字檔並不是參數，而是輸入

例 1：

```bash
tr "a" "W" < data.txt # 把 data.txt 代為輸入
```

例 2：

- 用 piping 方式，單引號或雙引號都可以

```bash
$ cat data.txt | tr 'a' 'W'
```

例 3：

```bash
$ tr "a" "W" < input.txt > replaced.txt # 輸出到 replaced.txt
$ tr -s " " ";" # -s 代表 squeeze，把連續的空格轉成分號
```

## 印出 CSV 文字檔的某部份

- 用`cut`指令

```bash
$ cut -d"," -f1 data.txt # -d=deliminator, -f1=第一欄
$ cut -d"," -f1,2 data.txt # -f1,2=第一二欄
```

註：用 piping 也可，例

```bash
$ cat data.txt | cut -d"," -f1,2
```

用分號取代連續空格(-s=squeeze)，-f5,9 印出第 5 和第 9 欄

```bash
$ ls -lh | tr -s " " ";" | cut -d";" -f5,9
```

## `head` 和 `tail`

```bash
$ head -5 data.txt # 相當於 cat 指令，但只印首 5 列
$ tail -5 data.txt # 印尾 5 列
$ tail -f data.txt # -f 印出最後10行，並保持在檔案結尾處(持續更新)
```

- 用`&`放在背景執行

```bash
$ tail -f data.txt &
[2] 13191
```

- 然後用`ps -x`列出

```bash
$ ps -x
  PID TTY      STAT   TIME COMMAND
 9277 pts/0    S      0:00 -sh
13191 pts/0    S      0:00 tail -f data.txt
```

## `jobs` 指令

- see by jobs 每個 job 有一個數字

```sh
$ jobs
[1]+  Running                 tail -f data.txt &
```

- 用`fg 數字`把 job 帶到前景(foreground)
- 用 Ctrl+Z 把程式放到背景

```bash
$ fg 1
```

## `sort`指令

- 排序文字檔
- 可配合 `>` 使用，例：`sort names.txt > sorted.txt`

| 參數 | 意義                                  |
| ---- | ------------------------------------- |
| -f   | 不分大小寫                            |
| -n   | 數字排序                              |
| -t   | 指定分隔符號，例如`-t","`             |
| -k   | 指定排序欄位，配合`-t`使用，例如`-k2` |
| -r   | 反向排序                              |
| -R   | 隨機排序                              |
| -h   | 人類辨識格式，配合`-n`使用            |

```sh
$ sort numbers.txt # 文字形式排序
$ sort -n numbers.txt # 數字形式排序
$ sort -f names.txt # 用-f 不分大小寫
$ sort -t"," -k2 -f data.txt # 用某一欄排序，-k 接數字代表排序該欄 key(field)
$ sort -t"," -n -h file.txt # -h 人類辯認格式，可以排序 K, M, G 等單位，配合-n 使用
$ sort -r # reverse order
$ sort -R # random order
```

## `uniq` 指令

- 移除**連續重複**的行
- `-i` 不分大小寫

```bash
$ uniq -i data.txt
```

- 用 pipe 把資料傳入，例 `sort ... | uniq -i`

例：

```bash
$ sort data.txt | uniq -i
```

# Permission

## Symbolic Permission

r=read  
w=write  
x=execute  
-rwx rwx rwx

|      | user | group | others |
| ---- | ---- | ----- | ------ |
| 代號 | u    | g     | o      |

```sh
$ chmod -w file.txt # 移除三組人的 w 權限
$ chmod +w file.txt # 為三組人加w權限
```

- 程式檔不需要 rw 權限，只需要 x 權限
- 第一個 dash 代表是一個 file，如果是 d 代表是 directory

```bash
$ chmod g-r file.txt # 移除 group(g)的權限
$ chmod g+r,o+w file.txt # 注意：逗號後沒有空格
$ chmod u=rwx,g=,o=r file.txt # 等號後為空，沒有任何權限
$ chmod ugo+rwx file.txt # 注意：ugo 順序固定
$ chmod a+rwx file.txt # a 代表全部人員
$ chmod u=w file.txt # 重設 user 權限，然後設成只有 w (set to w)
```

| 符號 | 意義   |
| ---- | ------ |
| +    | add    |
| -    | remove |
| =    | set    |

## Octal Permission

```sh
$ chmod 754 file.txt
```

r -> 4  
w -> 2  
x -> 1

```sh
$ chmod 17 file.txt # 考試要求寫齊3個數字，但 Linux 接受 1 個或 2 個數字
```

### umask (user file-creation mode)

- umask is a Linux command that lets you set up default permissions for newly created files and folders.

例：

```sh
$ umask 022
```

| 十進制 | 0   | 2   | 2   |
| ------ | --- | --- | --- |
| 二進制 | 000 | 010 | 010 |
| 反向   | 111 | 101 | 101 |
|        | rw- | r-- | r-- |

- 把新增的檔案設成反向後的權限
- 例：`touch file.txt`，權限即為 rw- r-- r--  
  因為它不是執行檔，所以自動略過 x

## Disk

### `du`

```sh
$ du # 顯示資料夾內的檔案大小 estimate file space usage
```

例：

```sh
anthony@TAM_HOME:~/a$ du
8       ./b
8       ./@eaDir
1556    .
```

用`-h`參數，可以顯示人類可讀的格式

```sh
$ du -h # size represented in human readable format
```

例：

```sh
anthony@TAM_HOME:~/a$ du -h
8.0K    ./b
8.0K    ./@eaDir
1.6M    .
```

### `df`

`df` (disk free) report file system disk space usage

例：

```sh
anthony@TAM_HOME:~/a$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sda1       10268416  366048   9891392   4% /
```

`-h`參數，可以顯示人類可讀的格式

## 查看 process

```bash
ps -x # 查看運行中的 process，-x 包含其他 session
ps -afux
# -u show username
# -a all with TTY
# -f full format
ps -u 使用者 # 只查看某一使用者
kill {process-id} # 關閉 process，發出SIGTERM
kill -9 {process-id} # 強制關閉 process -9，發出SIGTKILL
```

# alias

- 設定別名，只對現時的 session 有效

```sh
alias list='ls -alh' # 用alias代表'ls -alh'
```

- 取消 alias

```sh
unalias list
```

## 每次登入時自動設定 alias

`~/.bash_profile`

當登入時會執行一次這個檔案的指令

在`~/.bash_profile` 最底加入`alias list='ls -alh'`，即可每次登入都執行一次

# 查看歷史指令

## `history`

- Print out command history

```sh
$ history | tail -20  # 用 pipe 印出最後20列的歷史指令
```

## `fc -l`

- list command history

```sh
$ fc -l
655      kill -9 135359
656      ps -u lftam2
657      ps -l -u lftam2
658      jobs
659      ps -l -u lftam2
$ !658 # 執行代號為658的歷史指令
jobs
```

指令儲存在~/.bash_history

# Links

## Hard Links

- 只能對 file 有效，不能對 directory

- `ln` stands for link

```sh
ln source_file destination_file
```

- 用`ls -li`查檔案的 inode  
  參數`i`代表 inode

`ls`結果顯示 source_file 和 destination_file 指向相同的 inode

```sh
$ ls -li
total 0
1234567 -rw-r--r-- 1 john users 0 Mar 11 16:59 destination_file
1234567 -rw-r--r-- 1 john users 0 Mar 11 16:59 source_file  # 這兩個檔案的 inode 相同
```

`rm destination_file`不會刪除 source_file

## Soft Links / Symbolic Links

- Soft link 用`ln -s`參數  
  與 Windows 捷徑相同  
  Soft link 可用於 directory  
  捷徑有自己的檔案位置和大小

```sh
$ ln -s file1 file2 # file2指向file1
lrwxrwxrwx 1 john users  5 Mar 11 17:00 file2 -> file1  # 最頭的l代表link
```

```bash
$ ln -s /root/file3 file4 # file4指向絕對路徑
lrwxrwxrwx 1 john users  5 Mar 11 17:01 file4 -> /root/file3
```

# Script

用途：自動化工作

## 變數

- Variable name is case sensitive
- Variable name cannot start with number
- 變數名稱開頭不用加`$`，但是在使用時要加$，例：`$school`

```sh
$ school=Seneca  # assign Seneca to 變數school 不能有空格
$ echo $school  # $代表是variable
Seneca
$ location="Greater Toronto Area"  # use quotes to include words with space in between
$ price="This watch costs \$1000" # 用\來escape特殊符號
$ echo "My school is $school"  # use $ to call variable
My school is Seneca
```

取消變數：

```sh
$ unset school
```

或

```sh
$ school=
```

- 用單引號不會顯示變數值

```
$ echo 'My school is $school'
My school is $school
```

在`~/.bash_profile`最底加入`school=Seneca`，即可每次登入都執行一次

通常用.sh 作為 script 的副檔名

**`script.sh`:**

```bash
#!/bin/bash
# This is a comment
echo "Hello World"
```

- 第一行叫做 **shebang line**, to indicate the script is a bash script  
  這行不一定要，但是加上可以確保 script 會用 bash 執行 (考試要求要加)  
  `#!`和`/bin/bash`之間不能有空格
- 在第二行開始用#，則是註解
- `/usr/bin/bash` is the location of bash  
  因為`/bin`是指向`/usr/bin`的軟連結，所以`/bin/bash`和`/usr/bin/bash`是一樣的

### 在執行前要先給予執行權限

```bash
$ chmod +x script.sh  # make the script executable
$ ./script.sh  # run the script
Hello world
```

用分號連接兩個指令

```bash
echo "Hello world"; echo "Hello world"
```

## read

- 用途：讓使用者輸入資料

```sh
read  # waiting for user's ENTER key
read colour  # prompt user to input, and store the input in variable colour
```

- 變數在.sh 以外就會失效

- `echo -n` 代表印出後不換行

```bash
echo -n "Enter your name: " # -n 代表不換行
read name
echo # echo 一行空白，即換行
echo "Hello $name"
```

以上的程式碼會等待使用者輸入名字，然後印出 Hello \{name\}

```bash
read -p "Enter your name: " name
# prompt user to input, and store the input in variable name (name前不用$)
echo "Hello $name"
```

以上的程式碼會等待使用者輸入名字，然後印出 Hello \{name\}  
`-p` 代表 prompt

---

## argument

- 在運行.sh 後加入參數(arguments)，script 會把參數順序放入 script 內的變數，例：

**`script.sh`:**

```bash
#!/bin/bash
echo "My first argument is $1"
echo "My second argument is $2"
```

試執行：

```
./script.sh John Peter
```

結果如下：

```
My first argument is John
My second argument is Peter
```

- 可以用`{}`包住變數，注意`$1`和`$10`  
  如果直接用`$10`，會被解讀成`$1`後面接`0`，所以要用`${10}`來表示

```bash
#!/bin/bash
echo "My first argument is $1"
echo "My tenth argument is ${10}"
```

- `$#` 代表參數的數量

```sh
#!/bin/bash
echo "The number of arguments is $#"
```

## if statement

- 用`if`, `then`和`fi`來包住條件式
- `[]` 內的兩側空格是必要的
- `=` 旁的空格也是必要的
- `else`和`elif`是可選的

**`try_if.sh`:**

```bash
#!/bin/bash
echo "The total number of arguments is $#"
if [ $# = 1 ]
then
	echo "You have entered ONE argument"
else
	echo "You have entered $# arguments"
fi
```

試執行：

```sh
./try_if.sh Peter Sam
```

結果如下：

```sh
The total number of arguments is 2
You have entered 2 arguments
```

## 比較運算子

- 有些 bash version 不支援`>`和`<`，所以要用`-gt`和`-lt`

| 運算子 | 意義                  |
| ------ | --------------------- |
| -eq    | equal                 |
| -ne    | not equal             |
| -lt    | less than             |
| -le    | less than or equal    |
| -gt    | greater than          |
| -ge    | greater than or equal |

## Exit Code

- 執行 bash script 或其他指令，會回傳一個數字，代表 script 的執行結果
- 0 代表成功，其他數字代表失敗
- Exit code 不是 error code，而是一個數字

```bash
#!/bin/bash
echo -n "Is everything alright? (y/n) "
read reponse
if [ $response = "y" ]
then
	echo "Good to hear!"
	exit 0
else
	echo "Something is wrong"
	exit 1
fi
```

- 在執行 script 後，可以用`echo $?`查看 exit code

```
./script.sh
echo $?
```

結果如下：

```
$ Is everything alright? (y/n) y
Good to hear!
$ echo $?
0
```

### 範例：自製有 error message 的 grep

**`script.sh`:**

```bash
#!/bin/bash
grep -i "$1" $2"
if [ $? != 0 ]
then
	echo "The word $1 is not found in the file $2"
	exit 99
fi
```

`!=` 代表 not equal，也可以用`-ne`

試執行：

```
./script.sh "hello" "file.txt"
The word hello is not found in the file file.txt
```

### 檢查檔案是否存在和可讀

| 代號 | 意義                                    |
| ---- | --------------------------------------- |
| -f   | file exists                             |
| -d   | directory exists                        |
| -e   | file or directory exists                |
| -s   | file exists and not empty (s 代表 size) |
| -r   | file exists and readable                |

```bash
#!/bin/bash
if [ -f $1 ]
then
	echo "The file $1 exists"
else
	echo "The file $1 does not exist"
    touch $1
    echo "The file $1 has been created"
fi
```

### 在 script 裡執行指令

用`$()`來執行指令

```bash
#!/bin/bash
echo "The current date and time is $(date)"
```

### 在 script 裡運算

用雙括號`$(( ))`來運算

```bash
#!/bin/bash
result=$(($1 + $2))
echo "The result is $result"
```

試執行：

```sh
$ ./script.sh 5 3
The result is 8
```

另一例子﹐運算不會跳出錯誤訊息，直接忽視：

```sh
$ ./script.sh 5 ops
The result is 5
```

## Loop

### for loop

用`do`和`done`來包住迴圈內的指令

```bash
#!/bin/bash
echo "The loop is now starting"
for i in 1 2 3 4 5
do
	echo "The number is $i"
done
```

試執行：

```
$ ./script.sh
The loop is now starting
The number is 1
The number is 2
The number is 3
The number is 4
The number is 5
```

另一例子，用`$@`來代表所有參數  
grab array argument individually

```bash
#!/bin/bash
for animals in $@
do
	echo "The animal is $animals"
    read
done
```

試執行：

```
$ ./script.sh cat dog bird
The animal is cat
The animal is dog
The animal is bird
```

- `$*`和`$@`一樣，代表所有參數

- 但在`for`用引號包住時效果不同  
  `"$@"`，與上方例子相同  
  `"$*"`則會把所有參數當成一個字串(grab all arguments as one item)，參考下方例子

```bash
#!/bin/bash
for animals in "$*"
do
	echo "The animal is $animals"
	read
done
```

試執行：

```
$ ./script.sh cat dog bird
The animal is cat dog bird
```

另一例子：

```bash
#!/bin/bash
echo "Loop is now starting"
for animals in $@
do
    echo "-----------------------------------------------"
    echo "The current value for 'animals' is $animals"
    everythingSoFar=$everythingSoFar" "$animals
    echo "So far we have: $everythingSoFar"
    echo "-----------------------------------------------"
    read
done
echo "Exiting Loop"
```

試執行：

```
$ ./script.sh cat dog bird
Loop is now starting
-----------------------------------------------
The current value for 'animals' is cat
So far we have: cat
-----------------------------------------------
-----------------------------------------------
The current value for 'animals' is dog
So far we have: cat dog
-----------------------------------------------
-----------------------------------------------
The current value for 'animals' is bird
So far we have: cat dog bird
-----------------------------------------------
Exiting Loop
```

## While loop

```bash
#!/bin/bash
echo "The loop is now starting"
counter=1
while [ $counter -lt 10 ]
do
	echo "The number is $counter"
	$counter=$(( $counter + 1 ))
done
```

## 例子：猜數字遊戲

```bash
#!/bin/bash
# run ./guess.sh 1 100
low=$1
high=$2
range=$(( $high - $low + 1 ))
secretNumber=$(( $low + RANDOM%range ))
counter=1
while true
do
    echo "Enter a number between $low and $high: "
    read guess
    if [ $guess -eq $secretNumber ]
	then
        echo "------------------------"
		echo "Congratulations!"
        echo "You used $counter tries"
        echo "------------------------"
		exit 0
	elif [ $guess -lt $secretNumber ]
	then
		echo "Too low, try guess a higher number"
	else
		echo "Too high, try guess a lower number"
	fi
    counter=$(( $counter + 1 ))
done
```

如果要使用者輸入`1-100`，可以用`cut`，修改如下：  
`echo`被包在`$()`裡，可以直接執行指令，但不會印出結果

```bash
low=$(echo $1 | cut -d"-" -f1)
high=$(echo $1 | cut -d"-" -f2)
```

### `awk`

- 代表 Aho, Weinberger, Kernighan
- 用途：用來處理文字檔案，能做出類似`cut`的功能

```
$ echo "this is a test" | awk '{print $1}'
```

```
$ tr -s ' ' ',' < cars.txt | cut -d"," -f2,5
```

用`awk`取代`tr`和`cut`  
cars.txt 是用空格分隔的文字檔案

```
$ awk '{print $2, $5}' cars.txt
```

用`,`取代空格  
結果如下：

```
Toyota 20000
Honda 22000
Nissan 25000
```

`NR`代表 number of record

```
$ awk '{print NR, $2, $5}' cars.txt
```

結果如下：

```
1 Toyota 20000
2 Honda 22000
3 Nissan 25000
```

其他用法：

```
$ awk 'NR == 1' cars.txt
```

| 語法                  | 意義                        | 例子                                              |
| --------------------- | --------------------------- | ------------------------------------------------- |
| NR == 5, NR == 10     | 第五行**至**第十行          | `awk 'NR == 5, NR == 10' {print $1} cars.txt`     |
| NR == 5 \|\| NR == 10 | 第五行**或**第十行          | `awk 'NR == 5 \|\| NR == 10' {print $1} cars.txt` |
| $2 == 22000           | 第二欄等於 22000            | `awk '$2 == 22000' cars.txt`                      |
| $2 < 6000             | 第二欄小於 6000             | `awk '$2 < 6000' cars.txt`                        |
| /Honda/               | 包含 Honda 的行             | `awk '/Honda/' cars.txt`                          |
| /Honda/ {print $2}    | 包含 Honda 的行，印出第二欄 | `awk '/Honda/ {print $2}' cars.txt`               |

ULI101

### Quiz Review

```
city="North York"
```

# Transfer Files

## Secure Copy (`scp`)

語法：`scp local_file remote_username@remote_ip:remote_folder`

```sh
$ scp file.txt user@192.168.0.2:/home/user
```
