Lecture 10. Recurrent Neural Networks [작성중]
==========================================
(**작성자**: 박혜지)

CS 231n lecture 10. RNN을 정리합니다.

(본 포스팅은 [CS231n 해당 강의](https://youtu.be/6niqTuYFZLQ)와 스터디원 임한동 님의 발표 내용, [스터디원 임한동 님의 발표자료](https://github.com/ai-robotics-kr/CS231n_study/blob/master/study_records/%EB%B0%9C%ED%91%9C%EC%9E%90%EB%A3%8C/cs231n_2019_lecture10.pptx), [CS231n 강의 슬라이드](http://cs231n.stanford.edu/slides/2019/cs231n_2019_lecture10.pdf), [ratsgo's blog(RNN과 LSTM을 이해해보자)](https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/), [colah's blog(Understanding LSTM Networks)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)를 참고하여 작성하였습니다.)

강의 자료는 아래 링크를 참고하면 됩니다.

[Lecture Video](https://youtu.be/6niqTuYFZLQ)

[Course Notes](http://cs231n.github.io/)
#
> In Lecture 10 we discuss the use of recurrent neural networks for modeling sequence data. We show how recurrent neural networks can be used for language modeling and image captioning, and how soft spatial attention can be incorporated into image captioning models. We discuss different architectures for recurrent neural networks, including Long Short Term Memory (LSTM) and Gated Recurrent Units (GRU).

#
![1](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/1.png)

- **one to one** : Vanilla RNN. 입력이 1개이면 출력이 1개 나오는, 가장 기본적인 RNN 구조입니다.

- **one to many** : 입력이 1개일 때 출력이 여러개인 구조입니다. 예를 들면, 강아지 그림을 입력으로 주면 '강아지가 뭘 하고 있다'라는 문장이 출력으로 나오는 image captioning이 있습니다.

- **many to one** : 입력이 여러개 일 때 출력이 1개인 구조입니다. 예시로는 Sentiment classification이 있는데, 네이버 영화 리뷰를 보고 리뷰가 긍정적인지 부정적인지 분류하는 것과 같은 것입니다.

- **many to many**: 입력이 여러개일 때 출력도 여러개 나오는 구조입니다. 예를 들어 machine translation (기계 번역)(프랑스어를 한국어로 번역하기), Video classification on frame level(비디오 프레임 마다의 분류)가 있습니다.

#
![2](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/2.png)

예시로, 이미지를 입력으로 받아서 어떤 숫자가 이미지에 나타나있는지를 분류하는 문제가 있는데, 각각 feed forward 과정을 하기 보다, 이 네트워크는 이미지를 살펴보고, 이미지의 서로 다른 부분들을 살펴본 후에, 해당 이미지가 어떤 숫자를 나타내는지 결론을 내립니다. 이러한 예시는 RNN을 가지고 어떤 다양한 모델들이 나올지 기대되는 부분이기도 합니다.

#
![4](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/4.png)

RNN의 기본 구조인 Vanilla RNN 입니다. 타입스텝 마다, 입력 x가 RNN으로 들어가면 RNN에 있던 internal hidden state가 갱신되고 internal hiddel state는 다시 모델로 들어가고 RNN에서 출력 y가 나옵니다. 

즉, 입력값이 들어오면 hidden state가 업데이트 되고 출력값이 나오는 구조입니다.

#
![5](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/5.png)

초록색 RNN block안에서 계산되는 공식을 보면,

function f는 weights w에 따라 달라지는데, function f는 이전 히든 스테이트인 h<sub>t-1</sub>와 현재 스테이트의 입력값인 x<sub>t</sub>를 받고 다음 히든 스테이트(갱신된 히든 스테이트)를 출력합니다.

다음 단계에서도 동일한 function f를 사용합니다.

#
![6](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/6.png)

매번, 타임스텝마다, 동일한 function f를 사용합니다.

#
![7](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/7.png)

sigmoid가 아닌 tanh함수를 사용하는 이유는 **-1에서 1 범위의 값들을 표현해줄 수 있어서** 입니다.

#
![10](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/10.png)

첫번째 타임 스텝에서 첫 히든 스테이트, 즉 h<sub>0</sub>과 현재의 입력값인 x<sub>t</sub>가 f<sub>w</sub>함수로 들어가서, 다음 히든 스테이트인 h<sub>1</sub>을 생성합니다. 그리고 다음 입력값을 받으면 같은 과정을 계속해서 반복합니다. 

그리고 여기서 주목해야할 부분이 있는데, **모든 time step에서 동일한 W matrix를 재사용하고 있습니다.**
이렇게 재사용하기 때문에 역전파(backpropagation) 과정에서 결국 각각의 타임 스텝에 해당하는 그래디언트들을 모두 합친 값이 최종 그래디언트 값이 됩니다.

#
![11](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/11.png)

각각의 타임 스텝에 해당하는 출력값 y<sub>t</sub>은 아마 class score 등등 같은 것일 겁니다.

#
![12](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/12.png)

각각의 loss들은 아마 softmax loss 같은 것일 거구요.

#
![13](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/13.png)

그리고 최종 loss는 아까 말했듯이 각각의 loss들을 모두 합친 값이 됩니다.

#
![14](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/14.png)

입력이 여러개일 때 출력이 1개인 RNN의 computational graph입니다.

#
![15](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/15.png)

입력이 1개일 때 출력이 여러개인 RNN의 computational graph입니다.

#
![16](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/16.png)

왼쪽 many-to-one은 encoder로 볼 수 있습니다. many-to-one에서 프랑스어 문장 전체를 입력으로 받으면 그 문장을 하나의 벡터로 만듭니다. 그러면 오른쪽에 있는 decoder인 one-to-many에서 그 벡터를 입력으로 받아 출력으로 한국어 문장 전체가 나옵니다.

#
RNN은 language modeling에서도 자주 쓰입니다.

character 하나 하나를 가지고 하는 character-level language model도 있고,
단어 하나 하나를 가지고 하는 word-level language model도 있습니다.

간단한 예시로 character-level language model을 보겠습니다.

#
![17](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/17.png)

연속된 character를 읽고 다음 character가 뭔지 예측하는 language model입니다.
단어 'hello'로 트레이닝하고자할 때, 각각의 character를 입력값으로 가지는데, 이 입력값들을 벡터(one hot vector)로 표현하면,
위와 같습니다.

#
![19](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/19.png)

입력값으로 글자 'h'를 받아 RNN으로 들어가고 다음 글자는 뭐가 될 가능성이 제일 큰지를 나타내는 출력값 y<sub>t</sub>가 나옵니다.

그런데 위 그림을 보면, 가장 확률이 높은 것을 골랐을 때에는, 정답과 다른 글자를 예측하고 있습니다.
그럼 결국에 높은 loss를 갖게 되는데, 
training을 계속 하다보면 조금씩 loss가 작아지면서 점점 더 예측을 잘하게 됩니다.

#
![20](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/20.png)

마지막 softmax function이 score들을 확률분포 값들로 바꿔줍니다. 

#
![21](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/21.png)

Sampling하는 이유와 test time에 one hot vector를 넣어주는 이유입니다.

#
![23](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/23.png)

이전 글자를 보고 다음 글자 예측을 잘하였습니다.

#
![24](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/24.png)

Backpropagation through time의 단점은 위키피디아 같은 문장들을 돌렸을 때 아주 아주 느려질 수 있다는 것입니다. 메모리도 엄청 많이 들거구요.

그래서 이 문제를 해결하는 방법으로,

#
![25](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/25.png)

#
![26](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/26.png)

#
![27](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/27.png)

일정 step마다 나눠서 loss를 구하고 backpropagation하는 "Truncated backpropation through time"이 있습니다.

#
![28](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/28.png)

다음은 RNN backpropagation 코드 입니다.

http://gist.github.com/karpathy/d4dee566867f8291f086

#
![29](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/29.png)

윌리엄 셰익스피어의 글을 가지고 트레이닝을 해볼 수도 있는데,

#
![30](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/30.png)

트레이닝 초반에는 뭔가 이상한 결과물을 내는데,
트레이닝 후반에는 좀 괜찮은 결과물들이 나오기 시작합니다.
트레이닝이 잘되고 나면, 셰익스피어가 쓴 것 같은 어투의 결과물이 이처럼 나옵니다.

#
![31](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/31.png)

트레이닝을 좀더 많이 해보면 이처럼 셰익스피어의 연극 대본같은 결과물도 낼 수 있습니다.

#
![33](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/33.png)

수학 교과서 내용으로 트레이닝해서 수학 공식이나 정의같은 결과물도 낼 수 있습니다.

#
![34](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/34.png)

#
![35](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/35.png)

#
![36](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/36.png)

#
![37](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/37.png)

#
![38](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/38.png)

#
![39](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/39.png)
![40](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/40.png)
![41](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/41.png)
![42](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/42.png)
![43](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/43.png)
![44](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/44.png)
![45](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/45.png)
![46](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/46.png)
![47](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/47.png)
![48](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/48.png)
![49](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/49.png)
![50](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/50.png)
![51](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/51.png)
![52](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/52.png)
![53](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/53.png)
![54](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/54.png)
![55](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/55.png)
![56](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/56.png)
![57](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/57.png)
![58](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/58.png)
![59](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/59.png)
![60](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/60.png)
![61](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/61.png)
![62](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/62.png)
![63](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/63.png)
![64](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/64.png)
![65](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/65.png)

- Image captioning
  - image -> CNN ---[feature vector W] ---> RNN
  - image -> CNN ---[feature vector W]--attention을 활용하여--> RNN


![66](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/66.png)
![67](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/67.png)

- Soft attention
  - gray-scaling image
  - CNN network에서 모든 feature vector를 뽑아냄 ( 모든 feature vector에 attention)

- Hard attention
  - 특정 feature vector만 뽑아내는 것 -> Non differentiable -> RL에 많이 씀

[참고자료][What is the difference between soft attention and hard attention in neural networks](https://www.quora.com/What-is-the-difference-between-soft-attention-and-hard-attention-in-neural-networks)


![68](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/68.png)
![69](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/69.png)
![70](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/70.png)
![71](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/71.png)
![72](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/72.png)
![73](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/73.png)
![74](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/74.png)
![75](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/75.png)
![76](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/76.png)
![77](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/77.png)
![78](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/78.png)
![79](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/79.png)
![80](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/80.png)
![81](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/81.png)
![82](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/82.png)
![83](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/83.png)
![84](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/84.png)
![85](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/85.png)
![86](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/86.png)
![87](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/87.png)
![88](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/88.png)
