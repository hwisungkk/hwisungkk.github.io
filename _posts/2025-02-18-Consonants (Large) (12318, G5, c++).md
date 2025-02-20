---
title: Consonants (Large) (12318, G5, c++)
date: 2025-02-18 21:11:23 +09:00
last_modified_at: 2025-02-18 21:24:11 +09:00
categories: [algorithm, boj]
tags:
  [
    '브루트 포스',
    '백준',
    '12318'
  ]
---
# **Consonants (Large)**

[Consonants (Large)](https://www.acmicpc.net/problem/12318)

마라톤에 나와 푼 영어 문제라 눈에 안 들어온다..<br>
코드 잼 2013년 문젠데 이게 왜 골드 5지.. 생각하기 어려운 애드 훅 같은데 

문제는 이런 입력에서
```
예제 입력 1 
4
quartz 3
straight 3
gcj 2
tsetse 2

예제 출력 1 
Case #1: 4
Case #2: 11
Case #3: 3
Case #4: 11
```
문자열의 모든 집합 중 자음이 뒤에 입력된 숫자보다 많이 연속으로 있는 집합의 개수를 센다.<br>
어떻게 해야할지 모르겠어서 editorial을 찾아서 풀었다.<br>
생각을 해보면 집합 중 뒤에 남은 부분 * 여태까지 안 한 앞의 부분이 되는 거기는 하네요. 어렵다.<br> 


```c++
#include <iostream>
#include <vector>
#include <string>
#include <tuple>
#include <stack>
#include <set>
#include <queue>
#include <algorithm>
#include <math.h>
using namespace std;
int main()
{
    ios_base ::sync_with_stdio(false); 
    cin.tie(NULL);
    cout.tie(NULL);
    
    long long n, m, k;
    int a, b, c;

    int t = 0;
    cin >> t;
    b=0;
    while(t-->0){
        b++;
        c=0, m=0;
        long long count=0;
        string s;
        cin >> s >> a;
        n = s.length();
        vector <int> v(n,0);
        for(int i=0;i<s.size();i++){
            if(s[i]!='a' && s[i]!='e' && s[i]!='i' && s[i]!='o' && s[i]!='u')c++;
            else c=0;
            v[i]=c;

            if(v[i]>=a){
                count+= (i-a-m+2)*(n-i);
                m = i-a+2;
            }
        }

        cout << "Case #" << b << ": " << count << '\n';
    }
    return 0;
}
```
