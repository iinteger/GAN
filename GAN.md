# GAN

* #### 등장 배경

  * 기존의 생성모델은 Markov-Chain이나 Variational Encoder와 같이 통계와 수학을 기반으로 하는 모델들인데, 발전을 거듭함에도 성능이 좋지 못했음

  * 2014년, Ian Goodfellow가 기존 방법론에서 벗어나, 두개의 Neural Network가 서로 경쟁하며 발전하는 방법을 제안함

    

* #### Adversarial Training

  * GAN을 이루는 두 네트워크인 Generator와 Descriminator는 주어진 입력 데이터를 두고 경쟁하며 학습함

    <img src="images/gan1.png" />

    

  * ##### Generator

    * Generator(생성자)는 Latent Space (일반적으로 Uniform Distribution, Gaussian Distribution)에서 random vector Z를 받아 Output Xf (fake)을 생성
    * 이때 Xf는 real data에서 sampling된 Xr (real)를 최대한 근사하도록 학습

  * ##### Discriminator

    * Discriminator(판별자)는 Xr을 True로, Xf를 False로 구분하도록 학습

      

    <img src="images/gan3.png" />

    * 학습이 진행되며 (b) 에서 D = Pdata(x) / (Pdata(x) + Pg(x))로 수렴
  * Generator가 학습을 거치며 Pg = Pdata로 수렴하게 되면 D가 두 분포를 구분하지 못하게 되어 D(x) = 0.5 에 수렴 (global optimum)
    
    * latent vector z도 실제 데이터와는 무관한 곳에 mapping되다가, G의 학습이 끝나면 real data distribution을 잘 표현할 수 있는 x로 mapping 

  

  

* #### Value Function

  <img src="images/gan2.png" />

  * Generator는 log D(x)를 0으로, D(G(z))를 1로 만들어야 함. 즉, 판별자가 실제 데이터는 거짓으로, 가짜 데이터는 참으로 구분하게 만들어야 하기 때문에 우항 두 term이 모두 0이 되어야 하므로 위 식을 minimize하는 방향으로 학습

  * Discriminator는 위와 반대로 log D(x)를 1로, D(G(z))를 0으로 만들어야 하기 때문에 위 식을 maximize 하는 방향으로 학습

  * G를 최적화하는것은 JSD(Pdata||Pg)를 최소화시키는것과 같음. 따라서 Generator가 최적화되었을 때, 생성된 데이터의 분포는 실제 데이터의 분포와 동일하게 됨. 이는 GAN이 주어진 dataset의 distribution를 알아내는 작업이라는 것을 의미

    

* #### Experiment

  * MNIST, TFD, CIFAR-10을 대상으로 실험 진행

    <img src="images/gan4.png" />

  * 실험 결과 실제 dataset과 유사한 데이터를 잘 생성함
  * 그러나 학습 초기에는 G가 생성하는 이미지가 쉽게 구분되며 이는 gradient를 0에 가깝게 만들어 경사가 손실되는 문제가 생김
  * 또한 G가 D를 잘 속이는 이미지를 발견했을 때, 계속 그 이미지만 생성하여 손실 함수가 작아지는 문제가 있음. 이를 모드 붕괴(Mode Collapse)라고 함