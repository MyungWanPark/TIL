## Dynamic_Programming
   
* 이것이 코딩테스트다
  * 그리디
  * 구현
  * DFS and BFS
  * 정렬
    * 선택정렬
    * 삽입정렬
    * 퀵정렬
    * 계수정렬
  * 이진탐색
  * ***Dynamic_Programming***
  * 최단거리
  * 그래프 알고리즘
  ***   
>	## When to use?
   
먼가 규칙이 없으면 못풀것 같은 문제 & 부분 값을 구한것이 계속 같아서 연산 횟수를 줄이고 싶을때
   
>	## How to use?

>   
    1. 보기의 값으로 예를 들어본다.   
    2. 앞에서 부터 차례대로 살펴보면서 규칙을 찾는다.   
    3. 앞에 1000개의 수열이 이미 있다고 생각하고, 1001번째, 1002번째 등 과의 관계를 살펴본다.   

**Bottom-up**(재귀를 이용하고, 연산 값을 배열에 저장하여 중복 연산을 줄인다.- Memoization)과 
**Top-down**(반복문을 이용하고, 연산 값을 배열에 저장하여 차곡 차곡 쌓는다. - DP table)
이 둘 중 Top Down을 더 선호한다. 재귀함수를 돌릴 수 있는 깊이가 한정되어 있기 때문이다.    
   

## 느낀점

처음에는 DP를 보고 어려워했다. 하지만 DP가 아니면 그 문제를 과연 어떻게 효과적으로 풀 수 있을까?

Problem) n가지 화폐 종류가 주어지고 m을 만들어야 할때, 최소한의 화폐 수를 구하는 문제가 있다.   
ex> 2,3,5의 화폐가 있고 15를 만들어야 할때, 최소한의 갯수는 어떻게 구할 것인가?

```python
# m이 최대 10000까지 되니, 1원의 화폐로도 만들 수 없는 최대값으로 10001을 놓았다.
# 그리고 m까지 될 수 있으니, m개로 인덱스를 DP 테이블에 넣어야 하니 m을 곱하였다.
d = [10001] * m

# a(n) = min(a(n-2)+1,a(n-3)+1,a(n-5)+1)

Unit = [2,3,5]
d[0] = 0

for i in Unit:
  for j in range(i,m+1):
    # 0을 바탕으로 d[2]가 되고, d[2]를 바탕으로 d[4]가 나온다.
    # 0을 바탕으로 d[3]이 되고, d[2]를 바탕으로 d[5]가 나온다. 즉, 앞의 값d[n-i]이 10001이 아니면, 
    # 화폐의 크기를 더한 d[n]값도 같이 나온다.
    # 이때 min이 있는 이유는 d[4]같은 경우, 화페의 크기가 2일때는 값이 나오지만, 화페의 크기가 3일때는 나오지 않는다.
    # 따라서 값이 10001이 아닌 최소값으로 해야 하므로, min이 붙는다.

    d[n] = min(d[n],d[n-i]+1)

```

### 금광 problem 
```python
TC = int(input())
# for 문 안에 테스트케이스를 넣었으니, 테스트 케이스마다 결과값을 프린트 한다.
for y in range(TC):
    n,m = map(int,input().split())
    array = list(map(int,input().split()))
    dp = []

    index = 0
    # 새롭게 안 지식! 한 줄로 배열이 주어질때, 마치 슬라이스를 해서 넣을 수 있는데 그 핵심은 슬라이싱 범위에 변수를 넣었기에 가능하다.
    for j in range(n):
        dp.append(array[index:index+m])
        index += m

    # 보통과 다르게 행령에서 열이 더 상위에 있는데, 그 이유는 하나의 열이 완성되기 위해 모든 행의 값들이 다 계산되어져야 하기 때문이다.
    for j in range(1,m):
        for i in range(n):

            # 가장 위에 있는 행렬일 경우
            if i == 0:
                # 왼쪽 위의 값이 존재하지 않으므로 왼쪽 값과 동일하게 만들어 준다.
                left_up = 0
            else:
                # 가장 위쪽에 있지만 않으면, 왼쪽 위의 값을 아래와 같이 표현할 수 있다.
                left_up = i-1
            #왼쪽 값은 언제든지 표현 할 수 있으므로 다음과 같이 표현한다.
            left = i

            # 열 안에서 맨 아래쪽에 있는 경우
            if i == n-1:
                # 왼쪽 아래의 값은 지정할 수 없으므로, 왼쪽 값과 동일하게 한다.
                left_down = n-1
            # 열 안에서 맨 아래쪽만 아니면, 왼쪽 아래의 값은 다음과 같이 지정할 수 있다.
            else:
                left_down = i+1
            # 가장 중요한 점화식! 규칙을 어떻게든 찾아내었고, 그 규칙은 다음과 같다.
            dp[i][j] = dp[i][j] + max(dp[left_up][j-1],dp[left][j-1],dp[left_down][j-1])


    result = 0

    for i in range(n):
            result = max(result,dp[i][m-1])

    print(result)
```

### 삼각형  problem 
```python
import copy

n = int(input())
a = []
count = 0
for i in range(n):
        a.append(list(map(int,input().split())))

d = copy.deepcopy(a)

d[0][0] = a[0][0]

for i in range(1,n):
    for j in range(i+1):
        if j == 0:
            d[i][j] = d[i-1][j] + a[i][j]
        elif i == j:
            d[i][j] = d[i-1][j-1] + a[i][j]
        else:
            d[i][j] = max(d[i-1][j-1],d[i-1][j]) + a[i][j]
        print(d[i][j], end = " ")
    print()

result = 0
for i in range(n):
    result = max(result, d[n-1][i])

print(result)
```
