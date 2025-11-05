---
title: Banana Bunches(23263, G1, c++)
date: 2025-10-28 11:05:34 +09:00
render_with_liquid: false
last_modified_at: 2025-10-28 11:34:11 +09:00
categories: [algorithm, boj]
tags:
  [
    '누적합',
    '다이나믹 프로그래밍',
    '백준',                  
    '23263'
  ]
---
# **Banana Bunches**

[Banana Bunches](https://www.acmicpc.net/problem/23263)

### 문제 
(gpt 번역)<br>
바버라는 앨런의 바나나 농장에 갔습니다. 농장에는 일렬로 나무가 N그루 있고, 이를 배열 B로 나타냅니다.<br>
i번째 나무에는 Bi개의 바나나 송이가 있습니다.<br>
모든 나무의 가격은 동일하며, 한 나무를 사면 그 나무의 바나나를 전부 얻습니다.

앨런은 줄에 빈틈이 너무 많이 생기는 것을 원치 않아, 바버라가 살 수 있는 구간을 많아야 두 개의 연속된 구간으로 제한합니다.

바버라는 장바구니 용량 K에 정확히 맞게 바나나를 담고 싶습니다.<br>
즉, 산 나무들에 달린 바나나 송이의 총합이 K가 되어야 합니다. 이때 지출을 최소화하고 싶습니다.<br>
최소 몇 그루의 나무를 사야 할까요?

### 입력
첫 줄에 테스트 케이스의 수 T가 주어집니다.<br>
각 테스트 케이스는 두 정수 N과 K로 시작합니다. N은 나무의 수, K는 바구니 용량입니다.<br>
다음 줄에는 길이 N의 비음수가 주어지며, i번째 값 Bi는 i번째 나무의 바나나 송이 수를 의미합니다.

### 출력
각 테스트 케이스마다 “Case #x: y”를 한 줄에 출력합니다. x는 1부터 시작하는 테스트 케이스 번호이고,<br>
y는 합이 K가 되도록 최대 두 개의 연속 구간만 사용해 살 때 필요한 최소 나무 수입니다.<br>
불가능하다면 -1을 출력합니다.

### 풀이
문제는 간단한데 배열에서 두 덩어리를 골라서 K개의 총합을 만들 수 있는지, 그 때의 최소 지출을 구한다.<br>
우선 한 덩어리일 때는 두 포인터로 O(N)에 쉽게 구할 수 있는 것을 알고 있다.<br>

그런데 두 덩어리를 고르는데도 조건이 N<=5000이므로 O(N^2)을 사용해야 하는 것 같다.<br>
간단하게 O(N^3)을 구현했더니 test set 2까지만 통과했다.<br>

그래서 어떻게 테크닉을 써야할까 고민하다가 1부터 n까지 이동하는 기준점 i를 하나 두고,<br>
배낭 문제를 풀 때처럼 덩어리의 총합을 index, 값을 가격으로 두는 BB 라는 배열을 만들었다.<br>

BB라는 배열은 i~1번째까지 누적 합처럼 계산한 값을 통해서 계속 업데이트 된다.<br>
그리고 i+2~n번째도 누적합처럼 계산하면서 BB[K-(i+2~n의 누적합)]에 값이 있으면 만들 수 있다는 것이므로<br>
ans에 min을 통해 값을 넣어주면 된다.<br>

BB 배열에는 계속 값이 쌓이기 때문에 모든 왼쪽 방향의 누적합으로 계산된 값이 저장되고<br>
오른쪽에서도 모든 케이스에 대해 계산을 하기에 모든 케이스에 대해 계산할 수 있다.<br>

```c++
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    long long mod = 998'244'353;
    using P=pair<ll,ll>;

    long long a, b, c, d;
    ll ans=0;

    cin >> t;
    for(int tc=1;tc<=t;tc++){
        ans=1e9;
        cin >> n >> m;
        
        vector<ll> v(n);
        for(int i=0;i<n;i++)cin>>v[i];

        vector<ll> BB(m+1, 1e9);

        for(int i=0;i<n;i++){ // 얘를 기준으로 잘라
            a=0;
            for(int j=i;j>=0;j--){
                a+=v[j];
                if(a>m)break;
                if(BB[a]>i-j+1){
                    BB[a]=i-j+1;
                }
            }
            a=0;
            for(int j=i+2;j<n;j++){
                a+=v[j];
                if(a>m)break;
                if(BB[m-a]!=1e9){
                    ans=min(ans, BB[m-a]+j-i-1);
                }
            }
        }
        ans=min(ans, BB[m]);
        // for(int i=0;i<=m;i++){
        //     cout << BB[i] << ' ';
        // }cout<<'\n';
        if(ans==1e9)ans=-1;
        cout << "Case #" << tc << ": " << ans << '\n';
    }

    return 0;
}
```