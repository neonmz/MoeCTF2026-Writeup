# [你会git吗？](https://ctf.xidian.edu.cn/games/37/challenges?challenge=1319)
##### 分值：100pts

## 题目描述：
### 原文：
xing 用 copperkoi 的 git 藏了个 flag，你能找到他吗

## 分析：
### git

## 解题步骤：
```sh
neon@MacBook-Air.local /tmp/flag % git restore '*'                                                                OK(0)
Found existing alias for "git restore". You should use: "grs"
[2026-10-14 14:52:14] git restore '*'
duration: 0.03s                                                                                                         
neon@MacBook-Air.local /tmp/flag % ls                                                                             OK(0)
[2026-10-14 14:52:24] ls -G
flag.txt
duration: 0.02s                                                                                                         
neon@MacBook-Air.local /tmp/flag % cat flag.txt                                                                   OK(0)
[2026-10-14 14:52:30] cat flag.txt
flag is me,use base64 and put in moectf{}:ZzE3XzE1X3NvXzNhU3k=
duration: 0.02s                                                                                                         
neon@MacBook-Air.local /tmp/flag % base64 -d                                                                      OK(0)
[2026-10-14 14:52:43] base64 -d
ZzE3XzE1X3NvXzNhU3k=
g17_15_so_3aSy%                                                                                                         duration: 4.55s

```

## Flag
moectf{g17_15_so_3aSy}