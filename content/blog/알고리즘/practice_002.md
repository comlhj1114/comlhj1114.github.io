---
title: 코드그라운드 연습문제 1번
date: "2020-01-29T22:00:00Z"
description: 코드그라운드 연습문제 1번 - 숫자 골라내기
draft: True
---

# 연습문제 \#2 프로그래밍 경진대회
출처: https://www.codeground.org/practice

## 문제

삼성 프로그래밍 경진대회는 권위 있는 대회이다. 대회는 여러 라운드를 통해서 진행되며, 모든 라운드에 총 N명의 응시자가 있다.
각 라운드 별로 1등은 N점, 2등은 N−1점 순으로 순차적으로 점수를 얻게 되고 뒤에서 2등은 2점, 뒤에서 1등은 1점을 얻게 된다.
그리고 각 라운드 별로 동점자는 없으며, 각 라운드 마다 받은 점수의 합이 제일 높은 사람이 우승하게 된다. 
마지막 라운드 직전까지의 점수 합이 주어졌을 때, 우승할 가능성이 있는 응시자의 수를 구하는 프로그램을 작성하시오.
(SCPC 실제 대회 규칙과는 관련이 없습니다.)

#### 입력
입력 파일에는 여러 테스트 케이스가 포함될 수 있다.
파일의 첫째 줄에 케이스의 개수 T가 주어지고, 후 차례로 T개 테스트 케이스가 주어진다. (1≤T≤5)
각각의 테스트 케이스의 첫 줄에는 응시자의 수 N이 주어지며 (1≤N≤300,000), 다음 N개의 줄에는 각 응시자가 마지막 라운드 전까지 받은 점수의 합을 나타내는 N개의 자연수가 한 줄에 하나씩 주어진다.
(여기서 점수 합은 2,000,000을 넘지 않는 음이 아닌 정수이다.)

#### 출력
각 테스트 케이스의 답을 순서대로 표준출력으로 출력하여야 하며, 각 테스트 케이스마다 첫 줄에 “Case #T”를 출력하여야 한다. 이때 T는 케이스의 번호이다.
각 케이스에 대해서 우승할 가능성이 있는 응시자의 수를 출력한다.

#### 입출력예
|   입력  |   출력  |
|--------|--------|
| 1 <br/>3 <br/>5 <br/>7 <br/>6  | Case #1 <br/> 3 |



## 풀이
연습문제 1번과 마찬가지로 자료구조와 알고리즘을 몰라도 풀 수 있는 문제이다. 아, 정렬은 알고 있어야 하는데, 직접 구현하지 않아도 내장된 라이브러리를 써도 되니까 큰 지장은 없다.

가장 중요한 부분은 우승할 가능성을 판단 하는 부분인데, 이 부분은 시간을 가지고 잠시 고민 해 보자.

먼저 우승할 가능성이 없는 사람들은 현재 점수가 너무 낮아서 다음 라운드에서 최고점을 받아도 1등 할 가능성이 없는 사람들이다. 그러면 다음라운드에서 1등을 할 가능성은 어떻게 알 수 있을까? 여기서 중요한 것은 **가능성** 이기 때문에 극단의 경우를 생각하여야 한다는 점이다.

다음 라운드에서 1등을 하기 위한 점수는 어떻게 구할 수 있을까? 단순히 생각하면 현재 1등이 다음 라운드에서 꼴지를 하는 경우의 점수를 기준으로 삼을 수 있겠다. 현재 1등의 점수를 x라고 하면, x+1점을 다음 라운드에서 우승 할 가능성이 있는 사람의 점수로 생각하는 것이다. 하지만, 이 경우는 현재 공동 1등이 존재 할 경우에 점수를 바르게 예측하지 못한다. 왜냐하면 공동 1등이 존재 할 경우, 다음 라운드에서 x+2점이 존재 할 수 밖에 없으므로, x+1점은 적절한 기준점이 되지 못한다.

따라서 정확하게 우승 할 수 있는 사람의 기준점을 구하려면, 다음라운드에서 모든 참가자의 점수를 고려하였을 때의 최고점을 기준으로 삼아야한다. 이 경우는 참가자들이 현재 순위의 역순위로 점수를 받게 되는 경우이다. 1등은 1점, 2등은 2점, ... N등은 N점을 받았을 경우의 최고점을 우승하기 위해 다음 라운드에서 성취해야 하는 점수로 보는 것이다.

따라서, 1) 현재 순위를 계산하고, 2) 현재 순위에 따라서 다음 라운드에 받을 수 있는 최하점을 더한 후, 3) 그 점수중 최고점을 우승하기 위한 기준점으로 삼는다. 마지막으로 현재 점수에서 다음라운드에 최고점을 받았을 경우, 기준점을 넘을 수 있는 사람의 수가 정답이 된다.

## 코드

1) 현재 순위를 구한다. (정렬) <br/>
2) 현재 순위만큼 점수를 더한다. (n등은 n점 획득) <br/>
3) 2)에서 계산한 점수 중 최고점이 우승하기 위한 기준점이 된다. <br/>
4) 다음라운드에 최고점(N점)을 받을 경우 3)에서 구한 기준점이 넘는 사람의 수를 구한다.

### C
```c
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include <stdio.h>
#include <stdlib.h>

int compare(const void *a, const void *b);

int main(void)
{
	int T;
	int test_case;
	int res, N, temp, i;
	int win_score;
	int score[300000];

	setbuf(stdout,NULL);

	scanf("%d",&T);

	for(test_case = 1; test_case <=T; test_case++)
	{
		res=0;
		win_score=0;
		scanf("%d",&N);
		for(i=0;i<N;i++)
		{
			scanf("%d",score+i);
		}
		
		// 1) 현재 순위를 구한다. (정렬)
		qsort(score,N,sizeof(score[0]),compare);

		for(i=0;i<N;i++)
		{
		    // 2) 현재 순위만큼 점수를 더한 뒤에,
		    // 3) 우승하기 위한 기준점을 구한다. (최대값)
			if(score[i]+i+1>win_score)
			{
				win_score = score[i]+i+1;
			}
		}

		for(i=0;i<N;i++)
		{
		    // 4) 다음라운드에 N점을 받을 경우 기준점을 넘는 사람수를 구한다.
			if(score[i]+N >= win_score)
				res++;
		}
			

		printf("Case #%d\n", test_case);
		printf("%d\n",res);
	}

	return 0;
}

int compare(const void *a, const void *b)
{
	return (*(int*)b-*(int*)a);
}
```
### C++
```c++
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include <iostream>
#include <algorithm>

using namespace std;

int Answer;

bool comp(int a, int b)
{
    return a>b;
}

int main(int argc, char** argv)
{
	int T, test_case;
	
	cin >> T;
	for(test_case = 0; test_case  < T; test_case++)
	{
        int i,N,next_score,win_score;
        int score[300000];
		Answer = 0;
		win_score=0;
		
		cin >> N;
		for(i=0;i<N;i++)
    		cin >> score[i];
		
		// 1) 현재 순위를 구한다. (정렬)
		sort(score,score+N,comp);
		
		for(i=0;i<N;i++)
		{
		    // 2) 현재 순위만큼 점수를 더한다.
		    next_score = score[i]+i+1;
		    
		    // 3) 우승 할 수 있는 기준점을 구한다. (최대값)
		    if(next_score>win_score)
		        win_score = next_score;		        
		}
		
		for(i=0;i<N;i++)
		{
		    // 4) 다음 라운드에 최고점(N점)을 받았을 경우 기준점을 넘는 사람수를 구한다.
		    next_score = score[i]+N;
		    if(next_score >= win_score)
		        Answer++;
		}
		
		
		cout<< "Case #" << test_case+1 << endl;
		cout << Answer << endl;
	}

	return 0;//Your program should return 0 on normal termination.
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
    
    Answer=0;
    
    N = int(inf.readline())
    
    score = []
    for _ in range(0,N):
        score.append(int(inf.readline()))
    
    # 1) 현재 등수를 구한다. (정렬)
    score.sort(reverse=True)
    
    win_score=0
    for i in range(0,N):
        # 2) 다음 라드운드 최하 점수를 구한다. 
        next_score = score[i]+i+1
        
        # 3) 우승하기 위한 기준점을 구한다. (최고점)
        if next_score > win_score:
            win_score = next_score
    
    for s in score:
        # 4) 다음라운드에 최고점을 받았을때,  기준점 이상이 될 수 있는 사람수를 구한다.
        next_score = s+N;
        
        if next_score >= win_score:
            Answer = Answer + 1;
	
    print('Case #%d' %(int(t)+1))    
    print(Answer)
inf.close()
```


