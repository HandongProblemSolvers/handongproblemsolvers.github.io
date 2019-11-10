---
title: 11주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

요즘 너무나도 바쁘기도 하고 ㅠㅠ

문제도 좀 어려운걸 내버렸네요...

그래도 모르는 걸 마주합시다ㅠㅠ

HPS 화이팅!

그럼, 문제해설 시작~

---
## 목차
1. [직각 삼각형의 두 변](#직각-삼각형의-두-변)
2. [점](#점)
3. [버스 노선](#버스-노선)
4. [주유소](#주유소)
4. [장애물 경기](#장애물-경기)

---

## **직각 삼각형의 두 변**
### [문제링크](https://www.acmicpc.net/problem/6322)

### 문제접근 및 알고리즘
1. 피타고라스 정의를 이용하면 된다
2. a^2 + b^2 = c^2를 만족하는 각 값을 찾는다
3. 여기서 만약 (a >= c || b >= c) 인 경우 삼각형이 만들어 지지 않는다.
4. 위 한가지 경우를 제외하고 a, b, c를 찾는다
5. a = sqrt(c^2-b^2)
6. b = sqrt(c^2-a^2)
7. c = sqrt(a^2+b^2)

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 6322 yechan
#include <bits/stdc++.h>
using namespace std;

int main() {
    int T = 1;
    while (1) {
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        if (!a && !b && !c) break;
        printf("Triangle #%d\n", T);
        if (a == -1) {
            if (b >= c) puts("Impossible.");
            else printf("a = %.3lf\n", sqrt(1.f*c*c-1.f*b*b));
        }
        if (b == -1) {
            if (a >= c) puts("Impossible.");
            else printf("b = %.3lf\n", sqrt(1.f*c*c-1.f*a*a));
        }
        if (c == -1) {
            printf("c = %.3lf\n", sqrt(1.f*a*a + 1.f*b*b));
        }
        puts("");
        T++;
    }
    return 0;
}
```

---

## **점**
### [문제링크](https://www.acmicpc.net/problem/2541)

### 문제접근 및 알고리즘
1. 일단 문제를 이해해야한다.
2. 시작점 a,b가 주어질때 5개의 점이 포함되는지 여부를 판단해야한다.
3. 이때 시작점으로 부터 3가지 규칙이 존재한다.
4. (규칙 1) (x,y)가 S에 있다면, (x+1, y+1)도 존재
5. (규칙 2) (x,y)가 S에 있고, x와 y 모두 짝수면 (x/2, y/2) 존재
6. (규칙 3) (x,y)와 (y,z)가 S에 있으면 (x,z)가 존재한다
7. 여기서 a,b에 대해 접근할때 패턴을 찾으려 하였다.
8. 3가지 곧 a > b, a == b, a < b 경우로 나눠서 생각하였다.
9. 다음 규칙에서 찾아지는 신기한 점을 설명하겠다.
10. (규칙 1) a,b 이후에 기울기 1인 직선 위에 모든 점이 집합에 존재한다.
11. (규칙 2) 규칙 1를 이용하면 (a,b) = (홀수,홀수)인 경우는 무조건 (짝수, 짝수)가 존재하며, a와 b의 차이가 반으로 줄어든다. (3,5)-(4,6)-(2,3)
12. (규칙 3) 만약 a > b라면, 규칙 1에 의하면 (a-b)만큼 증가하면 곧 (a+(a-b), b+(a-b)) = (2a-b,a)로, **규칙 2로 (2a-b,a) (a,b) -> (2a-b, b)**가 존재하며 이를 계속 반복하면, **(a+(a-b)*n, b)**가 존재한다.
13. 여기서 a-b차이는 11번 형태로 a-b차이는 항상 홀수임을 알 수 있다.
14. a-b 값이 홀수 이면, (a, b)에서 a값이 짝수인 경우, (a, b+홀수*n)형태에서 (짝수, 짝수)를 찾을 수 있으며 이러한 규칙에서 (a,b)가 주어지면 (1,1+(a-b)) 규칙까지 모두 찾을 수 있다.
15. 결국 a-b의 차이가 홀수가 되도록 2로 계속 나눈다.
16. a-b값이 홀수가 되면, x-y차이가 a-b로 나뉘어 지면 선상에 존재함을 알 수 있다.
17. 여기서 (a-b)와 (x-y) 부호가 같아야 하기 때문에, (a-b) * (x-y) > 0 를 만족해야한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2541 yechan
#include <bits/stdc++.h>
using namespace std;

int main() {
    int a, b;
    scanf("%d%d", &a, &b);
    int n = abs(a-b);
    while (n>0 && n%2==0) n/=2;

    for (int i=0; i<5; i++) {
        int x, y;
        scanf("%d%d", &x, &y);
        int c = x-y;
        if (n == 0) puts((c==0)? "Y":"N");
        else puts(((abs(c)%n==0) && (1LL*c*(a-b)>0)) ? "Y":"N");
    }
    return 0;
}
```

---

## **버스 노선**
### [문제링크](https://www.acmicpc.net/problem/10165)

### 문제접근 및 알고리즘
1. 일단 이 문제는 N 값이 50만으로 자신을 포함하는 모든 요소를 확인하는 O(N^2) 알고리즘은 불가능 하다는 것을 알 수 있다.
2. 일단 생각을 단순화 하기 위해서 버스노선이 (a,b)에서 a < b를 만족한다고 하자.
3. 이때, 시작을 기준으로 정렬 적용하면, 포함되는지 여부를 O(NlogN)만에 알 수 있다.
4. 포함되는 관계는 (a1,b1), (a2,b2)에서 (a1 <= a2) && (b2 <= b1)를 만족할때 (a2,b2) 버스 노선은 (a1,b1) 버스 노선에 포함된다.
5. 여기서 정렬이 된 상태면 (i < j)인 (ai,bi)와 (aj, bj)에 대해서 (ai <= aj)를 항상 만족한다. 또한 k<=i인 bi중 가장 큰값을 bmax라 하면, (bj <= bmax)를 만족하는 경우 이 버스노선을 포함됨을 알 수 있다.
6. 자 이제 a > b인 경우를 살펴보아야한다.
7. 여기서 모든 a > b들간 관계는 5.에서 말한 내용과 같이 적용된다
8. 문제는 a > b와 a < b 간에 관계를 살펴 보아야한다.
9. 일단 a > b를 만족하는 버스중 가장 큰 a 값을 amax라고 하자. 이때 0~amax의 범위는 이미 버스가 존재하므로, a < b 버스중 b <= amax인 모든 버스는 포함된다.
10. 또한 a > b를 만족하는 버스중 가장 작은 b값을 bmax라고 하자. 이때 bmax~N까지 이미 버스가 존재하므로, a < b 버스중 bmax <= a를 만족하는 모든 버스는 포함된다.
11. 이후에 포함되지 않은 버스를 출력한다.
12. O(NlogN)으로 시간은 충분하다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10165 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=500001;

struct Point{
    int x, y, idx;
    Point():Point(0,0,0){}
    Point(int x1, int y1, int idx1):x(x1),y(y1),idx(idx1){}
    bool operator<(const Point &o) {
        if (x == o.x) return y > o.y;
        return x < o.x;
    }
};

int N, M;
vector<Point> ga, gb;
bool visited[MAX_N];

void cancel(vector<Point> &v) {
    int bound = -1;
    sort(v.begin(), v.end());
    for (Point &p: v) {
        int b = p.y;
        int idx = p.idx;
        if (b <= bound) visited[idx]=true;
        bound = max(bound, b);
    }
}

int main() {
    scanf("%d%d", &N, &M);
    int x=N, y=-1;
    for (int i=0; i<M; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        if (a < b) {
            ga.push_back(Point(a, b, i));
        } else {
            x = min(x, a);
            y = max(y, b);
            gb.push_back(Point(a, b, i));
        }
    }

    for (Point &p: ga) {
        int a = p.x;
        int b = p.y;
        int idx = p.idx;
        if (x <= a || b <= y) visited[idx]=true;
    }

    cancel(ga);
    cancel(gb);

    for (int i=0; i<M; i++)
        if (!visited[i])
            printf("%d ", i+1);
    puts("");
    return 0;
}
```

---

## **주유소**
### [문제링크](https://www.acmicpc.net/problem/13308)

### 문제접근 및 알고리즘
1. 시작점과 도착점이 정해져 있다.
2. 여기서 중요한 컨셉은 다음과 같다.
3. 기름통의 크기는 제한이 없기 때문에 그전에 미리 다 사놓을 수 있다.
4. 어느지점 까지 방문했을때, 지금까지 방문한 지점중 기름값이 제일 싼 곳에서 사는 것이 더 좋다는 것이 당연하다.
5. 여기서 이제 DP형태로 문제를 나눌 수 있다.
6. DP table를 DP(u, w)로 상황을 표현할 수 있다.
7. DP(u,w)정의는 **지점 u**에서 **1에서부터 방문하면서 가지는 최소 기름값이 w**라고 할때, **u에서 도착점 까지 가기 위한 최소 비용**이다.
8. 또한 시작점과 도착점이 정해져 있기 때문에 다익스트라 알고리즘으로 minimum cost 기준으로 방문할 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 13308 yechan
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using P = pair<int,int>;
using PP = pair<ll, P>;

const int MAX_N=2501;
const int MAX_M=4001;
const int MAX_C=2501;
const ll MAX_INF=(1LL<<60);

int N,M;
int cost[MAX_N];
bool visited[MAX_N][MAX_C];
ll dist[MAX_N][MAX_C];
vector<P> adj[MAX_N];

ll Dikstra() {
    priority_queue<PP, vector<PP>, greater<PP>> PQ;
    PQ.push(PP(0,P(cost[0],0)));
    for (int i=0; i<MAX_N; i++)
        for (int j=0; j<MAX_N; j++)
            dist[i][j] = MAX_INF;
    dist[0][cost[0]]=0;
    while (!PQ.empty()) {
        int cur_v, cur_w;
        do {
            cur_v = PQ.top().second.second;
            cur_w = PQ.top().second.first;
            PQ.pop();
        } while(!PQ.empty() && visited[cur_v][cur_w]);
        if (visited[cur_v][cur_w]) break;
        visited[cur_v][cur_w]=true;
        for (P &p: adj[cur_v]) {
            int nx = p.first;
            int d = p.second;
            int nw = min(cur_w,cost[nx]);
            if (dist[nx][nw] > dist[cur_v][cur_w] + d*cur_w) {
                dist[nx][nw] = dist[cur_v][cur_w] + d*cur_w;
                PQ.push(PP(dist[nx][nw],P(nw, nx)));
            }
        }
    }

    ll ret = MAX_INF;
    for (int i=0; i<MAX_C; i++)
        ret = min(ret, dist[N-1][i]);
    return ret;
}

int main() {
    scanf("%d%d", &N, &M);
    for (int i=0; i<N; i++)
        scanf("%d", &cost[i]);

    for (int i=0; i<M; i++) {
        int u, v, d;
        scanf("%d%d%d", &u, &v, &d);
        u--, v--;
        adj[u].push_back(P(v,d));
        adj[v].push_back(P(u,d));
    }
    printf("%lld\n", Dikstra());
    return 0;
}
```

---

## 장애물 경기
### [문제링크](https://www.acmicpc.net/problem/13303)

### 문제접근 및 알고리즘
1. 각 선들은 y축과 평행하고 겹치는 경우가 존재하지 않는다.
2. 여기서 모든 선들의 좌표를 입력받고, 먼저 x축으로 정렬하고 x축 값이 작은곳 부터 접근하자.
3. 위에서 정렬 순으로 접근할때, 존재하는 시작점의 좌표와 y축으로 움직인 거리를 set에 미리 넣어 놓을 수 있다.
4. 위에 set에 lower_bound와 upper_bound를 이용하면, 각 선들이 시작점을 포함하고 있는지 없는지를 알 수 있다.
5. 여기서 이 선 안에 있는 경우, 사이에 있는 출발점을 모두 지우고 현재 선으로 부터 만들어지는 2개 중 최소값을 set에 넣는다.
6. 모든 선을 본 뒤에, set에 담겨 있는 minimum cost 값과 개수를 새고 출력하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 13303 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_INF=1e9;

int N, sy, ex;
vector<tuple<int,int,int>> lines;

int main() {
    scanf("%d%d%d", &N, &sy, &ex);
    for (int i=0; i<N; i++) {
        int x, yl, yh;
        scanf("%d%d%d", &x, &yl, &yh);
        lines.push_back(make_tuple(x, yl, yh));
    }
    sort(lines.begin(), lines.end());
    set<pair<int,int>> s;
    s.insert(make_pair(sy,0));
    for (int i=0; i<N; i++) {
        int x, yl, yh;
        tie(x, yl, yh) = lines[i];
        auto lb = s.lower_bound(make_pair(yl, -MAX_INF));
        auto ub = s.upper_bound(make_pair(yh,  MAX_INF));
        int top = MAX_INF, bottom = MAX_INF;
        for (auto it=lb; it!=ub; it++) {
            top = min(top, it->second + (yh - it->first));
            bottom = min(bottom, it->second + (it->first - yl));
        }
        s.erase(lb, ub);
        s.insert(make_pair(yh, top));
        s.insert(make_pair(yl, bottom));
    }

    int minV = MAX_INF;
    int minCnt = 0;
    for (auto it=s.begin(); it!=s.end(); it++) {
        if (minV == it->second) {
            minCnt++;
        } else if (it->second < minV) {
            minV = it->second;
            minCnt=1;
        }
    }
    printf("%d\n%d ", minV+ex, minCnt);
    for (auto it=s.begin(); it!=s.end(); it++)
        if (minV == it->second)
            printf("%d ", it->first);
    puts("");
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

