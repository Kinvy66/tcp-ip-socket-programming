##  第2章 套接字类型与协议设置


### 2.1 套接字协议及其数据传输特性

创建套接字的api

```cpp
#include <sys/socket.h>
int socket(int domain, int type, int protocol)
```
参数：
- `domain` ： 套接字中使用的协议簇
- `type`：套接字数据传输类型信息
- `protocol`: 计算机间通信中使用的协议信息

#### `domain` 协议族
| 名称 |  协议族 |
| ---  | --- |
| `PF_INET` | IPv4 |
| `PF_INET6` | IPv6 |
| `PF_LOCAL` | 本地通信的UNIX协议族 |
| `PF_PACKET` | 底层套接字的协议族 |
| `PF_IPX` | IPX Novell 协议族 |


#### 套接字类型(`type`)
- `SOCK_STREAM` 面向连接的套接字（TCP）
- `SOCK_DGRAM` 面向信息的套接字 (UDP)



#### 协议(`protocol`)
前面两个参数已经可以确定协议信息和传输方式，第三个参数通常设置为 0