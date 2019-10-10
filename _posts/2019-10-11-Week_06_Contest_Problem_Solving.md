---
title: 6주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

이번학기에 처음으로 연습 대회를 진행하기 시작하여
부족한 점이 많습니다.

학회가 없는 관계로 문제 풀이를 교류할 수 없는 상황이지만
이렇게라도 교류를 하고자 합니다.

혹시 문제 해설에 문제가 있거나, 다른 해법이 있다면
언제든지 연락주시면 업데이트 하겠습니다!

그럼, 문제해설 시작~

---
## 목차
1. [돌다리](#돌다리)
2. [균형잡힌 세상](#균형잡힌-세상)
3. [적록색약](#적록색약)
4. [역사](#역사)
5. [고대 동굴 탐사](#고대-동굴-탐사)

---

## **돌다리**
### [문제링크](https://www.acmicpc.net/problem/12761)

### 문제접근 및 알고리즘
1. N에서 M까지 도착하는 최소 시간 찾기!
2. 스카이 콩콩을 이용하면 시간 1동안 (1, -1, A, -A, B, -B) 만큼 움직일 수 있다.
3. 현재 위치가 i인 경우 i*A 또는 i*B 로 움직일 수 있다.
4. 이 8가지 경우를 이용하는데, 이를 `BFS`로 표현한다.
5. 더 적은 시간으로 좌표 i 로 가는 경우가 있다면 이후 시간에 i 로 갈 이유가 없다.
6. 고로, visited 로 갔던 좌표를 다시 가지 않도록 `memoization` 한다. 사실상 DP이다.
7. BFS도중 M에 도착하면 시간을 출력한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 12761 yechan
#include <cstdio>
#include <algorithm>
#include <queue>
#include <utility>
#include <vector>
using namespace std;

typedef pair<int,int> P;
const int MAX_N=100001;
int N, M, A, B;
bool visited[MAX_N];
vector<int> dir;

int BFS() {
    queue<int> q;
    q.push(N);
    dir.push_back(-1); dir.push_back(1);
    dir.push_back(-A); dir.push_back(A);
    dir.push_back(-B); dir.push_back(B);

    int t = 0;
    visited[N] = true;
    while (!q.empty()) {
        int qSize = q.size();
        while (qSize--) {
            int cur = q.front();
            if (cur == M) return t;
            q.pop();
            for (int i=0; i<dir.size(); i++) {
                int nx = cur + dir[i];
                if (nx < 0 || nx >= MAX_N) continue;
                if (visited[nx]) continue;
                visited[nx] = true;
                q.push(nx);
            }
            int nx = cur * A;
            if (!(nx < 0 || nx >= MAX_N)) {
                if (!visited[nx]) {
                    visited[nx] = true;
                    q.push(nx);
                }
            }
            nx = cur * B;
            if (!(nx < 0 || nx >= MAX_N)) {
                if (!visited[nx]) {
                    visited[nx] = true;
                    q.push(nx);
                }
            }
        }
        t++;
    }
    return -1;
}

int main()
{
    scanf("%d%d%d%d", &A, &B, &N, &M);
    printf("%d\n", BFS());
    return 0;
}
```

---

## **균형잡힌 세상**
### [문제링크](https://www.acmicpc.net/problem/4949)

### 문제접근 및 알고리즘
1. 열린 괄호와 닫힌 괄호가 올바르게 쓰였는지 묻는 문제이다.
2. 이 문제는 `스택`을 사용하는 것으로 유우우우명하다.
3. fgets 또는 %100[^\n]s 를 이용하여 입력을 한줄씩 받는다.
4. 문자열 앞에서 부터 탐색하며, 스택에 `(` 또는 `[`를 넣고 `)` 또는 `]`를 만나면 스택에서 하나씩 빼면서 `) -> (`, `] -> [`로 올바르게 매치 되고 있는지 확인한다. 또한 스택이 비어있으면 안된다.
5. 모든 탐색 후에 스택이 비어 있어야 한다.
6. 이를 위 2가지 조건을 확인한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 4949 yechan
#include <cstdio>
#include <cstring>
#include <stack>
#include <algorithm>
using namespace std;
const int MAX_N=111;

char S[MAX_N];

int main() {
    while (1) {
        scanf("%100[^\n]s", S);
        getchar();
        if (S[0] == '.' && strlen(S) == 1) break;
        stack<char> st;
        bool flag = true;
        for (int i=0; S[i]; i++) {
            if (S[i] == '(' || S[i] == '[') st.push(S[i]);
            if (S[i] == ')') {
                if (st.empty() || st.top() != '(') {
                    flag=false;
                    break;
                }
                st.pop();
            }
            if (S[i] == ']') {
                if (st.empty() || st.top() != '[') {
                    flag=false;
                    break;
                }
                st.pop();
            }
        }
        if (!st.empty()) flag=false;
        printf("%s\n", flag ? "yes" : "no");
    }
    return 0;
}
```

---

## **적록색약**
### [문제링크](https://www.acmicpc.net/problem/10026)

### 문제접근 및 알고리즘
1. 각 NxN 좌표를 하나의 Vertex(정점)으로 보자.
2. 정점은 상하좌우 연결되어 있다고 가정하자.
3. 여기서 각 좌표로 부터 연결되어 있는 곧 같은 색으로 연결된 구역을 `DFS`로 찾는다.
4. DFS를 2종류로 나누었다.
5. dfs0 는 정상인이 보는 경우 구역수를 찾는다.
6. dfs1 는 색맹이 보는 경우 구역수를 찾는다.
7. 상하좌우중 자신의 색과 같은 경우 같은 구역이다.
8. 이때 dfs0는 현재 색과 같아야 하지만, dfs1인 색맹은 자신이 빨강 초록이라면 주변색이 빨강 초록인 경우도 같은 구역이다.
9. dir로 4가지 상하좌우 좌표를 표현하고 for문으로 탐색해 간다.
10. visited 변수를 2차원으로 만들어 방문한 곳은 더이상 방문하지 않도록 한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10026 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

const int dir[4][2] = { {0, -1}, {0, 1}, {-1, 0}, {1, 0} };
const int MAX_N=101;

int N;
char board[MAX_N][MAX_N];
bool visited[2][MAX_N][MAX_N];

void dfs0(int x, int y, char c) {
    visited[0][x][y] = true;
    for (int d=0; d<4; d++) {
        int nx = x + dir[d][0];
        int ny = y + dir[d][1];
        if (nx < 0 || nx >= N || ny < 0 || ny >= N) continue;
        if (visited[0][nx][ny]) continue;
        if (c == board[nx][ny]) {
            dfs0(nx, ny, c);
        }
    }
}

void dfs1(int x, int y, char c) {
    visited[1][x][y] = true;
    for (int d=0; d<4; d++) {
        int nx = x + dir[d][0];
        int ny = y + dir[d][1];
        if (nx < 0 || nx >= N || ny < 0 || ny >= N) continue;
        if (visited[1][nx][ny]) continue;
        if (c == 'R' || c == 'G') {
            if (board[nx][ny] == 'B') continue;
            dfs1(nx, ny, c);
        } else {
            if (board[nx][ny] == 'B') {
                dfs1(nx, ny, c);
            }
        }
    }

}

int main()
{
    scanf("%d", &N);
    for (int i=0; i<N; i++)
        scanf("%s", board[i]);

    int ret = 0;
    for (int i=0; i<N; i++) {
        for (int j=0; j<N; j++) {
            if (!visited[0][i][j]) {
                dfs0(i, j, board[i][j]);
                ret++;
            }
        }
    }
    printf("%d ", ret);
    ret = 0;
    for (int i=0; i<N; i++) {
        for (int j=0; j<N; j++) {
            if (!visited[1][i][j]) {
                dfs1(i, j, board[i][j]);
                ret++;
            }
        }
    }
    printf("%d\n", ret);
    return 0;
}
```

---

## **역사**
### [문제링크](https://www.acmicpc.net/problem/1613)

### 문제접근 및 알고리즘
1. 일단 사건들을 하나의 Vertex(정점)으로 둔다.
2. 사건간의 전후 관계를 그래프의 간선으로 표현한다.
3. A 사건이 B보다 먼저 일어난 경우 A->B로 표현하고 adj(A)(B) = true 로 정한다.
4. 이 adj 배열을 이용하면 만약 adj(i)(j) = true 라면 i -> j 로 i사건이 j사건 보다 먼저 일어 났음을 알 수 있다.
5. 모든 관계를 간선으로 나타낸 뒤에 `플로이드 와샬` 알고리즘을 그래프의 적용시킨다.
6. 이를 이용하면 A->B와 B->C 인 경우 A->C 인 것을 알 수 있다. 곧 A->C로의 경로가 존재하면 알 수 있다는 이야기이며, 이는 `플로이드 와샬` 알고리즘으로 각 정점간에 연결 여부를 확인할 수 있다. O(N^3) N은 최대 400으로 충분하다.
7. 이에 따라 출력하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1613 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

const int MAX_N=501;
int N, K;
bool adj[MAX_N][MAX_N];

int main() {
    scanf("%d%d", &N, &K);
    for (int i=0; i<K; i++) {
        int u, v;
        scanf("%d%d", &u, &v);
        adj[u][v]=true;
    }

    // 플로이드 와샬 알고리즘
    for (int k=1; k<=N; k++) {
        for (int i=1; i<=N; i++) {
            for (int j=1; j<=N; j++) {
                if (i == j || i == k || j == k) continue;
                // i->k와 k->j가 연결 되어 있다면, i->j도 연결되어 있다.
                adj[i][j] |= adj[i][k] && adj[k][j];
            }
        }
    }

    int S;
    scanf("%d", &S);
    for (int i=0; i<S; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        if (adj[a][b]) puts("-1");
        else if (adj[b][a]) puts("1");
        else puts("0");
    }

    return 0;
}
```

---

## 고대 동굴 탐사
### [문제링크](https://www.acmicpc.net/problem/10273)

### 문제접근 및 알고리즘
1. 전형적인 DP Tracking 문제이다.
2. 동굴에 들어가기 위한 비용과 이익이 주어진다.
3. 여기서 각 동굴을 Vertex(정점)으로 둔다.
4. A동굴에서 B동굴로 가기위해서 C의 비용이 필요하다고 하자고 하자.
5. A -> B 까지 가중치 값이 C인 간선으로 생각할 수 있다.
6. 이러한 형태로 모든 그래프를 완성하자.
7. 이 그래프는 사이클이 존재하지 않으며, 단방향 간선으로 구성되어 있다.
8. 이를 DAG(Directed Acyclic Graph)라고 한다. 알고때 배움 아마 ㅇㅇ.
9. 이 DAG은 위상정렬(Topological sorting)이 가능한데 굿이 이 문제에는 적용 안했다.
10. 이 문제는 `각 동굴이 얻을 수 있는 최대 이윤값`을 DP에 저장하므로써 Sub-problem 관계를 정의할 수 있다.
11. 위 각 동굴이 얻을 수 있는 최대 이윤값이란, 끝까지 들어가서 최대 값일 수 있으며, 현재 동굴까지만 들어가는게 최대 이윤 값 일 수 있다. 이러한 상황을 확인하고 현재 동굴이 얻을 수 있는 최대 값을 DP Table에 기록(memo)하는 것이다. 이를 memoization이라한다.
12. N은 최대 20000으로 메모리에 충분히 들어간다 ㅎㅎ
13. 자 DP 점화식을 세워보자. (Sub-Problem 관계를 규명하자)
14. 정의, DP(i): i번째 동굴이 얻을 수 있는 최대 이윤
15. DP(i) = DP(j) - w(i->j) (단 j는 i->j로 연결된 간선이 존재한다.)
16. DP(j) - w(i->j) 가 음수 값인 경우 포함하지 않고 현재 동굴의 이득만 취하는 것이 이익이다. 또한 DP(j) - w(i-j) 중 최대 값을 maxDP(j)라고 하면 DP(i) = maxDP(j) + cave(i) 이다. cave(i)는 현재 동굴 방문시 얻는 이익이다.
17. 이제 Tracking 즉 탐색을 진행하자.
18. 최대 이윤값을 알고 있으며, 위 점화식을 이용하면 `역추적`이 가능하다.
19. DP(i) = maxDP(j) + cave(i) 이므로 `DP(j) = DP(i) - cave(i) + w(i->j)`를 만족하는 j는 최대 이윤을 가진다.
20. 이러한 j를 계속 찾아서 DP(i)가 0이 될때 까지 찾는다. (더이상 들어가지 않는 경우)
21. 이 탐색 순서 `Queue`에 저장한 뒤에 LILO (List In List Out)를 시전한다. 개꿀
22. 사실 동굴 개수를 출력할 이유가 없다면 동굴을 들어가면서 출력하면 된다.
23. 팁으로 순서를 뒤집어야 하는 경우에는 `Stack`를 이용하자!

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10273 yechan
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>
#include <vector>
using namespace std;

typedef pair<int,int> P;
const int MAX_N=20001;
const int MIN_INF=-2e9;

int T, N, E, cave[MAX_N], ret;
vector<vector<P> > adj;
int cost[MAX_N];
queue<int> path;
int dret;

int dfs(int here) {
    int &ret = cost[here]; 
    if (ret != MIN_INF) return ret;

    ret = 0;
    for (int i=0; i<adj[here].size(); i++)
        ret = max(ret, dfs(adj[here][i].first) - adj[here][i].second);

    return ret = ret + cave[here];
}

void findroot(int here, int money) {
    path.push(here);
    money -= cave[here];
    if (!money) {
        return;
    }

    for (int i=0; i<adj[here].size(); i++) {
        int nx = adj[here][i].first;
        int c = adj[here][i].second;
        if (cost[nx] - c == money) {
            findroot(nx, money + c);
            return;
        }
    }
}

int main() {
    scanf("%d", &T);
    while (T--) {
        fill(cost, cost+MAX_N, MIN_INF);
        scanf("%d%d", &N, &E);
        adj = vector<vector<P> >(N+1);
        for (int i=1; i<=N; i++)
            scanf("%d", &cave[i]);

        for (int i=0; i<E; i++) {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            adj[a].push_back(P(b, c));
        }

        ret = dfs(1);
        findroot(1, ret);
        printf("%d %d\n", ret, path.size());
        while (!path.empty()) {
            printf("%d ", path.front());
            path.pop();
        }
        puts("");
    }
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

