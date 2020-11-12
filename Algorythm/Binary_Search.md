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
  * 다이나믹 프로그래밍
  * 최단거리
  * 그래프 알고리즘
        
***   
>	## When to use?
      
         
**연산 횟수**가 2000만번이 넘어가면 시간초과가 되기 때문에 입력조건 및 연산 횟수를 계산해서
~~ㅈㄴ~~ 많아질것 같으면 O(log N) 탐색법인 이진탐색 사용을 고려해본다. 이때  O(log N)인 이유는 탐색범위를 절반씩 날려가면서 탐색하기 때문이다.
   
#다면 배열이 정렬이 되어있어야 한다. 
   
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
