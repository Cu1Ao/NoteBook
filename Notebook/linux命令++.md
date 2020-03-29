# Linux命令++
> 在这里记录下遇到的所有linux命令

### 查看Linux内核版本方法
  ```
  (base) ☁  (/home/ao)  uname -a
  Linux ao-Inspiron 5.3.0-40-generic #32-Ubuntu SMP Fri Jan 31 20:24:34 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
  ```

  ```
  (base) ☁  (/home/ao)  cat /proc/version
  Linux version 5.3.0-40-generic (buildd@lcy01-amd64-026) (gcc version 9.2.1 20191008 (Ubuntu 9.2.1-9ubuntu2)) #32-Ubuntu SMP Fri Jan 31 20:24:34 UTC 2020
  ```

### 查看Linux发行版命令
  ```
  (base) ☁  (/home/ao)  lsb_release -a
  No LSB modules are available.
  Distributor ID:	Ubuntu
  Description:	Ubuntu 19.10
  Release:	19.10
  Codename:	eoan
  ```
  ```
  (base) ☁  (/home/ao)  cat /etc/issue 
  Ubuntu 19.10 \n \l
  ```
> 均适用于所有发行版




### 一次性移动多个文件

```
mv  a.dir  b.dir   c.dir  1.txt  2.txt  -t  des.dir
```

### 压缩和解压文件
#### zip格式
* 压缩文件夹
```
zip -r name.zip file
```

* 解压
```
unzip name.zip
```

#### tar格式
tar是linux中最常见的文件夹打包格式，只消耗非常少的CPU以及时间去打包一个文件夹。但是tar仅仅是一个打包工具，并不负责压缩。
* 打包目录
```
tar -cvf name.tar file
```
> -c 即是打包的选项

* 解包
```
tar -xvf name.tar
```

#### tar.gz格式
`tar.gz`是另一种非常常见的压缩格式，具有非常理想的压缩率。
* 压缩目录
```
tar -zcvf name.tar.gz file
```

* 解压缩
```
tar -zxvf name.tar.gz ./extract-here/
```
> 这个命令会默认将文档解开在当前目录下面



