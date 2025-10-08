---
title: (cryptohack) register, Introduction
date: 2025-10-08 04:23:02 +09:00
last_modified_at: 2025-10-08 05:33:32 +09:00
categories: [wargame, cryptohack.org]
tags:
  [
    'cryptohack.org',
    'regiester'
  ]
---

예전에 암호학 잠깐 공부할 때 풀었던 사이트인데<br>맨 처음부터 다시 풀고자 계정도 새로 만들고 공부하려고 합니다.

## register
암호학 CTF 사이트 답게 가입할 때 카이사르 암호를 풀게 한다.<br>

![image](/assets/img/security/cryptohack/register_1.PNG)

주어진 문자열을 간단하게 파이썬으로 한글자씩 돌려보면 된다.<br>

```python
s="EQNMS RGZES RKZL UHBHNTR"

for i in range(0, 26):
    for j in s:
        if j!=' ':
            if chr(ord(j)+i)>'Z':
                print(chr(ord(j)+i-26), end="")
            else:
                print(chr(ord(j)+i), end="")
        else:
            print(" ", end="")
    print()
```

의 결과를 보면 
```
EQNMS RGZES RKZL UHBHNTR
FRONT SHAFT SLAM VICIOUS
GSPOU TIBGU TMBN WJDJPVT
HTQPV UJCHV UNCO XKEKQWU
IURQW VKDIW VODP YLFLRXV
JVSRX WLEJX WPEQ ZMGMSYW
...
```
2번째에 문자같은 것이 있으니 그걸 넣어주면 됐다.<br>

## introduction

간단하게 파이썬 코드 돌려보기, pwntools 써보기다.<br>
주어진 코드를 실행하면 flag가 나오니 집어넣으면 된다.