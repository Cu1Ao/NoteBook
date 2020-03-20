# Report  Ao 20200312
## 主要工作
 这周的工作：
根据之前的讨论，我们的首要目的是在riscv的linux系统上运行容器化工具，方法是打包一个已安装docker的linux镜像，并在qemu中运行。这周我按照这个思路去做了几次实验：

* 首先，我在linux系统中安装并运行docker，用它来创建一个.img文件，但是我使用qemu-system-riscv64命令运行它的时候失败了，报错的原因是“　qemu-system-riscv64: -drive file=,format=raw,id=hd0: A block device must be specified for "file"　”。我发现是因qemu的启动过程中缺少了一些二进制文件。是因为编译linux内核的时候，一些关于架构的设置没有设置好，比如之前编译的时候设置是这样的：
  ``` 
  cp ../busybear-linux/conf/linux.config .config
  make ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu- olddefconfig
  ```
用busybear-linux中已有的配置文件来配置linxu内核的编译。
所以我在重新尝试不同的配置文件，让包含docker的镜像支持riscv架构

* 感觉现在的方向很明确，但是需要解决很多细节方面的问题。我想这周继续解决这方面工作的问题。