# chapter 2. 완전검색 / 그리디

2020.05.06

> 학습 목표
>
> - 재귀적 알고리즘의 특성 이해, 이를 구현하기 위한 재귀 호출에 대해 학습
> - 완전 검색의 개념 이해, 완전 검색을 통한 문제 해결 방법에 대해 학습
> - 조합적 문제 (Combinatorial Problems)에 대한 완전 검색 방법에 대해 이해
>   - 순열, 조합, 부분집합을 생성하는 알고리즘 학습
> - 탐욕 알고리즘 기법의 개념과 주요 특성 이해

## 반복(Iteration)과 재귀(Recursion)

- 반복과 재귀는 유사한 작업 수행 가능
- 반복은 수행하는 작업이 완료될 때까지 계속 반복 → 루프 (for, while 구조)
- 재귀는 주어진 문제의 해를 구하기 위해 동일하면서 더 작은 문제의 해를 이용하는 방법
  - 하나의 큰 문제를 해결할 수 있는(해결하기 쉬운) 더 작은 문제로 쪼개고 결과들을 결합
  - 재귀 함수로 구현

### 반복 구조

1. 초기화 → (조건검사 → 명령문 실행 → 업데이트) 괄호를 반복하다 조건검사를 통해 끝남  
2. 초기화 → (명령문 실행 → 업데이트 → 조건검사) 괄호를 반복하다 조건검사를 통해 끝남

- 초기화 : 반복되는 명령문 실행 전(한번만) 조건 검사에 사용할 변수의 초기값 설정
- 조건 검사 (check control expression)
- 반복할 명령문 실행 (action)
- 업데이트 (loop update) : 무한 루프 (infinite loop) 가 되지 않게 조건이 거짓(false)이 되게 함.

```python
# 반복을 이용한 선택 정렬
def SelectionSort(A):
    n = len(A)
    for i in range(0, n-1):
        min = i
        for j in range(i+1, n):
            if A[j] < A[min]:
                min = j
        A[min], A[i] = A[i], A[min]
```

### 재귀적 알고리즘

- 재귀적 정의는 두 부분으로 나뉜다.

  1. 하나 또는 그 이상의 기본 경우 (basic case or rule)

     집합에 포함되어 있는 원소로 induction을 생성하기 위한 시드(seed) 역할

     스택 overflow 방지, 기초가 되는 부분

  2. 하나 또는 그 이상의 유도된 경우 (inductive case or rule)

     새로운 집합의 원소를 생성하기 위해 결합되어지는 방법

### 재귀 함수 (recursive function)

- 함수 내부에서 직접 혹은 간접적으로 자기 자신을 호출하는 함수

- 일반적으로 재귀적 정의를 이용해서 재귀 함수를 구현

- **기본 부분(basic part)**와 **유도 부분(inductive part)**로 구성

- 재귀적 프로그램을 작성하는 것은 반복 구조에 비해 간결하고 이해하기 쉬움

- 함수 호출은 프로그램 메모리 구조에서 스택을 사용

  재귀 호출은 반복적인 스택의 사용을 의미, 메모리 및 속도에 성능 저하 발생

  `호출 → 호출 → ... return → return ... : 프로그램 메모리 구조`

#### 펙토리얼 재귀 함수

- 재귀적 정의

  - Basic rule : N <= 1인 경우 n = 1

  - Inductive rule : N > 1, n! = n X (n-1)!

    ```python
    def fact(n):
        if n<= 1: return 1 # Basic part
        else: return n * fact(n-1) # Inductive part
    ```

### 반복 vs 재귀

- 해결할 문제를 고려해서 반복이나 재귀의 반복 선택
- 재귀는 문제 해결을 위한 알고리즘 설계가 간단하고 자연스러움.
  - 추상 자료형(List, tree 등)의 알고리즘은 재귀적 구현이 간단하고 자연스러운 경우가 많음.
- 일반적으로 재귀적 알고리즘은 반복 알고리즘보다 더 많은 메모리와 연산이 필요로 한다.

※ 입력 값이 n이 커질수록 재귀 알고리즘은 반복에 비해 비효율적일 수 있다!

|                    |                        재귀                        |          반복          |
| :----------------: | :------------------------------------------------: | :--------------------: |
|      **종료**      | 재귀 함수 호출이 종료되는 베이스 케이스(base case) |   반복문의 종료 조건   |
|   **수행 시간**    |                   (상대적) 느림                    |          빠름          |
|  **메모리 공간**   |                 (상대적) 많이 사용                 |       적게 사용        |
| **소스 코드 길이** |                     짧고 간결                      |          길다          |
| **소스 코드 형태** |              선택 구조 (if ... else)               | 반복 구조 (for, while) |
|  **무한 반복시**   |                  스택 오버플로우                   |  CPU를 반복해서 점유   |

#### 2^k 연산에 대한 재귀와 반복

- 재귀

  ```python
  def Power_of_2(k): # Output : 2^k
      if k == 0:
          return 1
      else:
          return 2 * Power_of_2(k-1)
  ```

- 반복

  ```python
  def Power_of_2(k): # Output : 2^k
      i = 0
      power = 1
      while i < k:
          power *= 2
          i += 1
      return power
  ```

### 연습문제 1

선택 정렬 함수 (SelecionSort)를 재귀적 알고리즘으로 작성해 보시오.

- 풀이 point

  basic 부분 : unsorted 부분이 1이 될 경우

  Inductive  부분 : sort, unsorted 부분 나누기, unsorted 부분 점점 줄어들도록!, unsorted 부분에서 가장 작은것을 앞으로 정렬하기

- 풀이 코드

  ```python
  def SelectionSort(A, s):
      n = len(A)
      if s == n-1: return
      min = s
      for i in range(s, n):
          if A[min] > A[i]:
              min = i
      A[s], A[min] = A[min], A[s]
      SelectionSort(A, s+1)
  
  
  A = [2, 4, 6, 1, 9, 8, 7, 0]
  SelectionSort(A, 0)
  print(A) # [0, 1, 2, 4, 6, 7, 8, 9]
  ```

## 완전검색기법

### 완전 검색

- 많은 종류의 문제들이 특정 조건을 만족하는 경우나 요소를 찾는 것이다.

- 전형적으로 순열(Permutation), 조합(Combination), 부분집합(subsets)과 같은 조합적 문제들(Combinatorial Problems)과 연관된다.

→ 완전 검색은 `조합적 문제에 대한 brute-force 방법`이다.

- 모든 경우의 수를 생성하고 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아내지 못할 확률이 작다.
  - 완전 검색은 입력의 크기를 작게 해서 간편하고 빠르게 답을 구하는 프로그램을 작성한다.
- 이를 기반으로 그리디 기법이나 동적 계획법을 이용해서 효율적인 알고리즘을 찾을 수 있다.
- 검정등에서 주어진 문제를 풀 때, 우선 **완전 검색으로 접근하여 해답을 도출**한 후, **성능 개선을 위해 다른 알고리즘을 사용하고 해답을 확인**하는 것이 바람직하다.

### 고지식한 방법 (brute-force)

- brute-force : 문제 해결을 위한 간단하고 쉬운 접근법
  - "Just do it"
  - force : 사람(지능)보다 컴퓨터의 force 의미
- brute-force 방법은 대부분의 문제에 적용 가능하다.
- 상대적으로 빠른 시간에 문제 해결(알고리즘 설계)를 할 수 있다.
- 문제에 포함된 자료(요소, 인스턴스)의 크기가 작다면 유용하다.
- 학술적 또는 교육적 목적을 위해 알고리즘의 효율성을 판단하기 위한척도로 사용된다.

#### Brute-force 탐색 (sequential search)

- 자료들의 리스트에서 키 값을 찾기 위해 첫 번째 자료부터 비교하면서 진행한다.

- 결과

  - 탐색 성공 / 탐색 실패

  ```python
  def SequentialSearch(A[0, ..., n], k):
      A[n] = k
      i = 0
      while A[i] != k:
          i += 1
      if i < n: return i
      else: return 1
  ```

## 조합적 문제

### 순열 (Permutation)

- 서로 다른 것들 중  몇 개를 뽑아서 한 줄로 나열하는 것

- 서로 다른 n 개 중 r개를 택하는 순열 표현 : n P r

  - nPr = n x (n-1) x (n-2) x ... x (n-r+1)
  - nPn = n!

- 다수의 알고리즘 문제들은 순서화된 요소들의 집합에서 최선의 방법을 찾는 것과 관련 있다.

  ex) TSP (Traveling Salesman Problem)

- N개의 요소들에 대해서 n! 개의 순열들이 존재한다.

  - 12! = 479,001,600
  - n > 12 인 경우, 시간 복잡도 폭발적으로 증가!

#### 단순하게 순열을 생성하는 방법

```python
# {1, 2, 3}을 포함하는 모든 수열을 생성하는 함수
for i1 in range(3):
    for i2 in range(3):
        if i2 != i1:
            for i3 in range(3):
                if i3 != i1 and i3 != i2:
                    print(i1, i2, i3)
```

#### 순열 생성 방법

- 사전적 순서 (Lexicographic-Order)
- 최소 변경을 통한 방법(Minimum-exchange requirement)
  
- 각각의 순열들은 이전의 상태에서 단지 두 개의 요소들 교환을 통해 생성
  
- **재귀 호출을 통한 순열 생성 1**

  교환을 이용하나 최소한의 경로는 아님.

  ```python
  # arr[] : 데이터가 저장된 배열
  
  def perm(k, N):
      if k == N:
          # 원하는 작업 수행
          print(arr)
      else:
          for i in range(k, N): # k 번째 원소를 i 번째 원소와 교환하여 저장
              arr[k], arr[i] = arr[i], arr[k]
              perm(k + 1, N)
              arr[k], arr[i] = arr[i], arr[k] # 리턴 후 i 번째 원소가 배제되지 않게 복구
  arr = ['A','B', 'C'] # 순열 만들 리스트
  perm(0, 3)
  ```

- **재귀 호출을 통한 순열 생성 2**

  ```python
  # arr[] : 데이터가 저장된 배열
  # n : 원소의 개수, i: 선택된 원소의 수
  # visited[N-1] : 방문 여부, t : 결과 저장 배열
  
  def perm(k):
      if (k==n): print(arr)
      else:
          for i in range(n-1):
              if not visited[i]:
                  t[k] = arr[i]
                  visited[i] = True
                  perm(k+1)
                  visited[i] = False
  ```

### 연습 문제 2 : Baby-gin Game

6자리 숫자에 대해서 완전 검색을 적용해서 Baby-gin 을 검사해보시오.

- About Baby-gin
  - 0 ~ 9 사이의 숫자 카드에서 임의의 카드 6장을 뽑았을 때, 3장의 카드가 연속적이 번호를 갖는 경우를 run이라 하고, 3장의 카드가 동일한 번호를 갖는 경우를 triplet이라고 한다.
  - 6장의 카드가 run과 triplet로만 구성된 경우를 baby-gin으로 부른다. 
  - 6자리의 숫자를 입력받아 baby-gin 여부를 판단하는 프로그램을 작성하라.

- 입력 예

  ```markdown
  124783
  667767
  054060
  101123
  ```

  - 667767은 두 개의 triplet이므로 baby-gin이다. (666, 777)
  - 054060은 한 개의 run과 한 개의 triplet이므로 baby-gin이다. (456, 000)
  - 101123은 한 개의 triplet만 존재하므로 baby-gin이 아니다.

- 접근 포인트

  1. 고려할 수 있는 모든 경우의 수 생성하기

     6개의 숫자로 만들 수 있는 모든 숫자 중복 포함하여 순열로 생성한다.

  2. 해답 테스트하기

     앞의 3자리와 뒤의 3자리를 잘라, run와 triplet 여부를 테스트하고 최종적으로 baby-gin을 판단한다.

- 코드 풀이

  ```python
  def perm2(n, k):
      global chk
      if chk: return
      if k == n:
          # print(ta)
          t = r = 0
          if ta[0] == ta[1] and ta[1] == ta[2]:
              t+= 1
          if ta[3] == ta[4] and ta[4] == ta[5]:
              t+= 1
          if ta[0] + 1 == ta[1] and ta[1] + 1 == ta[2]:
              r+= 1
          if ta[3] + 1 == ta[4] and ta[4] + 1 == ta[5]:
              r+= 1
          if t + r == 3: chk = True
      else: # if k!= n
          for i in range(n):
              if not visited[i]:
                  ta[k] = arr[i]
                  visited[i] = True
                  perm2(n, k + 1)
                  visited[i] = False    
  ```

### 부분 집합

- 집합에 포함된 원소들을 선택하는 것이다.

- 다수의 중요 알고리즘들이 원소들의 그룹에서 최적의 부분 집합을 찾는 것이다.

  ex) 배낭 짐싸기 (knapsack)

- N 개의 원소를 포함한 집합

  - 자기 자신과 공집합 포함한 모든 부분집합(power set)의 개수 : 2^n 개
  - 원소의 수가 증가하면 부분집합의 개수는 지수적으로 증가

#### 단순하게 모든 부분 집합 생성하는 방법

```python
# 3개 원소를 포함한 집합에 대한 power set 구하기
for i1 in range(2):
    bit[0] = i1 # 0번째 원소
    for i2 in range(2):
        bit[i] = i2 # 1번째 원소
        for i3 in range(2):
            bit[2] = i3 # 2번째 원소
            print(array)
```

#### 부분집합 생성 방법

##### 바이너리 카운팅 (Binary Counting)

- 원소 수에 해당하는 N개의 비트열을 이용한다.
- n번째 비트값이 1이면 n번째 원소가 포함되었음을 의미한다.

| 10진수 | 이진수 | {A, B, C, D} |
| :----: | :----: | :----------: |
|   0    |  0000  |      {}      |
|   1    |  0001  |     {A}      |
|   2    |  0010  |     {B}      |
|   3    |  0011  |    {A, B}    |
|   4    |  0100  |     {C}      |
|   5    |  0101  |    {A, C}    |
|   6    |  0110  |    {B, C}    |
|   7    |  0111  |  {A, B, C}   |
|   8    |  1000  |     {D}      |
|   9    |  1001  |    {A, D}    |
|   10   |  1010  |    {B, D}    |
|   11   |  1011  |  {A, B, D}   |
|   12   |  1100  |    {C, D}    |
|   13   |  1101  |  {A, C, D}   |
|   14   |  1110  |  {B, C, D}   |
|   15   |  1111  | {A, B, C, D} |

##### 바이너리 카운팅을 통한 부분집합 생성 코드 예

```python
arr = [3, 6, 7, 1, 5, 4]
n = len(arr)
for i in range(0, (1<<n)): # 1<<n : 부분집합의 개수
    for j in range(0, n): # 원소의 수만큼 비트를 비교함
        if i & (1<<j): # i의 j번째 비트가 1이면 j 번째 원소 출력
            print('%d'%arr[j], end='')
    print()
```

### 연습문제 3 : 부분집합 합 문제

- 아래의 10개 정수 집합에 대한 모든 부분 집합 중 원소의 합이 0 이 되는 부분 집합을 모두 출력하시오.

- 예 : {-1, 3, -9, 6, 7, -6, 1, 5, 4, -2}

- 코드 풀이

  ```python
  arr = [-1, 3, -9, 6, 7, -6, 1, 5, 4, -2]
  n = len(arr)
  
  for i in range(0, 1<<n):
      tr = []
      for j in range(0, n): # 원소의 수만큼 비트를 비교함
          if i & (1<<j): # i의 j번째 비트가 1이면 j번째 원소 출력
              tr.append(arr[j])
              # print('%d'%arr[j], end='')
      if sum(tr) == 0:
          print(tr)
  ```

### 조합

- 서로 다른 n개의 원소 중 r개를 순서 없이 골라낸 것을 조합 (Combination)이라 한다.

- 조합의 수식
  - nCr = n!/((n-r)! * r!), (n >= r)
  - nCr = n-1Cr-1 + n-1Cr `재귀적 표현`
  - nC0 = 1

#### 재귀 호출을 이용한 조합 생성 알고리즘 1

```python
# an[] : n개의 원소를 가지고 있는 배열
# tr[] : r개 크기의 배열, 조합이 임시 저장될 배열

def comb(n, r):
    if (r==0): print(arr)
    elif (n < r): return # 예외케이스 처리를 위해
    else:
        tr[r-1] = an[n-1]
        comb(n-1, r-1) # 데이터 있을 경우
        comb(n-1, r) # 데이터 없을 경우
```

![image-20200506152113375](C:\Users\youbi\AppData\Roaming\Typora\typora-user-images\image-20200506152113375.png)

![image-20200506152234944](C:\Users\youbi\AppData\Roaming\Typora\typora-user-images\image-20200506152234944.png)

#### 재귀 호출을 이용한 조합 생성 알고리즘 2

```python
# an[] : n개의 원소를 가지고 있는 배열
# tr[] : r개 크기의 배열, 조합이 임시 저장될 배열

def comb(k, s): # 깊이, 시작 숫자
    if (r==0): print(arr)
    else:
        for i in range(s, n-r+k):
            t[k] = a[i]
            comb(k+1, i+1)
```
