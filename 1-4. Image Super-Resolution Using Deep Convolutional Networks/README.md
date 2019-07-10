##### single low-resolution image를 high-resolution image로 recovering 하는 것은 ill-posed문제이다. 즉 여러 solution이 존재한다. 하나의 입력에 복수의 결과물이 나올 수 있다는 뜻이다. 이와 같은 문제는 강력한 사전 정보를 가지고 있는 solution space를 제한함으로써 완화될 수 있다. 이 사전 정보를 학습하기 위해 exampled-based(patch-based)라는 전략이 사용되곤 했는데 

##### Example-based 저해상도-고해상도 쌍을 미리 뽑아놓고 대칭시키는 방식(학습모델 구축 어려움)

##### 이 논문에서는 기존의 external example-based approach와 다른 방법을 택했다. dictionary나 manifold를 학습하지 않았다. 이것은 hidden layer를 통해 암시적으로 학습되었다.
##### Patch라는 단어가 쓰이는데 무슨 의미일까? 어떤 이미지의 부분을 이야기 하는 듯... 전체 이미지를 다 안쓰고 일부만 잘라서 쓰는듯...
##### Patch extraction과 aggregation 또한 conv layers에 표현되었다.

##### 이 논문에서 소개된 CNN을 SRCNN이라 하였다. Super-Resolution Convolutional Neural Network
##### SRCNN의 몇가지 장점이 있다면
##### 1. 간단한 구조이면서 기존에 나온 state-of-the-art example-based 방법보다 훨씬 좋은 정확도를 보인다.
##### 2. 적당한 필터와 layer수이기 때문에 cpu에서도 빠른 학습 속도를 보인다. 또한 다른 방법보다도 더 빠르다.
##### 3. 데이터셋의 숫자가 많고 다양할 때, 크고 깊은 모델이 사용될 때 restoration quality가 많이 향상되었다

##### 전반적으로 이 논문에서 기여한 바는 다음과 같다.
##### 1. 간단한 pre/post processing으로 low-resolution과 high-resolution 간의 end-to-end mapping을 학습한다.
##### 2. 기존 전통적인 방법과 새로운 방법간의 관계를 정립하였다. 이는 네트워크의 디자인 방향을 제시하였다.
##### 3. 딥러닝 방법이 좋음을 확인하였다.
##### 4. Super Resolution에서 처음으로 CNN 사용

##### 3. Convolutional Neural Networks for Super-Resolution
##### 3.1 Formulation
##### 한장의 low-resolution image를 가지고 desired size로 bicubic interpolation을 이용해 upscale을 하였다. bicubic interpolation은 https://kipl.tistory.com/55을 참고
##### upscale을 하는 이유는 다음과 같다. 내가 만약 10x10 이미지의 해상도를 30x30으로 만들어 주고 싶다. 그러면 10x10 image를 input으로 넣는 것이 아니라 upscale을 먼저 하여 30x30으로 만든 후 input으로 넣어줘야 한다.
##### 추후에는 이 upsampling조차도 학습하게 된다.

##### upsampling을 어디에 놓느냐도 연구가 많이 되었는데
##### 1. Pre-upsampling - 가장 초기에 사용한 방식, 네트워크의 input과 output 크기가 동일하게 유지되므로 계산량에서 손해를 보게 됨
##### 2. Post-upsampling - Transposed Convolution을 사용하여 네트워크 마지막 부분에서만 upsampling을 하므로 계산량이 작아져 학습이 빠름, 대부분의 모델이 이 방식 사용
##### 3. Progressive upsampling - 마지막에서 한번에 HR 이미지 크기로 올린다. 따라서 배율이 높은 SR 모델 학습이 어렵다.
##### 4. Iterative up-and-down sampling - 최근에 제안된 모델

##### 네트워크 디자인도 연구가 많이 됨 - 이 부분은 좀 나중에 생각하자
##### 큰줄기는 다음과 같음
##### 1. Residual Learning
##### 2. Recursive Learning
##### 3. Deeper and Denser Learning
##### 4. EDSR
##### 5. Exploiting Non-local or Attention modules
##### 6. Generative Adversarial Networks in SR



##### 목표는 interpolated된 이미지 Y를 이용하여 recover한 한 이미지 F(Y)를 만들어 고해상도의 ground-truth인 X와 비슷하게 하는 것이다.
##### mapping을 배우기 위해 3가지가 필요하다.
##### 1. Patch extraction and representation : Y로부터 overlapping한 patch를 추출하여 벡터로 만든다. 이 벡터는 일련의 feature map으로 구성되어 있고 벡터의 차원과 같은 크기이다.???????
##### 2. Non-linear mapping : 이 과정은 비선형 mapping을 한다. 각각의 mapping된 vector는 개념적으로 high-resolution patch를 표현한다. 이 벡터는 다른 feature map의 set으로 구성되어 있따.
##### 3. Reconstruction : 이 과정은 최종 고해상도의 이미지를 얻기 위해 위의 고해상도 patch-wise를 쌓는다. 이 이미지는 ground-truth인 X와 유사하다.

##### 식으로 표현하면
##### F1(Y) = ReLU(W1 * Y + B1)
##### F2(Y) = ReLU(W2 * F1(Y) + B2)
##### F(Y) = W3 * F2(Y) + B3

##### 91개의 이미지에서 stride를 14로 주었을 때 33x33 패치 수가 약 25000개 정도 나옴

##### loss 함수로는 MSE를 썼다.
##### 필터사이즈를 늘렸을 때 성능이 더 좋아짐을 확인

##### 논문에는 Y채널만 적용을 했는데 발표 후 RGB 채널로도 바꿔봤는데 성능이 잘 나왔다.

##### inference 할 때는 전체 이미지를 넣는다.

##### 발표에 따르면 ImageNet으로 학습하고 위성 이미지를 Super Resolution 해봤는데 도메인 정보가 많이 달라서 그런지 blur 되는 부분이 많았다고 한다.

##### 모델의 독창성이 높다고는 할 수 없지만 이 논문 이후에 거의 모든 Super_Resolution 문제는 SRCNN을 기반으로 하였다.





https://jamiekang.github.io/2017/04/24/image-super-resolution-using-deep-convolutional-networks/ 
http://jaejunyoo.blogspot.com/2019/05/deep-learning-for-SISR-survey-1.html 참고


##### 조건수란 argument에서의 작은 변화의 비율에 대해 함수가 얼마나 변할 수 있는지에 대한 argument measure이다. 어떤 오차에 대해 argument의 큰 변화에도 오차 범위 안에 든다면 ill-conditioned라고 할 수 있을조건수에 대한 내용은 http://engidic.blogspot.com/2013/11/condition-number.html를 참고