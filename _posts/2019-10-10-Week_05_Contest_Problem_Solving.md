---
title: 5주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

이번학기에 처음으로 연습 대회를 진행하기 시작하여
부족한 점이 많습니다.

이번학기에 학회모임이 없는 관계로 문제 풀이를 교류할 수 없는 상황이지만
이렇게라도 교류를 하고자 합니다.

혹시 문제 해설에 문제가 있거나, 다른 해법이 있다면
언제든지 연락주시면 업데이트 하겠습니다!

그럼, 문제해설 시작~

---
## 목차
1. [쉽게 푸는 문제](#쉽게-푸는-문제)
2. [LCM](#lcm)
3. [관중석](#관중석)
4. [동전 게임](#동전-게임)
5. [리조트](#리조트)

---

## **쉽게 푸는 문제**
### [문제링크](https://www.acmicpc.net/problem/1292)

### 문제접근 및 알고리즘
1. 문제이해를 정확하게 했다면, A와 B의 값이 1000임을 확인한다.
2. 위 수열을 만들기 위해서 어떻게 해야할지 생각한다.
3. 수열은 단순히 count 값을 이용하면 단순하게 만들 수 있다.
4. 수열 순서를 기억하여, A번째부터 B번째 까지 수를 더한다.

### 소스 코드
c++ 코드입니다.
```c++
#include <cstdio>
#include <algorithm>
using namespace std;

int A, B;

int main() {
    scanf("%d%d", &A, &B);
    int sum = 0;
    int value = 1;
    int cnt = 0;
    for (int i=1; i<=B; i++) {
        if (A <= i) sum += value;
        if (++cnt == value) cnt=0, value++;
    }
    printf("%d\n", sum)
    return 0;
}
```

---

## **LCM**
### [문제링크](https://www.acmicpc.net/problem/5347)

### 문제접근 및 알고리즘
1. 최소 공배수를 이해해야한다.
2. A,B의 `최소 공배수는 A*B/(A,B의 최대공약수)`
3. 최대 공약수는 `유클리드 알고리즘`으로 구할 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
#include <cstdio>
#include <algorithm>
using namespace std;

typedef long long ll;

int gcd(int a, int b) {
    return b ? gcd(b, a%b): a;
}

int T, A, B;

int main() {
    scanf("%d", &T);
    while (T--) {
        scanf("%d%d", &A, &B);
        int g = gcd(A, B);
        printf("%lld\n", 1LL*A*B/g);
    }
}
```

---

## **관중석**
### [문제링크](https://www.acmicpc.net/problem/10166)

### 문제접근 및 알고리즘
1. 문제이해가 처음에 어려웠다... ㄷㄷ
2. 반지름이 A인 원과 B인 원이 서로소가 아닌 경우 곂치는 좌석이 생긴다.
3. 모두 시작은 12시 부터 시작하므로 반지름이 A인 경우 시계방향으로 0 ~ A-1로 좌석의 번호를 매겨주었다.
4. 반지름이 6인 경우, 앞 좌석중 1, 2, 3, 4 좌표와 겹쳐지는 것을 확인할 수 있다.
5. 각각의 Case를 분석해 보았다.
6. (1, 6) -> idx 0 좌표가 겹친다.
7. (2, 6) -> idx 0, 3 좌표가 겹친다.
8. (3, 6) -> idx 0, 2, 4 좌표가 겹친다.
9. (4, 6) -> idx 0, 3 좌표가 겹친다.
10. 결국 반지름 6인 장소는 0, 2, 3, 4은 관람 불가이다.
11. 문제에서 D1, D2 반지름이 주어진다면 알고리즘은 다음과 같다.
12. 반지름 i는, D1 ~ i-1 간의 GCD 관계를 확인하고 겹치는 좌표를 제거한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10166 yechan
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

const int MAX_N=2001;

bool visited[MAX_N];

int gcd(int a, int b) {
    return b ? gcd(b, a%b) : a;
}

int main()
{
    int D1, D2;
    scanf("%d%d", &D1, &D2);
    int ret = 0;
    for (int i=D1; i<=D2; i++) {
        memset(visited, 0, sizeof(visited));
        ret += i;
        for (int j=D1; j<i; j++) {
            int g = gcd(i, j);
            for (int k=0; k<i; k+= i/g) {
                if (!visited[k]) {
                    visited[k]=true, ret--;
                }
            }
        }
    }
    printf("%d\n", ret);
    return 0;
}
```

---

## **동전 게임**
### [문제링크](https://www.acmicpc.net/problem/10837)

### 문제접근 및 알고리즘
1. 불가능한 경우에 대해 정확히 알아야 된다 생각했다.
2. 불가능 조건은 이미 이전에 `자신이 모두 이기더라도 지는 상황` 인 경우이다.
3. 두 플레이어를 A와 B라 하고, A -> B -> A -> ... 순서로 게임이 진행된다하자.
4. 불가능 조건은 이기고 있는 `서로 점수 차이-1 > 남은 경기수 `입니다.
5. 예외로 여기서 A가 이기고 있을때에는 B의 경기수가 1번 더 많아 `remain+1`를 해줍니다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10837 yechan
#include <cstdio>
#include <algorithm>
using namespace std;

int main() {
    int K, C, M, N;
    scanf("%d%d", &K, &C);
    for (int i=0; i<C; i++) {
        scanf("%d%d", &M, &N);
        int cur_K = min(M, N);
        int diff = abs(M-N); // 점수 차이
        int remain = K - cur_K - diff; // 남은 경기수
        bool check = true;
        if (M > N && diff-1 > remain+1) check=false; // 한경기 더 가능
        if (M < N && diff-1 > remain) check=false;
        puts(check ? "1" : "0");
    }
    return 0;
}
```

---

## 리조트
### [문제링크](https://www.acmicpc.net/problem/13302)

### 문제접근 및 알고리즘
1. 여름방학 일수인 N = 100 이며, 쿠폰은 최대 40장 얻을 수 있다.
2. 위를 생각하고 DP 테이블을 잡아준다.
3. DP란 Dynamic Programming의 줄임말로, 작은 문제를 풀어 큰 문제를 풀어가는 형태이다.
4. Divide-Conquer와 DP의 다른점은 문제의 크기가 Divide-Conquer와 다르게 다이나믹? 하다는 점이다. 즉 Divide-Conquer는 작은 문제의 크기가 같지만 DP는 제각각이다.
5. 이제 잡소리?는 그만하고 문제의 집중하자.
6. 이 문제는 왜 DP인가에 대한 이야기를 하자.
7. 각 날마다 리조트에서 하루, 연속 3일 또는 연속 5일을 고르는 3가지 상황에 따라 가격이 다르다.
8. 또한 연속권을 살 경우 쿠폰이 주어진다.
9. 모든 것을 확인하는 형태는 최소 못해도 3^20 으로 시간초과이다.
10. 여기서 결국 `중복되는 형태 또는 상황에 대해 반복할 이유가 없다.`
11. 이러한 중복되는 상황을 state로 정의하여 DP Table에 저장할 수 있다.
12. 이제 위 문제 상황을 state로 정의 할 수 있는지 확인하자.
13. DP state = (표를 사는 날짜)(쿠폰 개수) 로 정의하자.
14. DP table = 100 * 40 = 최대 4000으로 메모리 저장 가능하다.
15. `DP(i, j) = i번째 날에 j개의 쿠폰을 가지고 있을때, i번째부터 여름 마지막 날까지 리조트를 이용하는 최소 금액` 으로 정의하자. 
16. DP의 큰 문제를 작은 문제로 풀어보자. 곧 Sub-problem을 구하자.
17. Case 1. 리조트의 갈 수 없는 날, `DP(i, j) = DP(i+1, j)`
18. Case 2. 1일권 살 경우, `DP(i, j) = DP(i+1, j) + 10000`
19. Case 3. 3일권 살 경우, `DP(i, j) = DP(i+3, j+1) + 25000`
20. Case 4. 5일권 살 경우, `DP(i, j) = DP(i+5, j+2) + 37000`
21. Case 5. 쿠폰 사용할 경우, 단 j >= 3 `DP(i, j) = DP(i+1, j-3)`
23. 위 상황을 코드로 구현하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 13302 yechan
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

const int MAX_N=101;
const int MAX_INF=2e9;

int N, M;
bool visited[MAX_N];
int dp[MAX_N][MAX_N];

int dfs(int here, int cp) {
    if (here >= N) return 0;
    if (visited[here]) return dfs(here+1, cp);
    int &ret = dp[here][cp];
    if (ret != -1) return ret;
    ret = MAX_INF;
    // buy one day
    ret = min(ret, dfs(here+1, cp) + 10000);
    // buy three day
    ret = min(ret, dfs(here+3, cp+1) + 25000);
    // buy five
    ret = min(ret, dfs(here+5, cp+2) + 37000);
    // use coupon
    if (cp >= 3) {
        ret = min(ret, dfs(here+1, cp-3));
    }
    return ret;
}

int main()
{
    memset(dp, -1, sizeof(dp));
    scanf("%d%d", &N, &M);
    for (int i=0; i<M; i++) {
        int x;
        scanf("%d", &x);
        visited[x-1]=true;
    }
    printf("%d\n",dfs(0, 0));
    return 0;
}
```

문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

