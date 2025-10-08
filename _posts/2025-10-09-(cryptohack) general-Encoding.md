---
title: (cryptohack) general-Encoding
date: 2025-10-09 18:10:01 +09:00
last_modified_at: 2025-10-09 23:44:15 +09:00
categories: [wargame, cryptohack.org]
tags:
  [
    'cryptohack.org',
    'general'
  ]
---

이어서 풀어보자.<br>

# General

## Encoding

### ASCII
```
ASCII is a 7-bit encoding standard which allows the representation of text using the integers 0-127.

Using the below integer array, convert the numbers to their corresponding ASCII characters to obtain a flag.

[99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
```

숫자를 문자 치환하면 되는 문제이다.<br>
```python
a=[99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

for i in a:
    print(chr(i), end="")
```
결과는 crypto{ASCII_pr1nt4bl3}

### Hex
```
63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d
```
hex 문자열을 알파벳으로 바꾸면 된다.

```python
s="63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"

print(bytes.fromhex(s))
```
결과는 b'crypto{You_will_be_working_with_hex_strings_a_lot}'

### base64
```
72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf
```
문자열이 주어지는데 byte값으로 한 번 바꾼다음 base64 인코딩을 해달라고 한다.<br>
```python
import base64
s="72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
s2=bytes.fromhex(s)
print(base64.b64encode(s2))
```
하면 b'crypto/Base+64+Encoding+is+Web+Safe/' 가 나온다.<br>

### Bytes and Big Integers
```
11515195063862318899931685488813747395775516287289682636499965282714637259206269
```
정수가 주어지고 이것을 bytes로 바꾸면 된다고 한다.<br>
```python
from Crypto.Util.number import *

a=11515195063862318899931685488813747395775516287289682636499965282714637259206269

s=long_to_bytes(a)

print(s)
```

결과는 crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}

### Encoding Challenge

```python
if encoding == "base64":
  encoded = base64.b64encode(self.challenge_words.encode()).decode() # wow so encode
elif encoding == "hex":
  encoded = self.challenge_words.encode().hex()
elif encoding == "rot13":
  encoded = codecs.encode(self.challenge_words, 'rot_13')
elif encoding == "bigint":
  encoded = hex(bytes_to_long(self.challenge_words.encode()))
elif encoding == "utf-8":
  encoded = [ord(b) for b in self.challenge_words]
```
이렇게 5가지의 암호문이 와서 오는대로 해독하는 과정을 100번 하면 된다.<br>

```python
from pwn import * # pip install pwntools
import json
import base64
import codecs
from Crypto.Util.number import long_to_bytes

r = remote('socket.cryptohack.org', 13377, level = 'debug')

def json_recv():
    line = r.recvline()
    return json.loads(line.decode())

def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.sendline(request)

while True:
    received = json_recv()

    print("Received type: ")
    print(received["type"])
    print("Received encoded value: ")
    print(received["encoded"])

    s=received["encoded"]

    if received["type"]=="hex":
        change = bytes.fromhex(s).decode()
    if received["type"]=="utf-8":
        change=""
        for i in s:
            change+=chr(i)
    if received["type"]=="base64":
        change=base64.b64decode(s).decode()
    if received["type"]=="bigint":
        change = long_to_bytes(int(s, 16)).decode()
    if received["type"]=="rot13":
        change = codecs.decode(s, 'rot_13')

    to_send = {
        "decoded": change
    }
    json_send(to_send)

```
앞까지 사용한 것들을 이용하는데, byte로 나오는 결과물은 .decode()로 str로 바꾼다.<br>
rot13은 모듈에 있는 것을 가져다가 썼고, bigint 주어지는 값이 hex라 int로 변환했다.