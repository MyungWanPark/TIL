### Problem 3
```python
data = list(map(int, input()))

count = 0

for i in range(len(data)-1):
    if data[i] == data[i+1]:
        continue
    count += 1

print( (count +1) // 2)
# ex) 010101 or 0000111000 에서 총 (count + 1) 덩어리가 있다.
# 따라서 (count + 1) // 2 만큼 수정하면 된다. 즉, 5덩어리면, 5 // 2 = 2번의 색깔을 변화 시키면 된다.
```
### Problem 4
```python
from itertools import combinations

n = int(input())
data = list(map(int, input().split()))
# 가능한 모든 조합 담은 리스트
all_cases = []
# 가능한 모든 조합의 합을 담은 리스트
sum_cases = []
data.sort()

# 1,2,3 부터 n개까지 만들 수 있는 조합을 다 만들어 all_cases에 넣는다.
for i in range(1,n+1):
    result = list(combinations(data,i))
    all_cases.append(result)

# 2차원 리스트(all_cases)의 길이는 n개 이고
for i in range(n):
    # 2차원 리스트에서 각 요소별 len(all_cases[i]) 만큼의 수가 있다.
    for j in range(len(all_cases[i])):
        sum_cases.append(sum(all_cases[i][j]))
sum_cases.sort()

print(sum_cases)

# 1부터 모든 요소의 합까지의 범위 안에서 없는 수를 찾는다.
for i in range(1, sum(data)+1):
    if i not in sum_cases:
      print(i)
      break

```