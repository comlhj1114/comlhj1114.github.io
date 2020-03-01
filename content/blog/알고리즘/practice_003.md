---
title: 코드그라운드 연습문제 1번
date: "2020-01-29T22:00:00Z"
description: 코드그라운드 연습문제 1번 - 숫자 골라내기
draft: True
---

# 연습문제 \#3 시험 공부
출처: https://www.codeground.org/practice

## 문제

초등학생인 정우는 시험 기간을 맞아 공부를 시작해야 한다.
정우가 다니는 학교에선 총 N개의 과목에 대해 시험을 보는데, 시간이 부족한 정우는 그 중 K개의 과목만을 골라서 공부할 수 있다.
정우는 매우 특이한 학생이라서 어떤 과목을 공부한다면 그 과목에 대해선 무조건 같은 점수를 받게 된다고 한다.
정우는 시험 점수 총합을 최대화하기 위해 K개의 과목을 골라야 한다.
하지만, 모든 과목을 공부할 시간이 없는 정우는, 당신에게 "최대 합계 점수"를 받을 수 있는 K개의 과목을 골라달라고 한다.
K개 과목을 골랐을 때 정우가 받을 수 있는 "최대 합계 점수"를 구하는 프로그램을 작성하라.

#### 입력
입력 파일에는 여러 테스트 케이스가 포함될 수 있다.
파일의 첫째 줄에 케이스의 개수를 나타내는 자연수 T가 주어지고, 이후 차례로 T개 테스트 케이스가 주어진다. (1≤T≤20)
입력의 첫 줄에는 테스트 케이스의 숫자가 주어진다. 각각의 테스트 케이스의 첫째 줄에는 과목의 수 N (N은 20만 이하의 자연수)과 정우가 공부할 수 있는 과목의 수 K
가 주어진다.
테스트 케이스의 둘째 줄에는 N개의 숫자들이 주어진다. 각 숫자는 100이하의 음이 아닌 정수이다.
그리고, 각 숫자는 차례대로 어떤해당하는 과목을 공부했을 때 정우가 받을 수 있는 점수를 의미한다.

#### 출력
각 테스트 케이스의 답을 순서대로 표준출력으로 출력하여야 하며, 각 테스트 케이스마다 첫 줄에 “Case #T”를 출력하여야 한다.
이때 T는 케이스의 번호이다. 
각 테스트케이스 마다 정우가 원하는 결과(K개 과목 총점의 최대값)를 출력한다.

#### 입출력예
|   입력  |   출력  |
|--------|--------|
| 1 <br/> 4 2 <br/>20 50 30 30 | Case #1 <br/> 80 |



## 풀이
문제를 다시 살펴 보면, N개의 숫자 중 K개를 더했을 경우, 최대가 나오는 때의 합을 구하라는 문제이다.
결국 이 문제는 N개의 숫자를 정렬 한 후에, 가장 큰 숫자 K개의 합을 구하면 된다.
정렬 알고리즘에도 여러가지가 있지만, 나는 구현되어있는 라이브러리를 사용하는 것이 좋다.

## 코드

1) N개의 숫자를 정렬한다. <br/>
2) 높은 순서대로 K개를 더한다. <br/>
*) 이번 문제는 너무 간단하니, 메모리 효율성을 위해 동적할당을 연습 해 보자.

### C
```c
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include<stdio.h>
#include<stdlib.h>

int comp(const void* a, const void* b)
{
    int a_val = *(int*)a;
    int b_val = *(int*)b;
    
    return b_val-a_val;
}

int main()
{
    int T,t;
    
    scanf("%d",&T);
    
    for(t=0;t<T;t++)
    {
        int N,K,i;
        int* score;
        int res=0;
        scanf("%d %d",&N,&K);
        
        score = (int*)malloc(sizeof(int)*N);
        
        for(i=0;i<N;i++)
            scanf("%d",&score[i]);
            
        qsort(score,N,sizeof(int),comp);
        
        for(i=0;i<K;i++)
            res+=score[i];
            
        printf("Case #%d\n%d\n",t+1,res);
    }
}
```
### C++
```c++
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int main()
{
    int T,t;
    
    cin>>T;
    for(t=0;t<T;t++)
    {
        int N,K,i;
        vector<int> score;
        int res=0;
        
        cin>>N>>K;
        for(i=0;i<N;i++)
        {
            int temp;
            cin>>temp;
            score.push_back(temp);
        }
        
        sort(score.begin(),score.end());
        
        for(i=N-1;i>=N-K;i--)
            res+=score.at(i);
            
        cout<<"Case #"<<t+1<<endl
            <<res<<endl;
    }
}
```
    
### Python
```python
# 코드그라운드로 알고리즘 공부하기
# https://github.com/DaksHoont/CodeGround

import sys

inf = sys.stdin 

T = inf.readline();

for t in range(0, int(T)):
    
    line = inf.readline().split();
    N = int(line[0]);
    K = int(line[1]);
    
    line = inf.readline().split();
    line = list(map(int,line));
    line.sort(reverse=True);
    res = sum(line[:K]);
    
    print('Case #%d' %(int(t)+1))    
    print(res)
inf.close()
```


