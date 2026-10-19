# [ez_f3mr4t](https://ctf.xidian.edu.cn/games/37/challenges?challenge=1347)
##### 分值：150pts

## 题目描述：
### 原文：

Thi5 rs4 s3ems e2.

## 分析：
### RSA

## 解题步骤：
查看代码：
```python
from Cryptodome.Util.number import *
from gmpy2 import next_prime
from secret import flag

p = getStrongPrime(512)
q = next_prime(p ^ ((1<<512)-1))
n = p * q
e = 65537

m = long_to_bytes(flag)
assert m < n
c = pow(m, e, n)

#n = 0x308244e7a7de386723c92ba62e35bc22c3ec1b93023e1551408344a4ba31c6203da849aedee0cadf26a3442f9fd652f7d97c053a3f2c298eab6c0f0c2d0b4642a81765ddb00b690425eb212d5520327ac2d53a22922448399fecb54fbc04dbb68fa33fee7666cb9e05278b5f5f1330b3918d3a7def580fcc00f6f596f16eba3b
#c = 0x13a77e34f09b8a2291cdb397ea0e5cb9e86f691c6565b35c76e6bb6248b67db24315e22fe1dc5321a8820320cdd9b51e3d431459aac2213948f4fbf01c4ce974428d1ed745b2c06f8aa92b22dfede3b7ceb59aa4eb22467129f55b60037a1a2de9b37f25fda3fe40323fccc7c8bbdd4446096b37cef4138337ee9a04c2e3d5fd
```
题目生成了一个512bit的素数p

p ^ ((1<<512)-1) = $2^{512} - 1 - p$

所以 $q = next\_prime(2^{512} - 1 - p)$ 

因为 p 512bit，q也是512bit，由于素数的间隔不大，所以 $q ≈ 2^{512} -1 - p$

所以 $p + q ≈ 2^{512} - 1$

而 $pq = n$

令 $s = p + q$

则 p, q 为方程$x^2 - sx + n = 0$ 的两根

$\sqrt{\Delta} = \sqrt{s^2 - 4n}$ 应为整数，即 $s^2 - 4n$ 应为平方数

因为 $q ≥ 2^{512} -1 - p$，所以 $s ≥ 2^{512} - 1$

搜索s
```python
for k in range(10000):
    s = (1 << 512) - 1 + k
    D = s*s - 4*n
    if D >= 0:
        root, is_square = iroot(D, 2)
        if is_square:
            print(f"found s: {s}")
```
```log
found s: 13407807929942597099574024998205846127479365820592393377723561443721764030073546976801874298166903427690031858186486050853753882811946569946433649006084140
```
确认s有解。
```python
p = (s + root) // 2
q = (s - root) // 2
print(f"p = {p}")
print(f"q = {q}")
d = inverse(65537, (p-1)*(q-1))
m = pow(c, d, n)
print(f"flag = {long_to_bytes(m).decode()}")
```
报错了，看来s不对...看来要多试试几个
```python
from Cryptodome.Util.number import *
from gmpy2 import iroot, next_prime

n = 0x308244e7a7de386723c92ba62e35bc22c3ec1b93023e1551408344a4ba31c6203da849aedee0cadf26a3442f9fd652f7d97c053a3f2c298eab6c0f0c2d0b4642a81765ddb00b690425eb212d5520327ac2d53a22922448399fecb54fbc04dbb68fa33fee7666cb9e05278b5f5f1330b3918d3a7def580fcc00f6f596f16eba3b
c = 0x13a77e34f09b8a2291cdb397ea0e5cb9e86f691c6565b35c76e6bb6248b67db24315e22fe1dc5321a8820320cdd9b51e3d431459aac2213948f4fbf01c4ce974428d1ed745b2c06f8aa92b22dfede3b7ceb59aa4eb22467129f55b60037a1a2de9b37f25fda3fe40323fccc7c8bbdd4446096b37cef4138337ee9a04c2e3d5fd

base_s = (1 << 512) - 1

for k in range(10000):
    s = base_s + k
    D = s*s - 4*n
    if D >= 0:
        root, is_square = iroot(D, 2)
        if is_square:
            p = (s + root) // 2
            q = (s - root) // 2
            if p * q == n:
                print(f"p = {p}")
                print(f"q = {q}")
                d = inverse(65537, (p-1)*(q-1))
                m = pow(c, d, n)
                print(f"flag = {long_to_bytes(m).decode()}")
                break
```
```log
p = 10002122530565118070152534162984174425405406029304477710845852334016328008398205070527901610727835003852024550254062656510034899493071544121224910911648163
q = 3405685399377479029421490835221671702073959791287915666877709109705436021675341906273972687439068423838007307932423394343718983318875025825208738094435977
flag = moectf{F3rm4t_i5_inde3d_7h3_k1ng_0f_@mat3ur_m4them@t1an5}
```
得到flag
## Flag
moectf{F3rm4t_i5_inde3d_7h3_k1ng_0f_@mat3ur_m4them@t1an5}