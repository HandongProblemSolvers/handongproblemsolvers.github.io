---
title: 라이님 블로그 완전탐색 해설

author:
  name: yechan
---

안녕하세요 HPS 학회원 유예찬입니다.

개인적으로 알고리즘 별로 **정말 적어도 5문제**는

체화시켜서 알고리즘 별 **코드 틀**을 갖춰야 한다고 생각합니다.

**체화**란, 잘 짠 사람 코드의 **각 요소, 각 줄의 의미, 데이터 구조** 등을

정확하게 이해하는 것을 의미합니다.

**코드 틀**은, 알고리즘의 각 상태를 **구현하기 위한 방법**에

대한 지식으로 생각하시면 됩니다.

일단 개인적으로 문제는 쉽든 어렵든 **무조건 많이 푸는 것**이 좋습니다.

또한 1시간 ~ 2시간 이상 풀리지 않으면 답을 보시는 것을 추천합니다.

그럼 문제 해설 시작!

---
## 목차
1. [일곱 난쟁이](#일곱-난쟁이)
2. [분해합](#분해합)
3. [사탕 게임](#사탕-게임)
4. [유레카 이론](#유레카-이론)
5. [숫자 야구](#숫자-야구)
6. [체스판 다시 칠하기](#체스판-다시-칠하기)
7. [부분수열의 합](#부분수열의-합)

---

## **일곱 난쟁이**
### [문제링크](https://www.acmicpc.net/problem/2309)

### 문제접근 및 알고리즘
1. 먼저 순서대로 출력하기 위해서 9명을 정렬한다.
2. 트릭은 아래와 같다.
3. 9명 중 7명을 찾는 것이 아닌, **제외할 2명**을 찾는 것이다.
4. 이중 반복문으로 찾을 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2309 yechan
#include <bits/stdc++.h>
using namespace std;

int sum, arr[11];

int main() {
    for (int i=0; i<9; i++) {
        scanf("%d", &arr[i]);
        sum += arr[i];
    }
    sort(arr, arr+9);

    for (int i=0; i<9; i++) {
        for (int j=i+1; j<9; j++) {
            if (sum - arr[i] - arr[j] == 100) {
                for (int k=0; k<9; k++) {
                    if (k == i || k == j) continue;
                    printf("%d\n", arr[k]);
                }
                return 0;
            }
        }
    }
    return 0;
}
```

## **분해합**
### [문제링크](https://www.acmicpc.net/problem/2231)

### 문제접근 및 알고리즘
1. N이 100만이며, 각 자리수 더하는 시간은 최대 7번이다.
2. 700만은 충분히 계산 가능한 시간이다.
3. 1부터 N-1까지 부분합을 구한다.
4. 구분합이 N과 같은 경우 출력
5. 1부터 N-1까지 부분합이 N과 같은 경우가 없다면 0을 출력

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2231 yechan
#include <bits/stdc++.h>
using namespace std;

int N;

bool isDivSum(int n) {
    int sum = n;
    while (n) {
        sum += n%10;
        n/=10;
    }
    return sum == N;
}

int main() {
    scanf("%d", &N);
    for (int i=1; i<N; i++)
        if (isDivSum(i))
            return !printf("%d\n", i);

    printf("%d\n", 0);
    return 0;
}
```

## **사탕 게임**
### [문제링크](https://www.acmicpc.net/problem/3085)

### 문제접근 및 알고리즘
1. 귀찮은 구현 문제이다.
2. N이 최대 50으로 정말 작다.
3. 작은 경우엔 완전 탐색 가능성을 의심한다.
4. 일단 사탕 교환하는 경우의 수는 최대 N^2 * 2개이다
5. 이후 가장 긴 연속 부분 행열 또한 N^2 * 2번으로 찾을 수 있다.
6. O(N^4)는 625만으로 충분하다.
7. 구현은 다음과 같다.
8. 각 (i,j)에서 오른쪽 또는 아래로 자리를 바꿔본다. (색이 다른 경우)
9. 이후, 가장 긴 연속 부분 행열을 찾는다.
10. 이를 모든 (i,j)에서 반복한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 3085 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=51;
const int dir[2][2]={{0,1},{1,0}};

int N;
char board[MAX_N][MAX_N];

int test() {
    int ret = 0;
    for (int i=0; i<N; i++) {
        int cnt = 0;
        char pv = '\0';
        for (int j=0; j<N; j++) {
            if (pv == board[i][j]) {
                cnt++;
            } else {
                cnt = 1;
                pv = board[i][j];
            }
            ret = max(ret, cnt);
        }
    }

    for (int j=0; j<N; j++) {
        int cnt = 0;
        char pv = '\0';
        for (int i=0; i<N; i++) {
            if (pv == board[i][j]) {
                cnt++;
            } else {
                cnt = 1;
                pv = board[i][j];
            }
            ret = max(ret, cnt);
        }
        ret = max(ret, cnt);
    }
    return ret;
}

int main() {
    scanf("%d", &N);
    for (int i=0; i<N; i++)
        scanf("%s", board[i]);

    int ret = 0;
    for (int i=0; i<N; i++) {
        for (int j=0; j<N; j++) {
            for (int d=0; d<2; d++) {
                int nx = i + dir[d][0];
                int ny = j + dir[d][1];
                if (nx >= N || ny >= N) continue;
                if (board[i][j] == board[nx][ny]) continue;
                swap(board[i][j], board[nx][ny]);
                ret = max(ret, test());
                swap(board[i][j], board[nx][ny]);
            }
        }
    }
    printf("%d\n", ret);

    return 0;
}
```

## **유레카 이론**
### [문제링크](https://www.acmicpc.net/problem/10448)

### 문제접근 및 알고리즘
1. 일단 삼각수 n * (n+1)/2 < 1000를 만족하는 n은 1~46이다.
2. 결국 46개의 삼각수를 더하는 경우의 수는 46^3이다.
3. 46^3은 약 10만으로 가능하다.
4. 여기서 삼각수로 만들어지는 수를 triple table에 기록하고 이를 참조한다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 10448 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=1001;
const int MAX_T=46;
int T, N;
bool triple[MAX_N];
int num[MAX_T];

int main() {
    for (int j=1; j<MAX_T; j++)
        num[j] = (j * (j+1)) / 2;

    for (int i=1; i<MAX_T; i++)
        for (int j=1; j<MAX_T; j++)
            for (int k=1; k<MAX_T; k++)
                if (num[i] + num[j] + num[k] <= 1000)
                    triple[num[i] + num[j] + num[k]] = true;

    scanf("%d", &T);
    while (T--) {
        scanf("%d", &N);
        printf("%d\n", triple[N]);
    }
    return 0;
}
```

## **숫자 야구**
### [문제링크](https://www.acmicpc.net/problem/2503)

### 문제접근 및 알고리즘
1. 생각의 전환이 중요한 문제이다.
2. 이는 **각 질문의 조건을 만족할 수 있는 숫자만 남기는 전략을 사용하는 것이다**
3. 약 1000개의 숫자가 각 질문을 만족하는지 판단하는데는 커봐야 3만 정도 걸린다.
4. 질문은 최대 100번으로 완전 탐색시 300만이다.
5. 가능함을 알 수 있다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2503 yechan
#include <bits/stdc++.h>
using namespace std;

int N;
bool ball[1001];

bool check(int a, int b, int strike, int ball) {
    int a1 = a/100, a2 = (a/10)%10, a3 = a%10;
    int b1 = b/100, b2 = (b/10)%10, b3 = b%10;
    int snum = (a1==b1) + (a2==b2) + (a3==b3);
    int bnum = (a1==b2) + (a1==b3) + (a2==b1) + (a2==b3) + (a3==b1) + (a3==b2);
    return (strike == snum) && (ball == bnum);
}

int main() {
    for (int i=1; i<10; i++)
        for (int j=1; j<10; j++)
            for (int k=1; k<10; k++) {
                if (i == j || j == k || i == k) continue;
                ball[i*100+j*10+k]=true;
            }

    scanf("%d", &N);
    for (int i=0; i<N; i++) {
        int Q, S, B;
        scanf("%d%d%d", &Q, &S, &B);
        for (int j=0; j<1000; j++) {
            if (!ball[j]) continue;
            if (!check(Q, j, S, B)) ball[j]=false;
        }
    }

    int ret = 0;
    for (int i=0; i<1000; i++)
        if (ball[i])
            ret++;
    printf("%d\n", ret);
    return 0;
}
```

## **체스판 다시 칠하기**
### [문제링크](https://www.acmicpc.net/problem/1018)

### 문제접근 및 알고리즘
1. 8 x 8로 자를 수 있는 모든 경우를 이중 반복문으로 잘라본다.
2. 이후 자른 판이 체스판이 되기위해서 바꿔야 하는 최소 색깔 수를 찾는다.
3. 이는 (x+y)가 홀수냐 짝수냐에 따라서 힌색 또는 검은색이 되어야한다.
4. 또한 재밋는 트릭은 체스판을 180도 회전하면 각 지점의 색깔이 반전된다.
5. 고로 체스판 (0,0)이 힌색일 때 바꿔야 하는 수가 x라고 하면 (0,0)이 검정색일 때 바꿔야 하는 색의 수는 64-x임을 알 수 있다.
6. 나의 코드는 (0,0)이 검은색인 체스판으로 생각하고 바꿔야 하는 색의 수를 찾았다.
7. 이후 최소로 바꿔야하는 색의 수를 찾았다.
8. 8 * 8 * N * M = 64 * 2500 = 16만으로 시간은 충분하다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1018 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=51;

int N, M;
char board[MAX_N][MAX_N];
const char check[3]="BW";

int main() {
    scanf("%d%d", &N, &M);
    for (int i=0; i<N; i++)
        scanf("%s", board[i]);

    int ret = 1e9;
    for (int si=0; si<N-7; si++) {
        for (int sj=0; sj<M-7; sj++) {
            int cnt = 0;
            for (int i=0; i<8; i++) {
                for (int j=0; j<8; j++) {
                    int x = si+i, y = sj+j;
                    if (board[x][y] == check[(x+y)%2]) cnt++;
                }
            }
            ret = min(ret, min(cnt, 64-cnt));
        }
    }
    printf("%d\n", ret);

    return 0;
}
```

## **부분수열의 합**
### [문제링크](https://www.acmicpc.net/problem/1182)

### 문제접근 및 알고리즘
1. 크기가 N인 집합의 부분집합 개수는 2^N개이다.
2. N이 최대 20으며, 2^20 = 100만으로 완전 탐색이 가능하다.
3. dfs를 이용하여 i번째가 선택되었을때 or 안 되었을때 두가지 경우로 나누어 들어간다.
4. 모두 선택했을때, 크기가 1이상이며 sum이 S임을 확인한다.
5. 개수를 센다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 1182 yechan
#include <bits/stdc++.h>
using namespace std;

const int MAX_N=21;

int N, S;
int arr[MAX_N];

int dfs(int here, int sum, int size) {
    if (here == N) return ((sum == S) && (size > 0));
    return dfs(here+1, sum+arr[here], size+1)+dfs(here+1, sum, size);
}

int main() {
    scanf("%d%d", &N, &S);
    for (int i=0; i<N; i++)
        scanf("%d", &arr[i]);
    printf("%d\n", dfs(0, 0, 0));
    return 0;
}
```

해설이 끝났습니다.

방학이지만 알고리즘 파이팅하시길 바랍니다.
