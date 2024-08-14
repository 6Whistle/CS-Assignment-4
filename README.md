# 컴퓨터구조 project

## 교수님 : 이성원 교수님

## 이름 : 이준휘

## 학번 : 2018202046

## 분반 : 월 3 , 수 4


### A. Basic

```
L1 cache는 Processor와 가장 가까운 cache이다. Harvard Arch.는 해당 cache를 Instruction과
Data를 따로 분리하는 방식을 사용하여 Pipeline processor에서의 동작이 더욱 원활하게 진
행되도록 돕고 있다. Cache의 동작은 다음과 같다. Memory address를 전달받으면 cache 내
에 해당 데이터와 일치하는 Set에 접근하여 tag들을 확인한다. 만약 동일한 tag를 발견한
경우 valid bit를 확인하여 해당 데이터가 쓰여진 데이터가 맞는 경우 이를 HIT 상태로 반
환하게 된다. 그리고 해당 way에 저장된 data를 processor로 전달하게 된다. 해당 과정은
매우 적은 cycle(1~2)에서 동작이 가능하다. 하지만 해당 과정에서 tag가 틀리거나 valid bit
가 꺼진 경우 MISS가 발생한다. L1의 Cache는 MISS가 발생하였을 때 아래 계층(memory or
L2 Cache)로 접근하게 된다. 해당 계층은 상위 계층보다 메모리가 크지만 느리기 때문에
더욱 많은 cycle(L2 : 20 cycle, Memory : 200 cycle)을 소비하게 된다.
```
```
기본적으로 데이터는 크기가 커지면 하나의 word에 보관하지 못하기 때문에 이를 나누어
담게 된다. 보통 나누어 담은 데이터는 바로 옆에 인접해 있기 때문에 우리는 이러한 인
접한 데이터의 집합을 Block이라 한다.
```
### B. Observation for Program Behaviors

#### 1. Bubble sort

해당 Bubble sort는 이전 project3에서 수행한 과제의 결과를 바탕으로 작성하였다.



위의 코드는 bubble sort의 assembly code와 실행 결과를 나타낸 것이다. 해당 bubble sort가 데이터
에 접근하는 방식을 자세히 보자. Bubble sort는 바로 옆에 있는 인자와 값을 비교해 나가면서 정
렬을 시도한다. 즉 바로 옆에 있는 인자 또한 cache에 있으면 hit의 확률이 증가하게 된다. 이를
위해서는 Block size를 키우거나, Sey를 키움으로써 이 과정에서 AMAT를 줄일 수 있다.

#### 2. Random access


##### 다음 코드와 실행 결과는 위 와 같이 나와있다. 해당 코드는 프로그램에 의해 무작위의 부분을

읽기 때문에 이로 인한 miss는 기본적으로 많은 코드다. 이를 Verilog를 통해 확인하면 더욱 쉽게
확인할 수 있다.


다음에서 볼 수 있듯이 육안으로 확인하였을 때 hit와 miss의 비중이 2: 1 정도가 나오는 것으로 판
단할 수 있다. 이러한 코드의 경우 데이터를 가져오는 위치가 계속 바뀌기 때문에 Set의 수를 늘
려줌으로써 더욱 많은 데이터를 보관하고 사용할 수 있도록 코드를 수정하면 miss의 빈도가 낮아
지고 hit의 빈도가 올라갈 것이다. 또한 Associativity를 높임으로써 같은 set이더라도 다른 way에
해당 데이터를 저장함으로써 AMAT를 높일 수 있다.

### C. Benchmark

#### 1. Benchmark program

#### M88ksim

```
해당 Benchmark는 SPEC CINT 95 중 하나로, Motorola 88100 microprocessor의 chip을 simulate
하기 위해 만들어졌으며 Dhrystone과 memory tester의 동작을 수행한다. 해당 benchmark를
통해 전반적인 chip의 성능을 가늠할 수 있다.
```
#### Compress 95

```
해당 Benchmark는 SPEC95에 포함되어 있다. 해당 프로그램은 in-memory buffer를 사용하며,
오래된 UNIX utility의 compression algorithm을 이용하여 압축을 진행한다.
```
#### Vortex

```
Vortex Benchmark는 object-orientied database를 확인하는데 사용한다. 3 개의 파트로 나눌 수
있으며 inter-related database를 만든다.
```
#### 2. Sim

M88ksim
# of Sets Unified cache
Miss rate

```
Unified cache
AMAT
```
```
Split cache Split cache
Inst. Miss rate Data Miss rate AMAT^
```

##### 64 0 .296 1 .0 4 +0.296*

##### 0 = 60. 04

##### 0. 316 0. 173 5 7.

##### 128 0 .247 1 .0 8 +0.247*

##### 0 = 50.4 8

##### 0 .278 0 .141 5 0.

##### 256 0 .176 1. 12 +0.176*

##### 0 = 36. 32

##### 0 .229 0 .130 4 2.1 5

##### 512 0 .113 1. 16 +0.113*

##### 0 = 23. 76

##### 0 .160 0 .107 3 0.

Compress
# of Sets Unified cache
Miss rate

```
Unified cache
AMAT
```
```
Split cache Split cache
Inst. Miss rate Data Miss rate AMAT^
64 0 .307 1 .04+0.307*
0 = 6 2.4 4
```
##### 0. 322 0. 159 5 6.

##### 128 0 .153 1 .0 8 +0.153*

##### 0 = 31.6 8

##### 0 .199 0 .124 3 6.

##### 256 0 .096 1. 12 +0.096*

##### 0 = 20. 32

##### 0 .059 0 .10 5 1 5.

##### 512 0 .038 1. 16 +0.038*

##### 0 = 8. 76

##### 0 .039 0 .087 1 1.

Vortex
# of Sets Unified cache
Miss rate

```
Unified cache
AMAT
```
```
Split cache Split cache
Inst. Miss rate Data Miss rate AMAT^
64 0 .2 05 1 .04+0.2 05 *
0 = 42.
```
##### 0 .211 0 .125 3 8.

##### 128 0 .179 1 .0 8 +0.179*

##### 0 = 36.8 8

##### 0 .189 0 .091 3 3.

##### 256 0 .142 1. 12 +0.142*

##### 0 = 2 9. 52

##### 0 .174 0 .045 2 9.

##### 512 0 .102 1. 16 +0.102*

##### 0 = 21. 58

##### 0. 132 0 .039 2 2.

Sim 1 의 경우 Unified cache와 Split cache의 차이, 그리고 Set의 크기에 따른 차이를 볼 수 있다. Set은
이론적으로 커지면 커질수록 기존의 데이터를 지울 확률이 줄어들기 때문에 miss가 줄어들게 된
다. 기본적으로 두 경우에서 모두 Set이 커질수록 AMAT가 작아지는 양상을 보인다. Unified cache와
Split cache의 차이는 기본적으로는 Set의 크기가 작을 때 Split cache의 AMAT가 더 작지만, Set의 크
기가 커질수록 Unified cache랑 비슷하거나 역전당하는 상황이 보인다.
위의 표를 확인하였을 때 Mk88ksim에 가장 잘 어울리는 Set은 256 Set의 Unified Cache가 AMAT,
cache size에 따른 cost를 따졌을 때 가장 좋은 효율을 낼 것으로 예상된다.
Compress95의 경우에는 같은 256 Sets이더라도 Split cache를 사용한 경우 더욱 가성비가 좋을 것으
로 예상된다.
Vortex는 기본적으로 Set의 향상에 따른 성능 개선이 비교적 적기 때문에 12 8 Split cache를 사용하
면 가성비가 비교적 양호해질 것이다.

```
Unified cache 64 Sets m88ksim
```

Compress95 vertex

Unified cache 128 Sets m88ksim

Compress95 vertex

Unified cache 256 Sets m88ksim


Compress95 vertex

Unified cache 51 2 Sets m88ksim

Compress95 vertex

Split cache 64 Sets m88ksim


Compress95 vertex

Split cache 128 Sets m88ksim

Compress95 vertex

Split cache 256 Sets m88ksim


```
Compress95 vertex
```
```
Split cache 512 Sets m88ksim
```
```
Compress95 vertex
```
#### 3. Sim

M88ksim
L1I/L1D/L2U Inst. Miss rate Data Miss rate Unified Cache
Miss rate

##### AMAT

##### 8 /8/1024 0 .385 0 .299 0 .173 2 0.


##### 16/16/512 0 .335 0 .227 0 .320 2 6.

##### 32/32/256 0 .316 0. 173 0 .530 3 6.

##### 64/64/128 0 .278 0 .141 0 .793 4 4.

##### 128/128/0 0 .229 0 .130 1 53. 2

Compress
L1I/L1D/L2U Inst. Miss rate Data Miss rate Unified Cache
Miss rate

##### AMAT

##### 8 /8/1024 0 .435 0 .368 0 .043 1 2.

##### 16/16/512 0 .403 0 .222 0 .09 1 1 4. 5

##### 32/32/256 0 .322 0 .159 0 .224 1 9.

##### 64/64/128 0 .199 0. 124 0 .422 1 9.

##### 128/128/0 0 .059 0 .105 1 19. 9

Vortex
L1I/L1D/L2U Inst. Miss rate Data Miss rate Unified Cache
Miss rate

##### AMAT

##### 8 /8/1024 0 .278 0 .213 0 .202 1 6.

##### 16/16/512 0 .225 0 .159 0 .428 2 2. 9

##### 32/32/256 0 .211 0 .125 0 .640 2 9.

##### 64/64/128 0 .189 0 .091 0 .873 3 3.

##### 128/128/0 0 .174 0 .045 1 36. 6

Sim2의 경우 L2의 크기와 L1/L2간의 크기 비율에 따른 성능의 차이를 확인할 수 있다. L 1 의 크기가
작더라도 L2의 크기가 클 경우 L1의 Miss rate는 높아지지만 L2의 Miss rate는 낮아지기 때문에 200
cycle의 동작을 최대한 피할 수 있다. 단 이러한 방향으로 제작할 경우 L2U가 너무 커지게 되면
가격적인 문제가 발생할 수 있다. 때문에 M88ksim은 32/32/256을, Compress95는 1 6/16/512를, Vortex
도 1 6/16/ 256 을 사용하는 것이 가걱과 AMAT를 고려하였을 때 가장 좋을 것으로 예상된다.

```
8/8/1024 cache m88ksim
```
```
Compress95 vertex
```

16/16/512 cache m88ksim

Compress95 vertex

32/32/256 cache m88ksim

Compress95 vertex


```
64/64/128 cache m88ksim
```
```
Compress95 vertex
```
#### 4. Sim

M88ksim
#of Sets Split Cache
Miss rate/AMAT
1 - way 2 - way 4 - way 8 - way
64 0 .281 57.2 0 .242 4 9.7 0 .208 4 2.9 0 .104 2 2.
128 0 .245 5 0.1 0 .200 4 1.2 0 .13 9 2 3.5 0 .045 1 0.
256 0 .210 4 2.15 0 .120 25.3 0 .04 7 10.7 0 .029 7.
512 0 .146 3 0.5 0 .067 1 4.7 0 .030 7 .4 0 .018 6.

Compress
#of Sets Split Cache
Miss rate/AMAT
1 - way 2 - way 4 - way 8 - way
64 0 .278 56.6 0. 097 1 9.6 0 .023 5. 7 0 .020 5.
128 0 .1 76 3 6.35 0 .068 1 4.7 0 .020 5 .3 0 .01 7 4.
256 0 .07 1 1 5.32 0 .021 5.43 0 .018 4 .8 0 .014 4.


##### 512 0 .051 1 1.4 0 .018 4 .8 0 .014 4 .1 0 .011 3.

Vortex
#of Sets Split Cache
Miss rate/AMAT
1 - way 2 - way 4 - way 8 - way
64 0. 189 3 8.9 0. 158 3 2.8 0. 136 2 8.4 0. 119 2 5.
128 0 .164 3 3.9 0 .139 2 9.0 0 .111 23.5 0 .04 3 9. 9
256 0 .141 2 9.4 0. 108 22.9 0 .0 49 1 1.0 0 .018 4. 9
512 0 .108 2 2.8 0 .06 0 1 3.2 0. 018 5 .0 0 .01 6 4.

Sim3의 경우 Associativity에 따른 AMAT의 차이를 확인할 수 있다. Associativity가 커질 경우 같은
Set의 정보가 들어왔을 때 이를 삭제하지 않고 보관할 수 있음으로써 hit의 경우가 늘어나게 된다.
특히 이는 어려 위치를 옮겨가는 프로그램에서 더욱 크게 힘을 발휘한다. 단 무작정 associativity
를 늘릴 경우에도 cache의 크기가 커지고 cache가 복잡해진다. 때문에 이에 따른 가격과의 비교를
통해 합의점을 찾아야 한다. M88ksim은 256 Set에 4 - way가 가장 성능 대비 가격이 좋을 것으로 예
상되며 Compress95는 64 Set에 4 - way가, Vortex는 128 에 8 - way가 가장 좋을 것으로 예상된다.

```
Split cache 64 Sets / 2-way Split cache 64 Sets / 4 - way
```
```
Split cache 64 Sets / 8 - way Split cache 128 Sets / 2 - way
```
```
Split cache 128 Sets / 4 - way Split cache 128 Sets / 8 - way
```

```
Split cache 256 Sets / 2 - way Split cache 256 Sets / 4 - way
```
```
Split cache 256 Sets / 8 - way Split cache 512 Sets / 2 - way
```
```
Split cache 512 Sets / 4 - way Split cache 512 Sets / 8 - way
```
#### 5. Sim 4

M88ksim
Block size Unified cache Miss rate AMAT
16 0 .113 2 3. 92
32 0 .046 1 0.


##### 64 0 .025 6.

##### 128 0 .00 9 3.

##### 256 0 .005 2.

Compress 95
Block size Unified cache Miss rate AMAT
16 0 .038 8. 92
32 0 .01 8 4.
64 0.012 3.
128 0 .011 3.
256 0 .004 2 .0 6

Vortex
Block size Unified cache Miss rate AMAT
16 0 .102 2 1. 72
32 0 .04 1 9.
64 0.024 6.
128 0 .009 3.
256 0 .006 2. 46

다음 Sim4는 Block size의 차이에 따른 성능의 차이를 확인할 수 있다. 기본적으로 프로그램을 내의
데이터의 크기가 클수록 인접한 memory의 위치할 확률이 커진다. 이는 Block size가 클 때 유리하
게 작동할 수 있다. 또한 Instruction의 경우 대부분 바로 다음의 코드를 읽기 때문에 이러한 부분
에서도 Block이 클수록 Instruction cache의 Miss rate가 줄어든다. Block size 또한 커질수록 cache가 커
지기 때문에 적절한 타협이 필요하다. M88ksim의 경우 64 size가, Compression9 5 는 32 size, Vortex도
32 size가 가격과 성능을 모두 잡을 것으로 예상된다.

```
Unified cache 32 Block size Unified cache 64 Block size
```
```
Unified cache 128 Block size Unified cache 256 Block size
```
(^)


위와 같은 Simulate를 종합하여 적절한 cache의 spec을 예상하였을 때 M88ksim은 256 set, 64 block,
4 - way, Unified L 1 cache와 1024 L 2 cache 성능과 가격 측면에서 가장 좋을 것으로 예상된다.
Compress 95 는 256 set, 32 block, 2 - way, Split L 1 cache와 102 4 L2 cache가 가장 좋을 것으로 예상된다.
Vortex는 128 set, 32 block, 8 - way, Split L1 cache와 512 L2 cache가 좋을 것이다.

## D. Consideration

위의 결과를 토대로 Set, associativity, Block size가 클수록 성능이 좋으며, L2 cache가 존재하고 L2의 크
기가 클수록 성능이 좋아진다는 것을 확인할 수 있었다. 단 이러한 성능의 상승폭은 점점 작아지
며 이에 따른 가격 또한 올라가기 때문에 가격과 성능의 중간점에서 타협을 봐야 한다는 점을 느
낄 수 있었다. M88ksim 프로그램의 경우 Block의 크기가 큰 것을 보았을 때 연속적인 값을 읽는
경우가 많을 것으로 예상되며, Compress95는 반복문이 적고 불러들여야 할 Instruction의 수가 많을
것으로 예상된다. Vortex는 associativity가 큰 것을 미루어 보았을 때 memory의 주소의 이동이 잦을
것으로 예상된다. 때문에 기존에 주어진 Bubble sort와 Random access 프로그램은 각각 M88ksim,
Vortex의 cache와 비슷한 양상으로 사용할 경우 성능이 좋을 것으로 예상된다.

## E. Reference

M 8 8ksim: https://www.spec.org/cpu95/CINT95/124.m88ksim/
Compress95: https://courses.cs.washington.edu/courses/cse471/06sp/hw/benchmark-guide.pdf
Vortex: https://www.spec.org/cpu95/CINT95/147.vortex/


