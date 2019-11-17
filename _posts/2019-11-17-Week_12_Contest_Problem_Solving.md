---
title: 11주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

요즘 너무나도 춥고 바쁘네요.

이번 문제는 대체적으로 좀 어려웠네요..._

문제 풀고 생각하시느라 고생많으셨고

HPS 화이팅!

그럼, 문제해설 시작~

---
## 목차
1. [카드 역배치](#카드-역배치)
2. [주사위 윷놀이](#주사위-윷놀이)
3. [L모양의 종이 자르기](#L모양의-종이-자르기)
4. [세금](#세금)
4. [데크 소트](#데크-소트)

---

## **카드 역배치**
### [문제링크](https://www.acmicpc.net/problem/10804)

### 문제접근 및 알고리즘
1. 단순한 구현문제로 생략합니다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10804 yechan
#include <bits/stdc++.h>
using namespace std;

int arr[20];

int main() {
    for (int i=0; i<20; i++) arr[i]=i+1;
    for (int i=0; i<10; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        a--, b--;
        for (int i=0; i<=(b-a)/2; i++)
            swap(arr[a+i], arr[b-i]);
    }

    for (int i=0; i<20; i++)
        printf("%d ", arr[i]);
    puts("");
    return 0;
}
```

---

## **주사위 윷놀이**
### [문제링크](https://www.acmicpc.net/problem/17825)

### 문제접근 및 알고리즘
1. 보드 상황을 구현해야한다.
2. 하드 코딩으로 board를 구성한다
3. 모든 점은 총 33개로 이를 매핑한다.
4. 파란색지점과 시작여부를 알수 있도록 구현한다.
5. 4개중 한 말을 움직였을때 움직일 수 있는 여부를 확인한다.
6. 여기서 4개의 말을 10번 선택하는 것으로 O(4^10*5) = 500만으로 완전탐색으로 충분하다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 17825 yechan
#include <bits/stdc++.h>
using namespace std;
using P=pair<int,int>;
const int MAX_N=44;
const int MAX_I=33;

P board[MAX_N][3];

void makeBoard() {
    board[0][0] = P(2,0);
    board[2][0] = P(4,0);
    board[4][0] = P(6,0);
    board[6][0] = P(8,0);
    board[8][0] = P(10,0);
    board[10][0] = P(12,0);

    board[12][0] = P(14,0);
    board[14][0] = P(16,0);
    board[16][0] = P(18,0);
    board[18][0] = P(20,0);
    board[20][0] = P(22,0);

    board[22][0] = P(24,0);
    board[24][0] = P(26,0);
    board[26][0] = P(28,0);
    board[28][0] = P(30,0);
    board[30][0] = P(32,0);

    board[30][0] = P(32,0);
    board[32][0] = P(34,0);
    board[34][0] = P(36,0);
    board[36][0] = P(38,0);
    board[38][0] = P(40,0);

    board[10][1] = P(13,1);
    board[13][1] = P(16,1);
    board[16][1] = P(19,1);
    board[19][1] = P(25,0);

    board[20][1] = P(22,1);
    board[22][1] = P(24,1);
    board[24][1] = P(25,0);

    board[30][1] = P(28,1);
    board[28][1] = P(27,1);
    board[27][1] = P(26,1);
    board[26][1] = P(25,0);

    board[25][0] = P(30,2);
    board[30][2] = P(35,2);
    board[35][2] = P(40,0);

    board[40][0] = P(0,1);
}

int idx[MAX_N][3];
P m[MAX_N];
bool isblue[MAX_N][3];

void setIdx() {
    idx[0][0]=0; m[0]=P(0,0);
    idx[2][0]=1; m[1]=P(2,0);
    idx[4][0]=2; m[2]=P(4,0);
    idx[6][0]=3; m[3]=P(6,0);
    idx[8][0]=4; m[4]=P(8,0);
    idx[10][0]=5; m[5]=P(10,0);
    idx[12][0]=6; m[6]=P(12,0);
    idx[14][0]=7; m[7]=P(14,0);
    idx[16][0]=8; m[8]=P(16,0);
    idx[18][0]=9; m[9]=P(18,0);
    idx[20][0]=10; m[10]=P(20,0);

    idx[13][1]=11; m[11]=P(13,1);
    idx[16][1]=12; m[12]=P(16,1);
    idx[19][1]=13; m[13]=P(19,1);

    idx[22][0]=14; m[14]=P(22,0);
    idx[24][0]=15; m[15]=P(24,0);
    idx[26][0]=16; m[16]=P(26,0);
    idx[28][0]=17; m[17]=P(28,0);
    idx[30][0]=18; m[18]=P(30,0);

    idx[22][1]=19; m[19]=P(22,1);
    idx[24][1]=20; m[20]=P(24,1);

    idx[28][1]=21; m[21]=P(28,1);
    idx[27][1]=22; m[22]=P(27,1);
    idx[26][1]=23; m[23]=P(26,1);

    idx[25][0]=24; m[24]=P(25,0);

    idx[30][2]=25; m[25]=P(30,2);
    idx[35][2]=26; m[26]=P(35,2);

    idx[32][0]=27; m[27]=P(32,0);
    idx[34][0]=28; m[28]=P(34,0);
    idx[36][0]=29; m[29]=P(36,0);
    idx[38][0]=30; m[30]=P(38,0);

    idx[40][0]=31; m[31]=P(40,0);

    idx[0][1]=32; m[32]=P(0,1);
    isblue[10][0]=isblue[20][0]=isblue[30][0]=true;
}

int seq[10];

int getIdx(P a) {
    return idx[a.first][a.second];
}

P move(P here, int cnt, int start) {
    if (cnt == 0) return here;
    if (getIdx(here) == 32) return here; // 마지막

    if (start && isblue[here.first][here.second]) return move(board[here.first][1], cnt-1, 0);
    else return move(board[here.first][here.second], cnt-1, 0);
}

bool match(int a, int b) {
    if (a == 0 || a == 32) return true;
    if (b == 0 || b == 32) return true;
    return (a != b);
}

bool check(int a, int b, int c, int d) {
    return match(a, b) && match(a, c) && match(a, d) && match(b, c) && match(b, d) && match(c, d);
}


int dfs(int here, int a, int b, int c, int d) {
    if (here == 10) return 0;

    int ret = 0;
    P naa = move(m[a], seq[here], 1);
    P nbb = move(m[b], seq[here], 1);
    P ncc = move(m[c], seq[here], 1);
    P ndd = move(m[d], seq[here], 1);
    int na = getIdx(naa);
    int nb = getIdx(nbb);
    int nc = getIdx(ncc);
    int nd = getIdx(ndd);
    if (check(na,b,c,d)) ret = max(ret, dfs(here+1, na, b, c, d)+naa.first);
    if (check(a,nb,c,d)) ret = max(ret, dfs(here+1, a, nb, c, d)+nbb.first);
    if (check(a,b,nc,d)) ret = max(ret, dfs(here+1, a, b, nc, d)+ncc.first);
    if (check(a,b,c,nd)) ret = max(ret, dfs(here+1, a, b, c, nd)+ndd.first);
    return ret;
}

int main() {
    makeBoard();
    setIdx();
    for (int i=0; i<10; i++)
        scanf("%d", &seq[i]);

    printf("%d\n", dfs(0, 0, 0, 0, 0));
    return 0;
}
```

---

## **L모양의 종이 자르기**
### [문제링크](https://www.acmicpc.net/problem/10805)

### 문제접근 및 알고리즘
1. L모양 종이에서 (w1, h1, w2, h2)를 이용하면 종이 상태를 표현할 수 있다.
2. 이러한 문제 상황을 DP에 표현한다. 값이 O(N^4 = 50^4 = 6250000)으로 메모리 저장이 가능하다.
3. 일단 w1 = h1이면서, w2 = h2 = 0일때 정사각형임을 안다.
4. 또한 L모양의 종이를 가로로 자르는 모든 경우와 세로로 자르는 모든 경우를 DFS를 이용하여 모두 탐색한다.
5. 이러한 L모양의 종이 중 가장 적은 정사각형의 개수를 DP state의 저장한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10805 yechan
#include <bits/stdc++.h>
using namespace std;
const int MAX_INF=1e9;

int dp[51][51][51][51];

int dfs(int h1, int w1, int h2, int w2) {
    // printf("dfs(%d, %d, %d, %d)\n",h1,w1,h2,w2);
    int &ret = dp[h1][w1][h2][w2];
    if (ret != -1) return ret;
    ret = MAX_INF;

    for (int i=1; i<w2; i++)
        ret = min(ret, dfs(h1, w1-i, h2, w2-i) + dfs(h1-h2, i, 0, 0));
    if (w2) ret = min(ret, dfs(h1, w1-w2, 0, 0) + dfs(h1-h2, w2, 0, 0));
    for (int i=w2+1; i<w1; i++)
        ret = min(ret, dfs(h1, w1-i, 0, 0) + dfs(h1, i, h2, w2));

    for (int i=1; i<h2; i++)
        ret = min(ret, dfs(h1-i, w1, h2-i, w2) + dfs(i, w1-w2, 0, 0));
    if (h2) ret = min(ret, dfs(h2, w1-w2, 0, 0) + dfs(h1-h2, w1, 0, 0));
    for (int i=h2+1; i<h1; i++)
        ret = min(ret, dfs(i, w1, h2, w2) + dfs(h1-i, w1, 0, 0));

    dp[w1][h1][w1][h2]=ret;
    return ret;
}

int main() {
    int h1, w1, h2, w2;
    memset(dp, -1, sizeof(dp));
    scanf("%d%d%d%d", &h1, &w1, &h2, &w2);
    for (int i=1; i<=50; i++)
        dp[i][i][0][0] = 1;
    printf("%d\n", dfs(h1,w1,h2,w2));
    return 0;
}
```

---

## **세금**
### [문제링크](https://www.acmicpc.net/problem/13907)

### 문제접근 및 알고리즘
1. 여기서 시작점으로부터 도착점까지 최단 거리는 다익스트라로 알 수 있다. O(NlogN)
2. 일반적인 최단 거리 문제과 다른점은 모든 간선의 가격이 동일하기 증가한다는 점이다.
3. 여기서 정말 간단한 수식을 캐치할 수 있다.
4. 시작점에서 끝점까지 지나가는 간선 개수를 K, 최단 거리를 D라고 하자.
5. 여기서 증가된 가격을 C라고 하면 **Total_D = K*C + D** 이다
6. 결국 도착지점에서 D, C와 K 값을 알면 Total_D를 알 수 있다.
7. 또한 최단거리를 찾을때 같은 지점을 두번 방문하는 경우는 존재하지 않으므로 K는 최대 N-1임을 알 수 있다.
8. 이를 DP state에 저장하면 O(N^2 = 100만)으로 메모리에 저장 가능하다.
9. DP(i,k)로 i번째 노드에서 k개의 간선을 지났을때 최소 거리로 정의한다.
10. 위는 다익스트라 알고리즘을 살짝 고치면 간단하게 만들 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 13907 yechan
#include <bits/stdc++.h>
using namespace std;
using P = pair<int,int>;
using PP = pair<int,P>;

const int MAX_INF=1e9;
const int MAX_N=1001;

int N, M, K;
vector<P> adj[MAX_N];

int S, D;
int dist[MAX_N][MAX_N]; // (here, numVertex)
bool visited[MAX_N][MAX_N];

int Dijkstra() {
    priority_queue<PP, vector<PP>, greater<PP>> PQ;
    PQ.push(PP(0, P(0, S)));
    dist[S][0]=0;
    while (!PQ.empty()) {
        int cur_len, cur_x;
        do {
            cur_len = PQ.top().second.first;
            cur_x = PQ.top().second.second;
            PQ.pop();
        } while (!PQ.empty() && visited[cur_x][cur_len]);
        if (visited[cur_x][cur_len]) break;
        visited[cur_x][cur_len] = true;

        for (P &p: adj[cur_x]) {
            int nx = p.first;
            int w = p.second;
            int nlen = cur_len + 1;
            if (nlen >= MAX_N) continue;
            if (dist[nx][nlen] > dist[cur_x][cur_len] + w) {
                dist[nx][nlen] = dist[cur_x][cur_len] + w;
                PQ.push(PP(dist[nx][nlen], P(nlen, nx)));
            }
        }
    }

    int ans = MAX_INF;
    for (int i=0; i<MAX_N; i++)
        ans = min(ans, dist[D][i]);
    return ans;
}

int main() {
    for (int i=0; i<MAX_N; i++)
        for (int j=0; j<MAX_N; j++)
            dist[i][j] = MAX_INF;

    scanf("%d%d%d", &N, &M, &K);
    scanf("%d%d", &S, &D);
    S--, D--;
    for (int i=0; i<M; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        u--, v--;
        adj[u].push_back(P(v, w));
        adj[v].push_back(P(u, w));
    }

    printf("%d\n", Dijkstra());

    int cost = 0;
    for (int i=0; i<K; i++) {
        int x; scanf("%d", &x);
        cost += x;
        int ans = MAX_INF;
        for (int j=0; j<MAX_N; j++)
            ans = min(ans, dist[D][j] + j*cost);
        printf("%d\n", ans);
    }
    return 0;
}
```

---

## 데크 소트
### [문제링크](https://www.acmicpc.net/problem/1624)

### 문제접근 및 알고리즘
1. 자 최소 deque로 정렬하는 방법에 대한 이야기이다.
2. 여기서 deque에 넣은뒤 서로를 이어 붙친다는점을 바라보자.
3. 이어 붙이는 점에서 각 deque는 **어떤 숫자 범위를 담당한다고 볼 수 있다.**
4. 예를 들어 입력이 예제에서 주어진 3 6 0 9 5 4 라고 하자
5. 이때 0~3 값은 Q1에, 4~9 값은 Q2에 넣는다고 생각할 수 있다.
6. 여기서 각 범위 a와 b (a < b) 사이에 있는 값을 1개의 deque에 넣을 수 있는지 여부를 O(N)으로 간단하게 판단할 수 있다.
7. 또한 각 a 또는 b의 값은 모든 값 범위를 볼 이유 없이 순열의 있는 값중 하나로 설정하면 된다.
8. 이제 문제를 풀어 나가는데, 먼저 순열의 가장 작은 숫자부터 deque가 담당하기 시작하여 가장 큰 숫자까지 deque에 넣게 되는 최소 deque개수를 구해야 한다.
9. 여기서 DP를 이용하여 문제를 풀어나갔다.
10. 먼저 A(i)는 순열의 있는 숫자들 중 i번째 작은 수를 말한다.
11. 여기서 DP(i)는 A(0)부터 A(i)까지 deque로 정렬하기 위한 최소 deque 개수를 말한다.
12. 다음 Sub Problem 점화식은 아래와 같다.
13. **DP(i) = min(DP(j-1) + 1)** if (check(j, i))
14. 여기서 Greedy한 최적화는 바로 A(j)~A(i)를 deque에 넣을 수 없다면, j보다 작은 모든 값에 대해서 deque에 넣을 수 없다는 점이다.
15. 이점에서 j값을 i부터 시작해서 0으로 줄이면서 불가능시 break를 시켰다.
16. 또한 check함수의 **b - a < 2** 부분은 deque는 최소 2개의 숫자는 넣을 수 있다는 점이다.
16. 위 점화식으로 DP의 마지막 값을 출력하면 정답이다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1624 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=1001;
const int MAX_V=2001;
const int MAX_INF=1e9;

int N, A[MAX_N];
int dp[MAX_N];
vector<int> xpos;

bool check(int a, int b) {
    if (b - a < 2) return true;
    a = xpos[a]; b = xpos[b];
    int s = MAX_INF, e = -MAX_INF;
    for (int i=0; i<N; i++) {
        if (a > A[i] || b < A[i]) continue;
        if (s < A[i] && A[i] < e) return false;
        s = min(s, A[i]);
        e = max(e, A[i]);
    }
    return true;
}

int main() {
    fill(dp, dp+MAX_N, MAX_INF);

    scanf("%d", &N);
    for (int i=0; i<N; i++) {
        scanf("%d", &A[i]);
        xpos.push_back(A[i]);
    }
    xpos.erase(unique(xpos.begin(), xpos.end()), xpos.end());
    sort(xpos.begin(), xpos.end());

    int xsize = xpos.size();

    for (int i=0; i<xsize; i++) {
        if (check(0, i)) {
            dp[i]=1;
            continue;
        }
        for (int j=i; j>0; j--) {
            if (!check(j,i)) break;
            dp[i] = min(dp[i], dp[j-1] + 1);
        }
    }

    printf("%d\n", dp[xsize-1]);
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

