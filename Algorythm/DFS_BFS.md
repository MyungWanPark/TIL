## DFS and BFS
   
* 이것이 코딩테스트다
  * 그리디
  * 구현
  * ***DFS and BFS***
  * 정렬
    * 선택정렬
    * 삽입정렬
    * 퀵정렬
    * 계수정렬
  * 이진탐색
  * Dynamic_Programming
  * 최단거리
  * 그래프 알고리즘
  ***   
  
>	## When to use?
   BFS: 연결된 것(그래프)안에서 재귀적으로 모든 곳을 탐색해야 할때
   DFS: 연결된 것(그래프)안에서 거리를 구하는 등 단계별로 확인해야 할때

>	## How to use?
    
>   
    BFS: 그래프 안에서 연결된 것을 재귀적으로 호출하여 탐색
         호출되는 대로 방문처리됨.
    DFS: 그래프 안에서 연결된 것을 queue에 넣고, 넣는 동시에 방문 처리. 
         queue에서 빠져나오는데로 즉, 원래 queue에 입력된 순서대로 방문.
    
### 연구소 문제
```python
# BFS 2번 연구소 문제
n, m = map(int, input().split())
data = []
temp = [[0]*m for _ in range(n)]
for i in range(n):
    data.append(list(map(int, input().split())))

#상하좌우 이동
dx = [-1,1,0,0]
dy = [0,0,-1,1]

# 바이러스 퍼지는 함수
def virus(x,y):

    for i in range(4):
        mx = x + dx[i]
        my = y + dy[i]

        if mx >= 0 and mx < n and my >= 0 and my < m:

            if temp[mx][my] == 0:
                temp[mx][my] = 2
                virus(mx,my)
count = 0
result = 0
def make_three_wall():

    global count,result
    # 밑에 벽 만드는 곳에서 무한 호출이 아닌, 3번만 딱 돌게 하기 위해 조건문을 붙인다
    # 벽이 3개일때 바이러스가 침투하지 못한 곳의 갯수를 구한다.
    if count == 3:
        # 벽이 바뀔때 마다, 세이프 존의 갯수가 달라지므로 매번 시뮬레이션 판을 만들어서 세이프존의 갯수를 센다.
        for i in range(n):
            for j in range(m):
                temp[i][j] = data[i][j]

        # 바이러스를 침투시킨다.
        for i in range(n):
            for j in range(m):
                if temp[i][j] == 2:
                    virus(i,j)
        # 벽이 3개일때 바이러스가 침투하지 못한 곳의 갯수를 구한다.
        safe = 0
        for i in range(n):
            for j in range(m):
                if temp[i][j] == 0:
                    safe += 1
        result = max(safe, result)
        # return을 해야 make_three_wall함수가 무한 호출 되지 않고 3개까지만 생성되고 다시 벽 1개를 없애고 새로운 1개를 만들 수 있다.
        return

    # 벽을 3개 만드는데, 가능한 모든 곳을 채우기 위한 조합의 모든 경우의 수를 돈다.
    for i in range(n):
        for j in range(m):
            if data[i][j] == 0:
                data[i][j] = 1
                count += 1
                make_three_wall()
                data[i][j] = 0
                count -= 1

make_three_wall()
print("result = ",result)

```