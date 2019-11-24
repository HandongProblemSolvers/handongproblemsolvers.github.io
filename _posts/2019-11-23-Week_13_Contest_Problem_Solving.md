---
title: 13주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

알고리즘 열심히하고 싶은데 요즘 정신이 없네요 ㅠㅠ

또 이번에는 뒤쪽문제는 처음보는 알고리즘도 나와서 당황했네요...

그래도 끝까지 해봅시다! HPS 화이팅!

그럼, 문제해설 시작~

---
## 목차
1. [아이 러브 크로아티아](#아이-러브-크로아티아)
2. [입국심사](#입국심사)
3. [캐슬 디펜스](#캐슬-디펜스)
4. [전봇대](#전봇대)
4. [크루스칼의 공](#크루스칼의-공)

---

## **아이 러브 크로아티아**
### [문제링크](https://www.acmicpc.net/problem/9517)

### 문제접근 및 알고리즘
1. 단순한 구현문제로 생략합니다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 9517 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=101;
const int BOOM_T=210;
const int DIV=8;

int K, N, T[MAX_N];
char Z[MAX_N];

int main() {
    scanf("%d%d", &K, &N);
    for (int i=0; i<N; i++)
        scanf("%d %c", &T[i], &Z[i]);

    int cur_t = 0;
    int cur_p = K - 1;
    for (int i=0; i<N; i++) {
        cur_t += T[i];
        if (cur_t >= BOOM_T) break;
        if (Z[i] == 'T') cur_p = (cur_p+1)%DIV;
    }
    printf("%d\n", cur_p+1);
    return 0;
}
```

---

## **입국심사**
### [문제링크](https://www.acmicpc.net/problem/3079)

### 문제접근 및 알고리즘
1. 이 문제는 파라메트릭 서치(이분탐색) 문제이다.
2. 여기서 파악해야하는 아래와 같다.
3. **시간이 T인 경우 각 심사대에서 완료된 사람 수를 구할 수 있다.**
4. 심사대 검사 시간이 a이며, 시간이 T 지난 경우 검사 할 수 사람수는 T/a 이다.
5. 각 시간 T가 주어진경우 검사할 수 있는 사람 수를 K라고 하면, O(N)의 시간 복잡도로 구할 수 있다.
6. 여기서 이 시간 T를 관찰하면 이분탐색이 가능하다.
7. 이분탐색은 아래와 같은 형태에서 가능하다
8. **모든 T보다 큰 시간**에 대해서 **검사할 수 있는 사람수는 K보다 같거나 크다**.
9. **모든 T보다 작은 시간**에 대해서 **검사할 수 있는 사람수는 K보다 같거나 작다**.
10. 위는 전형적인 파라메트릭 서치의 지표이다.
11. 이를 이용하면 한시간 T를 판단하면, **T보다 작은시간 또는 큰 시간에 대해서만 탐색** 해보면 되기 때문에 **확인해야 하는 시간 영역**을 **절반씩 줄여나갈 수 있다.**
12. 위에서 K값을 M과 비교하여 K >= M 인 경우 시간을 줄이고, K < M 인 경우 시간을 늘리는 형태로 진행하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 3079 yechan
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int MAX_N = 100001;
const ll MAX_INF = (1LL<<62);

ll N, M;
ll arr[MAX_N];

bool check(ll t) {
    ll num = 0;
    for (int i=0; i<N; i++) {
        num += t/arr[i];
        if (num >= M) return true;
    }
    return num >= M;
}

int main() {
    scanf("%lld%lld", &N, &M);
    for (int i=0; i<N; i++)
        scanf("%lld", &arr[i]);

    ll left=0, right=MAX_INF;
    ll ret=MAX_INF;
    while (left <= right) {
        ll mid = (left+right)/2;
        if (check(mid)) right = mid - 1, ret = min(ret, mid);
        else left = mid + 1;
    }
    printf("%lld\n", ret);

    return 0;
}
```

---

## **캐슬 디펜스**
### [문제링크](https://www.acmicpc.net/problem/17135)

### 문제접근 및 알고리즘
1. 삼성 A형 문제이다.
2. N,M < 15이며, D는 10으로 아주 작다.
3. 3명의 궁수를 놓을 수 있으므로 N^3경우의 수가 있다.
4. 완전탐색은 O(N^6)로 아아아주 충분하다. 구현문제이다.
5. N^6 = N^3(궁수배치) * N(각 턴) * N^2(보드 탐색)
5. 각 3명의 궁수 놓을 수 있는 모든 경우의 수에 놓고 시뮬레이션 한다.
6. 가장 많이 죽이는 경우를 출력한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 17135 yechan
#include <bits/stdc++.h>
using namespace std;
using P = pair<int,int>;
const int MAX_N=16;

int N, M, D;
int board[MAX_N][MAX_N];
int tBoard[MAX_N][MAX_N];

void move() {
    for (int i=1; i<N; i++) {
        for (int j=0; j<M; j++) {
            tBoard[i-1][j] = tBoard[i][j];
            tBoard[i][j] = 0;
        }
    }
}

int getDist(int p, int x, int y) {
    return (x+1)+abs(p-y);
}

P killOne(int p) {
    int minDist = 1e9;
    P ret = P(-1,-1);
    for (int i=0; i<N; i++) {
        for (int j=0; j<M; j++) {
            if (!tBoard[i][j]) continue;
            int tmp = getDist(p, i, j);
            if (tmp > D) continue;
            if (tmp < minDist) {
                minDist = tmp;
                ret = P(i, j);
            } else if (tmp == minDist) {
                if (j < ret.second) {
                    ret = P(i, j);
                }
            }
        }
    }
    return ret;
}

int remove(P a) {
    int x = a.first, y = a.second;
    if (x == -1 || y == -1) return 0;
    if (tBoard[x][y] == 0) return 0;
    tBoard[x][y] = 0;
    return 1;
}

int killThree(int a, int b, int c) {
    P aa = killOne(a);
    P bb = killOne(b);
    P cc = killOne(c);

    int ret = 0;
    ret += remove(aa);
    ret += remove(bb);
    ret += remove(cc);
    return ret;
}

int dfs(int a, int b, int c, int num) {
    if (num == 0) return 0;
    int ret = killThree(a, b, c);
    move();
    ret += dfs(a, b, c, num-1);
    return ret;
}

void copy_board() {
    for (int i=0; i<N; i++) for (int j=0; j<M; j++)
        tBoard[i][j] = board[i][j];
}

int main() {
    scanf("%d%d%d", &N, &M, &D);
    for (int i=N-1; i>=0; i--)
        for (int j=0; j<M; j++)
            scanf("%d", &board[i][j]);

    int ret = 0;
    for (int a=0; a<M; a++)
        for (int b=a+1; b<M; b++)
            for (int c=b+1; c<M; c++) {
                copy_board();
                ret = max(ret, dfs(a, b, c, N));
            }
    printf("%d\n", ret);
    return 0;
}
```

---

## **전봇대**
### [문제링크](https://www.acmicpc.net/problem/8986)

### 문제접근 및 알고리즘
1. 전봇대 문제는 재밋는? 관점으로 풀어야한다.
2. 이 문제도 이분탐색과 비슷한 관점인 파라메트릭 서치로 접근해야 한다.
3. 이 의미는 각 좌표 값에 따라 정해진다고 생각하는 것이 아닌, **알맞은 전봇대 간격을 찾아가야한다**는 의미이다.
4. 각 간격에서 3가지 정보로 생각할 수 있는 부분이 있다.
5. 전봇대 간격이 k라고 할때, xi번째 전봇대는 k * i 좌표로 이동해야한다.
6. 이때 N의 전봇대들이 **k * i 보다 작은 전봇대 개수를 a**, **k * i 과 같은 전봇대 개수를 b**, **k * i 보다 큰 전봇대 개수를 c**라고 하자.
7. 여기서 **간격이 k+1**이 되었을때, **전봇대의 움직임 값의 변화는 a + b - c** 이며, **간격이 k-1**이 되었을때 **전봇대의 움직임 값 변화는 c + b - a** 이다.
8. 또한 여기서 전봇대 간격이 k에서 k+1이 된 경우 항상 a의 값은 증가하며, c값은 감소한다.
9. 결국 이를 잘 관찰하면 전봇대가 움직여야하는 움직임 값은 **이차 방정식** 형태임을 알 수 있다.
10. 이러한 이차 방정식은 **삼분 탐색**을 이용하면 최소 값을 찾아 갈 수 있다.
11. 삼분 탐색은 [갓 라이](http://m.blog.naver.com/kks227/221432986308)님이 설명해 주실 것이다.
12. 삼분 탐색 컨셉과 코드는 아주 단순하다. 우리는 이 값 분포가 이차 방정식 형태임을 찾아내는 것이 중요한 컨셉임을 이해하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 8986 yechan
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int MAX_N=100001;
const ll MAX_INF=1e9;
const ll MAX_V=(1LL<<62);

int N;
ll arr[MAX_N];

ll dist(ll d) {
    ll ret = 0;
    for (int i=1; i<N; i++)
        ret += abs(arr[i]-d*i);
    return ret;
}

int main() {
    scanf("%d", &N);
    for (int i=0; i<N; i++)
        scanf("%lld", &arr[i]);

    ll left=0, right=MAX_INF;
    while (right - left > 10) {
        ll mid1 = (left * 2 + right)/3;
        ll mid2 = (left + right*2)/3;
        ll d1 = dist(mid1);
        ll d2 = dist(mid2);
        if (d1 == d2) left = mid1, right = mid2;
        else if (d1 > d2) left = mid1;
        else right = mid2;
    }
    ll ret = MAX_V;
    for (ll i=left; i<=right; i++)
        ret = min(ret, dist(i));
    printf("%lld\n", ret);
    return 0;
}
```

---

## 크루스칼의 공
### [문제링크](https://www.acmicpc.net/problem/1396)

### 문제접근 및 알고리즘
- 이 문제는 2가지 접근 형태가 있다.
- 나는 처음에 접근한 형태는 그래도 알고 있었던 **LCA**형태이다.
- 나머지 한가지는 **parallel binary search**이다.

#### LCA 알고리즘 풀이
1. 이 알고리즘은 일반적으로 우리가 Query(질문)들을 순차적으로 풀어가는 Online Query로 처리하는 알고리즘이다.
2. 이 문제는 자세히 보면 아아주 친절하게도 모든 간선의 온도 값이 다르다.
3. 또한 MST 문제를 접해본 사람이라면 이러한 문제는 MST 문제임을 직감 할 것이다.
4. MST의 크루스칼 알고리즘은 O((N+M)logN) 이며 각 쿼리에 대해서 처리하면 O(Q(N+M)logN)으로 최적화를 위한 알고리즘이 필요함을 느낀다.
5. 여기서 우리는 크루스칼 알고리즘에서 merge 하는 과정에서 두 노드를 연결하는 과정을 Tree 처럼 새롭게 구성할 수 있다.
6. 여기서 Tree의 리프 노드는 각 정점으로 생각하고, 트리를 올라가는 경우는 온도가 증가하는 형태로 형성됨을 눈치 챌 수 있다.
7. 결국 각 Query에서 2개의 정점의 최소 공통 조상을 찾을 수 있다.
8. 이 공통조상이 가지는 리프 노드 개수와 온도 값이 각 Query에 대한 정답이다.
9. 여기서 이러한 탐색을 logN의 시간을 요구하므로 시간복잡도는 다음과 같다.
10. O((N+M+Q)logN)임을 알 수 있다.

#### Parallel binary search 알고리즘
1. 나도 이번에 처음 공부한 알고리즘이다.
2. 이 크루스칼 공은 이 Parallel binary search에 대표적인 문제이다.
3. 이 문제는 또한 offline query문제의 하나라고 한다.
4. 나는 위 알고리즘을 이해는 하긴 했지만, 미천하기에 갓 라이님 블로그를 추천한다
5. [블로그](http://blog.naver.com/PostView.nhn?blogId=kks227&logNo=221410398513&parentCategoryNo=&categoryNo=299&viewDate=&isShowPopularPosts=true&from=search)
6. 일단 컨셉은 위 lca와 같이 크루스칼 알고리즘으로 간선 M개중, K가 연결 되어 있을 경우 각 정점이 연결되어 있는지 아닌지 확인 가능하다.
7. 연결되어 있다면 K보다 적은 간선 개수에서 가능 할 수 있다. 또한 연결되어 있지 않다면 K보다 큰 간선 개수에서 가능 할 가능성이 있다.
8. 이러한 것을 크루스칼을 돌리면서 Q개의 Query가 가지는 lo와 hi값을 모두 업데이트하는 형태이다.
9. 이러한 형태를 이용하면 위 각 쿼리는 logM 만에 정답을 찾을 수 있기 때문에 O(QlogM)만에 정답을 찾게 되며 전체 시간 복잡도는 O(logM(N+Q)logN)이다.

#### 소스 코드 1
LCA c++ 코드입니다.
```c++
// baekjoon 1396 yechan
#include <bits/stdc++.h>
using namespace std;
using P = pair<int,int>;
using T = tuple<int,int,int>;

const int MAX_N=200001;
const int MAX_K=19;

int N, M, Q;
int root[MAX_N];
int par[MAX_N][MAX_K];
int temp[MAX_N];
int depth[MAX_N];
int sz[MAX_N];
vector<int> adj[MAX_N];
T node[MAX_N];

int find(int x) {
    if (root[x] < 0) return x;
    return root[x] = find(root[x]);
}

void merge(int a, int b) {
    a = find(a);
    b = find(b);
    if (a == b) return;
    root[b] = a;
}

void dfs(int curr, int prev) {
    for (int next: adj[curr]) {
        if (next == prev) continue;
        par[next][0] = curr;
        depth[next] = depth[curr] + 1;
        dfs(next, curr);
    }
}

int getlca(int a, int b) {
    if (depth[a] > depth[b])
        swap(a,b);

    int diff = depth[b] - depth[a];
    for (int i=0; diff; i++) {
        if (diff & 1) b = par[b][i];
        diff /= 2;
    }
    if (a == b) return a;

    for (int i=MAX_K-1; i >= 0; i--) {
        if (par[a][i] != par[b][i]) {
            a = par[a][i];
            b = par[b][i];
        }
    }
    return par[a][0];
}

int main() {
    memset(root, -1, sizeof(root));
    fill(sz, sz+MAX_N, 1);
    scanf("%d%d", &N, &M);
    for (int i=0; i<M; i++) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        node[i] = T(c, a, b);
    }
    sort(node, node+M);

    for (int i=N+1; i<=N+M; i++) {
        int u,v,w;
        tie(w,u,v) = node[i-N-1];
        if (find(u) == find(v)) continue;
        sz[i] = sz[find(u)] + sz[find(v)];
        temp[i] = w;
        adj[i].push_back(find(u));
        adj[i].push_back(find(v));
        merge(i, u);
        merge(i, v);
    }

    for (int i=N+M; i>N; i--)
        if (!depth[i])
            dfs(i,0);

    for (int j=1; j<MAX_K; j++)
        for (int i=1; i<=N+M; i++)
            par[i][j] = par[par[i][j - 1]][j-1];

    scanf("%d", &Q);
    for (int i=0; i<Q; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        if (find(a) != find(b)) {
            puts("-1");
            continue;
        }
        int lca = getlca(a, b);
        printf("%d %d\n", temp[lca], sz[lca]);
    }

    return 0;
}
```

#### 소스 코드 2
Parellel binary search c++ 코드입니다.
```c++
#include <cstdio>
#include <vector>
#include <algorithm>
#include <tuple>
using namespace std;
const int MAX = 100000;
typedef tuple<int, int, int> Edge;
 
// 유니온 파인드 구조체 정의
struct UnionFind{
    // ...
};
 
int main(){
    // 입력받기
    int N, M, Q, q[MAX][2];
    Edge edge[MAX];
    scanf("%d %d", &N, &M);
    for(int i = 0; i < M; ++i){
        int u, v, w;
        scanf("%d %d %d", &u, &v, &w);
        edge[i] = Edge(w, u-1, v-1);
    }
 
    // 간선 정렬
    sort(edge, edge+M);
 
    // 쿼리들을 전부 입력받아 둠
    scanf("%d", &Q);
    for(int i = 0; i < Q; ++i){
        scanf("%d %d", &q[i][0], &q[i][1]);
        --q[i][0]; --q[i][1];
    }
 
    // 각 쿼리의 lo, hi 값들을 초기화: hi 값은 M+1로 초기화해 둠
    // hi개의 간선까지 크루스칼 알고리즘을 진행시키면 두 정점이 같은 집합 내에 있음
    // lo = M인 채로 이분 탐색이 전부 끝나면 답은 -1
    int lo[MAX] = {0}, hi[MAX], result[MAX][2];
    fill(hi, hi+Q, M+1);
 
    // 병렬 이분 탐색
    while(1){
        // G[i]: mid = i인 쿼리들을 담아 둠, 따라서 1~M의 인덱스가 필요
        bool flag = false;
        vector<int> G[MAX+1];
        for(int i = 0; i < Q; ++i){
            // lo[i]+1 == hi[i]면 이미 i번 쿼리는 이분 탐색이 끝난 상태
            if(lo[i]+1 < hi[i]){
                flag = true;
                G[(lo[i]+hi[i])/2].push_back(i);
            }
        }
        if(!flag) break;
 
        // 크루스칼 알고리즘을 고유값 오름차순으로 간선 한 개씩 진행시킴
        UnionFind UF;
        for(int i = 0; i < M; ++i){
            // 간선 한 개를 뽑아 union 시도
            int u, v, w;
            tie(w, u, v) = edge[i];
            UF.u(u, v);
 
            // 이제 i+1개의 간선을 봤으므로, G[i+1]의 모든 쿼리들 이분 탐색 한 단계씩 진행
            for(int j: G[i+1]){
                int a = UF.f(q[j][0]), b = UF.f(q[j][1]);
                if(a == b){
                    // 두 정점이 같은 집합 안에 있음: 결과 갱신
                    hi[j] = i+1;
                    result[j][0] = w;
                    result[j][1] = -UF.p[UF.f(a)];
                }
                else lo[j] = i+1;
            }
        }
    }
 
    // 결과 출력
    for(int i = 0; i < Q; ++i){
        if(lo[i] == M) puts("-1");
        else printf("%d %d\n", result[i][0], result[i][1]);
    }
}
[출처] 병렬 이분 탐색(Parallel Binary Search) (수정: 2019-01-15)|작성자 라이
```


문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

