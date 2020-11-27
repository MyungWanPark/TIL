# Binary_Search      
* 이것이 코딩테스트다
  * 그리디
  * 구현
  * DFS and BFS
  * 정렬
    * 선택정렬
    * 삽입정렬
    * 퀵정렬
    * 계수정렬
  * **이진탐색**
  * Dynamic_Programming
  * 최단거리
  * 그래프 알고리즘
        
***   
>	## When to use?
      
         
**연산 횟수**가 2000만번이 넘어가면 시간초과가 되기 때문에 입력조건 및 연산 횟수를 계산해서
~~ㅈㄴ~~ 많아질것 같으면 O(log N) 탐색법인 이진탐색 사용을 고려해본다. 이때  O(log N)인 이유는 탐색범위를 절반씩 날려가면서 탐색하기 때문이다.
   
##### 다만 배열이 정렬이 되어있어야 한다. 
   
```python
def binary_search(array,target,start,end):

    #start와 end가 계속해서 가까워져 가는데 이때 start > end 가 되면 반복문을 나온다.
    while start <= end:
        # (start+end) // 2 인 이유는 소숫점이하의 수는 버리기 때문이다. 
        mid = (start+end)//2

        if array[mid] == target:
            return mid
        elif array[mid] < target:
            start = mid+1
        else:
            end = mid-1
    
    return None

array1 = [3,5,7,1,2,4,6,8]
find = 3

array1.sort()
index = binary_search(array1,find,0,len(array1))

print(index)
```

### 정렬된 배열에서 특정 수의 갯수 구하기 Problem

```python
n,key = map(int, input().split())
array = list(map(int, input().split()))

# 가장 먼저 존재하는 key와 가장 나중에 존재하는 key의 인덱스의 차이로 값을 구할 수 있다는 아이디어가 중요하다.
def binary_search(array,key,start,end):

    # 먼저 가장 먼저 있는 key의 인덱스를 찾기 위한 함수
    def find_first(array,key,start,end):

        while start <= end:

            mid = (start + end)//2

            # key가 여러개 일 수 있기 때문에 array[mid] == key 외에도 다른 조건이 추가되었다.
            # 정렬된 수열 중에 가장 왼쪽에 있어야 하고 1 1 2 2 2일때 1 < 2 이어야 한다는 조건
            # 2 2 2 3 3 3 3 일 경우 3의 왼쪽 2 2 2가 남고 거기서 다시 start == end가 되는
            # 2 하나만 남아야 하므로 이 조건이 붙는다.
            if (array[mid] == key) and (array[mid] > array[mid-1] or start == end) :
                return mid

            # 등호가 붙는 이유는 1 1 1 2 2 2 2 2 2 일때, array의 값이 key보다 클때 뿐만 아니라
            # array값과 key 값이 같을때도 array의 작은 수가 있는 방향으로 범위를 줄여야 하므로
            # end = mid-1로 줄인다.
            elif array[mid] >= key:
                end = mid-1

            else:
                start = mid+1

        return False

    def find_last(array,key,start,end):

        while start <= end:

            mid = (start + end) // 2

            # start == end의 조건이 붙는 이유는 1 1 2 2 2 2에서 마지막의 값은 start == end이기 때문이다.
            if (array[mid] == key) and (array[mid] < array[mid+1] or  start == end):
                return mid

            # 여기서 등호가 붙는 이유는 1 1 2 2 2 2에서 key 중에서 가장 마지막 값을 찾으므로 위의 if문에 해당되지
            # 않으므로 뒤에 2가 마지막이 아니라 더 있다는 상황, 따라서 key랑 같을지라도 절반은 버린다.
            elif array[mid] <= key:
                start = mid+1

            else:
                end = mid-1

    a = find_first(array,key,start,end)
    b = find_last(array,key,start,end)

    # 1개라도 있다면 이런식으로 구한다.
    if a != False:
        print(b-a+1)
    else:
        print(-1)

binary_search(array,key,0,n-1)
```

### 고정점 찾기 problem

```python
n = int(input())
array = list(map(int,input().split()))

# 이 문제가 이진탐색으로 풀 수 있다고 어떻게 생각할 수 있을까? 첫째줄에 입력값이 백만이 나온다는 것.
def binary_search(array,start,end):

    while start <= end:

        mid = (start+end)//2

        if array[mid] == mid:
            return mid
        # -1 0 1 3 5 일 경우 array[mid] = 1, mid = 2이다. 즉, array에서 인덱스가 2 미만으로 작아질때, array는
        # 1 미만으로 작아진다. 즉, 절대 같아질 수 없다. 따라서 그 이전꺼는 볼 필요가 없으므로
        elif array[mid] < mid:
            start = mid+1

        # 1 3 4 5 6 일때 array[mid] = 4, mid = 2이다. 즉, 서로 겹치는 숫자가 없다고 문제에서 주어졌으므로,
        # index가 3,4,5,6으로 증가할때, array의 값들은 최소 5,6,7,8 순으로 증가할 것이다. 따라서 그 이후는
        # 볼 필요가 없다.
        else:
            end = mid-1

    return False

result = binary_search(array,0,len(array)-1)
if result != False:
    print(result)
else:
    print(-1)
```