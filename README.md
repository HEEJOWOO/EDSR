# EDSR : https://arxiv.org/abs/1707.02921
#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Enhanced Deep Residual Networks for Single Image Super-Resolution
--------------------------------------------------------------------------
![image](https://user-images.githubusercontent.com/61686244/94802175-26209200-0422-11eb-97a3-cee3fbba01cc.png)
* EDSR은 기존에 ResNet구조를 SR에 접목시켜 나온 SRResNet을 개선을 하여 나온 네트워크
* SRResNet은 기존의 ResNet구조를 그대로 사용하여 좋은 결과를 보여주었고 또한 ResNet이 super resolution에 적합한 네트워크임을 인정
* 하지만 ResNet은 원래 Img Classification과 같은 High-Level의 문제를 해결하도록 나온 네트워크였음
* 본 논문의 저자들은 ResNet을 그대로 SR에 사용하는건 Low-Level 문제를 풀기위한 최적의 선택이 아니라고 생각
* SRResNet를 변형하여 SISR에 최적화된 EDSR을 만들어내었음
* Batch Normalization은 네트워크의 특징을 normalize시켜 네트워크의 유연성을 저하시키는데 영향을 미쳤는데 이를 제거하여 네트워크가 좀 더 유연하게 동작할 수 있도록 만들었음
* Batch Normalization을 제거 함으로써 메모리 사용량을 40% 정도 절감을 하였고 메모리 사용량이 절감된 만큼 더 큰 네트워클를 학습 시킬 수 있었음

EDSR Architecture
-----------------
![image](https://user-images.githubusercontent.com/61686244/94802334-6ed84b00-0422-11eb-9e11-ebe45df6b59b.png)
* 두가지의 네트워크를 제안해주었는데 첫 번째로 EDSR의 구조
* EDSR은 Single-scale-model을 학습시키는 네트워크로써 영상의 해상도를 2배, 3배, 4배 올리는 경우 각각에 대해 따로따로 네트워크를 학습 시킴
* 2배 3배 4배 스케일에 대해 각각의 네트워크 구조는 모두 동일하지만 Upsampling하는 부분만 상황에 맞게 서로 다른 구조를 가지고 있음
* Residual block을 보게 되면 Multiplication Layer층이 존재
* Multiplication Layer에서는 Residual Scaling을 적용하게 되는데 왜냐하면 깊은 residual Network를 훈련하게 되면 학습중 네트워크가 죽어버리는 현상이 일어나는데 Residual Scaling을 적용하여 Residual의 끝에 0.1~0.3을 곱하여 네트워크가 죽는 현상을 막아 조금 더 안정적으로 학습될 수 있게 만들었다고함
* 본 논문에서 성능을 끌어올리는 방법을 소개해줬는데 2배 스케일의 이미지는 처음부터 학습을 시켰지만 3배, 4배 스케일 이미지에 대해서는 기존에 2배 이미지를 학습 시켜준 사전 학습된 네트워크를 이용하여 학습을 시켰다고함
* 오른쪽에 보이는 그래프를 보시면 2배 이미지로 사전 학습된 네트워크와 처음부터 학습을 한 네트워크의 비교 그래프인데 확실히 사전 학습된 네트워크를 이용한것이 더 빠르게 높은 PSNR에 도달하는 것을 확인 할 수 있음

MDSR ARchitecture
-----------------
![image](https://user-images.githubusercontent.com/61686244/94802497-c4145c80-0422-11eb-8fc5-272f59a1e1c4.png)
* 두번째로 제안을 해준 multi deep residual network MDSR 
* MDSR 네트워크는 하나의 네트워크로 각각의 스케일을 학습 시키는 EDSR과 다르게 여러 스케일의 SISR에 적용이 가능한 네트워크로 네트워크의 중앙에 하나의 학습된 모델을 공유하고 각 스케일에 대처할 수 있도록 각 스케일에 서로 다르게 적용되는 pre-processing module을 적용했고, 마지막층인  upscaling 도 각 스케일에 대응하는 구조를 따로 배치를 하였음
* 완벽한 하나의 네트워크라고 보기는 어렵지만 중앙에 Residual Block를 공유함으로써 각 스케일에 대응하기 위해 EDSR 3개를 만드는 것에 비해 더 적은 수의 파라미터를 사용할 수 있었다고함
* EDSR로 2배 3배 4배 의 스케일을 학습할 경우 각각 1.5M의 파라미터수를 가져 총 4.5M의 파라미터수를 가지게 되고 MDSR로 학습을 하게되는경우 3.2M의 파라미터수를 가지게 된다고함

Conclusions
-----------
![image](https://user-images.githubusercontent.com/61686244/94802763-2cfbd480-0423-11eb-8f24-fd08e8e7a524.png)
* DIV2K 데이터 셋 이용하여 각각의 네트워크들을 2배 3배 4배 스케일로 비교한 결과
![image](https://user-images.githubusercontent.com/61686244/94802897-646a8100-0423-11eb-95cb-742a613d0c74.png)
![image](https://user-images.githubusercontent.com/61686244/94802925-72b89d00-0423-11eb-843d-2f9577c24be2.png)
![image](https://user-images.githubusercontent.com/61686244/94802945-7b10d800-0423-11eb-8b40-f1ae13d5ade2.png)

**불필요한 Batch Normalization을 제거**
**메모리 획득과, 획득된 메모리를 사용하여 깊은 네트워크 사용**
**학습 능률을 위한 Geometric self-ensemble사용 **
**EDSR,MDSR 두 네트워크 모두 많은  parameter를 이용하였고, 그 결과 SRResNet에 비해 더 좋은 성능을 보여줌**
**하나의 스케일을 사용하는 EDSR, 다양한 스케일을 사용하는 MDSR**

