# Multiplexing-Web-Server

The Assignment 2 report from NTU102-1 [Systems Programming (Cloud Computing Program)](http://www.csie.ntu.edu.tw/~pjcheng/course/sp2013cc/) course

**by NTU [Michael Hsu](http://michaelhsu.tw/ "blog")**

## 執行環境
- [Public Domain Workstation Lab (R217)](http://wslab.csie.ntu.edu.tw/ssh/).
- Linux linux19 3.10-3-amd64 #1 SMP Debian 3.10.11-1 (2013-09-10) x86_64
- gcc version 4.8.1 (Debian 4.8.1-10)

## 如何執行

### 1. GCC compiler

```
$ gcc -Wall sp_pa2_httpd.c -o run
```

### 2. run httpd server [`port` `log_file`]

```
$ ./run 8002 logs.txt
```

### 3. telnet connect

```
$ telnet linux19.csie.ntu.edu.tw 8002
$ > GET /large1 HTTP/1.1
```

![telnet connect](https://raw.github.com/evenchange4/102-1_SP_PA2_Multiplexing-Web-Server/master/image/telnet%20connect.png)

### 4. Browser `url:port/file`
- [http://linux19.csie.ntu.edu.tw:8002/large2](http://linux19.csie.ntu.edu.tw:8002/large2)


## Problem description
### Browser Busy：
在連線建立完成，開始傳輸（write）資料的時候，不知道為什麼 telnet 都可以很順利的完成，但是 Browser 總是會出現 Broken pipe，然後只印出部分的內容，這個問題讓我想了很久，最後請教了助教才發現原來會出現 Browser busy 的情況，所以 `nwritten = write( requestP[conn_fd].conn_fd,` 回傳的值可能會是 `-1`，後來我也發現的確會如此。

![Browser Busy](https://raw.github.com/evenchange4/102-1_SP_PA2_Multiplexing-Web-Server/master/image/browser%20busy%20return%20-1.png)

### Solution：
因為這個情況有時候會發生會時候不會，所以在考慮上需要把 connection 的 `File Descriptor (fd)` 給 `FD_SET` 到 `$writefds` 的 set 裏面，然後當碰到 `-1`，的情況的時候就可以直接排除 `-1` 的回傳結果了。

![Solution busy (-1 情況)](https://raw.github.com/evenchange4/102-1_SP_PA2_Multiplexing-Web-Server/master/image/solution%20busy.png)

## Result
按照 Assignment 2 -- Multiplexing Web Server `Sample Input.pdf` 這份件依序做模擬操作的結果如下圖：

![large files Congratulations (browser)](https://raw.github.com/evenchange4/102-1_SP_PA2_Multiplexing-Web-Server/master/image/browser%20Congratulations.png)

![large files Congratulations (telnet)](https://raw.github.com/evenchange4/102-1_SP_PA2_Multiplexing-Web-Server/master/image/telnet%20Congratulations.png)


## Discussions

## Reference
- [Handle multiple socket connections with fd_set and select on Linux](http://www.binarytides.com/multiple-socket-connections-fdset-select-linux/)
- [Is a return value of 0 from write(2) in C an error](http://stackoverflow.com/questions/2176443/is-a-return-value-of-0-from-write2-in-c-an-error)
- [◎read與write系統呼叫◎](http://bbs3.nsysu.edu.tw/txtVersion/treasure/mis-OS/M.952573483.A/M.952663880.P.html)
- ptt2 `看板《SysProgram` 的精華區。