# 이미지 분류
> Private accuracy: 74.9524%, Private F1 Score: 0.6716
![image](https://user-images.githubusercontent.com/54899906/120734780-6e99cd80-c524-11eb-9c0a-dab5adce4f26.png)
---
## 대회 개요
주어지는 사진 속 사람의 마스크 착용 여부, 나이, 성별을 예측하는 Task
- 데이터
  1. 전체 사람 명 수 : 4500
  2. 한 사람당 사진의 개수 : 7 (마스크 착용 5장, 이상하게 착용(코스크, 턱스크) 1장, 미착용 1장)
  3. 이미지 크기 : (384, 512)
- 평가 방법
  - 18가지 class에 대한 f1 score
![image](https://user-images.githubusercontent.com/54899906/120731783-452a7300-c51f-11eb-9ef4-1287ab98c6db.png)
---
## 최종 모델
- model : efficientnet-b3(pretrained)
- optimizer : SGDP
- Loss : FocalLoss
- Learning Rate : 1e-3
- epoch : 20
- augmentation : resize(380,380), Normalize
![image](https://user-images.githubusercontent.com/54899906/120732542-79526380-c520-11eb-96e1-0c16a8dcf5ad.png)
---
## EDA
- age가 60대 이상인 data가 매우 적다. data imbalance 문제가 크다.
---
## 가설과 검증
- 성공 목록
  - paperswithcode에 나오는대로라면 resnet50보다 efficientnet 계열의 성능이 더 좋을 것이다. -> 성능향상!
  - imbalance 데이터 처리에 유리한 FocalLoss를 사용하면 성능이 증가할 것이다. -> 성능 향상!
  - 비교적 최신 optimizer인 adamp와 sgdp를 사용해보자  -> 성능 향상!
- 실패 목록
  - efficientnet으로 넘어오면서 efficientnet-b3를 b7으로 교체하면 당연히 성능이 오를 것이라고 생각함 -> 성능 하락!
    내가 생각하는 원인 : 무거운 모델을 돌리기 위해 (280,280)으로 resize를 해준 것이 오히려 성능을 저하시키는 원인이 되었을 것
  - Gaussian Noise 사용 -> 성능 하락!
    내가 생각하는 원인 : 마스크에 가려져 얼마 보이지 않는 얼굴로 성별과 나이를 예측해야하는 task인데 얼굴에 노이즈를 더 넣는게 독이 되었을 것
---
## 아쉬운 점
- augmentation을 다양하게 적용해봤어야했는데 첫 대회의 초창기라 적응하는데에 시간이 많이 걸렸다.
- 부스트캠프 끝무렵인 지금 생각해보면 lr도 조정하고 loss 앙상블도 해봤을 것 같은데.. 지금보니 실험내역이 너무 초라하다..ㅠㅠ
