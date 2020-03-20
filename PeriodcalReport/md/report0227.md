# Report  Ao 20200227
## 主要工作
* 重新编译Linux内核和BBL (Berkeley Boot Loader)文件，解决上周出现的不停打印错误信息的问题： 
```
root@busybear:~# ntpd: bad address '1.pool.ntp.org'
ntpd: bad address '0.pool.ntp.org'
ntpd: bad address '1.pool.ntp.org'
```
原因是系统时间读取不对：
```
root@busybear:~# date
Thu Jan  1 01:12:02 UTC 1970
```
重新编译后的内核信息：
```
root@busybear:~# uname -a
Linux busybear 4.19.0-rc3 #1 SMP Fri Feb 21 04:26:13 CST 2020 riscv64 GNU/Linux
root@busybear:~# cat /proc/interrupts 
           CPU0       
  7:          3  SiFive PLIC   7  virtio1
  8:        727  SiFive PLIC   8  virtio0
 10:      11818  SiFive PLIC  10  ttyS0
root@busybear:~# cat /proc/cpuinfo 
hart	: 0
isa	: rv64imafdcsu
mmu	: sv48
```
* linux系统无法连接网络，尝试按照`SiFive`的文档设置了网络参数，但是依然存在问题：
  
  设置完成后的网络参数
```
  root@busybear:~# ifconfig 
  eth0         Link encap:Ethernet  HWaddr 52:54:00:12:34:56  
                    inet addr:192.168.122.2  Bcast:192.168.  122.255  Mask:255.255.255.0
                    inet6 addr: fe80::5054:ff:fe12:3456/64 Scope:Link
                    UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                    RX packets:2 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:567 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:1000 
                    RX bytes:176 (176.0 B)  TX bytes:24358 (23.7 KiB)
  lo              Link encap:Local Loopback  
                    inet addr:127.0.0.1  Mask:255.0.0.0
                    inet6 addr: ::1/128 Scope:Host
                    UP LOOPBACK RUNNING  MTU:65536  Metric:1
                    RX packets:374 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:374 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:1000 
                    RX bytes:33054 (32.2 KiB)  TX bytes:33054 (32.2 KiB)
  ```
## 下周工作
* 这个linux镜像太简陋，很多命令不支持（如包管理命令、gcc等），下周我想尝试重新配置、编译内核，添加必要的命令，同时继续调试网络。