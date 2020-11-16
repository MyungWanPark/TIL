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