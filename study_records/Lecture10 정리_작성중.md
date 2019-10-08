Lecture 10. Recurrent Neural Networks [작성중]
==========================================

스터디원 임한동 님의 발표 내용을 바탕으로 cs231n lecture 10. RNN을 정리합니다.

(본 포스팅은 [CS231n_study/study_records/발표자료/cs231n_2019_lecture10.pptx](https://github.com/ai-robotics-kr/CS231n_study/blob/master/study_records/%EB%B0%9C%ED%91%9C%EC%9E%90%EB%A3%8C/cs231n_2019_lecture10.pptx)와 [CS231n 강의 슬라이드](http://cs231n.stanford.edu/slides/2019/cs231n_2019_lecture10.pdf)를 참고하여 작성하였습니다.)

강의 자료는 아래 링크를 참고하면 됩니다.

[Lecture Video](https://www.youtube.com/watch?v=6niqTuYFZLQ&list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv&index=10)

[Course Notes](http://cs231n.github.io/)
#
> In Lecture 10 we discuss the use of recurrent neural networks for modeling sequence data. We show how recurrent neural networks can be used for language modeling and image captioning, and how soft spatial attention can be incorporated into image captioning models. We discuss different architectures for recurrent neural networks, including Long Short Term Memory (LSTM) and Gated Recurrent Units (GRU).

#
![1](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/1.png)

- **one to one** : Vanilla RNN. 입력이 1개이면 출력이 1개 나오는, 가장 기본적인 RNN 구조입니다.

- **one to many** : 입력이 1개일 때 출력이 여러개인 구조입니다. 예를 들면, 강아지 그림을 인풋으로 주면 '강아지가 뭘 하고 있다'라는 문장이 아웃풋으로 나오는 image captioning이 있습니다.

- **many to one** : 입력이 여러개 일 때 출력이 1개인 구조입니다. 예시로는 Sentiment classification이 있는데, 네이버 영화 리뷰를 보고 리뷰가 긍정적인지 부정적인지 분류하는 것과 같은 것입니다.

- **many to many**: 입력이 여러개일 때 출력도 여러개 나오는 구조입니다. 예를 들어 machine translation (기계 번역)(프랑스어를 한국어로 번역하기), Video classification on frame level(비디오 프레임 마다의 분류)가 있습니다.

#
![2](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/2.png)

예시로 Sequential data 뿐만 아니라, 이런 Non-sequential data에 대해서 적용된 예시가 나오는데, 이 예시는 MNIST 그림 일부분을 연속적으로 보면서 해당 글자가 어떤 건지 예측하는 RNN모델입니다.

#
![4](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/4.png)

RNN의 기본 구조인 Vanilla RNN 입니다. 입력 x가 들어가서 internal state가 발생하고 출력 y가 나오는 구조입니다.

#
![5](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/5.png)

  - h<sub>t</sub> : NEW state
  - h<sub>t-1</sub>: OLD state
  - t: time step
  
#
![6](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/6.png)

The same function ( the same W matrix) [f<sub>w</sub>] : "같은 W 매트릭스를 통해서 hidden state를 구한다"

![7](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/7.png)
![8](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/8.png)

sigmoid가 아닌 tanh를 씀 : 음수 범위도 표현해줄 수 있어서


![9](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/9.png)
![10](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/10.png)
![11](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/11.png)
![12](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/12.png)
![13](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/13.png)
![14](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/14.png)
![15](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/15.png)
![16](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/16.png)
![17](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/17.png)
![18](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/18.png)
![19](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/19.png)
![20](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/20.png)
![21](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/21.png)
![22](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/22.png)
![23](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/23.png)
![24](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/24.png)
![25](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/25.png)

Truncated backpropagation through time : loss를 일정 step마다 나눠서 backpropagation


![26](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/26.png)
![27](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/27.png)
![28](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/28.png)
![29](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/29.png)
![30](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/30.png)
![31](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/31.png)
![32](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/32.png)
![33](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/33.png)
![34](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/34.png)
![35](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/35.png)
![36](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/36.png)
![37](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/37.png)
![38](https://raw.githubusercontent.com/ai-robotics-kr/CS231n_study/master/images/lecture10/38.png)
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
