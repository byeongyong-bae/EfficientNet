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

