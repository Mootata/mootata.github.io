---
title: N Queen 문제 (백준 9663.N_Queen with python)
date: 2022-01-04 23:01:45
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

알고리즘 공부를 시작하면서 여러 문제를 풀게 될 텐데 앞으로 문제를 풀다가 기억해두고 싶은 것이 생길 때마다 하나씩 블로그에 포스팅해 볼 예정입니다.

## N Queen 문제

[백준 9663.N_Queen 바로가기](https://www.acmicpc.net/problem/9663)
N Queen 문제는 백트래킹으로 풀이하는 대표적인 문제입니다.

```python
def n_queen(i): # i는 현재 행을 의미
    global case
    if i == n: # 현재 행이 마지막 행인 경우, = 탐색이 모두 끝났다는 뜻
        case += 1
        return
    for j in range(n): # 각 열에 퀸을 놓는 경우의 수를 확인
        col[i] = j # i행 j열에 퀸을 놓음
        if promising(i): # i행 j열에 퀸을 놓는 경우 유망한지 확인 (겹치는 퀸이 있는지 확인)
            n_queen(i + 1) # 겹치는 퀸이 없다면 다음 행으로 이동 해서 다시 각 열에 퀸을 놓는 경우를 확인

def promising(i): # 퀸이 다른 퀸을 공격할 수 있는지 확인 i는 행
    for j in range(i):                                        # col[i] == col[j] -> 같은 열에 놓이는 경우
        if col[i] == col[j] or abs(col[i] - col[j]) == i - j: # abs(col[i] - col[j]) == i - j 열의 차이의 절대값과 행의차이가 같다면
            return False                                      # 같은 대각선에 놓였다는 뜻
    return True # 위의 조건식이 만족되지 않는다면 유망하다고 판단 True 리턴

n = int(input()) # n x n 크기의 체스판 위에 n개의 퀸
case = 0 # N개의 퀸이 서로 공격할 수 없게 놓는 경우의 수
col=[0] * n # 깊이가 n인 트리 생성, col[i] = j 는 i행 j열에 퀸을 놓았다는 뜻(j가 0인 경우 비어있는 칸)
n_queen(0)
print(case)
```

위의 코드가 처음에 구현했던 것인데 실행해 보니 체스판의 크기가 13을 넘어가면 시간이 상당히 오래 걸리는 것을 볼 수 있었고, 아니나 다를까 제출 결과는 시간 초과였습니다..

따라서 이번 포스팅에서는 N Queen 문제의 최적화에 대해서 써보려고 합니다.

## N Queen 문제의 특징

- **N이 짝수일 경우**

  ![4queens](https://user-images.githubusercontent.com/75708329/148212001-a7f26e89-2cf3-4b09-b3f5-5933bfa32e29.PNG)

  위의 그림처럼 N Queen 문제에서는 N이 짝수일 경우, N개의 퀸이 서로 공격할 수 없도록 놓는 경우가 대칭이 되는 것을 알 수 있습니다.
  따라서 N이 짝수인 경우에는 왼쪽 절반의 경우의 수 \* 2를 한다면 연산의 수를 줄이면서 답을 구할 수 있게 됩니다.

* **N이 홀수일 경우**

  ![5queens](https://user-images.githubusercontent.com/75708329/148225274-1c3c051f-095f-4bbf-a152-e9be3726e90c.png)

  N이 홀수인 경우에도 비슷하게 진행합니다. 다만, 가운뎃 줄을 제외하고 짝수일 때와 같이 왼쪽 절반의 경우의 수 \* 2 로 경우의 수를 구합니다.
  이후 시도하지 않았던 가운뎃 줄의 경우의 수도 찾아줍니다. 예를 들어 N이 5일 때 퀸이 (1,1)과 (1,2)에 놓이는 경우의 수를 모두 구하고 그 값에 2를 곱한 뒤, 퀸이 (1,3)에 놓이는 경우의 수를 더해주는 겁니다.
  (N x N 크기의 체스판에 N개의 퀸을 서로 공격할 수 없게 두기 위해서는 반드시 퀸이 한 줄에 한 개씩 놓여야 한다는 것이 이 방식을 이해하는 데 도움이 될 수도 있을 것 같습니다.)

  다시 한번 정리해보면 (1,2)와 (1,3)에 놓이는 경우의 수에 2를 곱함으로써 (1,4)와 (1,5)에 놓이는 경우의 수까지 구할 수 있고, 마지막에 (1,3)에 놓이는 경우의 수를 더해 줌으로서 모든 경우의 수를 구할 수 있게 됩니다.

위의 특징들을 이용해 다시 문제를 풀어보았습니다.

```python
다시푼 코드
```
