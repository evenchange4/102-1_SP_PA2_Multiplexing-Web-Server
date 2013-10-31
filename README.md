# Multiplexing-Web-Server

The Assignment 2 report from NTU102-1 [Systems Programming (Cloud Computing Program)](http://www.csie.ntu.edu.tw/~pjcheng/course/sp2013cc/) course

**by NTU [Michael Hsu](http://michaelhsu.tw/ "blog")**

## 執行環境
- [Public Domain Workstation Lab (R217)](http://wslab.csie.ntu.edu.tw/ssh/).
- Linux linux19 3.10-3-amd64 #1 SMP Debian 3.10.11-1 (2013-09-10) x86_64
- gcc version 4.8.1 (Debian 4.8.1-10)

## 如何執行

### GCC compiler

```
$ gcc -Wall sp_hw1_httpd.c -o run
```

### run httpd server [`port` `log_file`]

```
$ ./run 8002 logs.txt
```

### telnet connect

```
$ telnet linux19.csie.ntu.edu.tw 8002
$ > GET /large1 HTTP/1.1
```

![telnet connect]()

- 

## Problem description

## solution, discussions