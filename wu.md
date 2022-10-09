### Level 0

ssh tới server thông qua port 2220 với username là bandit0 và password là bandit0

```console
ssh bandit0@bandit.labs.overthewire.org -p 2220
bandit0@bandit.labs.overthewire.org's password: bandit0 
```
![Hinh](img/lv0.jpg)
### Level 0 → Level 1

Đọc file readme và lấy password

```console
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```
### Level 1 → Level 2

Đọc file ```-``` và lấy password

```console
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```
![Hinh](img/lv1.jpg)
### Level 2 → Level 3

Đọc file ```spaces in this filename``` và lấy password

```console
bandit2@bandit:~$ cat 'spaces in this filename'
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```
![Hinh](img/lv2.jpg)
### Level 3 → Level 4

Dùng lệnh ls -al để hiện file ẩn

```console
bandit3@bandit:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 Sep  1 06:30 .
drwxr-xr-x 3 root    root    4096 Sep  1 06:30 ..
-rw-r----- 1 bandit4 bandit3   33 Sep  1 06:30 .hidden
```

Đọc file ẩn

```console
bandit3@bandit:~/inhere$ cat .hidden 
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```
![Hinh](img/lv3.jpg)
### Level 4 → Level 5

Đọc tất cả các file trong thư mục để tìm được chuỗi ở dạng human-readable

```console
bandit4@bandit:~/inhere$ cat ./*
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```
![Hinh](img/lv4.jpg)
### Level 5 → Level 6

Dùng lệnh find với option -size để tìm kiếm theo size, option -type để tìm kiếm file , ! -excutable để tìm các  file không phải file thực thi

```console
bandit5@bandit:~/inhere$ find ./ -size 1033c -type f ! -executable
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```
![Hinh](img/lv5.jpg)
### Level 6 → Level 7

Dùng lệnh find với option -user để tìm kiếm theo chủ sở hữu, -group để tìm kiếm theo group sở hữu, -type f để tìm kiếm file  (không tìm kiếm thư mục)

```console
find / -user bandit7 -group bandit6 -size 33c -type f | grep bandit7
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```
![Hinh](img/lv6.jpg)
![Hinh](img/lv6-2.jpg)
### Level 7 → Level 8

Dùng grep để tìm đến vị trí của dòng có từ 'millionth'

```console
bandit7@bandit:~$ cat data.txt | grep 'millionth'
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
![Hinh](img/lv7.jpg)
### Level 8 → Level 9

Sort lại nội dung file theo thứ tự alphabet và dùng lệnh uniq với option -u để tìm các dòng chỉ hiển thị duy nhất một lần.

```console
bandit8@bandit:~$ cat data.txt | sort | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t

```
![Hinh](img/lv8.jpg)
### Level 9 → Level 10

Tìm kiếm các dòng có dấu '=' trong file để thấy password

```console
bandit9@bandit:~$ strings data.txt | grep "="
=id6
========== the
gO=89
5+&R=
V>%=
bu========== password
iwAw=
M'j=_
4iu========== is
b~==P
ED=Fpe
,=fX
x=f+
O=6pF
=do%
:26=
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
=@dZ
u-;=
=#U?
2BEK=q
@!6=
```
![Hinh](img/lv9.jpg)
### Level 10 → Level 11

Đọc nội dung file và decode mã base64 bằng lệnh base64 với option --decode

```console
bandit10@bandit:~$ cat data.txt | base64 --decode
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
![Hinh](img/lv10.jpg)
### Level 11 → Level 12

Dùng lệnh tr như dưới để decrypt ROT13

```console
bandit11@bandit:~$ cat data.txt | tr '[N-ZA-Mn-za-m]' '[A-Za-z]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
![Hinh](img/lv11.jpg)
### Level 12 → Level 13

Tạo thư mục il3sor và hexdump file data.txt 

```console
bandit12@bandit:~$ mkdir /tmp/il3sor
bandit12@bandit:~$ xxd -r data.txt /tmp/il3sor/data
bandit12@bandit:~$ cd /tmp/il3sor
```

Kiểm tra file type của file data và giải nén lần 1

```console
bandit12@bandit:/tmp/il3sor$ file data
data: gzip compressed data, was "data2.bin", last modified: Thu Sep  1 06:30:09 2022, max compression, from Unix, original size modulo 2^32 575
bandit12@bandit:/tmp/il3sor$ cp data data.gz
bandit12@bandit:/tmp/il3sor$ gzip -d -f data.gz
```

Tiếp tục kiểm tra file type và giải nén lần 2

```console
bandit12@bandit:/tmp/il3sor$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/il3sor$ cp data data.bz2
bandit12@bandit:/tmp/il3sor$ bzip2 -d -f data.bz2
```

Tiếp tục kiểm tra file type và giải nén lần 3

```console
bandit12@bandit:/tmp/il3sor$ file data
data: gzip compressed data, was "data4.bin", last modified: Thu Sep  1 06:30:09 2022, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/il3sor$ cp data data.gz
bandit12@bandit:/tmp/il3sor$ gzip -d -f data.gz
```

Tiếp tục kiểm tra file type và giải nén lần 4

```console
bandit12@bandit:/tmp/il3sor$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/il3sor$ tar -xf data

bandit12@bandit:/tmp/il3sor$ ls
data  data5.bin
```

Tiếp tục kiểm tra file type và giải nén lần 5

```console
bandit12@bandit:/tmp/il3sor$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/il3sor$ tar -xf data5.bin
```

Tiếp tục kiểm tra file type và giải nén lần 6

```console
bandit12@bandit:/tmp/il3sor$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/il3sor$ bzip2 -d -f data6.bin
```

Tiếp tục kiểm tra file type và giải nén lần 7

```console
bandit12@bandit:/tmp/il3sor$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/il3sor$ tar -xf data6.bin.out

bandit12@bandit:/tmp/il3sor$ ls
data  data5.bin  data6.bin.out  data8.bin
```

Tiếp tục kiểm tra file type và giải nén lần 8

```console
bandit12@bandit:/tmp/il3sor$ file data8.bin
data8.bin: gzip compressed data

bandit12@bandit:/tmp/il3sor$  cp data8.bin data8.bin.gz
bandit12@bandit:/tmp/il3sor$ ls
data  data5.bin  data6.bin.out  data8.bin  data8.bin.gz
bandit12@bandit:/tmp/il3sor$ gzip -d -f data8.bin.gz
```

Kiểm tra file type thấy đã giải nén về file text rồi, ta cat file để lấy flag

```console

bandit12@bandit:/tmp/il3sor$ file data8.bin 
data8.bin: ASCII text
bandit12@bandit:/tmp/il3sor$ cat data8.bin 
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```
![Hinh](img/lv12.jpg)
### Level 13 → Level 14

Kiểm tra file ẩn ta thấy được ssh private key để log in vào level tiếp theo

```console
bandit13@bandit:~$ ls -al
total 24
drwxr-xr-x  2 root     root     4096 Sep  1 06:30 .
drwxr-xr-x 49 root     root     4096 Sep  1 06:30 ..
-rw-r--r--  1 root     root      220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root     root     3771 Jan  6  2022 .bashrc
-rw-r--r--  1 root     root      807 Jan  6  2022 .profile
-rw-r-----  1 bandit14 bandit13 1679 Sep  1 06:30 sshkey.private
```
![Hinh](img/lv13.jpg)
### Level 14 → Level 15

Tiếp tục ở user bandit13, dùng private key để ssh vào server với vai trò user bandit14 và gửi password đến port 30000 bằng netcat

```console
bandit13@bandit:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 | nc localhost 30000
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```
![Hinh](img/lv14.jpg)
![Hinh](img/lv14-2.jpg)
### Level 15 → Level 16

Dùng password ở level trên gửi đến port 30001 qua ssl để lấy password cho level tiếp theo

```console
bandit15@bandit:~$ echo jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt | openssl s_client -connect localhost:30001  -ign_eof

read R BLOCK
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1
```
![Hinh](img/lv15.jpg)
![Hinh](img/lv15-2.jpg)
### Level 16 → Level 17

Dùng nmap quét tất cả các cổng từ port 31000 đến port 32000 với option -sV để xem port nào đang dùng service ssl, sau đó gửi password đến port đó để lấy credential cho level tiếp theo

```console
bandit16@bandit:~$ nmap localhost -p 31000-32000 -sV
Starting Nmap 7.80 ( https://nmap.org ) at 2022-10-01 04:18 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00010s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo

echo JQttfApK4SeyHwDlI9SXGR50qclOAil1 | openssl s_client -connect localhost:31790 -ign_eof
```
![Hinh](img/lv16.jpg)
### Level 17 → Level 18

Dùng private key có được từ level trên, tạo tệp bandit17 để lưu nó (dùng lệnh gedit để mở file và paste private key vào). Sau đó dùng lệnh diff để xem dòng khác nhau giữa 2 file, và đó chính là passoword.

```console
echo > bandit17
gedit bandit17
```

```console
ssh -i bandit17 bandit17@bandit.labs.overthewire.org -p 2220

diff passwords.*
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```
![Hinh](img/lv17.jpg)
### Level 18 → Level 19

### Level 19 → Level 20

### Level 20 → Level 21

### Level 21 → Level 22

### Level 22 → Level 23

### Level 23 → Level 24

### Level 24 → Level 25

### Level 25 → Level 26

### Level 26 → Level 27

### Level 27 → Level 28

### Level 28 → Level 29

### Level 28 → Level 30

### Level 28 → Level 31

### Level 28 → Level 32

### Level 28 → Level 33

### Level 28 → Level 34
