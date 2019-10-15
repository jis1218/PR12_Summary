##### Depthwise Seperable Convolution은 2개의 과정으로 나뉘어져 있다.

##### 1. Depthwise Convolution
##### 특정 크기의 필더의 detph를 1로 하여 input의 채널의 크기만큼 수행한다.

##### 2. Pointwise Convolution
##### 1X1 크기의 필터를 output 채널의 크기만큼 Convolution하여 채널의 크기를 늘린다.

##### Depthwise와 Pointwise Convolution을 합치면 기존의 Standard한 Convolution보다 연산량이 많이 줄어든다.