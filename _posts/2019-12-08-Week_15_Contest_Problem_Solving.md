---
title: 15주차 문제 해설

author:
  name: yechan
---

안녕하세요~!! HPS 학회원 유예찬입니다.

알고리즘 열심히하고 싶은데 요즘 정신이 없네요 ㅠㅠ

요즘 너무 바쁜관계로 코드와 간단한 컨셉만 적겠습니다.

5번은 못 풀었습니다 ㅎㅎ...

모두 시험 화이팅 하시고...

그럼, 문제해설 시작~

---
## 목차
1. [기숙사 바닥](#기숙사-바닥)
2. [초콜릿 식사](#초콜릿-식사)
3. [전시장](#전시장)
4. [암기왕](#암기왕)
5. [직사각형 만들기](#직사각형-만들기)

---

## **기숙사 바닥**
### [문제링크](https://www.acmicpc.net/problem/2858)

### 문제접근 및 알고리즘
1. 이차방정식을 풀면된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2858 yechan
#include <bits/stdc++.h>
using namespace std;

double R, B;

void go(double a, double b, double c) {
    double d = -b/2;
    double e = sqrt(b*b-4*a*c)/2;
    printf("%.0lf %.0lf\n", d+e, d-e);
}

int main() {
    scanf("%lf%lf", &R, &B);
    go(1, -(R/2+2), R+B);
    return 0;
}
```

---

## **초콜릿 식사**
### [문제링크](https://www.acmicpc.net/problem/2885)

### 문제접근 및 알고리즘
1. `K <= 2^n`를 만족하는 가장 작은 n를 찾는다. (첫번째 정답)
2. 여기서 K의 이진수에서 처음으로 1의 값을 가지는 자리를 찾는다. `mCnt`
3. `n - mCnt` (두번째 정답)가 쪼개야 하는 수이다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2885 yechan
#include <bits/stdc++.h>
using namespace std;

int K;

int main() {
    scanf("%d", &K);
    int cnt=0;
    int mCnt=1e9;
    while ((1<<cnt) < K) {
        if ((K>>cnt) & 1) mCnt=min(mCnt, cnt);
        cnt++;
    }
    if ((K>>cnt) & 1) mCnt=min(mCnt, cnt);
    printf("%d %d\n", (1<<cnt), cnt-mCnt);

    return 0;
}
```

---

## **전시장**
### [문제링크](https://www.acmicpc.net/problem/2515)

### 문제접근 및 알고리즘
1. 간단한 DP이다.
2. 바텀업으로 짜는게 포인트이다.
3. 높이로 sort한다.
4. i번째 그림 현재 높이가 H일때, H-S를 만족하는 DP 좌표 j를 찾아서 `DP(i) = max(DP(i-1), DP(j)+i번째 그림 가격)`를 하면 된다.
5. DP 정의는 `1 ~ i번째 까지 그림을 사용하여 얻을 수 있는 최대 가격`이다.
6. lower_bound를 사용하기 위해서 0번째 idx에 작은 값을 넣는 것도 재밋는 포인트이다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2515 yechan
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using P = pair<int, int>;

const int MAX_N=300001;

int N, S;
P data[MAX_N];
ll dp[MAX_N];

int main() {
    scanf("%d%d", &N, &S);
    for (int i=1; i<=N; i++)
        scanf("%d%d", &data[i].first, &data[i].second);
    sort(data+1, data+N+1);
    data[0]=P(-1e9, 0);
    dp[0]=0;

    for (int i=1; i<=N; i++) {
        int idx = lower_bound(data, data+N+1, P(data[i].first-S+1, -1e9)) - data;
        idx--;
        dp[i] = max(dp[idx]+data[i].second, dp[i-1]);
    }
    printf("%lld\n", dp[N]);
    return 0;
}
```

---

## **암기왕**
### [문제링크](https://www.acmicpc.net/problem/2776)

### 문제접근 및 알고리즘
1. 간단하게 sort하여, 투포인터로 체크하면 된다.

### 소스 코드
c++ 코드입니다.
```c++
// baekjoon 2776 yechan
#include <bits/stdc++.h>
using namespace std;
using P = pair<int,int>;

const int MAX_N=1000001;

int T, N, M;
int A[MAX_N];
P B[MAX_N];
bool ans[MAX_N];

int main() {
    scanf("%d", &T);
    while (T--) {
        memset(ans, 0, sizeof(ans));

        scanf("%d", &N);
        for (int i=0; i<N; i++)
            scanf("%d", &A[i]);
        sort(A, A+N);

        scanf("%d", &M);
        for (int i=0; i<M; i++)
            scanf("%d", &B[i].first), B[i].second=i;
        sort(B, B+M);

        int Apos = 0, Bpos = 0;
        while (Apos < N && Bpos < M) {
            if (A[Apos] == B[Bpos].first) ans[B[Bpos].second]=true, Bpos++;
            else if (A[Apos] < B[Bpos].first) Apos++;
            else Bpos++;
        }
        for (int i=0; i<M; i++)
            puts(ans[i] ? "1":"0");
    }
    return 0;
}
```

---

## **직사각형 만들기**
### [문제링크](https://www.acmicpc.net/problem/1801)

### 문제접근 및 알고리즘
1. 못풀었다.
2. DP 문제이며, N이 16, 값이 10으로 완탐으로 풀린다고 한다.
3. 완전 탐색시 가지치기를 해주어야한다.
4. w*h > 여태까지 구한 답
5. w <= h 일 때 2*w + 2*h <= 막대기 전체 길이
6. w <= h 일 때 4*w <= 막대기의 전체길이
7. 위 3가지 조건을 만족해야 함 만족하지 않는 경우 가지치기

#### 소스 코드
c++ 코드입니다.
```c++
못품 풀면 올림.
```


문제 해설이 끝났습니다.

읽느라 수고하셨습니다.

문제가 있거나 궁금한점은 문의 부탁드립니다.

HPS 화이팅~!!

