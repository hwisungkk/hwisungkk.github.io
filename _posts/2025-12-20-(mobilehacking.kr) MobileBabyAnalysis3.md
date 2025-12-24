---
title: (mobilehacking.kr) MobileBabyAnalysis3
date: 2025-12-20 08:11:02 +09:00
last_modified_at: 2025-12-20 08:30:25 +09:00
categories: [wargame, mobilehacking.kr]
tags:
  [
    'mobilehacking.kr',
    'reversing'
  ]
---

MobileBabyAnalysis3 문제이다.<br>
앱을 실행해보면

![image](/assets/img/wargame/mobilehacking.kr/MobileBabyAnalysis3_1.PNG)

힌트이다.

![image](/assets/img/wargame/mobilehacking.kr/MobileBabyAnalysis3_2.PNG)

root cause를 분석하라는데 이것이 native code를 분석하라는 뜻 같다.<br>

apk파일을 열어보면 libbabyanalysis.so 파일이 있고 ida로 까보면 generate_flag함수가 있다.<br>

```c
__int64 __fastcall generate_flag(__int64 a1)
{
  __int64 i; // rax
  __int64 v2; // rbx

  *(_OWORD *)a1 = 0;
  *(_QWORD *)(a1 + 16) = 0;
  for ( i = 0; i != 36; i = v2 )
  {
    v2 = i + 1;
    std::string::push_back(a1, (unsigned int)(char)((i + 1) ^ byte_13970[i]));
  }
  return a1;
}
```
이 후 byte_13970의 값을 가지고 역연산 해주면 flag가 나온다.
```
.rodata:0000000000013970 byte_13970      db 67h, 6Eh, 62h, 63h, 7Eh, 74h, 62h, 7Eh, 6Ch, 2 dup(78h)
.rodata:0000000000013970                                         ; DATA XREF: generate_flag(void)+18↓o
.rodata:0000000000013970                                         ; encrypt_flag(void)+2F↓o
```

flag{reverse_the_native_code_and_me}