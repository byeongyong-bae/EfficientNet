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
   
![depth](https://user-images.githubusercontent.com/59756209/74713285-a87d8c80-526b-11ea-90df-f156ac6d5d88.PNG)   
   
가장 흔한 scale-up 방법으로 depth의 수가 증가할수록 성능이 높아진다.   
하지만, depth 수가 계속 증가하는 것은 오히려 성능이 감소된다.   

### (2) width   

![width](https://user-images.githubusercontent.com/59756209/74713312-b7fcd580-526b-11ea-928f-47fb3ab90dfc.PNG)   

width를 넓게 할수록 미세한 정보들을 더 많이 담을 수 있다.   
그러므로 width가 넓어질수록 성능이 높아진다.   
하지만, widht 수가 증가할수록 saturate되는 문제가 있다.   

### (3) resolution   

![resolution](https://user-images.githubusercontent.com/59756209/74713980-f5159780-526c-11ea-9ee8-ff4e66950eb4.PNG)      
   
input layer에 더 큰 image를 넣으면 성능이 높아진다.   
224 x 224 크기의 image보다 331 x 331 크기의 image가 더 좋은 성능을 내고, 점점 크기가 클 수록 성능이 좋아지지만, 이것도 saturate되는 문제가 있다.   
   
이를 종합해보자면, depth / width / resolution 을 키우면 키울수록 성능이 높아지지만 얻을 수 있는 이득은 적어진다.   
   
## 3. Compound Scaling   
   
![compounding_scale](https://user-images.githubusercontent.com/59756209/74716009-111b3800-5271-11ea-8a2e-abe709ac5d3b.PNG)   
   
depth와 resolution을 고정한 채로 width 값을 변화시키면서 테스트   
이 떄 다양한 크기의 depth과 resolution을 테스트   
동일한 FLOPS에서 width, depth, resolution 조합에 따라 다양한 성능의 차이를 보인다.   
최고의 정확도를 갖기 위한 가장 최적의 width, depth, resolution 조합을 찾아낸다.   
기존의 수동적으로 찾는 작업의 연구가 아닌 새로운 방식인 compound scaling method 방식을 제안한다.   
   
![compound_scale](https://user-images.githubusercontent.com/59756209/74716369-bdf5b500-5271-11ea-90ef-577d84af7466.PNG)   
   
a, b, r은 상수이고 grid search를 이용하여 찾는다.   
파이는 사용자가 제어할 수 있는 적당한 값을 넣는다.   
depth는 2배가 되면 2배의 FLOPS가 되므로 제곱식 X   
하지만, width와 resolution은 2배가 되면 4배의 FLOPS가 되기때문에 제곱식을 적용   
최종적인 FLOPS는 (a, b**2, r**2)**파이 에 의해 결정된다.   
   
## 4. EfficientNet   
   
base 모델이 어떤 모델이냐에 따라 기본 성능 차이가 많이 발생하므로 좋은 base 모델을 사용하는 것이 주용하다.   
moblie-size baseline인 EfficientNet을 AutoML을 통해 구축한다.   
이 때, 사용한 AutoML 방식은 multi-objective neural architecture search를 사용하여 accuracy와 FLOPS 모두를 opimize하는 network를 찾도록 했다.   
   
