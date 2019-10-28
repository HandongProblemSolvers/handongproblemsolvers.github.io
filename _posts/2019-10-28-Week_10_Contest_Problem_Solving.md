---
title: 10주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다. (학회장아니에요ㅋ)

축제기간에 PS 하시느라 고생이 많으셨습니다.

그럼, 문제해설 시작~

---
## 목차
1. [달팽이는 올라가고 싶다](#달팽이는-올라가고-싶다)
2. [구슬 찾기](#구슬-찾기)
3. [파이프옮기기1](#파이프옮기기1)
4. [제독](#제독)
4. [RPG](#rpg)

---

## **달팽이는 올라가고 싶다**
### [문제링크](https://www.acmicpc.net/problem/2869)

### 문제접근 및 알고리즘
1. 달팽이의 위치는 하루하루가 지날 수록 2가지로 정해진다.
2. 달팽이 현재 높이가 x라고 하자.
3. 도달해야 하는 높이가 V라고 할때 두가지 경우로 나뉜다.
4. **V-x <= A** 인 경우 하루 뒤 도착한다.
4. **V-x > A** 인 경우, 다음날 높이는 x + (A-B) 이다.
5. 결국 현재 날짜가 t라고 할때, 달팽이가 도달하지 못했다면 **높이 = (A-B) * t**이다.
6. 여기서 **높이 >= V-A**가 되면 하루 뒤 도착한다.
7. 결국 **높이 = (A-B) * t >= V-A**를 만족하는 t를 찾으면 **t+1**이 정답임을 알 수 있다.
8. **t >= (V-A)/(A-B)** 여기서 올림을 해주기 위해 (A-B-1)를 분자에 더해주면, **t >= (V-A+(A-B-1))/(A-B)**가 된다.
9. 결국 **답 = 1 + t = 1 + (V-B-1)/(A-B)** 여기서 (V-B-1)이 - 값이 되면 안되므로 **max(0, (V-B-1)/(A-B))**을 해준다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2869 yechan
#include <bits/stdc++.h>
using namespace std;

int A, B, V;

int main() {
    scanf("%d%d%d", &A, &B, &V);
    printf("%d\n", 1+(max(0,V-B-1))/(A-B));
    return 0;
}
```

---

## **구슬 찾기**
### [문제링크](https://www.acmicpc.net/problem/2617)

### 문제접근 및 알고리즘
1. 이 문제는 정말 다양한 접근 방식을 가지고 있겠지만(DFS, Union-find 등), 나는 가장 익숙하게 본 형태인 **플로이드 와샬**로 접근하였다.
2. 이유는 전에 풀어보었던 [키 순서](https://www.acmicpc.net/problem/2458)와 형태가 비슷하였기 때문이다.
3. 우리는 먼저 **자신보다 무게가 작은 경우를 연결**하는 그래프와 **자신보다 무게가 큰 경우를 연결**하는 그래프를 구성한다.
4. 두 그래프에 대해서 플로이드 와샬 그래프를 적용한다.
5. 만약 두 지점 i,j에 대해서 distance가 존재한다면 연결되어 있는 경우이다.
6. 위 그래프를 이용하면 **자신보다 무게가 큰 구슬과 작은 구슬의 수**를 각각 구할 수 있다.
7. **자신보다 무게가 큰 구슬 수를 X**, **자신보다 무게가 작은 구슬 수를 Y**라고 하자
8. 첫번째로 **X > N/2** 인 경우에는 **자신보다 무게가 큰 구슬 중에 중앙값이 존재**하므로 **자기 자신은 중앙값일 수 없다**.
9. 두번째로 **Y > N/2** 인 경우에는 **자신보다 작은 구슬 중에 중앙값이 존재**하므로 **자기 자신은 중앙값일 수 없다**.
10. 이 두 경우에 속하는 구슬 수를 센다.
11. 시간 복잡도는 O(N^3)으로 충분하다.
12. DFS를 이용하면 O(N)에도 가능할것 같지만 귀찮다. **DFS로 푼사람은 코드 제공해주세요~!**

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2617 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=101;

int N, M;
bool uboard[MAX_N][MAX_N];
bool lboard[MAX_N][MAX_N];

int main() {
    scanf("%d%d", &N, &M);
    for (int i=0; i<M; i++) {
        int u, v;
        scanf("%d%d", &u, &v);
        u--,v--;
        uboard[v][u]=true; // u가 v보다 무게가 무겁다.
        lboard[u][v]=true; // v가 u보다 무게가 가볍다.
    }

    // 플로이드 와샬 알고리즘
    for (int k=0; k<N; k++) {
        for (int i=0; i<N; i++) {
            for (int j=0; j<N; j++) {
                if (i == j || j == k) continue;
                uboard[i][j] |= uboard[i][k] && uboard[k][j];
                lboard[i][j] |= lboard[i][k] && lboard[k][j];
            }
        }
    }

    int ret = 0;
    for (int i=0; i<N; i++) {
        int up = 0, down = 0;
        // 자신 보다 크거나 작은 구슬 개수 세기
        for (int j=0; j<N; j++) {
            up += uboard[i][j];
            down += lboard[i][j];
        }
        if (up > N/2 || down > N/2) ret++;
    }
    printf("%d\n", ret);
    return 0;
}
```

---

## **파이프옮기기1**
### [문제링크](https://www.acmicpc.net/problem/17070)

### 문제접근 및 알고리즘
1. 가장 기본적인 DP 문제이다.
2. 완벽하게 비슷한 문제는 없지만 비슷한 컨셉은 [아이템 먹기](https://www.acmicpc.net/problem/2411)와 [스티커](https://www.acmicpc.net/problem/9465)라고 생각한다? 물론 나의 개인적인 의견이다~
3. DP는 상태 정의와 Subproblem 관계를 찾는 것이 가장 중요하다.
4. 먼저 DP 상태 정의는 같은 문제 **상황 또는 상태**에서는 같은 정답을 가지도록 문제의 상황을 정의하는 것이다.
5. 이번 문제에서 중요하게 관찰해야하는 점은 파이프는 **우측, 아래, 우측 하단 대각선** 3가지 경우만 존재한다는 점이다.
6. 여기서 (x,y)까지 파이프를 놓았을 경우 현재 x또는 y보다 작은 지점으로 돌아가는 경우는 없다는 점이다.
7. 결국 문제의 상황은 **(x,y)까지 파이프를 놓았을 경우** 와 **마지막으로 어떤 파이프를 놓았는가**로 상황을 표현할 수 있다.
8. 파이프의 종류는 3가지로 dp(N)(M)(파이프의 종류)로 충분히 DP Table를 사용할 수 있다.
9. 이제 Subproblem를 나타내야한다. 경우는 3가지로 나뉘어 진다.
10. 첫번째 (x,y) 파이프에 **가로 파이프**가 놓인 경우, 파이프를 가로와 대각선으로 놓을 수 있다.
11. 두번째 (x,y) 파이프에 **세로 파이프**가 놓인 경우, 파이프를 세로와 대각선으로 놓을 수 있다.
12. 세번째 (x,y) 파이프에 **대각선 파이프**가 놓인 경우, 파이프를 가로, 세로와 대각선 모두 놓을 수 있다.
13. 위 파이프를 놓는 형태에 따라 다음 놓여질 좌표가 정해진다.
14. 또한 벽이 있는지 없는지 여부를 판단하여 놓아야 한다.
15. 나는 **가로 파이프**를 0으로, **세로 파이프**를 1로, **대각선 파이프**를 2로 정의하였다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 17070 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=17;

int N;
int board[MAX_N][MAX_N];
int dp[MAX_N][MAX_N][3];

int main() {
    scanf("%d", &N);
    for (int i=0; i<N; i++)
        for (int j=0; j<N; j++)
            scanf("%d", &board[i][j]);

    // 초기 파이프 시작 점
    dp[0][1][0]=1;
    for (int i=0; i<N; i++) {
        for (int j=0; j<N; j++) {
            if (board[i][j]) continue;
            for (int k=0; k<3; k++) {
                // 가로 파이프 놓기
                if (!board[i][j+1] && k != 1) dp[i][j+1][0] += dp[i][j][k];
                // 세로 파이프 놓기
                if (!board[i+1][j] && k != 0) dp[i+1][j][1] += dp[i][j][k];
                // 대각선 파이프 놓기
                if (board[i+1][j] + board[i][j+1] + board[i+1][j+1] == 0) dp[i+1][j+1][2] += dp[i][j][k];
            }
        }
    }
    printf("%d\n", dp[N-1][N-1][0]+dp[N-1][N-1][1]+dp[N-1][N-1][2]);
    return 0;
}
```

---

## **제독**
### [문제링크](https://www.acmicpc.net/problem/3640)

### 문제접근 및 알고리즘
1. 음... 나는 그전에 MCMF에 대해서 공부한적이 있기 때문에 바로 MCMF 문제임을 알았다?
2. MCMF란 Minimum Cost Maximum Flow의 줄임말로, 최대 유량을 가지면서 최소 Cost를 찾는 문제이다.
3. 이 문제를 이해하기 위해서는 먼저 Network Flow의 기초적인 부분을 이해해야 한다.
4. 내가 생각하는 문제의 키 포인트를 알기 위한 문제 풀이 순서는 아래와 같다.
5. [Network Flow 기본 개념](https://m.blog.naver.com/kks227/220804885235)
6. [Network Flow 기본 문제](https://www.acmicpc.net/problem/17412)
7. [Network Flow 살짝 심화 문제](https://www.acmicpc.net/problem/2316)
8. [MCMF 기본 개념](https://m.blog.naver.com/kks227/220810623254)
9. [MCMF 기본 문제](https://www.acmicpc.net/problem/11405)
10. 자 위 개념 이해와 위 기본 문제를 모두 풀고 나면 아래 말이 이해가 될 것이다.
11. 일단 제독 문제는 weight 값이 모두 1인 간선으로 이루어져 있으며 flow를 1에서 v까지 2개의 경로를 가지고 있어야한다.
12. 여기서 경로를 찾는 방식은 갓 라이님이 설명한 SPFA(shortest path faster algorithm)를 이용하여 경로를 찾는다.
13. 2개의 경로만을 찾아도 되므로 Maximum flow 조건은 무시한다.
14. 여기서 **Network Flow 살짝 심화 문제**의 주된 컨셉인 한 정점을 2개로 분리시켜 유량을 1로 만들어 주면 각 정점을 한번씩만 사용하는 조건을 표현할 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 3640 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_V = 1000*2;
const int MAX_INF = 1e9;

struct Edge{
    int to, c, f, d;
    Edge *dual;
    Edge():Edge(-1, 0, 0){}
    Edge(int to1, int c1, int d1):to(to1),c(c1),d(d1),f(0),dual(nullptr){}
    int spare() {
        return c - f;
    }
    void addFlow(int f1) {
        f += f1;
        dual->f -= f1;
    }
};

int N, M;
vector<vector<Edge*>> adj;
int pv[MAX_V], dist[MAX_V];
bool inQ[MAX_V];
Edge *path[MAX_V];

int main() {
    while (scanf("%d%d", &N, &M) != -1) {
        const int S = 1;
        const int E = (N-1)*2;
        adj = vector<vector<Edge*>>(MAX_V);
        for (int i=0; i<N; i++) {
            Edge *e1, *e2;
            e1 = new Edge(i*2+1,1,0);
            e2 = new Edge(i*2,0,0);
            e1->dual = e2;
            e2->dual = e1;
            adj[i*2].push_back(e1);
            adj[i*2+1].push_back(e2);
        }

        for (int i=0; i<M; i++) {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            a--, b--;
            Edge *e1, *e2;
            e1 = new Edge(b*2, 1, c);
            e2 = new Edge(a*2+1, 0, -c);
            e1->dual = e2;
            e2->dual = e1;
            adj[a*2+1].push_back(e1);
            adj[b*2].push_back(e2);
        }
        int ret = 0;
        for (int t=0; t<2; t++) {
            memset(inQ, 0, sizeof(inQ));
            memset(pv, -1, sizeof(pv));
            memset(path, 0, sizeof(path));
            fill(dist, dist+MAX_V, MAX_INF);
            queue<int> Q;

            dist[S] = 0;
            inQ[S] = true;
            Q.push(S);

            while (!Q.empty()) {
                int curr = Q.front();
                Q.pop();
                inQ[curr] = false;
                for (Edge *e: adj[curr]) {
                    int next = e->to;
                    int d = e->d;
                    if (e->spare() > 0 && dist[next] > dist[curr] + d) {
                        dist[next] = dist[curr] + d;
                        pv[next] = curr;
                        path[next] = e;
                        if (!inQ[next]) {
                            Q.push(next);
                            inQ[next] = true;
                        }
                    }
                }
            }
            if (pv[E] == -1) break;

            for (int i=E; i!=S; i=pv[i]) {
                ret += path[i]->d;
                path[i]->addFlow(1);
            }
        }
        printf("%d\n", ret);
    }
    return 0;
}
```

---

## RPG
### [문제링크](https://www.acmicpc.net/problem/1315)

### 문제접근 및 알고리즘
1. 이 문제는 N값이 100으로 작은 값을 가지고 있다.
2. 여기서 중요한 포인트는 **힘이 s, 인트가 i 값을 가질 때**에 **깰 수 있는 Quest의 수는 항상 동일하다**는 점이다.
3. 여기서 문제의 상황을 힘과 인트의 값으로 표현할 수 있음을 느낄 수 있다. 100만은 충분히 할당 가능하다.
4. 이제 각 s와 i 상황에서 총 포인트 P를 얻었다고 하자. 이때 **새롭게 찍을 수 있는 스탯은 P-s-i+2**임을 알 수 있다. (초기 스텟이 1,1이므로 +2)
5. 위에서 얻은 새롭게 찍을 수 있는 모든 경우를 탐색한다.
6. 100 * 1000 * 1000 = 1억이므로 시간은 충분하다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1315 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=101;
const int MAX_S=1001;
const int MAX_I=1001;

struct Quest{
    int s, i, p;
    Quest():Quest(0,0,0){}
    Quest(int s1, int i1, int p1):s(s1),i(i1),p(p1){}
    int done(int s1, int i1) {
        return ((s1 >= s) || (i1 >= i)) ? p:0;
    }
};

int N;
int dp[MAX_S][MAX_I];
Quest quest[MAX_N];

int dfs(int s, int i) {
    int &ret = dp[s][i];
    if (ret != -1) return ret;

    ret = 0;
    int p = 0;
    for (int j=0; j<N; j++) {
        int tmp = quest[j].done(s,i);
        if (tmp) ret++;
        p += tmp;
    }
    if (ret == N) return ret;
    p -= (s+i-2);
    for (int j=0; j<=p && p; j++)
        ret = max(ret, dfs(min(1000,s+j),min(1000,i+p-j)));
    return ret;
}

int main() {
    memset(dp, -1, sizeof(dp));
    scanf("%d", &N);
    for (int j=0; j<N; j++) {
        int s, i, p;
        scanf("%d%d%d", &s, &i, &p);
        quest[j] = Quest(s, i, p);
    }
    printf("%d\n", dfs(1,1));
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

