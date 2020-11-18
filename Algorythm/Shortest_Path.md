## 최단거리
   
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
  * Dynamic_Programming
  * ***최단거리***
  * 그래프 알고리즘
***   

>## Which Cases?

1. 지정된 곳에서 다른 지점까지의 최단 거리인가?
2. 모든 지점에서 모든 지점까지의 최단 거리인가?

## 1-1 다익스트라 알고리즘 part 1 O(V^2)
   
<point:> 
* 매번 현재 갈 수 있는 노드 중 최소거리를 갖는 노드를 찾는다.
* 최단경로테이블을 step마다 갱신해준다. 
   
```python

INF = int(1e9)

#노드의 갯수:m, 간선의 갯수를 m
n,m = map(int, input().split())

#시작노드: start
start = int(input())

#간선 정보를 저장할 graph 배열을 만든다.
graph = [[]]

#초기 최단경로 테이블을 INF로 설정
distance = [INF] * (n+1)

#방문 여부를 나타내기 위한 리스트
visited = [False] * (n+1)

for i in range(m):
    a,b,c = map(int,input().split())
    graph[a].append((b,c))

#방문하지 않은 노드 중에 가장 가까운 노드 찾기
def get_smallest():

    min_value = INF
    index = 0

    for i in range(1,n+1):
        if distance[i] < min_value and not visited[i]:
            min_value = distance[i]
            index = i
    
    return index

def dijkstra(start):

    distance[start] = 0
    visited[start] = True

    #start노드와 연결된 노드에 대해 distance[] 리스트 갱신
    for j in graph[start]:
        distace[j[0]] = j[1]
    
    #start노드를 제외한 n-1개의 노드에 대해 진행한다.
    for i in range(n-1):
        
        now = get_smallest()
        visited[now] = True

        #가장 가까운 노드를 통해 갈 수 있는 최단경로 테이블 갱신
        for j in graph[now]:
            cost = distance[now]+ j[1]:
            if distance[j[0]] > cost:
                distance[j[0]] = cost

dijkstra(start)

for i in range(1,n+1):
    if distance[i] == INF:
        print("도달할 수 없음")
    else:
        print(distance[i])

```

## 1-2 다익스트라 알고리즘 part 2 O(ElogV)

<point:>
1. 파이썬의 최소 힙을 이용한 자료구조인 우선순위 큐를 이용한다.
    1-1. 힙을 이용한 우선순위 큐는 값을 넣거나 추출할때 모두 O(logN)의 시간이 걸린다.
2. 우선순위 큐를 이용하므로 현재 이동할 수 있는 노드중 가장 작은 거리를 찾는 함수O(V) 를 없앨 수 있다.
3. 방문했는지 여부에 대한 확인은 따로 visited =[] 배열을 만들지 않는다. 
우선순위 큐에서 나왔고 조건을 통과한다면 방문한다는 의미이다. 
왜냐면 현재 갈 수 있는 곳 중 가장 거리가 짧은 노드를 방문하기 때문. 
하지만 조건인 기존의 distance[now] 보다 큐에서 갓 나온 dist 의 거리가 더 크다면, 
이미 더 작은 거리의 노드가 우선순위 큐에서 나왔다는 의미이므로(즉, 방문함)무시한다. 
4. 개인적이 추측이지만 O(ElogV)가 될 수 있는 이유는 while q(q에 들어갈 수 있는 노드의 숫자는 최대 E개가 될 수 있다 -> 점점 작아지는 값이 들어가 계속 갱신되는 경우) 
X(=곱하기) 
heapq.heappop(q) (우선순위 큐에서 값을 뺄때 O(logE)만큼 된다.) 그리고 중복 간선을 포함하지 않는 경우는 
E는 V2보다 작다. 따라서 O(ElogE) -> O(ElogV^2) -> O(ElogV)

```python

INF = int(1e9)

# 차이점1, 우선순위 큐를 만들기 위해 heapq사용!
import heapq

#노드의 갯수:m, 간선의 갯수를 m
n,m = map(int, input().split())

#시작노드: start
start = int(input())

#간선 정보를 저장할 graph 배열을 만든다.
graph = [[]]

#초기 최단경로 테이블을 INF로 설정
distance = [INF] * (n+1)

for i in range(m):
    a,b,c = map(int,input().split())
    graph[a].append((b,c))

#차이점3 우선순위큐에서 가장 작은 거리를 가진 노드가 나오므로 가장 작은 거리의 노드를 구하는 함수를 없앤다.

def dijkstra(start):

    distance[start] = 0
    
    #차이점4 우선순위큐를 이용하므로 큐를 만든 후, 큐에 (거리,좌표) 튜플인 (0,start)를 삽입한다.
    q = []
    heapq.heappush(q,(0,start))

    #visited[start] = True
    
    #차이점5, 우선순위큐를 이용한 방식을 이용했을때, start를 거쳐 갈 수 있는 값 까지 포함되므로 우선순위큐 방식을
    #실시한다.

    #차이점6, 우선순위 큐 방식  
    #queue가 비면 끝난다.
    while q:
        #현재 갈 수 있는 노드 중 최단거리의 노드를 꺼낸다.
        dist, now = heapq.heappop(q)

        #방문한적 있는지 확인. 이미 방문했다면, 다른 그 다음 노드를 꺼낸다.
        if distance[now] < dist:
            continue
        #가장 가까운 노드를 통해 갈 수 있는 최단경로 테이블 갱신
        for j in graph[now]:
            cost = distance[now]+ j[1]:
            if distance[j[0]] > cost:
                distance[j[0]] = cost
                #차이점7, 거리가 기존 거리 리스트의 값 보다 더 작은 값이 존재한다면, 
                #방문 후보로 올리도록 우선순위 큐에 넣는다.
                heapq.heappush(q,(cost,j[0]))

dijkstra(start)

for i in range(1,n+1):
    if distance[i] == INF:
        print("도달할 수 없음")
    else:
        print(distance[i])

```
   

## 2 플로이드 워셜 알고리즘 O(N^3)
<point:>   
1. D(ab) = min( D(ab), D(ak)+D(kb) )   
즉, 어떤 곳으로 가는데 곧 바로 'A에서 B로 가는 비용'과 'A에서 K를 거쳐 B로 가는 비용'을 비교하여    
더 작은 값으로 갱신. 이때 K는 각각의 모든 노드를 말함.
2. 거리에 대한 데이터를 2차원 배열에 저장한다.
   
   
```python

INF = int(1e9)
#노드의 갯수와 간선의 갯수를 입력받는다. 
n,m = map(int, input().split())

#초기에 모든 노드끼리의 거리는 무한으로 세팅해놓는다.
graph = [[INF]*(n+1) for _ in range(n+1)]

#간선의 데이터로 초기 거리의 그래프를 만든다.
for i in range(m):
    a,b,c = map(int, input().split())
    graph[a][b] = c

#자기 자신으로 향하는 거리는 0으로 세팅
for i in range(1,n+1):
    for j in range(1,n+1):
        if i == j:
            graph[i][j] == 0

# 플로이트 워셜 알고리즘
for k in range(1,n+1):
    for a in range(1,n+1):
        for b in range(1,n+1):
            graph[a][b] = min(graph[a][b],graph[a][k]+graph[k][b])

for i in range(1,n+1):
    for j in range(1,n+1):
        if graph[i][j] == INF:
            print("도달 불가",end = " ")
        else:
            print(graph[i][j],end = " ")
    print()

```

