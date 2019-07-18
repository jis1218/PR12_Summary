##### R-CNN

##### R-CNN의 목표는 하나의 이미지에서 주요 객체들을 박스(Bounding box)로 표현하여 정확히 식별하는 것에 있다.
##### Input - 이미지, Outputs - 박스로 영역 표시 + 각 객체의 라벨링

##### 1. Region proposals
##### Selective Search - Region Proposal 생성 알고리즘
##### Selective Search는 Exhaustive search 방식 + Segmentation 방식을 결합함
##### Exhausitve search는 후보가 될 만한 모든 영역을 샅샅이 뒤지는 것
##### Segmentation은 영상 데이터의 특성(색상, 모양, 무늬)에 기반한 방식
##### Selective Search를 통해 선택된 region은 크기가 같은 이미지로 만들어진다. (227x227)

##### 2. Feature Extraction
##### AlexNet을 사용

##### Test Time
##### Selective Search로 2000개의 region proposals를 뽑아냄
##### 뽑아낸 후 CNN 돌려 feature 뽑아냄
##### Classification을 위해 SVM을 썼음, 일단 SVM이 성능이 더 좋았기 떄문이고 오래 쓰기도 했고

##### Bounding Box 개선하기
##### Selective Search를 통해 찾아낸 Box는 불완전한가보다. 그래서 위치를 개선하기 위해 linear regression을 이용한다.
##### 이 Linear Regression의 input, output은 다음과 같다.
##### Input - 객체에 해당하는 이미지의 부분, Output - 입력 부분에 맞는 새로운 bounding box의 좌표들

##### Training 하는 방법
##### 1. ImageNet을 이용하여 AlexNet의 Feature Map을 학습시킨다.
##### 2. 맨 마지막 layer(1000 dimension)을 떼어내고 21 dimension(20 object + background) layer를 붙인다.
##### 3. 모든 region proposal을 뽑아낸 뒤 CNN input size로 warp 한 후 CNN에 통과시켜 맨 마지막 feature값을 disk에 저장한다.
##### 4. 이 region feature를 이용해 SVM을 학습시킨다.
##### 5. IoU가 0.5 이상인 것들은 positive, 아닌 것들은 negative


##### 참고
##### https://blog.naver.com/kyb7902/221012988041
##### https://blog.naver.com/lyshyn/221319139310

##### Fast R-CNN
##### Conv feature map을 RoI pooling을 한다. 각각 다른 크기의 RoI가 나오겠지만 똑같은 크기의 output이 나오도록 max pooling을 한다.
##### 엉뚱한 걸 잡으면 BBox에 대한 loss function은 사라짐
##### 하지만 Selective Search 하는데 시간이 많이 걸림(CPU에서 돈다)

##### Faster R-CNN
##### Region Proposal Network + Fast R-CNN
##### RPN은 어떻게?
##### Network를 가볍게 하기 위해 classification -> 물체가 있는지 없는지만 확인
##### Sliding은 k개의 Anchor Box, 논문에서는 9개의 Box 사용

