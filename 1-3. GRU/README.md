#GRU

##### RNN의 대표적인 셀 가운데 하나
##### LSTM의 장점을 유지하면서 계산복잡성을 낮춘 셀 구조(게이트의 일부 생략)
```
ht = tanh(Wxt+rt*Uht-1)
```
##### Wxt -> 현 시점 정보
##### U*ht-1 -> 과거 정보 (*은 element-wise product(Hadamard product))
##### rt -> reset gate
##### reset gate의 활성함수는 시그모이드 -> 0~1 사이의 범위를 갖는다.

##### 업데이트 하는 식
```
Ht = zt*ht-1+(1-zt)*ht
```
##### ht-1 -> 과거 정보
##### ht -> 현재 정보
##### zt -> update gate
##### zt의 값에 따라 현재 정보와 과거 정보를 얼마나 조합할지 결정

#### 참고자료
##### https://ratsgo.github.io/deep%20learning/2017/05/13/GRU/



