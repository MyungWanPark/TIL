## Dynamic_Programming
   
* 이것이 코딩테스트다
  * 그리디
  * 구현
  * DFS and BFS
  * ***정렬***
    * 선택정렬
    * 삽입정렬
    * 퀵정렬
    * 계수정렬
  * 이진탐색
  * Dynamic_Programming
  * 최단거리
  * 그래프 알고리즘
  ***   

### 고정점 찾기 problem 

```python
n = int(input())
location = list(map(int, input().split()))

location.sort()
distance = [1000000]* n

#안테나가 i에 있을때 거리
for i in range(n):
    temp = 0
    # 집이 각각 j에 위치함.
    for j in range(n):

        # 예를 들어 1,5,7,9의 위치에 집이 있다고 생각해보자.
        # 안테나가 1에 있을때, 거리의 합은 abs(1-1) + abs(1-5) + abs(1-7) + abs(1-9) 이렇다.
        temp += abs(location[i]-location[j])
    distance[i] = temp


min_index = 0
# 가장 거리를 작게해주는 location 인덱스 찾기
for i in range(1,n):
    if distance[min_index] > distance[i]:
        min_index = i

print(location[min_index])
```