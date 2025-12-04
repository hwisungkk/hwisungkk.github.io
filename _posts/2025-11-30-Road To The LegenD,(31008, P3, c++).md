---
title: Road To The LegenD, (31008, P3, c++)
date: 2025-11-30 05:33:41 +09:00
render_with_liquid: false
last_modified_at: 2025-11-30 07:12:44 +09:00
categories: [algorithm, boj]
tags:
  [
    '다익스트라',
    '최단 경로',
    '백준',                  
    '31008'
  ]
---
# **Road To The LegenD,**

[Road To The LegenD,](https://www.acmicpc.net/problem/31008)

### 문제 
토니는 LegenD가 되기 위한 여정에 나섰다.<br>
기나긴 여정 중에는 1부터 N까지 번호가 붙은 N개의 마을을 거칠 수 있고, u번 마을에는 마을의 격 h_u가 있다.<br>
토니는 처음에 1번 마을에 있다. 신이 정한 특정한 마을에 도달하면 토니는 LegenD가 될 수 있다.

마을 사이를 잇는 길은 두 종류로 편한 길과 고행의 길이 있다.

편한 길은 고대부터 존재하던 길로 총 M개가 있으며 i번째 편한 길을 통하면 u_i번 마을에서 v_i번 마을로<br>
시간 t_i를 들여 이동할 수 있다. 같은 마을 번호 쌍 (u_i, v_i)에 대해 u_i번 마을에서 v_i번 마을로<br>
이동 가능한 편한 길이 둘 이상 존재할 수 있다.

고행의 길은 선대 LegenD에 의해 0개 이상 설치되었다.<br>
u번 마을과 u번 마을로부터 편한 길 정확히 하나를 지나 도착할 수 있는 마을들 중 가장 격이 높은 마을의 격을 H_u라고 하자.<br.>
1 이상 N 이하의 정수 u, v에 대해 H_u < h_v 라면 u번 마을에서 v번 마을로 가는 고행의 길이 설치되어 있다.<br>
u번 마을에서 출발하는 고행의 길 하나를 통해 v번 마을로 가는 데에는 시간 p_u가 걸린다.<br>

신은 도착하면 LegenD가 되는 마을을 정할 때,<br>
토니가 도달할 수 있는 마을 중 해당 마을에 도달하기까지 걸리는 최소시간이 가장 긴 마을을 정했다.<br>
토니가 LegenD가 되는 데 성공했다면, 걸린 시간은 최소 얼마일까?

### 입력
첫째 줄에 마을의 수 N과 편한 길의 수 M이 공백으로 구분되어 주어진다.<br>
둘째 줄에 각 마을의 격을 나타내는 정수 h_1, h_2, …, h_N이 공백으로 주어진다.

셋째 줄에 각 마을에서 출발하는 고행의 길을 지날 때 걸리는 시간을 나타내는 정수 p_1, p_2, …, p_N이 공백으로 주어진다.<br>
다음 M개 줄의 i번째 줄에 i번째 편한 길의 출발 마을 u_i, 도착 마을 v_i, 이동 시간 t_i가 주어진다.

출발 마을과 도착 마을이 같은 편한 길이 둘 이상 존재할 수 있다.

### 출력
첫째 줄에 토니가 LegenD가 되는 데 걸린 시간의 최솟값을 출력한다.

### 풀이
우선 편한 길을 그래프로 만들어주고, 격 배열도 간단히 만들 수 있다.<br>
그 다음 고행의 길을 이어줘야 하는데, 이것을 전부 그으면 N^2개의 길이 생긴다.<br>

답을 구하기 위해서는 다익스트라를 돌려야하는데, 이 선분을 모두 그을 수는 없다.<br>
그렇기에 사용할 수 있는 방법은 보조점을 그어서 그래프 모델을 새로 구성하는 것이다.<br>

![image](/assets/img/algorithm/legenD_1.PNG)

이렇게 고행의 길이 모델링 되어있다고 하자.<br>
4개의 점의 H[i]가 전부 다르고, 격을 오름차순으로 정렬한 상태라면 그림처럼 된다.<br>
이렇게 모든 선을 그으면 N^2개의 길을 긋게 된다는 것을 알 수 있다.<br>

그런데 보조점을 그리면 
![image](/assets/img/algorithm/legenD_2.PNG)

그림처럼 표현할 수 있다.<br>
오각형이 각각의 점에 대한 새로운 보조점이고, 이동하는 길은 t[i]만큼 시간이 걸린다.<br>
그리고 고행의 길은 격 배열에 따라 오름차순으로 이어지므로 서로를 이어주고, 돌아가는 길은 0이다.<br>
이렇게 하면 N개의 보조점을 통해서 고행의 길을 표현할 수 있고, 다익스트라로 문제를 해결할 수 있다.<br>

보조점을 그려서 그래프를 새로 모델링한다는 아이디어가 재밌고 감동이 있었다.

```c++
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    using P=pair<ll,ll>;

    long long mod = 1e9+7;
        
    bool flag=0; 
    long long a, b, c, d;
    long long n, m, t, k;
    long long ans = -1;

    cin >> n >> m;

    vector<ll> gyuk(n+1);
    vector<ll> tt(n+1);
    vector<ll> H(n+1);

    for(int i=1;i<=n;i++){
        cin>>gyuk[i];
        H[i]=gyuk[i];
    }
    for(int i=1;i<=n;i++){
        cin>>tt[i];
    }

    vector<vector<P>> graph(2*n+1);
    
    for(int i=0;i<m;i++){
        cin >> a >> b >> c;
        graph[a].push_back({b,c});

        H[a]=max(H[a], gyuk[b]);
    }

    // for(int i=1;i<=n;i++){
    //     cout<<H[i]<<" ";
    // }cout<<"\n";

    set<ll> gyuk_set;
    for(int i=1;i<=n;i++){
        gyuk_set.insert(gyuk[i]);
    }

    map<ll,ll> gyuk_map; //좌표를 만들어줄 곳임.
    vector<ll> gyuk_sort;
    k=0;
    for(auto at:gyuk_set){
        gyuk_map[at]=n+1+k; //gyuk[i]가 인 것의 보조점.
        k++;
        gyuk_sort.push_back(at);
    }

    for(int i=1;i<=n;i++){
        auto it=upper_bound(gyuk_sort.begin(), gyuk_sort.end(), H[i]);
        if(it!=gyuk_sort.end()){
            graph[i].push_back({gyuk_map[*it], tt[i]});
        }
    }

    for(int i=0;i<gyuk_sort.size()-1;i++){
        graph[gyuk_map[gyuk_sort[i]]].push_back({gyuk_map[gyuk_sort[i+1]], 0});
    }
    for(int i=1;i<=n;i++){
        graph[gyuk_map[gyuk[i]]].push_back({i, 0}); //돌아가는애?
    }

    vector<ll> dst(2*n+1, 1e16);
    priority_queue<P, vector<P>, greater<P>> pq;
    pq.push({0,1});
    dst[1]=0;

    while(!pq.empty()){
        ll val=pq.top().first;
        ll cur=pq.top().second;
        pq.pop();

        if(dst[cur]<val)continue;

        for(auto at:graph[cur]){
            if(dst[at.first]>val+at.second){
                dst[at.first]=val+at.second;
                pq.push({dst[at.first], at.first});
            }
        }
    }
    for(int i=1;i<=n;i++){
        //cout << dst[i] << ' ';
        if(dst[i]!=1e16 && ans<dst[i])ans=dst[i];
    }//cout<<'\n';
    cout<<ans;

    return 0;
}
```