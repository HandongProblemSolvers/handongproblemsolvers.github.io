---
title: 7주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

이번학기에 처음으로 연습 대회를 진행하기 시작하여
부족한 점이 많습니다.

이번학기에 학회 모임이 없는 관계로 문제 풀이를 교류할 수 없는 상황이지만
이렇게라도 교류를 하고자 합니다.

혹시 문제 해설에 문제가 있거나, 다른 해법이 있다면
언제든지 연락주시면 업데이트 하겠습니다!

그럼, 문제해설 시작~

---
## 목차
1. [지뢰찾기](#지뢰찾기)
2. [팰린드롬인지 확인하기](#팰린드롬인지-확인하기)
3. [경비행기](#경비행기)
4. [채점](#채점)
5. [물통](#물통)

---

## **지뢰찾기**
### [문제링크](https://www.acmicpc.net/problem/4108)

### 문제접근 및 알고리즘
1. 말그대로 지뢰찾기이다.
2. 각 좌표에서 지뢰가 존재하지 않는다면, 8방위에 지뢰 개수를 센다.
3. 8방위는 dir로 표현한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 4108 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

const int MAX_N=101;
const int dir[8][2] = { {-1, -1},{-1, 0},{-1, 1},{0, -1},{0, 1},{1, -1},{1, 0},{1, 1} };
int N, M;
char board[MAX_N][MAX_N];

int main() {
    while (1) {
        scanf("%d%d", &N, &M);
        if (N==0 && M==0) break;

        for (int i=0; i<N; i++)
            scanf("%s", board[i]);

        for (int i=0; i<N; i++) {
            for (int j=0; j<M; j++) {
                if (board[i][j] == '*') printf("*");
                else {
                    int num = 0;
                    for (int d=0; d<8; d++) {
                        int nx = i + dir[d][0];
                        int ny = j + dir[d][1];
                        if (nx < 0 || nx >= N || ny < 0 || ny >= M) continue;
                        if (board[nx][ny] == '*') {
                            num++;
                        }
                    }
                    printf("%d", num);
                }
            }
            puts("");
        }
    }
    return 0;
}
```

---

## **팰린드롬인지 확인하기**
### [문제링크](https://www.acmicpc.net/problem/10988)

### 문제접근 및 알고리즘
1. 문자열이 팰린드롬인지 확인한다.
2. 확인 방법은 다음과 같다
3. 먼저 문자열 S의 길이가 N이라고 하자.
4. 0 <= i && i < N 이라고 하자.
5. 정수 i에 대해 S(i) == S(N-1-i)를 모두 만족하는지 확인한다.
6. i가 N/2보다 큰 경우 대칭이므로 판단하지 않아도 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10988 yechan
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

const int MAX_N=101;
int slen;
char str[MAX_N];

bool check() {
    for (int i=0; i<=slen/2; i++)
        if (str[i] != str[slen-1-i])
            return false;
    return true;
}

int main() {
    scanf("%s", str);
    slen = (int)strlen(str);
    printf("%d\n", check());
    return 0;
}
```

---

## **경비행기**
### [문제링크](https://www.acmicpc.net/problem/2585)

### 문제접근 및 알고리즘
1. 일단 이 문제는 시작점 (0,0)에서 도착점 지점 (10000,10000) 까지 최대 K번 착륙 할 수 있을때, 최소 연료통에 크기에 대해 묻는다.
2. 먼저 이 문제에서 중요한 부분은 바로 **연료통의 크기에 따른 착륙 횟수**이다.
3. 첫번째로, **연료통의 크기가 작아지면, 착륙횟수가 증가한다.** 라는 점.
4. 두번째로, **연료통 크기가 A일때 K번만에 도착 가능하다면, A<B을 만족하는 모든 B에 대해서 K번 만에 도착 가능하다.**
5. 이러한 관점에서 연료통 크기가 정해지면, 도착지점까지 최소 착륙 횟수를 알 수 있다. 또는 도착 불가능 여부 또한 알 수 있다.
6. 자 (0,0) ~ (10000, 10000) 까지의 거리는 sqrt(2) * 10000 < 약 15000이다.
7. 고로 연료통 크기가 1500 이상인 경우는 한번에 시작점에서 도착지점까지 갈 수 있다.
8. 연료통 크기는 1 ~ 1500 사이에 존재함을 알 수 있다.
9. 이후, 도착지점까지 최소 착륙 횟수를 알기 위해서는 각 비행장을 Vertex(정점)으로 생각하는 그래프로 생각한다.
10. 각 두 비행장 **u와 v 사이에 거리를 가중치 값으로 하는 양방향 간선**을 생성한다.
11. 이러한 그래프에서 **시작점에서 도착점까지 다익스트라 알고리즘**을 적용한다
12. 여기서 다익스트라 알고리즘의 distance를 판단할때 조금의 트릭?를 사용한다.
13. 일반적으로 다익스트라는 시작지점에서 u까지의 최단 거리를 dist(u)에 저장한다.
14. 하지만 우리는 거리보다, **최소 착륙횟수**가 중요하다. 고로 dist 값에 최소 착륙횟수를 저장한다.
15. u->v 간에 가중치 값이 현재 연료통으로 갈 수 있는지 여부를 체크한다.
16. 이후에 dist(v) > dist(u) + 1 인 경우 다익스트라 알고리즘으로 업데이트한다.
17. 시작점으로부터 각각 정점(비행장)까지의 최단거리를 얻은뒤, 도착점 까지 최단거리를 반환한다.
18. 이제 이 **최단거리 값이 K보다 같거나 작은 경우 K번 이하의 착륙으로 도착 가능**하다. 고로 **현재 연료통 크기보다 큰 경우는 모두 K번 이하로 착륙 가능**하며, 현재보다 작은 연료통에 대해서 K번 이하로 가능한지 여부를 판단한다.
19. 이 **최단거리 값이 K보다 큰 경우 K번 이하의 착륙으로 도착 불가능**하다. 고로 **현재 연료통 크기보다 작은 경우 모두 K번 이하로 착륙 불가능**하며, 현재보다 큰 연료통에 대해서 K번 이하로 가능한지 여부를 판단한다.
20. 최단거리 값이 K이하인 연료통중 최소값이 정답이다.
21. 혹시 개념이 이해되지 않는다면, **이분탐색**, **파라메트릭 서치** 와 **다익스트라 알고리즘**을 이해하길 바란다.

### 소스 코드
c++ 코드입니다.
```c++
#include <cstdio>
#include <queue>
#include <vector>
#include <functional>
#include <cmath>
#include <utility>
#include <algorithm>
using namespace std;

#define SQ(x) ((x)*(x))
typedef pair<double, double> P;
typedef pair<int,int> Pii;

const int MAX_INF = 1e9;
const int MAX_N = 1003;
const int MAX_L = 1500;

int N, K, dist[MAX_N];
P pos[MAX_N];
bool visited[MAX_N];

int getDistance(P &a, P &b) {
    double ret = sqrt(SQ(a.first - b.first)+SQ(a.second - b.second));
    return (ret-(int)ret > 0) ? ((int)ret/10) + 1 : ((int)ret/10);
}

int Dijkstra(int L) {

    fill(visited, visited+MAX_N, false);
    fill(dist, dist+MAX_N, MAX_INF);

    dist[0] = 0;
    priority_queue<Pii, vector<Pii>, greater<Pii> > PQ;

    PQ.push(Pii(0, 0));
    while (!PQ.empty()) {
        int curr;
        do {
            curr = PQ.top().second;
            PQ.pop();
        } while (!PQ.empty() && visited[curr]);
        if (visited[curr]) break;

        visited[curr] = true;

        for (int i=1; i<N; i++) {
            if (i == curr) continue;
            if (getDistance(pos[curr], pos[i]) > L) continue;
            if (dist[i] > dist[curr] + 1) {
                dist[i] = dist[curr] + 1;
                PQ.push(Pii(dist[i], i));
            }
        }
    }

    return dist[N-1]-1;
}

int main() {
    scanf("%d%d", &N, &K);
    for (int i=1; i<=N; i++)
        scanf("%lf%lf", &pos[i].first, &pos[i].second);
    N+=2;
    pos[0].first = pos[0].second = 0.f;
    pos[N-1].first = pos[N-1].second = 10000.f;

    int ret = MAX_L;
    int left = 0, right = MAX_L;
    while (left <= right) {
        int mid = (left+right) / 2;
        if (Dijkstra(mid) > K) left = mid + 1;
        else ret = min(ret, mid), right = mid - 1;
    }
    printf("%d\n", ret);
    return 0;
}
```

---

## **채점**
### [문제링크](https://www.acmicpc.net/problem/6989)

### 문제접근 및 알고리즘
1. 일단 처음에 문제접근은 아래와 같다.
2. 일단 DFS로 모든 경우를 확인하는 것(완전탐색)은 2^150으로 불가능이 자명하다.
3. 이러한 경우 **그리디 알고리즘**, **다이나믹 프로그래밍** 혹은 **이분탐색** 형태를 생각한다.
4. 일단 **그리디 알고리즘**은 일정한 패턴, 즉 항상 올바른 최적해가 존재해야 하지만 이번 문제와 같은 경우 예외가 정말 많은것 같아서 제외시켰다.
5. 다음 다이나믹 프로그래밍 같은 경우 State를 나누는 것이 정말 중요하다. 여기서 State로 나눠본 형태는 다음과 같다.
6. 문제번호 N=150, 최대 점수 = 100 * (150 * (150+1))/2 = 1132500, 이전 정답 합=150*100 = 15000
7. 첫번째, DP State : (문제번호, 이전 정답 합, 현재 점수) >> 메모리 초과
8. 두번째, DP State : (문제번호, 현재 점수) >> 메모리 초과
9. ? 이때 맨붕이 왔다.
10. 이후에 최대한 두번째 DP 형태로 문제를 해석해 나갔다.
11. 일단 DP Table를 만들수 없기 때문에, 처음 접근 형태는 i번 부터 j번 까지 정답으로 맞았을때 얻을 수 있는 정답을 tsum(i)(j)에 저장하였다.
12. 이후에 tsum 배열을 활용하여 최대한 subproblem 형태로 접근했다.
13. 설명하기 애매하여 그림으로 설명하겠다.
14. DP Table은 (점수, 문제번호)의 형태로 사용하였다. (vector에 저장)
15. 여기서 j-2번까지 만들 수 있었던 점수를 이용하여 tsum(j ~ i+1)(i) 과의 합으로 새로운 점수를 만들 수 있다.
16. 새로운 점수가 만들어 졌다면, vector에 저장한다.
17. 이를 반복하고 visited 배열을 확인한다.
![poster]({{ site.baseurl }}/img/contest/week7_4_1.JPG)
18. 이렇게 하면 1204ms로 꾸역꾸역 해결한다... ㅠ_ㅠ
19. 이제 DP 최적화를 하겠다. 이 개념을 이해하기 위해서는 **Bit-Masking** 개념을 이해해야한다.
20. DP 배열에서 (점수, 문제번호) state는 0 또는 1 만을 저장한다.
21. 여기서 integer는 32bit으로 각 비트로 표현하여 메모리를 32배 절약할 수 있다.
22. 개념 이해를 위해 문제 번호를 저장하는 형태를 그림으로 설명하겠다.
23. 아래는 예시를 위해 저장공간이 32bit가 아닌 8bit라고 가정하자.
24. 처음에는 모두 0으로 채워져 있으며, 오른쪽에서부터 0 ~ 7까지 bit 자리를 나타낸다.
![poster]({{ site.baseurl }}/img/contest/week7_4_2_1.JPG)
25. 여기서 점수가 문제번호 x에서 점수 3점을 나타낼 수 있다고 하자.
26. 이때 아래 그림과 같이 DP에 저장할 수 있다.
![poster]({{ site.baseurl }}/img/contest/week7_4_2_2.JPG)
27. 추가로 문제번호 x에서 점수 13점을 나타낼 수 있다고 하면, 아래 그림과 같이 저장된다
![poster]({{ site.baseurl }}/img/contest/week7_4_2_3.JPG)
28. 이러한 식으로 DP Table를 사용하면 DP(문제번호, 점수/32)로 150 * 1132500/32 < 150 * 40000 = 600만 으로 저장 가능하다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 6989 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

typedef pair<int,int> P;


const int MAX_N=152;
// 최대 점수 = 100*(150*(150+1))/2 = 1132500 < 40000*32 = 1280000
const int MAX_V=40000; 

int N, K, cur, s;
int arr[MAX_N], sum[MAX_N], tsum[MAX_N][MAX_N]; // tsum(s, e)
unsigned int dp[MAX_N][MAX_V];

int main() {
    scanf("%d", &N);
    for (int i=1; i<=N; i++) {
        scanf("%d", &arr[i]);
        sum[i] = sum[i-1] + arr[i];
        tsum[1][i] = tsum[1][i-1] + sum[i];
        for (int j=2; j<=i; j++)
            tsum[j][i] = tsum[j][i-1] + sum[i] - sum[j-1];
    }
    scanf("%d", &K);
    if (K > tsum[1][N]) return !printf("%d\n", K);

    dp[0][0]=1;
    for (int i=1;i<=N;i++) {
        dp[i][0]=1;
        for (int j=1;j<=i+1;j++) {
            int q = tsum[j][i]/32, r = tsum[j][i]%32;
            dp[i][q] |= (1<<r);
            if (j==1 || j==2) continue;
            for (int k=0;k<=tsum[1][j-2]/32+1;k++) {
                if(r) dp[i][k+q+1] |= (dp[j-2][k]>>(32-r));
                dp[i][k+q] |= (dp[j-2][k]<<r);
            }
        }
    }

    for (int k=K; ;k++) {
        int q = k/32, r = k%32;
        if (dp[N][q] & (1 << r)) continue;
        return !printf("%d\n", k);
    }
    return 0;
}
```

---

## 물통
### [문제링크](https://www.acmicpc.net/problem/14867)

### 문제접근 및 알고리즘
1. 일단 처음에는 무식하게 접근하였다.
2. 각 상황에서 할 수 있는 행동은 6가지이다.
3. Fill A, Fill B, Empty A, Empty B, A to B, B to A이다.
4. 여기서 visited 배열을 사용하여 A,B의 대한 BFS를 진행한다.
5. 여기서 트릭으로 A와 B의 값이 10만으로 이차원 visited 배열을 만들지 못한다.
6. 이를 해결하기 위해서 set를 사용하여 check한다.
7. 위는 (A물통의 양, B물통의 양)을 Vertex로 생각하는 그래프 접근이다.
8. 여기는 다른 문제해법이 존제한다.
9. 물통 횟수를 찾는 최적의 접근이 존재한다. 곧, **그리디 알고리즘**이 존재한다.
9. 물통을 사용하는 형태는 2가지중 한가지로 반복된다.
10. 첫번째는, A물통을 채우고, B물통을 비우는 형태
11. 두번째는, B물통을 채우고, A물통을 비우는 형태이다.
12. 이 두가지는 서로 역행 관계를 가진다?
13. 역행이란 무슨 뜻인가?, 전체 순회하는데 걸리는 횟수가 X라고 하자.
14. A에서 (c, d)가 Y번째 나온다고하면, B에서는 (c, d)가 X-Y번째에 나온다. ㅎㅎ...
15. 이러한 관점에서 첫번째 패턴만 반복해도 두번째 패턴에서 얻어지는 값을 알 수 있다.
16. 둘중 최소 값을 출력하고, 존재하지 않으면 -1 출력한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 14876 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

int N, M, a, b, wa, wb, ret, total;

int main() {
    scanf("%d%d%d%d", &N, &M, &a, &b);

    if (a > N || b > M) return !printf("-1\n");
    if (a == N && b == M) return !printf("2\n");
    if (a == 0 && b == 0) return !printf("0\n");

    while (!(wa == N && wb == M)) {
        if (wa == 0) wa = N; // fill A
        else if (wb == M) wb = 0; // empty B
        else {
            if (wa > M - wb) { // A -> B
                wa = wa - (M - wb);
                wb = M;
            }
            else { // A <- B
                wb += wa;
                wa = 0;
            }
        }
        total++;
        if (wa == a && wb == b) ret = total;
    }
    if (!ret) return !printf("-1\n");

    printf("%d\n", min(ret, total-ret)); // reverse case
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

