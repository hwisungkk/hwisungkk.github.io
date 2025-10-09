---
title: (cryptohack) general, Mathematics
date: 2025-10-09 15:36:08 +09:00
last_modified_at: 2025-10-09 17:24:54 +09:00
categories: [wargame, cryptohack.org]
tags:
  [
    'cryptohack.org',
    'general'
  ]
---

이번엔 Mathematics 관련 문제들이다.<br>
GCD와 moduler 연산 문제들이었다.

# General

## Mathematics

### Greatest Common Divisor

66528과 52920의 최대공약수를 찾으라고 한다.

```python
a=66528
b=52920

def gcd(x, y):
    while y!=0:
        tmp=x%y
        x=y
        y=tmp
    return x

print(gcd(a,b))
```
하면 1512가 정답이다.

### XOR Properties
```
KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
```

XOR의 특성을 이용한 문제이다.<br>
(A^B)^B == A 이기 때문에 같은 값을 xor 연산 해주면 FLAG를 얻을 수 있다.<br>
위의 KEY부터 하나하나 계산해주면 된다.

```python
from pwn import * # pip install pwntools
import json
import base64
import codecs
from Crypto.Util.number import long_to_bytes

KEY1 = 0xa6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 = 0x37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e ^ KEY1
print(KEY2)
KEY3 = 0xc1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1 ^ KEY2

FLAG=0x04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf ^ KEY1 ^ KEY2 ^ KEY3
print(long_to_bytes(FLAG))
#KEY1 = 0xa6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
#KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
#KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
#FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
```

b'crypto{x0r_i5_ass0c1at1v3}'가 flag다.

### Favourite byte
```
73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d
```
라는 문자열이 주어지고 xor 키는 1바이트라고 한다.

key가 1바이트이므로 0부터 255까지 반복문을 돌리며 xor 연산을 해서 말이 되는 문자열을 찾는다.<br>
flag양식이 crypto{} 이기에 c가 되는 key를 찾아도 되지만 그냥 브포로 풀어줬다.

```python
from Crypto.Util.number import *
s="73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d"

s2=bytes.fromhex(s)

for k in range(256):
    for ss in s2:
        print(chr(ss^k), end="")
    print()
```

```
sbi`dk h! O!%O}iOv$f eb!'#Ori'um
rchae~j!i !N $N|hNw%g!dc &"Nsh&tl
q`kbf}i"j#"M#'MkMt&d"g`#%!Mpk%wo
pajcg|h#k"#L"&L~jLu'e#fa"$ Lqj$vn
wfmd`{o$l%$K%!KymKr b$af%#'Kvm#qi
vgleazn%m$%J$ JxlJs!c%`g$"&Jwl"ph
udofbym&n'&I'#I{oIp"`&cd'!%Ito!sk
tengcxl'o&'H&"HznHq#a'be& $Hun rj
{jahlwc(`)(G)-GuaG~,n(mj)/+Gza/}e
zk`imvb)a()F(,Ft`F-o)lk(.*F{`.|d
yhcjnua*b+*E+/EwcE|.l*oh+-)Exc-g
xibkot`+c*+D*.DvbD}/m+ni*,(Dyb,~f
nelhsg,d-,C-)CqeCz(j,in-+/C~e+ya
~odmirf-e,-B,(BpdB{)k-ho,*.Bd*x`
}lgnjqe.f/.A/+AsgAx*h.kl/)-A|g){c
|mfokpd/g./@.*@rf@y+i/jm.(,@}f(zb
crypto{0x10_15_my_f4v0ur173_by7e}
```
이렇게 key는 16번째에서 나왔고 crypto{0x10_15_my_f4v0ur173_by7e}이다.

### You either know, XOR you don't
```
0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104
```
숫자가 주어지는데 xor key가 무엇인지는 안 알려준다.<br>
우선 crypto{FLAG}의 형식을 가진 것을 알고 있기에

```python
from Crypto.Util.number import *
s="0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"

s2=bytes.fromhex(s)

s3="crypto{"

print(len(s2))

for i in range(len(s3)):
    print(chr(ord(s3[i])^s2[i]), end="")
```
의 코드를 실행하면 myXORke라는 값이 나온다.<br>
이것으로 myXORkey를 유추할 수도 있지만 마지막이 "}"인 것도 알고 있기에 구해보면
```python
print(chr(0x04^ord("}")))
```
의 결과는 y다.<br>

그러므로 key=myXORkey로 잡고서 연산을 해주면 
```python
key="myXORkey"

for i in range(len(s2)):
    print(chr(ord(key[i%8])^s2[i]), end="")
```
FLAG가 나온다. <br>
crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}

### Lemur XOR
lemur.png와 flag.png가 주어지고 두개를 XOR 연산하면 된다고 한다.<br>
GPT의 도움을 받아서 작성했다.<br>

pillow 이미지를 numpy 배열로 변환해서 RGB만 XOR 연산을 해서 사진을 얻었다.<br>
```python
# pip install pillow numpy
from PIL import Image
import numpy as np

A = Image.open("flag.png").convert("RGBA") 
B = Image.open("lemur.png").convert("RGBA")

a = np.array(A, dtype=np.uint8)  
b = np.array(B, dtype=np.uint8)

rgb = a[..., :3] ^ b[..., :3]
alpha = np.full((a.shape[0], a.shape[1], 1), 255, np.uint8)

out = np.concatenate([rgb, alpha], axis=2)
Image.fromarray(out, mode="RGBA").save("xor.png")
```

![image](/assets/img/security/cryptohack/xor.png)
crypto{X0Rly_n0t!}