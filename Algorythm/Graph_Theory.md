## 그래프 알고리즘
   
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
  * 최단거리
  * ***그래프 알고리즘***
  ***   



```python
# 서로소 집합 알고리즘 -> 사이클 판별 -> 크루스칼 알고리즘

#부모를 루트로 설정, 찾기
def find_parent(parent, x):

    if parent[x] != x:
        parent[x] = find_parent(parent,parent[x])
    
    return parent[x]

#union 연산 수행
def union(parent,x,y):

    a = find_parent(parent,x)
    b = find_parent(parent,y)

    if a > b:
        parent[a] = b
    else:
        parent[b] = a

#노드의 갯수와 간선의 갯수 입력받기 
v,e = map(int, input().split())
parent = [0] * (v+1)

#부모테이블에서 부모를 자기 자신으로 초기화
for i in range(1,v+1):
    parent[i] = i
'''
#union 연산 수행
for i in range(e):
    a,b = map(int , input().split())
    union(parent,a,b)
'''

'''
# CYCLE 판별하기
CYCLE = False

for i in range(e):
    a,b = map(int , input().split())
    #사이클이 발생한 경우, 종료
    if find_parent(parent,a) == find_parent(parent,b):
        CYCLE = True
    #사이클이 발생하지 않았다면, 합집합 수행
    else:
        union(parent,a,b)
'''
#크루스칼 알고리즘

edges = []
result = []

for i in range(e):
    a,b,cost = map(int, input().split())
    #비용순으로 정렬하기 위해 비용을 앞으로 뺀 튜플로 입력함
    edges.append((cost,a,b))

edges.sort()

for edge in edges:

    cost,a,b = edge
    if find_parent(parent,a) != find_parent(parent,b):
        union(parent,a,b)
        result += cost

'''
#각 원소가 속한 집합 찾기
print("각 원소가 속한 집합:")
for i in range(1,v+1):
    print(find_parent(parent,i), end = " ")

print()

# 부모테이블 출력
print("부모테이블 출력")
for i in range(1,v+1):
    print(parent[i],end= " ")
'''
# 순서를 고려한 위상정렬
from collections import deque

v,e = map(int, input().split())
#진입차수를 넣기 위해서
indegree = [0]*(v+1)

# 방향 그래프를 입력하기 위해서
graph = [[] for i in range(v+1)]

for i in range(e):
    a,b = map(int, input().split())
    graph[a].append(b)
    indegree[b] += 1

def topology_sort():
    
    result = []
    q = deque()

    for i in range(1,v+1):
        if indegree[i] == 0 :
            q.append(i)

    while q:

        now = q.popleft()
        result.append(now)

        for i in graph[now]:
            indegree[i] -= 1
            if indegree[i] == 0:
                q.append(i) 

    for i in result:
        print(i, end = ' ')

topology_sort()

```

### Problem 1

```python
def find_parent(parent,x):
    if parent[x] != x:
        parent[x] = find_parent(parent,parent[x])
    return parent[x]

def union_parent(parent,x,y):

    a = find_parent(parent,x)
    b = find_parent(parent,y)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

n,m = map(int, input().split())
parent = [0] * (n+1)

for i in range(1,n+1):
    parent[i] = i

for i in range(1,n+1):
    # 데이터를 리스트로 받아와야 '1'이 있는지 그때 그때 확인할 수 있다.
    data = list(map(int, input().split()))
    # n이 클 수도 있으니 반복문으로 사용!
    for j in range(n):
        if data[j] == 1:
            # j는 index이니 +1을 해준 값과 합집합을 한다.
            union_parent(parent,i,j+1)

plan = list(map(int,input().split()))

result = True

# 하나라도 안돼(?)면 이어지지 않는 것
for i in range(m-1):
    if find_parent(parent,plan[i]) != find_parent(parent,plan[i+1]):
        result = False

if result:
    print("yes")
else:
    print("no")
```

### Problem 2

```python
g = int(input())
p = int(input())
gi = []

# 탑승구 입력받기
for i in range(p):
    gi.append(int(input()))

def find_parent(parent,x):
    if parent[x] != x:
        parent[x] = find_parent(parent,parent[x])

    return parent[x]

def union(parent,a,b):

    x = find_parent(parent,a)
    y = find_parent(parent,b)

    if x < y:
        parent[y] = x
    else:
        parent[x] = y

parent = [0] * (g+1)

for i in range(g+1):
    parent[i] = i

# 가능횟수를 세기 위한 count 변수
count = 0
for i in range(p):

    # 합집합을 할 대상을 루트 -1로 잡기. 그래야 그리디 마냥 '이하의 수'에 대응시킬 수 있다.
    union_with = find_parent(parent,gi[i])-1

    if union_with < 0
        break

    union(parent,gi[i],union_with)
    count += 1

print(count)

```