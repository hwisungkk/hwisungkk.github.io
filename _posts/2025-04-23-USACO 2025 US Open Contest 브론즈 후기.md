---
title: USACO 2025 US Open Contest 브론즈 후기
date: 2025-04-23 01:15:26 +09:00
last_modified_at: 2025-04-23 03:25:58 +09:00
categories: [algorithm, USACO]
tags:
  [
    'USACO',
    '유사코',
  ]
---
# **USACO 2025 US Open Contest bronze**

![image](/assets/img/algorithm/USACO_bronze_1.PNG)<br>

예전에 과외 찾아볼 때 USACO 기준으로 수업해주세요~<br>
이런 얘기를 몇 번 들었어서 나중에 풀어봐야겠다고 생각했고 3.21~3.24에 있는 open contest에 참여했다.<br>
문제 난이도는 브론즈, 실버, 골드, 플래티넘이 있고 전 단계를 풀아야지 다음 것을 풀 수 있다.<br>

각각 3문제씩 5시간 안에 풀면 되고 시간 내에 올솔하면 바로 승급하고, 아니면 상위 퍼센티지에 따라 올라간다.<br>

그래도 백준 플레도 찍었고, 문제 풀다 나오는 USACO gold 정도는 그럭저럭 풀만 했어서 골드까진 달지 않을까?<br>
라는 기대를 했는데 생각보다 너무 어려웠다.<br>

브론즈는 3시간? 정도 써서 다 풀었는데, 실버 문제들은 3개 전부 손도 못댔다.<br>
그나마 위안이 되는 것은 백준에 올라오고 티어를 봤을 때 브론즈에서 가장 어려운 문제가 골드1~플레5 라는 것.<br>
그리고 실버 문제는 플레 2개, 아마 남은 것도 그 정도지 않을까 싶다.<br>

이걸 5시간안에 풀어야지 실버가 될 수 있다니. 허들이 상당히 높다.

![image](/assets/img/algorithm/USACO_bronze_2.PNG)<br>

## 간단한 풀이

### Hoof Paper Scissors Minus One(33769, S4)
가위바위보 하나 빼기를 해서 상대가 낼 조합을 알 때 내가 내서 이길수 있는 조합의 경우의 수를 구하는 문제이다.<br>
대신 낼 수 있는 기호가 (1<=N<=5000) 개 이고, 각각 기호끼리의 승패 여부가 주어진다.
```
D 
WD 
LWD
```
이렇게 주어지면 a[i][j]=W라면 i 기호가 j 기호에 대해 이긴다는 것이고 L이면 진다는 것이다.<br>
이를 비트마스킹으로 W일 때 1로 저장한 다음 구현하면 훨씬 쉽다.

상대가 낸 두개 중 어떤 것을 내더라도 내가 이길 수 있는 기호를 찾아야 한다.<br>
그 개수를 구하고, 만들 수 있는 경우의 수를 계산해주면 답을 구할 수 있다.

```c++
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <stack>
#include <set>
#include <functional>
#include <algorithm>
#include <math.h>
#include <map>
using namespace std;
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    int a, b, c, d;
    int n, m, t;
    char ch;
    string s;
    bool flag=0;
    cin >> n >> m;
    vector<vector<int>> win(n+1, vector<int>(n+1, 0));
    for(int i=1;i<=n;i++){
        cin >> s;
        for(int j=0;j<s.size()-1;j++){
            if(s[j]=='L'){
                win[i][j+1]=1;
            }
            else if(s[j]=='W'){
                win[j+1][i]=1;
            }
        }
    }
    for(int i=0;i<m;i++){
        cin >> a >> b;
        int cnt=0;
        long long ans=0;
        for(int j=1;j<=n;j++){
            if(win[a][j]==1 && win[b][j]==1){
                cnt++;
            }
        }
        ans += (2*n-1)*cnt;

        ans -= cnt*(cnt-1);

        cout << ans << '\n';
    }
    // for(int i=1;i<=n;i++){
    //     for(int j=1;j<=n;j++){
    //         cout << win[i][j] << ' ';
    //     }cout << '\n';
    // }



    return 0;
}
```

### More Cow Photos(33770, S3)
소의 마리 수와 키가 주어지고, 키는 (1<=N<=10^5)이다.<br>

가장 키가 큰 소를 가운데에 두고, 양 옆으로 대칭을 맞춰서 소를 세우려고 한다.<br>
소를 제거 가능할 때 최대 몇 마리의 소를 그렇게 놓을 수 있는가? 라는 문제이다.<br>
또, 키가 같은 소가 붙어있으면 안되고 올라갔다가 내려가는 산 모양으로 지켜져야 한다.<br>

간단하게 배열의 index를 소의 키, 값을 마리 수로 준 다음, 가장 처음 나온 소에서 1을 추가하고<br>
키를 줄여가며 배열의 값을 확인해서 2보다 크거나 같은 값이 있으면 ans에 2씩 더해주면 된다.

```c++
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <stack>
#include <set>
#include <functional>
#include <algorithm>
#include <math.h>
#include <map>
using namespace std;
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    int a, b, c, d;
    int n, m, t;
    char ch;
    bool flag=0;
    cin >> t;

    while(t-->0){
        cin >> n;
        long long ans=0;
        vector<int> v(n+1, 0);
        for(int i=0;i<n;i++){
            cin >> a;
            v[a]++;
        }
        bool flag=0;
        for(int i=n;i>=1;i--){
            if(flag==0 && v[i]>=1){
                ans+=1;
                flag=1;
            }
            else if(flag){
                if(v[i]>=2)ans+=2;
            }
        }
        cout << ans << '\n';
    }
    return 0;
}
```

### It's Mooin' Time III(33771, P5)
풀 때 굉장히 힘들었던 것이 이분 탐색 문제라서 index가 조금 어긋나면 답이 안나오는데<br>
실수가 있었어서 디버깅 하는 과정이 힘들었다.

abcabbacabac 이런 문자열이 하나 주어지고, s[i] != s[j], s[j]==s[k] 조건을 만족하는 i,j,k를 구한다.<br>
이 때 (j-i)(k-j)가 최대가 되도록 하는 i, j, k를 구하고 계산한 결과를 각 쿼리에 대해 출력한다.<br>
쿼리는 최대 3*10^4개가 주어지고, 문자열에서 i, j, k를 구할 범위 index를 준다.<br>

문자열을 이루는 것은 모두 알파벳 소문자이고, 선택할 i, j, k를 보면 s[j]==s[k]이다.<br>
우선 문자열에 대해서 2차원 배열로 어떤 글자가 몇번째 index에서 등장하는지 저장한다.<br>

쿼리로 주어진 값을 각각 a, b라고 하면 s[a]!=s[j]일 때는 i가 a일 때 (j-i)(k-j)가 된다.<br>
이 때가 j-i가 최대로 잡을 수 있기 때문이다.<br>

s[a]를 i로 고정한 후, s[j], s[k]의 위치를 이분 탐색을 통해서 (j-i)(k-j)가 가장 크게 될 때를 구해준다.<br>
다음 으로 s[a]가 i가 아닌 경우가 있을 수도 있다.<br>

그 말은 s[j]==s[k]==s[a]이고, s[i]만 따로 있는 것이므로 비슷한 방법으로 구할 수 있다.<br>

```c++
using namespace std;
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    int a, b, c, d;
    int n, m, t;
    char ch;
    string s;
    bool flag=0;
    cin >> n >> m;
    cin >> s;
    vector<vector<int>> v(26);
    for(int i=0;i<s.size();i++){
        v[s[i]-'a'].push_back(i+1);
    }
    for(int i=0;i<m;i++){
        long long ans=-1;
        cin >> a >> b;
        ch = s[a-1]; // start

        for(int j=0;j<26;j++){
            char ff = 'a'+j;
            if(ch == ff)continue;
            int l=0, r=v[j].size();
            while(l+1<r){
                int mid = (l+r)>>1;
                if(v[j][mid]<=b){ //close b
                    l = mid;
                }
                else r = mid;
            }
            if(l<=0)continue;
            //cout << v[j][l] << '\n';

            int ll=0, rr=l-1;
            while(ll+1<rr){
                int mid = (ll+rr)>>1;
                if(v[j][mid]<=(a+v[j][l])/2){ //close b
                    ll = mid;
                }
                else rr = mid;
            }
            if(ll<0 || ll >= l)continue;
            else ans = max(ans, ((long long)v[j][l]-v[j][ll])*(v[j][ll]-a));
            if(rr<0 || rr >= l)continue;
            else ans = max(ans, ((long long)v[j][l]-v[j][rr])*(v[j][rr]-a));
        }
        int j = ch-'a';
        while(s[a-1]==ch){
            a++;
            if(a==b){
                a=1e9;
                break;
            }
        }
        int l=0, r=v[j].size();
        while(l+1<r){
            int mid = (l+r)>>1;
            if(v[j][mid]<=b){ //close b
                l = mid;
            }
            else r = mid;
        }
        //cout << v[j][l] << '\n';

        int ll=0, rr=l-1;
        while(ll+1<rr){
            int mid = (ll+rr)>>1;
            if(v[j][mid]<=(a+v[j][l])/2){ //close b
                ll = mid;
            }
            else rr = mid;
        }
        if(ll>=0 && ll<l) ans = max(ans, ((long long)v[j][l]-v[j][ll])*(v[j][ll]-a));
        if(rr>=0 && rr<l) ans = max(ans, ((long long)v[j][l]-v[j][rr])*(v[j][rr]-a));

        cout << ans << '\n';
    }
    return 0;
}
```

내년에는 usaco gold를 목표로.. 열심히 하자.