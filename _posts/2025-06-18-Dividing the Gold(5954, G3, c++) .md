---
title: Dividing the Gold(5954, G3, c++)
date: 2025-06-18 00:01:34 +09:00
last_modified_at: 2025-06-18 00:34:18 +09:00
categories: [algorithm, boj]
tags:
  [
    '다이나믹 프로그래밍',
    '배낭 문제',
    '백준',
    '5954'
  ]
---
# **Dividing the Gold**

[Dividing the Gold](https://www.acmicpc.net/problem/5954)

### 문제 
**(GPT 번역)**<br>
Bessie와 Canmuu가 금화 N개가 든 자루를 발견했고, 이걸 가능한 한 공평하게 둘로 나누려 합니다.<br>
i번째 동전의 가치는 vi이며, 각 값은 1 이상 2000 이하입니다.<br>
두 소가 더미를 최대한 비슷한 가치가 되도록 나누고 싶지만, 항상 정확히 같게 나눌 수 있는 것은 아닙니다.<br>
두 더미의 가치 차이가 최소가 되도록 나눌 때 그 차이의 최솟값을 구하세요.

또한, 그 최소 차이를 달성하는 나눔 방법이 여러 가지일 수 있습니다.<br>
두 소는 그렇게 “가장 공정하게” 나누는 방법의 가짓수도 알고 싶어 합니다.<br>
만약 정확히 같게 나눌 수 없다면, Bessie가 더 가치가 큰 쪽을 가져갑니다.

예를 들어, 동전 다섯 개의 가치가 2, 1, 8, 4, 16이라면, 한 쪽 더미는 16 하나,<br>
다른 쪽은 1+2+4+8=15가 됩니다. 차이는 16-15=1입니다.<br>
이렇게 나누는 방법은 이 경우 딱 한 가지뿐이므로, 최적 분할의 가짓수는 1입니다.

같은 가치를 가진 동전들은 서로 자리를 바꿔 더 많은 최적 분할을 만들 수도 있습니다.<br>
예를 들어 {1, 1, 1, 1}은 두 동전씩 나누는 최적 분할이 서로 다른 방법으로 여섯 가지가 됩니다.

### 입력
첫 줄: 정수 N (1<=N<=250)<br>
둘째 줄부터 N+1줄: i+1번째 줄에 vi가 주어집니다.

### 출력
첫째 줄에는 두 더미 가치 차이의 최솟값, 둘째 줄에는 위의 줄에서의 최소 차이를 달성하는 분할 방법의 가짓수.<br>
이 값이 매우 클 수 있으므로 1,000,000으로 나눈 나머지를 출력합니다.

### 풀이
가치의 총합의 최대는 2000*250 = 50만으로 배낭 문제로 해결할 수 있었다.<br>
절반을 기준으로 나누기에 입력 받은 가치의 총 합을 tt로 놓고 tt/2의 범위 안에<br>
각각 물건이 들어갈 수 있는지 이차원 배열로 확인하면서 더해주면 된다.<br>

mod가 있을 때 주의해야 하는 부분이 있어서 기억에 남는데 만약 %mod로 처리를 한다면 값이 0이 될 수 있다.<br>
그러면 chk 배열이 0이 되는 것이므로 값을 찾을 수가 없게 된다.. 그래서 mod를 넘어갈 때 -=mod를 해서 해결했다.<br>
다른 것은 배낭 문제로 전환하는 아이디어만 떠올린다면 쉽게 풀 수 있던 문제 같다.

```c++
#define ll long long
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <stack>
#include <set>
#include <functional>
#include <algorithm>
#include <math.h>
#include <iomanip>
#include <map>
using namespace std;
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    long long mod = 1'000'000;

    bool flag = 0;
    long long a, b, c, d;
    long long n, m, t, k = 0;
    long long ans = 1e16;

    cin >> n;
    vector<int> v(n);
    int tt=0;
    for(int i=0;i<n;i++){
        cin >> v[i];
        tt+=v[i];
    }
    vector<int> chk(tt/2+1, 0);
    if(v[0]<=tt/2)chk[v[0]]=1;

    for(int i=1;i<n;i++){
        for(int j=tt/2;j>=0;j--){
            if(chk[j]){
                if(j+v[i]<=tt/2){
                    chk[j+v[i]]+=chk[j];
                    if(chk[j+v[i]]>mod)chk[j+v[i]]-=mod;   
                }
            }
        }
        if(v[i]<=tt/2){
            chk[v[i]]++;
        }
    }
    for(int i=tt/2;i>=0;i--){
        if(chk[i]){
            cout << tt-2*i<<'\n';
            cout << chk[i]%mod;
            break;
        }
    }cout<<'\n';
    if(n==1)cout<<v[0]<<'\n'<<"1";

    return 0;
}
```