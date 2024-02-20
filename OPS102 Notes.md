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

    find {path} - name "{file-name-to-be-find}"
    find {path} - iname "{file-name-to-be-find}" // case insensitive

## 印出在 CLI 或在檔案

    echo "something I want to print"
    echo "something I want to save" > file.txt // overwrite the whole file
    echo "another thing I want to save" >> file.txt // append in the file bottom
    2> // write error message if any
    2>> // append error message if any

diff -y file1.txt file2.txt // compare files

## grep

    grep "mark" data.txt // 在 data.txt 尋找 mark，印出有該文字的行
    //參數 -i 代表不分大小寫

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

用 pipe 把資料傳入，例 sort ... | uniq -i

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
    chmod ugo+rwx file.txt // 注意：ugo 順序固定
    chmod a+rwx file.txt // a代表全部人員
    chmod u=w file.txt // 重設 user 權限，然後設成只有w (set to w)

| 符號 | 意義   |
| ---- | ------ |
| +    | add    |
| -    | remove |
| =    | set    |

## Octal Permission

    chmod 754 file.txt

r -> 4  
w -> 2  
x -> 1

    chmod 17 file.txt // 考試要求寫齊3個數字，但 Linux 接受 1 個或 2 個數字

### umask (user file-creation mode)

umask is a Linux command that lets you set up default permissions for newly created files and folders.

例：

    umask 022

| 十進制 | 0   | 2   | 2   |
| ------ | --- | --- | --- |
| 二進制 | 000 | 010 | 010 |
| 反向   | 111 | 101 | 101 |
|        | rw- | r-- | r-- |

把新增的檔案設成反向後的權限
例：touch file.txt，權限即為 rw- r-- r--  
因為它不是執行檔，所以自動略過 x

## Disk

    du // 顯示資料夾內的檔案大小 estimate file space usage
    du -h // size represented in human readable format

    df // (disk free) report file system disk space usage

## 查看 process

    ps -x // 查看運行中的 process，-x 包含其他 session
    ps -afux
    // -u show username
    // -a all with TTY
    // -f full format
    ps -u {使用者} // 只查看某一使用者

    kill {process-id} // 關閉 process，發出SIGTERM
    kill -9 {process-id} // 強制關閉 process -9，發出SIGTKILL
