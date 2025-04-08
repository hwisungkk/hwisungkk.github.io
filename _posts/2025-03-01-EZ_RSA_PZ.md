---
title: EZ_RSA_PZ
date: 2025-03-01 02:23:02 +09:00
last_modified_at: 2025-03-01 03:14:55 +09:00
categories: [wargame, dreamhack]
tags:
  [
    'dreamhack',
    'crypto',
    'level 4'
  ]
---

![image](/assets/img/wargame/dreamhack/ezrsapz_4.PNG)

[EZ-RSA-PZ](https://dreamhack.io/wargame/challenges/991)

오랜만에 드림핵 문제를 붙잡고 풀어봤다.<br>
방학 때 암호학 스터디를 하면서 계속 암호학 공부를 하다보니 문제가 풀고 싶어졌다.<br>

딱 풀고서 블로그에 깔끔하게 정리해야지 생각을 했는데 찾아보니 dreamhack 롸업은 별로 없는 것 같다.<br>
기본 새싹~레벨 1 정도까지는 많은데 그 이상은 없고 다 비공개 처리한 글들 밖에 없는 것 같아요.<br>

아무래도 치팅때문에 flag 공개는 커녕 풀이도 공개하면 안되는 것 같은 느낌이다.<br>
문제를 풀어야지만 드림핵 사이트에서만 보게 할 수 있는 느낌으로 관리하는 것 같아..<br>
그렇기에 해설은 따로 notion에다가 정리하고 문제 후기? 처럼만 적어야 할 것 같다.<br>

어차피 치팅으로 올린 것은 실력이 아닌데 이렇게 방지할 필요가 있나 싶기도 하지만 문제 출제자 입장에서<br>
자기가 만든 문제 답이 퍼지거나, 아무나 치팅 딸깍해서 내고 있는 걸 보면 좀 화날 것 같기도 하고.. 그렇네요.

### 문제

![image](/assets/img/wargame/dreamhack/ezrsapz_1.PNG)

문제 파일에는 html 하나와 각 문제의 flag를 저장할 수 있는 파일이 주어진다.<br>
열어보면 총 9개의 문제가 주어지고, 각각이 RSA 문제여서 flag를 구하면 된다.<br>

![image](/assets/img/wargame/dreamhack/ezrsapz_2.PNG)

문제가 되게 귀여운게 한 문제를 풀 때마다 퍼즐이 나오고 거기에 FLAG가 적혀있다.<br>
총 9문제니까 3x3 퍼즐이 맞춰져서 답을 구할 수 있다.<br>

문제에 각각 힌트도 써져있어서 기본적인 RSA 계산 방법부터 확장 유클리드를 이용한 공격, 하스타드 공격<br>
Franklin-Reiter Related Message Attack 까지 한 문제로 여러 문제를 풀 수 있다는게 재밌었다.<br>

```python
from sage.all import *
```
이 sage.all 모듈을 처음 써봤는데 굉장히 유용할 것 같다.<br>
식을 만들어서 미지수 계산하는 것도 되고, 큰 수 계산에도 좋고, 제곱근도 잘 구해준다.<br>
점점 수학 문제들에 부딪히고 있는데 사용 방법에 익숙해지는 것이 좋을 것 같다.<br>

아무튼 이렇게 다 풀면 FLAG가 나옵니다.<br>
![image](/assets/img/wargame/dreamhack/ezrsapz_3.PNG)

정답 맞추고 싱글벙글 블로그에 해설 쓸 생각이었는데 아쉽게도 혼자만 봐야 할 것 같다.<br>
앞으로 풀 문제들도 다 이렇게 적어야하려나.. 