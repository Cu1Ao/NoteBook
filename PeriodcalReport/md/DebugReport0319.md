# Report Ao 0319
## 工作简介
要在`riscv`架构的linux系统中安装并运行`Docker`，实现容器化
![１、系统架构图]()

但是目前Doker放出的


具体而言，遇到的问题有：



## 方法
### 1 直接编译安装
  >  [参考页面](https://docs.docker.com/install/linux/docker-ce/fedora/)https://docs.docker.com/install/linux/docker-ce/fedora/

* 可以正确设置Docker的存储库，但不能安装：
![网络安装报错截图]()

* 脚本安装报错：
  ![脚本安装报错截图]()

### 2 在文件系统中安装

### 3 安装静态二进制包
> [参考页面](https://docs.docker.com/install/linux/docker-ce/binaries/)

#### １）安装依赖 
> **安装环境**:
> 底层：QEMU
> linux内核版本：Linux stage4.fedoraproject.org 4.19.0-rc8 #1 SMP Wed Oct 17 15:11:25 UTC 2018 riscv64 riscv64 riscv64 GNU/Linux
> 发行版：Fedora release 28 (Rawhide)

* 安装git:
  ```
  [root@stage4 ~]# dnf install git
  Last metadata expiration check: 5:51:15 ago on Mon Mar 16 14:30:42 2020. Dependencies resolved
  ================================================================================
  Package                 Arch     Version                          Repository
                                                                           Size
                                                                           ================================================================================
                                                                           Installing:
                                                                           git                     riscv64  2.16.2-1.0.riscv64.fc28          local  3.9 M
  ```

* 查看`iptables`版本

```
[root@stage4 ~]# rpm -q iptables
iptables-1.6.2-2.fc28.riscv64
```
**错误原因：**　静态二进制包不适配`riscv`架构
（适配的架构：aarch64/armel/armhf/ppc64le/s390x/x86_64/）
![支持架构]()
![报错信息]()


## 新的解决方案
* 模拟目标硬件
  通过运行一个全功能模拟器，我们可以启动一个可以运行 Linux 操作系统的通用 ARM 虚拟机，然后在虚拟机中设置开发环境，编译应用程序。
  
  但是，如果仔细思考下，一个全功能虚拟机有一些浪费资源。在该模式下，QEMU 会模拟整个系统，包括诸如定时器、内存控制器、SPI 和 I2C 总线控制器等硬件。但是大部分情况下，我们编译应用程序不会关心以上所提到的硬件特性。

* 通过 binfmt_misc 模拟目标架构的用户空间
  在 Linux 系统上，QEMU 有另外一种操作模式，可以通过用户模式模拟器来运行非本地架构的二进制程序。该模式下，QEMU 会跳过方法 2 中描述的对整个目标系统硬件的模拟，取而代之的是通过 binfmt_misc 在 Linux 内核注册一个二进制格式处理程序，将陌生二进制代码拦截并转换后再执行，同时将系统调用按需从目标系统转换成当前系统。最终对于用户来说，他们会发现可以在本机运行这些异构二进制程序。