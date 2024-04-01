# OPS102

Saso Kocev  
Monday 17:10, Wednesday 09:50

## 日期 (cal, date)

cal 年 月

例：

    cal 2024 06

    date

## List

    ls -l // -l=long view
    ls -a = all // -a -l OR -al OR -la 效果相同
    ls {file_name} // 只印出完全符合檔名的檔案
    ls {file_name}* // 用wildcard character for any number of characters
    ls {file_name}? // match one character
    ls [AbC] // match any one inside the bracket
    ls [C-Y] // provide a range of alphabets，通常是a-z
    ls [0-9]
    ls [a-zA-Z0-9]
    ls [!ABC] // exclude A B or C

    man {指令名稱} # user manual

    who // to show who logged in
    w // like who with more information

Result: WHAT -bash means not running any program

## Text Editors

    nano (text editor) exit -> ctrl+X
    pico (old version of nano)
    cat -n file_name (-n shows line number)
    more file_name (to show the content page by page, press space for next page)
    less file_name (better than more, able to scroll up and down)
    vi file_name
    vim file_name

## File Operations

    cp source_file destination_file (copy file)
    cp -r // copy recursively a nonempty directory

    mv (same as above)

    tree -C // draw directory tree with colour
    mkdir 目錄名
    rmdir 目錄名 // remove empty directory
    rm -r // remove nonempty directory, r=recursive


    pwd = present working directory
    use slash to access absolute path, e.g. cd /home/users/

## 用檔名尋找目錄下的檔案(包含子目錄)

檔名要符合 regular expression

    find 路徑 - name "{file-name-to-be-find}"
    find 路徑 - iname "{file-name-to-be-find}" // case insensitive

## 印出在 CLI 或在檔案

```sh
echo "something I want to print"
echo "something I want to save" > file.txt // overwrite the whole file
echo "another thing I want to save" >> file.txt // append in the file bottom
```

用 2>把錯誤訊息存到檔案  
用 2>>把錯誤訊息附加到檔案底部

```sh
diff -y file1.txt file2.txt // compare files
```

## grep

    grep "mark" data.txt
    // 在 data.txt 尋找 mark，印出有該文字的行
    // 參數 -i 代表不分大小寫
    // quotes are optional when searching for a single word

## cat

印出檔案內容  
-n 參數印出行號

```sh
cat data.txt
```

## pipe

用 | (pipe)連接兩個指令，把第一個指令的結果送給第二個指令  
例：

    grep "mark" data.txt | wc -l

## 文字處理

### wc 代表 word count

結果：3 個數字，lines words characters
參數：-l (這是小寫 L) 代表 count line

### tr 代表 translate

例 1：

    tr "a" "W" < data.txt // 把 data.txt 代為輸入

例 2：

    cat data.txt | tr 'a' 'W' // 用 piping 方式，單引號或雙引號都可以

例 3：

    tr "a" "W" < input.txt > replaced.txt // 輸出到 replaced.txt
    tr -s " " ";" // -s 代表 squeeze，把連續的空格轉成分號

### 印出 CSV 文字檔的某部份

    cut -d"," -f1 data.txt // -d=deliminator, -f1=第一欄
    cut -d"," -f1,2 data.txt // -f1,2=第一二欄

註：用 piping 也可，例

    cat data.txt | cut -d"," -f1,2

用分號取代連續空格(-s=squeeze)，-f5,9 印出第 5 和第 9 欄

    ls -lh | tr -s " " ";" | cut -d";" -f5,9

## head 和 tail

    head -5 data.txt // 相當於 cat 指令，但只印首 5 列
    tail -5 data.txt // 印尾 5 列
    tail -f data.txt // -f 保持在檔案結尾處(持續更新)
    tail -f data.txt & // 用&放在背景執行，可用 ps -x 列出

## jobs 指令

    jobs // see by jobs 每個 job 有一個數字
    fg {數字} // 把 job 帶到前景(foreground)

用 Ctrl+Z 把程式放到背景

## sort 指令

    sort numbers.txt // 文字形式排序
    sort -n numbers.txt // 數字形式排序
    sort -f names.txt // 用-f 不分大小寫
    // 可配合 > 使用，例：sort names.txt > sorted.txt
    sort -t"," -k2 -f data.txt // 用某一欄排序，-k 接數字代表排序該欄 key(field)
    sort -t"," -n -h file.txt // -h 人類辯認格式，可以排序 K, M, G 等單位，配合-n 使用
    sort -r // reverse order
    sort -R // random order

    uniq -i // 移除重覆，-i 不分大小寫

用 pipe 把資料傳入，例 `sort ... | uniq -i`

# Permission

## Symbolic Permission

r=read  
w=write  
x=execute  
-rwx rwx rwx

|      | user | group | others |
| ---- | ---- | ----- | ------ |
| 代號 | u    | g     | o      |

    chmod -w file.txt // 移除三組人的 w 權限
    chmod +w file.txt // 為三組人加w權限

程式檔不需要 rw 權限  
第一個 dash 代表是一個 file，如果是 d 代表是 directory

    chmod g-r file.txt // 移除 group(g)的權限
    chmod g+r,o+w file.txt // 注意：逗號後沒有空格
    chmod u=rwx,g=,o=r file.txt // 等號後為空，沒有任何權限
    chmod ugo+rwx file.txt // 注意：ugo 順序固定
    chmod a+rwx file.txt // a代表全部人員
    chmod u=w file.txt // 重設 user 權限，然後設成只有w (set to w)

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
$ chmod 17 file.txt // 考試要求寫齊3個數字，但 Linux 接受 1 個或 2 個數字
```

### umask (user file-creation mode)

umask is a Linux command that lets you set up default permissions for newly created files and folders.

例：

```sh
$ umask 022
```

| 十進制 | 0   | 2   | 2   |
| ------ | --- | --- | --- |
| 二進制 | 000 | 010 | 010 |
| 反向   | 111 | 101 | 101 |
|        | rw- | r-- | r-- |

把新增的檔案設成反向後的權限
例：`touch file.txt`，權限即為 rw- r-- r--  
因為它不是執行檔，所以自動略過 x

## Disk

```sh
$ du // 顯示資料夾內的檔案大小 estimate file space usage
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
$ du -h // size represented in human readable format
```

例：

```sh
anthony@TAM_HOME:~/a$ du -h
8.0K    ./b
8.0K    ./@eaDir
1.6M    .
```

df (disk free) report file system disk space usage

```sh
$ df
```

## 查看 process

    ps -x // 查看運行中的 process，-x 包含其他 session
    ps -afux
    // -u show username
    // -a all with TTY
    // -f full format
    ps -u {使用者} // 只查看某一使用者

    kill {process-id} // 關閉 process，發出SIGTERM
    kill -9 {process-id} // 強制關閉 process -9，發出SIGTKILL

# alias

設定別名，只對現時的 session 有效

```sh
alias list='ls -alh' // 用alias代表'ls -alh'
```

取消 alias

```sh
unalias list
```

~/.bash_profile

當登入時會執行一次這個檔案的指令

在~/.bash_profile 最底加入`alias list='ls -alh'`，即可每次登入都執行一次

# history 及 fc -l

Print out command history

```sh
$ history | tail -20  // 用 pipe 印出最後20列的歷史指令

$ fc -l
655      kill -9 135359
656      ps -u lftam2
657      ps -l -u lftam2
658      jobs
659      ps -l -u lftam2
$ !658 // 執行代號為658的歷史指令
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
1234567 -rw-r--r-- 1 john users 0 Mar 11 16:59 source_file  // 這兩個檔案的 inode 相同
```

`rm destination_file`不會刪除 source_file

## Soft Links / Symbolic Links

Soft link 用`ln -s`參數  
與 Windows 捷徑相同  
Soft link 可用於 directory  
捷徑有自己的檔案位置和大小

```sh
$ ln -s file1 file2 // file2指向file1
lrwxrwxrwx 1 john users  5 Mar 11 17:00 file2 -> file1  // 最頭的l代表link
$ ln -s /root/file3 file4 // file4指向絕對路徑
lrwxrwxrwx 1 john users  5 Mar 11 17:01 file4 -> /root/file3
```

# Script

## 變數

- Variable name is case sensitive
- Variable name cannot start with number

```sh
$ school=Seneca  // assign Seneca to 變數school 不能有空格
$ echo $school  // $代表是variable
Seneca
$ location="Greater Toronto Area"  // use quotes to include words with space in between
$ price="This watch costs \$1000"
$ echo "My school is $school"  // use $ to call variable
My school is Seneca
```

- 用單引號不會顯示變數值

```sh
$ echo 'My school is $school'
My school is $school
```

在~/.bash_profile 最底加入`$school=Seneca`，即可每次登入都執行一次

通常用.sh 作為 script 的副檔名

**`script.sh`:**

```sh
#!/bin/bash
# This is a comment
echo "Hello World"
```

- 第一行叫做 shebang line, to indicate the script is a bash script  
  這行不一定要，但是加上可以確保 script 會用 bash 執行 (考試要求要加)  
  usr/bin/bash is the location of bash  
  在第二行開始用#，則是註解

### 在執行前要先給予執行權限

```sh
$ chmod +x script.sh  // make the script executable
$ ./script.sh  // run the script
Hello world
```

```sh
echo "Hello world"; echo "Hello world"  // 用分號連接兩個指令
```

## read

- 用途：讓使用者輸入資料

```sh
read  # waiting for user's ENTER key
read colour  # prompt user to input, and store the input in variable colour
```

- 變數在.sh 以外就會失效

- `echo -n` 代表印出後不換行

```sh
echo -n "Enter your name: "
# -n 代表不換行
read name
echo
# echo 一行空白，即換行
echo "Hello $name"
```

以上的程式碼會等待使用者輸入名字，然後印出 Hello \{name\}

`echo -p` 會自動換行，`-p` 代表 prompt

```sh
read -p "Enter your name: " name
# prompt user to input, and store the input in variable name (name前不用$)
echo "Hello $name"
```

以上的程式碼會等待使用者輸入名字，然後印出 Hello \{name\}

---

## argument

- 在運行.sh 後加入參數(arguments)，script 會把參數順序放入 script 內的變數，例：

**`script.sh`:**

```sh
#!/bin/bash
echo "My first argument is $1"
echo "My second argument is $2"
```

試執行：

```sh
./script.sh John Peter
```

結果如下：

```
My first argument is John
My second argument is Peter
```

- 可以用`{}`包住變數，注意$1和$10  
如果直接用`$10`，會被解讀成`$1`後面接`0`，所以要用`${10}`來表示

```sh
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

```sh
#!/bin/bash
echo "The total number of arguments is $#"
if [ $# = 1 ]
then
	echo "You have entered 1 argument"
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

```sh
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

```sh
./script.sh
echo $?
```

結果如下：

```sh
$ Is everything alright? (y/n) y
Good to hear!
$ echo $?
0
```

### 自制有 error message 的 grep

**`script.sh`:**

```sh
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

```sh
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

```sh
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

```sh
#!/bin/bash
echo "The current date and time is $(date)"
```

### 在 script 裡運算

用雙括號`$(( ))`來運算

```sh
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

```sh
#!/bin/bash
echo "The loop is now starting"
for i in 1 2 3 4 5
do
	echo "The number is $i"
done
```
