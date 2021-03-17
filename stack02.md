# 재귀호출

> 재귀 함수에서, 순차적으로 자기 자신을 호출하고 수행한 후 복귀하는 구조가 스택의 후입선출과 같은 구조이므로 함께 다룬다.

- 자기 자신을 호출하여 순환 수행되는 것
- 함수에서 실행해야 하는 작업의 특성에 따라 일반적인 호출방식보다 재귀호출방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성할 수 있다.

## 재귀호출 예시: factorial(!)

```python
def fact(n):
    # basic 파트
    if n -- 1:
        return 1
    # inductive 유도 파트
    else:
        return n * fact(n-1)
```

## 재귀호출 예시: 피보나치

- 0와 1로 시작하고 이전의 두 수 합을 다음 항으로 하는 수열을 피보나치라 한다.
- 피보나치 수열의 i번째 값을 계산하는 함수 F를 정의하면 다음과 같다:

    ```python
    def fibo(n) :
    		if n < 2 : 
    				return n
    		else: 
    				return fibo(n-1) + fibo(n-2)
    ```

- 단, 계속하여 함수값이 중복하여 호출된다는 문제점이 발생한다.
    - 이를 해결하기 위해 필요한 방법이 메모이제이션!

# 메모이제이션 Memoization

- 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행 속도를 빠르게 하는 기술이다. 동적 계획법의 핵심!
- `memoization`은 글자 그래도 해석하면 *메모리에 넣기(to put in memory)*라는 의미이며, *기억되어야 할 것*이라는 뜻의 라틴어 memorandum에서 파생됐다. 흔히 기억하기, 암기하기라는 의미의 memorization과 혼동하지만 정확한 단어는 memoization이다. 동사형은 memoize이다.

## 메모이제이션 예시: 피보나치

- 피보나치 수를 구하는 알고리즘에서 fibo(n)의 값을 계산하자마자 저장하면 실행시간을 O(2^n)에서 O(n)으로 줄일 수 있다.

```python
# memo를 위한 배열을 할당하고, 모두 0으로 초기화한다.
# memo[0]을 0으로, memo[1]은 1로 초기화한다.
memo = [0, 1]

def fibo(n) :
    # memo는 리스트이기 때문에 참조형(RW) 전역변수이므로 꼭 global로 선언할 필요는 없다.
    global memo
    
    # n이 2일때까지의 값을 구해져 있으므로 n이 2 이상인 경우만 찾으며,
    # memo의 길이를 이용해 해당 값이 이미 구해져있는지 여부를 판단한다.
    if n >= 2 and len(memo) <= n:
        memo.append(fibo(n-1) + fibo(n-2))
    return memo[n]
```

# 동적 프로그래밍 DP, Dynamic Programming

- 동적 계획 알고리즘은 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘이다.

    💡`최적화 문제` : 복수 개의 답이 존재할 때, 가장 좋은 답을 찾는 알고리즘. 결정 문제(decision problem) 이라고도 한다. 

- 먼저 입력 크기가 작은 부분 문제들을 모두 해결한 후에, 그 해들을 이용하여 큰 크기의 부분 문제들을 해결하는 방식으로, 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘이다.

## DP 예시 : 피보나치

- 피보나치 수는 부분 문제의 답으로부터 본 문제의 답을 얻을 수 있으므로, 최적 부분 구조로 이루어져 있다.

### 접근하기

1. 문제를 부분 문제로 분할한다.
    - fibo(n)은 fibo(n-1)과 fibo(n-2)의 합이며,
    - fibo(n-1)은 fibo(n-2)와 fibo(n-3)의 합이고,
    - 결국 fibo(n)은 fibo(n-1), fibo(n-2), ...... , fibo(1), fibo(0)으로 구성되어 있다.
2. 부분 문제로 나누는 일을 끝냈으면, 가장 작은 부분 문제부터 해를 구한다.
3. 그 결과를 테이블에 저장하고, 테이블에 저장된 부분 문제의 해를 이용하여 상위 문제의 해를 구한다.

```python
def fibo(n):
    f = [0, 1]
    
    for i in range(2, n + 1):
        f.append(f[i-1] + f[i-2])
        
    return f[n]
```

- 재귀와 달리 반복문으로 구성되어 있으므로, 속도가 훨씬 빠르고 효율적일 수 있다!

```python
f = [0, 1]

for i in range(2, n + 1):
    f.append(f[i-1] + f[i-2])

print(f(n))
```

## DP 구현 방식

- recursive 방식:

    ```python
     def fibo(n) :
        # memo는 리스트이기 때문에 참조형(RW) 전역변수이므로 꼭 global로 선언할 필요는 없다.
        global memo
        
        # n이 2일때까지의 값을 구해져 있으므로 n이 2 이상인 경우만 찾으며,
        # memo의 길이를 이용해 해당 값이 이미 구해져있는지 여부를 판단한다.
        if n >= 2 and len(memo) <= n:
            memo.append(fibo(n-1) + fibo(n-2))
        return memo[n]
    ```

- iterative 방식:

    ```python
    f = [0, 1]

    for i in range(2, n + 1):
        f.append(f[i-1] + f[i-2])

    print(f(n))
    ```

- 이 때, memorization을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적이다.
    - 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문이다.

--------

ⓒ 2021. `chloeelog` all rights reserved.
