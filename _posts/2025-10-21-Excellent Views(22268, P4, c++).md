---
title: Excellent Views(22268, P4, c++)
date: 2025-10-21 19:02:58 +09:00
render_with_liquid: false
last_modified_at: 2025-10-21 21:33:15 +09:00
categories: [algorithm, boj]
tags:
  [
    '정렬',
    '백준',                  
    '22268'
  ]
---
# **Excellent Views**

[Excellent Views](https://www.acmicpc.net/problem/22268)

### 문제 
Shiny City는 단 한 개의 거리, 모든 건물의 높이가 서로 다르다.<br>
그리고 건물 옥상에서 보이는 압도적인 경관으로 유명한 아름다운 도시다.

팬데믹 이후 관광객이 크게 줄었다. 당신은 멋진 블로그 글을 써서 더 많은 관광객을 끌어오고,<br>
사랑스럽지만 비효율적인 도시의 재정 위기를 막고자 한다. 하지만 아직 블로그에 필요한 정보가 조금 부족하다.

Shiny City에는 건물이 N개 있으며, i번째 건물의 위치는 i로 표시된다.<br>
건물 i에서 건물 j로 이동하는 데 걸리는 시간은 |i − j|분이다.<br>
각 건물의 높이 Hi는 모두 다르고, 건물이 높을수록 옥상 전망이 더 좋다.

어떤 건물에 있을 때, 더 나은 전망을 위해 다른 건물로 이동할 가치가 있을 수 있다.<br>
다만 이동 시간 때문에, 같은 시간 이하로 도달할 수 있는 더 높은 건물이 있다면 그 건물로 갈 가치는 없다.

정확히는, 건물 i에서 다른 건물 j로 가는 것이 가치가 있으려면 다음을 만족해야 한다:<br>
```
∣i−k∣≤∣i−j∣ 이면서 Hj<Hk인 건물 k가 존재하지 않아야 한다. (여기서 k는 i일 수도 있다.)
```
당신은 블로그에, 각 건물 i 에서 이동할 가치가 있는 다른 건물의 개수를 쓰고 싶다.<br>
이 정보를 모아 도시를 구해 보자.

### 입력
첫째 줄에 건물의 수 N(1≤N≤10^5)이 주어진다.<br>
둘째 줄에 서로 다른 정수 H1, H2, .. HN이 주어지며 (1<=Hi<=10^9)가 주어지며<br>
Hi는 i번째 건물의 높이이다.<br>

### 출력
한 줄에 정수 N개를 출력한다.<br>
i번째 정수는 건물 i에서 이동할 가치가 있는 건물의 개수를 나타낸다.

### 풀이
그리디 문제로 아이디어랑 set을 이용해야 한다는 것이 어려웠다.<br>
좌표 압축의 아이디어와도 비슷했다고 생각이 든다.<br>

각각의 위치에서 거리 상 가까우면서 어떤 건물보다 높은 건물이 없다면 해당 건물로 이동할 수 있다.<br>
출력하는 값은 각각의 위치에서 최대 몇 개의 건물을 이동할 수 있는지 묻는 문제이다.

이 문제를 다르게 보면 어떤 위치에서 자신에게 올 수 있는 건물을 파악하고 그 위치에 +1씩 해주는 문제이다.<br>
각 위치에서 왼쪽과 오른쪽에 있는 자신보다 큰 건물을 찾으면, 큰 건물과 자기 위치 사이에 있는 건물들을 보면<br>
모두 자기보다 작은 건물이다. 이 중 큰 건물보다 자신에 가까운 건물들은 자신에게 올 수 있는 건물들이다.<br>

그 범위에 대해 +1씩 해주면 되고 이는 imos법을 사용하면 구할 수 있다.<br>
그러면 왼쪽과 오른쪽에 있는 높은 건물을 찾는 방법을 생각해야 하는데 정렬해서 높은 건물부터 탐색을 시작하고,<br>
탐색 후에 그 건물의 index를 set에 넣어준다.<br>
그러면 set안에는 탐색하는 건물보다 큰 건물의 index만 들어가기 때문에 lower_bound를 통해서 찾을 수 있게 된다.

```c++
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    long long mod = 998'244'353;
    using P=pair<ll,ll>;

    long long a, b, c, d;
    long long ans = 0;

    cin >> n;

    vector<int> v(n);
    vector<P> tmp(n);
    for(int i=0;i<n;i++){
        cin>>v[i];
        tmp[i].first=v[i];
        tmp[i].second=i;
    }
    sort(tmp.begin(), tmp.end()); //높이랑 index
    vector<int> nu(n+1, 0);

    set<ll> st; // 더 높은 건물의 index를 여기에 담게 되는 거지

    int right;
    int left;
    for(int i=n-1;i>=0;i--){
        auto it=st.lower_bound(tmp[i].second);
        if(it!=st.begin()) left=*prev(it);
        else left=-1;
        if(it!=st.end()) right=*it;
        else right=-1;

        //우선 왼쪽부터
        if(left==-1){ //처음부터 증가해서
            nu[0]++;
        }
        else{
            int cha=tmp[i].second-left;
            nu[tmp[i].second-(cha-1)/2]++;
        }
        nu[tmp[i].second]--;
        if(right==-1){ //자기 다음부터 끝까지
            nu[tmp[i].second+1]++;
        }
        else{
            int cha=right-tmp[i].second;
            nu[tmp[i].second+1]++;
            nu[tmp[i].second+(cha+1)/2]--;
        }
        st.insert(tmp[i].second); 
        // cout << tmp[i].second << ' ' << left << ' ' << right << '\n';
        // for(int k=0;k<n;k++){
        //     cout << nu[k] << ' ';
        // }cout << '\n';
    }

    string sp="";
    for(int i=0;i<n;i++){
        if(i>0)nu[i]+=nu[i-1];
        cout << sp << nu[i];
        sp=" ";
    }


    return 0;
}

//3 2 1 2 2 1 0 1 2 1
```