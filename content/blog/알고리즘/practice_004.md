---
title: 코드그라운드 연습문제 1번
date: "2020-01-29T22:00:00Z"
description: 코드그라운드 연습문제 1번 - 숫자 골라내기
draft: True
---

# 연습문제 \#4 다트 공부
출처: https://www.codeground.org/practice

## 문제

많은 펍(Pub)에서는 다트를 할 수 있게 다트 판이있다. 
이 다트판의 구성은 다음과같다.

①SINGLE :숫자의점수그대로 <br/>
②DOUBLE :해당점수의두배 <br/>
③TRIPLE :해당점수의세배 <br/>
④OUT BOARD :득점이없는바깥영역 <br/>
⑤BULL : 50점 <br/>

![그림 : www.codeground.org/practice](https://cdn.codeground.org/resources/2320e52a0b/AWNoMGlNAAhpX_LD.png)<br/>
출처: https://www.codeground.org/practice
 
다트핀이 꽂힌 위치를 쉽게 설명하기 위해, 그림과 같이 다트판의 중심을 원점 (0, 0)으로 생각한 좌표평면으로 옮겨 생각하자.
오랜만에 펍에 놀러간 대희는 다트를 한판하려고한다.
다트핀을 N번 던졌을때, 얻은 총 점수를 구하여라.

#### 입력
입력파일에는여러테스트케이스가포함될수있다.
파일의첫째줄에케이스의개수를나타내는자연수 T가주어지고, 이후 차례로 T개 테스트케이스가 주어진다. (1≤T≤20)
각각의 테스트 케이스 첫 번째 줄에는 BULL의 반지름 A, TRIPLE 시작구간의 반지름 B, TRIPLE 종료구간의 반지름 C, DOUBLE 시작구간의 반지름 D, DOUBLE 종료구간의 반지름 E가 각각 순서대로 주어진다. (1≤A<B<C<D<E≤20000, A,B,C,D,E는 정수)
테스트 케이스의 두번째 줄에는 대희가 쏜 다트의 개수 N 이 주어진다. (1 ≤ N ≤ 10000) 
테스트 케이스의 세번째 줄부터 N개의 줄에는 각 다트의 위치를 의미하는 x,y (−30000≤x,y≤30000)가 공백을 사이에 두고 차례로주어진다.
이는 다트판의 중심을 원점 (0, 0)으로 두었을 때 다트의 위치이다.
이 때, 다트핀은 절대로 점수 구간의 경계에 꽂히지 않는 다는 것을 보장한다.

#### 출력
각 테스트 케이스의 답을 순서대로 표준출력으로 출력하여야 하며, 각 테스트 케이스마다 첫 줄에 “Case #T”를 출력하여야 한다.
이때 T는 케이스의 번호이다.
그 다음 줄에는 각 테스트케이스에 대해 대희가 다트를 던져 얻은 총 점수를 출력한다.

#### 입출력예
|   입력  |   출력  |
|--------|--------|
| 1 <br/> 10 50 60 80 90 <br/> 5 <br/> 5 5 <br/> 0 55 <br/> 45 -50 <br/> -77 88 <br/> -85 0 | Case #1 <br/> 134 |



## 풀이
이번 문제도 자료구조와 알고리즘의 이해가 없어도 충분히 해결 할 수 있는 문제이다.
나는 Cartesian 좌표인 (x,y)를 Polar coordinate (극좌표)로 변경하여 문제를 해결하였다. 극좌표로 다트판을 표시하면 직사각형 모양으로 표현 할 수 있다.

## 코드

1) 다트판을 극좌표로 변환하여 영역을 나눈다. <br/>
2) 다트가 꽂힌 위치 (x,y)를 극좌표로 변환하여 점수를 계산한다. <br/>

### C
```c
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include <stdio.h>
#include <math.h>

#define PI 3.141592

int main(void)
{
	int T;
	int test_case;
	int rb,rt1,rt2,rd1,rd2;
	int score,res;
	int N,i,s_ind;
	float x,y;
	float r,theta;
	int dart[20]={11,8,16,7,19,3,17,2,15,10,6,13,4,18,1,20,5,12,9,14};

	setbuf(stdout,NULL);

	scanf("%d",&T);

	for(test_case = 1; test_case <=T; test_case++)
	{
		res=0;

		scanf("%d %d %d %d %d",&rb,&rt1,&rt2,&rd1,&rd2);
		scanf("%d",&N);

		for(i=0;i<N;i++)
		{
			scanf("%f %f",&x,&y);
			
			score=0;
			r=sqrt(x*x+y*y);
			if(r < rd2)
			{
				if(r < rb)
					score = 50;

				else
				{
					theta = 180.0/PI*atan2(y,x); // -180도 ~ 180도로 표현 ex) -9도~9도는 6임
					s_ind = 10+(int)round(theta/18.0); // 배열을 쓰기 위해서 다트의 칸 수 만큼 나누어주고, 음수를 양수로 바꿔줌.  따라서 11점 부터 반 시계 방향으로 인덱스가 만들어짐
					s_ind %= 20; // 마지막 영역 처리
					score = dart[s_ind]; // 점수 계산
					
				//	printf("%f %d %d\n",theta,s_ind,score);
					
					if( rt1<r && r<rt2 ) score*=3; // 트리플 일 경우
					if( rd1<r && r<rd2 ) score*=2; // 더블 일 경우
				}
				res+=score;
			}
		}

		printf("Case #%d\n", test_case);
		printf("%d\n",res);
	}

	return 0;
}
```
### C++
```c++
// 코드그라운드로 알고리즘 공부하기
// https://github.com/DaksHoont/CodeGround

#include<iostream>
#include<cmath>
using namespace std;

struct _dart
{
    int x;
    int y;
};

int A,B,C,D,E;
int N;
int res;
_dart dart[10001];

void problem_in()
{
    int i,x,y;
    cin>>A>>B>>C>>D>>E;
    cin>>N;
    for(i=0;i<N;i++)
    {
        cin>>x>>y;
        dart[i].x=x;
        dart[i].y=y;
    }
}

void problem_out(int T)
{
    cout<<"Case #"<<T<<endl
        <<res<<endl;
}

void solving()
{
        int i,x,y;
        float r,theta;
        int s_ind;
        int score[20]={11,8,16,7,19,3,17,2,15,10,6,13,4,18,1,20,5,12,9,14};

        res=0;
        for(i=0;i<N;i++)
        {
            x=dart[i].x;
            y=dart[i].y;

            r = sqrt(x*x+y*y);
            theta = atan2(y,x); // 극좌표로 변환
            s_ind = round((theta*180/3.141592)/18); // 점수 영역을 인덱스로 변환
            s_ind = (s_ind+10)%20; // 음수를 양수로 바꾸기 (score배열이 11점 부터 시작한 이유)

            if(r<A) res+=50;
            else if(B<r && r<C) res+=3*score[s_ind];
            else if(D<r && r<E) res+=2*score[s_ind];
            else if(r<E) res+=score[s_ind];
        }
}

int main()
{
        setbuf(stdout,NULL);

        int T,it;
        cin>>T;

        for(it=1;it<=T;it++)
        {
            problem_in();
            solving();
            problem_out(it);
        }

        return 0;
}
```
    
### Python
```python
# 코드그라운드로 알고리즘 공부하기
# https://github.com/DaksHoont/CodeGround

import sys
import math

inf = sys.stdin 

T = inf.readline();

def calc_score(r,theta,dart_radius):
    dart = [11,8,16,7,19,3,17,2,15,10,6,13,4,18,1,20,5,12,9,14]
    
    s_ind = round((theta*180/3.141592)/18)
    s_ind = (s_ind+10)%20
    score = dart[s_ind]
    
    if r<dart_radius[0]:
        return 50
        
    elif dart_radius[1]<r and r<dart_radius[2]:
        return score*3
        
    elif dart_radius[3]<r and r<dart_radius[4]:
        return score*2
        
    elif dart_radius[4]<r:
        return 0
        
    else:
        return score
    

for t in range(0, int(T)):
    
    dart_radius = list(map(int,inf.readline().split()));
    N = int(inf.readline());
    
    res=0
    for _ in range(N):
        dart_xy = list(map(int,inf.readline().split()))
        r = math.sqrt(dart_xy[0]**2+dart_xy[1]**2);
        theta = math.atan2(dart_xy[1],dart_xy[0])
        res = res+calc_score(r,theta,dart_radius)
    
    print('Case #%d' %(int(t)+1))    
    print(res)
inf.close()
```


