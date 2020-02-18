# EfficientNet   
2019년 5월에 발표된 [논문](https://arxiv.org/pdf/1905.11946.pdf)   
   
![effic](https://user-images.githubusercontent.com/59756209/74518169-b0c98500-4f56-11ea-83dd-07b121185c26.PNG)   
   
image classification에서 굉장히 성능이 좋은 model   
최근 image classification competition에서 제일 많이 사용되고 순위권에 항상있는 model   
   
## 1. scale-up   
성능을 높이기 위해 Convolution scale-up은 일반적인 일이다.   
ResNet은 ResNet-18 부터 ResNet-200까지 신경망의 depth를 늘려 성능을 향상시켰다.   
이와 같이 scale-up을 하게 되면 성능은 좋아지며, scale-up의 방식은 크게 3가지가 있다.   
1. 신경망의 depth를 늘리기   
2. channel width를 늘리기   
3. input image의 resolution을 올리기   
   
EfficientNet은 세가지의 방법을 fixed sclaing coefficients을 이용하여 최적의 조합을 찾는 기법이다.   

## 2. Compound Model Scaling
   
![eff_layer](https://user-images.githubusercontent.com/59756209/74712539-e5488400-5269-11ea-9145-7d710155afb2.PNG)   
   
F : 연산자   
X : input tensor, X의 size는 <H, W, C>
Y : output tensor   
N : 여러개의 F(연산자)들이 모인 layer 결합체   
일부 신경망들은 전체 layer를 몇개의 stage로 나눌 수 있다.   
예로써, ResNet은 5개의 stage로 나뉜다.   
그리고 모든 stage는 모두 동일한 convolution 연산을 수행한다.   
일반적인 N(신경망)은 depth, width, resolutoin 들이 서로 독립적인 관계가 아니다.   
하지만, 기존에는 각각에 대해서만 변화를 가하는 작업들을 진행하였다.   
   
### (1) depth   

  
