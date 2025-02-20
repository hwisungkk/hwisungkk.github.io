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

마라톤에 나와 푼 영어 문제라 눈에 안 들어온다..

---
### 풀이
MCMF라는 유체 알고리즘 문제라는데 그리디로 풀었다.<br>
구현이 좀 길긴한데 단순해서 생각난 대로 짰더니 잘 됐다.

아이디어는 A거리 - B거리의 절댓값으로 정렬해서 A에 가는게 이득이면 A에 넣고,<br>
B에 가는게 이득이면 B에 넣고 한 쪽이 꽉차면 남은거 전부 다른 쪽에서 가져오게 했다.<br>
총 풍선의 개수가 구할 풍선의 개수보다 작다고 써있기 때문에 가능하다. 

입력 케이스가 왜 0 0 0으로 끝나나 했는데 while(1)로 여러 테스크 케이스를 받아야 풀리는 문제였다.<br>
시간초과도 안 나는게 O(N)으로 종료된다.

```c++
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <array>
#include <tuple>
#include <algorithm>
#include <math.h>
using namespace std;
int al = 0;
int max = 10001;
int dx[4] = {0, 0, -1, 1};
int dy[4] = {-1, 1, 0, 0};
int ar[102][102] = {0};
int main()
{
    ios_base ::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    while (1)
    {
        long long a, b, c, total = 0;
        long long block = 0;
        int n, m, k;
        int bal, ad, bd;
        cin >> n >> a >> b;
        if(n+a+b==0)break;
        vector<tuple<int, int, int>> v;
        vector<pair<int, int>> index1;
        vector<pair<int, int>> index2;
        for (int i = 0; i < n; i++)
        {
            cin >> bal >> ad >> bd;
            v.push_back({bal, ad, bd});
            if (ad - bd >= 0)
                index1.push_back({ad - bd, i});
            if (ad - bd < 0)
                index2.push_back({ad - bd, i});
        }

        sort(index1.begin(), index1.end(), greater<>()); // 양수면 큰 순서 //b에 가야함
        sort(index2.begin(), index2.end());              // 음수면 작은 순서 //a에가야함
        int check_a = 0, check_b = 0;
        for (int i = 0; i < index1.size(); i++)
        {
            if (b >= get<0>(v[index1[i].second]))
            {
                b -= get<0>(v[index1[i].second]);
                total += get<0>(v[index1[i].second]) * get<2>(v[index1[i].second]);
                get<0>(v[index1[i].second]) = 0;
            }
            else
            {
                total += get<2>(v[index1[i].second]) * b;
                get<0>(v[index1[i].second]) -= b;
                b = 0;
                check_b = 1;
            }
        }
        for (int i = 0; i < index2.size(); i++)
        {
            if (a >= get<0>(v[index2[i].second]))
            {
                a -= get<0>(v[index2[i].second]);
                total += get<0>(v[index2[i].second]) * get<1>(v[index2[i].second]);
                get<0>(v[index2[i].second]) = 0;
            }
            else
            {
                total += get<1>(v[index2[i].second]) * a;
                get<0>(v[index2[i].second]) -= a;
                a = 0;
                check_a = 1;
            }
        }
        if (check_a)
        { // a가 0이면 b에 다 줘야함.
            for (int i = 0; i < index1.size(); i++)
            {
                total += get<0>(v[index1[i].second]) * get<2>(v[index1[i].second]);
            }
            for (int i = 0; i < index2.size(); i++)
            {
                total += get<0>(v[index2[i].second]) * get<2>(v[index2[i].second]);
            }
        }
        else if (check_b)
        { // b가 0이면 a에 다 줘야함.
            for (int i = 0; i < index1.size(); i++)
            {
                total += get<0>(v[index1[i].second]) * get<1>(v[index1[i].second]);
            }
            for (int i = 0; i < index2.size(); i++)
            {
                total += get<0>(v[index2[i].second]) * get<1>(v[index2[i].second]);
            }
        }
        // 음수면 a쪽
        // 양수면 b쪽
        cout << total << '\n';
    }

    return 0;
}
```
