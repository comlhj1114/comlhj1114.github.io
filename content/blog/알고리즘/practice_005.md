---
title: 코드그라운드 연습문제 1번
date: "2020-01-29T22:00:00Z"
description: 코드그라운드 연습문제 1번 - 숫자 골라내기
draft: True
---

# 연습문제 \#5 이항계수의 합
출처: https://www.codeground.org/practice

## 문제
이항계수 ![(nk)](http://latex.codecogs.com/gif.latex?%5Cdpi%7B100%7D%20%5Cbg_white%20%5Cbg_white%20%5Cfn_phv%20%5Cmathrm%7B%28%7D_%7Bk%7D%5E%7Bn%7D%5Cmathrm%7B%29%7D) 는 n개의 원소 중에서 서로 다른 k개의 원소를 순서를 상관하지 않고 뽑을 때 가능한 경우의 수이다.
N, M이 주어질 때, 
 

![∑Ni=0∑Mj=0(i+ji)](http://latex.codecogs.com/gif.latex?%5Cdpi%7B100%7D%20%5Cbg_white%20%5Cbg_white%20%5Cfn_phv%20%5Csum_%7Bi%3D0%7D%5EN%5Csum_%7Bj%3D0%7D%5EM%20%5Cmathrm%7B%28%7D_%7Bi%7D%5E%7Bi&plus;j%7D%20%5Cmathrm%7B%29%7D)

의 합을 빠르게 계산하는 프로그램을 작성하라.

- 제한시간: 전체 테스트 케이스는 10,000개 이하이며, 전체 수행 시간은 1초 이내. (Java 2초 이내) 
    제한 시간을 초과하면 제출한 소스코드의 프로그램이 즉시 종료되며, 그때까지 수행한 결과에서 테스트 케이스를 1개 그룹 이상 통과하였더라도 점수는 0점이 됩니다.
    그러나, 제한 시간을 초과하더라도 테스트 케이스를 1개 그룹 이상 통과하였다면 '부분 점수(0< 점수< 만점)'를 받을 수 있으며, 이를 위해서는, C / C++ 에서 "printf 함수" 사용할 경우, 프로그램 시작부분에서 "setbuf(stdout, NULL);"를 한번만 사용하십시오.
    
    C++에서는 "setbuf(stdout, NULL);"와 "printf 함수" 대신 "cout"를 사용하고, Java에서는 "System.out.printIn"을 사용하시면, 제한 시간을 초과하더라도 '부분 점수'를 받을 수 있습니다.
    
    ※ 언어별 기본 제공 소스코드 내용 참고
    
    만약, 제한 시간을 초과하지 않았는데도 '부분 점수'를 받았다면, 일부 테스트 케이스를 통과하지 못한 경우 입니다.

- 메모리 사용 제한 : heap, global, static 총계 256MB, stack 1MB


#### 입력
입력 파일에는 여러 테스트 케이스가 포함될 수 있다.
파일의 첫째 줄에 케이스의 개수를 나타내는 T가 주어지고, 이후 차례로 TC개 테스트 케이스가 주어진다.
(1≤TC≤10,000)
 각각의 테스트 케이스는 단 한 줄이며, 0이상 1,000,000이하의 정수 N, M이 주어진다.

출력
각 테스트 케이스마다 첫 줄에 “Case #T”를 출력하여야 한다. 이때 T는 케이스의 번호이다.

각 케이스에 대해서 계산해야 하는 값을 1,000,000,007로 나눈 나머지를 출력한다.

이 수는 너무 커질 수 있기 때문에, 1,000,000,007로 나눈 나머지를 출력한다.

#### 입출력예
|   입력  |   출력  |
|--------|--------|
| 1 <br/> 2 2 | Case #1 <br/> 19 |



## 풀이
https://becoder.tistory.com/entry/%EC%97%B0%EC%8A%B5%EB%AC%B8%EC%A0%9C-5%EB%B2%88

## 코드

### C
```c
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

```
### C++
```c++
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

```
    
### Python
```python
# 코드그라운드로 알고리즘 공부하기
# https://github.com/DaksHoont/CodeGround


```


