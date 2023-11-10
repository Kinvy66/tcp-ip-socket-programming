## 第3章 地址族与数据序列

### 3.1 分配给套接字的IP地址与端口

地址，指向的是主机，通过地址识别网络中的不同的主机  
端口，指向的是主机中的应用，不同的应用具有不同的端口号，一台主机上一个端口不能同时分配个多个引用



### 3.2 地址信息表示

地址信息结构体
```cpp
struct sockaddr_in {
    sa_family_t     sin_family;     // 地址族
    unint16_t       sin_port;       // 16位端口号
    struct in_addr  sin_addr;       // 32位IP地址
    char            size_zero[8];   // 不使用
}

struct in_addr {
    In_addr_t       s_addr;     // 32位IPv4地址
}
```


### 3.3 网络字节序与地址变换

- 大端序： 高位字节存放到低地址
- 小端序： 高位字节存放到高地址

为了统一不同主机之间字节序的差别，网络传输数据统一使用网络字节序, 网络字节序使用的是大端字节序


#### 字节序转换
```cpp
unsigned short htons(unsigned short);
unsigned short ntohs(unsigned short);
unsigned long htonl(unsigned long);
unsigned long ntohl(unsigned long);
```

函数名：h: host, n: network, s: short, l: long


### 3.4 网络地址的初始化与分配

#### 字符串和整数型地址信息转换
地址信息结构中表示IP地址的信息的数据类型是一个整型数据，但是通常使用的IP地址是点分十进制的字符串。可以使用下面函数进行转换
```cpp
#include <arpa/inet.h>
int_addr_t inet_addr(const char* string);
// 成功返回32位网络字节序的整型数值，失败返回 INADDR_NONE
```
参数：
  - `string` ： 点分十进制的IP地址

另一个同样功能的函数：
```cpp
#include <arpa/inet.h>
int inet_aton(const char* string, struct in_addr* addr);
// 成功返回1， 失败返回0
```
参数：
- `string`: 点分十进制IP地址字符串
- `addr`: `in_addr`地址信息结构体，输出参数，转换后的IP地址存储在该该结构体的`sin_addr.s_addr`


整型地址转换为点分十进制字符串
```cpp
#include <arpa/inet.h>
char* inet_ntoa(struct in_addr adr);
// 成功时返回转换的字符串地址值， 失败返回-1
```
参数：
- `adr` ： IP地址结构体



#### 网络地址初始化

```cpp
struct sockaddr_in serv_addr;

memset(&serv_addr, 0, sizeof(serv_addr));
serv_addr.sin_family = AF_INET;
serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
serv_addr.sin_port = htons(atoi(argv[1]));
```


`INADDR_ANY` 表示获取当前设备的IP地址，通常用于服务器端IP的设置



### 3.5 基于Windows 实现
略