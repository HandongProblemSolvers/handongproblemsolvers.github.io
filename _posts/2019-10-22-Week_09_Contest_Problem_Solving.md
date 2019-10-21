---
title: 9주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다. (학회장아니에요ㅋ)

다들 시험기간 고생 많으셨습니다ㅠㅠ

모두 승리하셨기를 기도하며 이제 PS로 돌아옵시다 `^_^?`

그럼, 문제해설 시작~

---
## 목차
1. [꿍의 우주여행](#꿍의-우주여행)
2. [놀이공원](#놀이공원)
3. [색종이3](#색종이3)
4. [택배](#택배)
5. [비용](#비용)

---

## **꿍의 우주여행**
### [문제링크](https://www.acmicpc.net/problem/9501)

### 문제접근 및 알고리즘
1. 자 우주여행가자
2. 각 우주선에 주어지는 값들은 아래와 같다.
3. v: 시간당 우주선 최고 속도 (m/s)
4. c: 우주선 연료통 크기 (l)
5. f: 시간당 사용하는 연료량 (l/s)
6. 우주선 최대거리 = v * c / f (m/s * l * s/l = m)
7. 우주선 최대거리 >= 원하는 거리 를 만족하는 우주선 수가 답.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 4108 yechan
#include <bits/stdc++.h>
using namespace std;

int T, N;
double D, v, c, f;

int main() {
    scanf("%d", &T);
    while (T--) {
        scanf("%d %lf", &N, &D);
        int ret = 0;
        for (int i=0; i<N; i++) {
            scanf("%lf %lf %lf", &v, &c, &f);
            double t = v*c/f;
            if (t >= D) ret++;
        }
        printf("%d\n", ret);
    }
}
```

---

## **놀이공원**
### [문제링크](https://www.acmicpc.net/problem/2594)

### 문제접근 및 알고리즘
1. 근로시간이 오전10시부터 오후 10시이다.
2. 여러 놀이기구의 운영 시간이 주어진다.
3. 먼저, 시간:분 단위로 이를 다루기에는 어렵다.
4. 고로 **모두 분 단위로** 바꿔준다.
5. ex) 10시 30분 -> 10 * 60 + 30 = 630
6. 이후 사용여부를 확인하기 위해서 work 배열 하나를 생성한다.
7. 각 놀이기구 시작시간부터 종료시간까지 work 배열에 표시한다.
8. 이후 10시부터 22시까지 work 배열을 확인한다.
9. 최대로 쉴수 있는 시간을 찾는다.
10. N=50으로 놀이동산 시간을 체크하는데 N * 1440 = 50 * 720 = 36000으로 시간이 충분하다.
11. O(NT)

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2594 yechan
#include <bits/stdc++.h>
using namespace std;

int N, x1, y1, x2, y2, S, E, ret, ans;
bool work[1440]; // 24*60 = 1440

int main() {
    scanf("%d", &N);
    for (int i=0; i<N; i++) {
        scanf("%02d%02d %02d%02d", &x1, &y1, &x2, &y2);
        S = x1*60 + y1 - 10;
        E = x2*60 + y2 + 10;
        for (int i=S; i<E; i++) work[i]=true;
    }

    for (int i=10*60; i<22*60; i++) {
        if (!work[i]) ans++, ret=max(ret, ans);
        else ans=0;
    }
    printf("%d\n", ret);
    return 0;
}
```

---

## **색종이3**
### [문제링크](https://www.acmicpc.net/problem/2571)

### 문제접근 및 알고리즘
1. 이 문제는 도화지 크기가 100x100으로 고정, 색종이도 10으로 고정이다.
2. 도화지를 표현하기 위한 board 배열을 설정한다.
3. 입력된 색종이 위치에 따라 board 배열에 색칠한다.
4. 이후 알고리즘이 좀 으렵다.
5. 일단 생각의 단순화를 위해서 x좌표와 y좌표의 생각을 구분시킨다.
6. 위 이야기는, 먼저 y좌표는 생각하지 않고 x좌표 관점만 확인한다.
7. x좌표 확인이란, 사각형은 width * height 이므로 width만 먼저 생각하는 이야기다.
8. 간단한 DP 점화식으로 각 (x, y)에서 현재까지 최대 width 길이를 알 수 있다.
9. 점화식: **DP(x,y) = (board(x)(y) ? DP(x-1, y) + 1 : 0)**
10. 이러한 점화식 형태 이후로 height에 대해서 관찰하게 되면 아아아아주 유명한 그리디 문제가 생각난다.
11. 바로 [히스토그램에서 가장 큰 직사각형](https://www.acmicpc.net/problem/6549) 문제이다.
12. 해설은 [여기](https://www.acmicpc.net/blog/view/12)를 참고하자.
13. 위를 이해했다면 히스토그램의 높이를 DP에 구한 width로 일반화 시키고 각 x좌표에서 y축을 돌면서 최대 크기를 찾으면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2571 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=100;

int N, dp[MAX_N][MAX_N];
bool board[MAX_N][MAX_N];

int main() {
    scanf("%d", &N);
    for (int t=0; t<N; t++) {
        int x, y;
        scanf("%d%d", &x, &y);
        for (int i=x; i<x+10; i++)
            for (int j=y; j<y+10; j++)
                board[i][j]=true;
    }

    for (int i=1; i<100; i++)
        for (int j=1; j<100; j++)
            if (board[i][j]) dp[i][j] = dp[i-1][j] + 1;

    stack<int> s;
    int ans = 0;
    for (int i=1; i<100; i++) {
        for (int j=1; j<100; j++) {
            int left = j;
            while (!s.empty() && dp[i][s.top()] > dp[i][j]) {
                int height = dp[i][s.top()];
                s.pop();
                int width = j;
                if (!s.empty()) {
                    width = (j - s.top() - 1);
                }
                ans = max(ans, width*height);
            }
            s.push(j);
        }
        while (!s.empty()) {
            int height = dp[i][s.top()];
            s.pop();
            int width = MAX_N;
            if (!s.empty()) {
                width = MAX_N-s.top()-1;
            }
            ans = max(ans, width*height);
        }
    }
    printf("%d\n", ans);
    return 0;
}
```

---

## **택배**
### [문제링크](https://www.acmicpc.net/problem/1866)

### 문제접근 및 알고리즘
1. 1~10000번까지 지점이 있고 0에 본점이 있다.
2. N개의 지점의 배송을 해야한다.
3. 이 문제를 조금만 고민해본다면 트럭과 헬리콥터의 형태를 느끼게된다.
4. 모든 지점을 트럭으로 보내던지, 헬리콥터로 한 지점으로 보낸 뒤, 일정 영역을 헬리콥터가 담당하는 형태이다.
5. 여기서 DP 점화식이 중요한데 2가지가 존재한다.
6. 이 DP 점화식을 위해서는 먼저 지점 값으로 N개를 정렬한다.
7. 이후 DP(i)를 정의하면, 1~i개를 담당하기 위한 최소 값으로 정의한다.
8. 트럭 비용을 T, 헬기비용을 H, 각 N개 지점 값이 A(i)배열에 있다고 하자.
9. 첫번째 점화식은 **DP(i)=DP(i-1)+A(i)xT**
10. 첫번째 점화식은... 자명하다. 그냥 본점에서 트럭으로 보내는거다.
11. 두번째 점화식이 어렵다. 일단 j <= i를 만족하는 자연수 j를 가정하자.
12. 하나의 헬기가 j+i+1개의 물품을 실어서 j ~ i중 한 지점에 내려 j ~ i지점을 모두 배송한다고 하자.
13. 여기서 재밋는 점은 수학적으로 **A((i+j)/2)**지점 곧 j ~ i지점의 중앙값 지점이 최적임을 알 수 있다.
14. 고로 이 중앙값을 이용하면 일일이 j ~ i지점에 헬기를 수송해보지 않아도 된다. (이를 하면 시간초과 O(N^3))
15. 자 두번째 점화식은 다음과 같다
16. **DP(i)=DP(j-1)+cost(j)**, **cost(i)=H**, **cost(j)=cost(j+1)+(A(i+j+1)/2-A(j))**
17. 위 점화식을 이용하면 O(N^2)으로 N개의 배달 최소비용을 알 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1866 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=3001;

int N, T, H, A[MAX_N], dp[MAX_N];

int main() {
    scanf("%d", &N);
    for (int i=1; i<=N; i++)
        scanf("%d", &A[i]);
    sort(A+1, A+N+1);
    scanf("%d%d", &T, &H);

    for (int i=1; i<=N; i++) {
        dp[i] = dp[i-1] + A[i]*T;
        int cost = H;
        for (int j=i; j>0; j--) {
            cost += (A[(i+j+1)/2]-A[j])*T;
            dp[i] = min(dp[i], dp[j-1]+cost);
        }
    }
    printf("%d\n", dp[N]);
    return 0;
}
```

---

## 비용
### [문제링크](https://www.acmicpc.net/problem/2463)

### 문제접근 및 알고리즘
1. 최소 가중치 간선 이라는 힌트를 보면 다들 생각나는 것이 있을 것이다.
2. 바로 MST(Minimum Spanning Tree)이다.
3. 여기서 보여주는 형태는 작은 weight 값을 가지는 간선이 먼저 사라진다고 할때, 두 지점 u, v 간에 연결이 끈어지는 비용을 cost(u,v)라고 정의한다.
4. 여기서 재밋는 것은 한번 끈어지면 다시 연결될 일이 없으며, 끈어지는 것은 순서가 있다는 점이다.
5. 여기서 발상의 전환으로 모두 끈어져 있는 상태로 생각하자.
6. 역순으로 가장 작은 weight값을 끈어보는 형태가 아닌, 가장 큰 weight값을 연결하는 형태로 진행하자.
7. 무엇이 생각나는가 바로 MST이다.
8. 여기서 Union-find를 이용하는데, 많이 사용하는 Trick인 Union-find 그룹의 크기를 저장하기 위해서 root의 음수 값의 절대값을 크기로 사용하는 것이다.
9. 이제 가장 큰 weight 값의 u, v 지점을 merge 할때, 이미 연결되었는지 아닌지를 Union-find로 확인할 수 있다.
10. 이미 연결되어 있다면 cost(u,v) 값은 이때 생기지 않는다.
11. Union-find로 두 지점 u,v를 연결되었다고 하자. 이때 u의 union 크기를 usize, v의 union 크기를 vsize라고 하자.
12. 여기서 cost가 얻어지는 개수는 usize * vsize 개수 만큼 발생한다는 것을 알 수 있다.
13. 이제 이 cost는 어떻게 구하는가? 아직 연결되어 있지 않은 간선 수가 cost 값이다.
14. 이는 이전에 미리 모두 더해 놓은뒤 연결할때 하나씩 weight값을 빼주면 된다.
15. 결과를 출력한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2463 yechan
#include <bits/stdc++.h>
using namespace std;
using P = pair<int,int>;
using PP = pair<int,P>;
using ll = long long;

const int MAX_N=100001;
const ll DIV_NUM=1e9;

int N, M;
PP node[MAX_N];
int root[MAX_N];

int find(int x) {
    if (root[x] < 0) return x;
    return root[x] = find(root[x]);
}

ll merge(int a, int b, int c) {
    a = find(a);
    b = find(b);
    if (a == b) return 0;
    ll ret = 1LL*c*root[a]*root[b];
    // 항상 a 그룹 크기가 크도록 만든다.
    if (root[a] > root[b]) swap(a,b);
    root[a] += root[b];
    root[b] = a;
    return ret;
}

int main() {
    memset(root, -1, sizeof(root));
    scanf("%d%d", &N, &M);
    ll sum = 0;
    for (int i=0; i<M; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        u--,v--;
        sum += w;
        node[i] = PP(-w, P(u,v));
    }
    sum %= DIV_NUM;
    sort(node, node+M);

    ll ret = 0;
    for (int i=0; i<M; i++) {
        int u = node[i].second.first;
        int v = node[i].second.second;
        int w = -node[i].first;
        ret = (ret + merge(u,v,sum))%DIV_NUM;
        sum = (DIV_NUM + sum - w)%DIV_NUM;
    }
    printf("%lld\n", ret);
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

