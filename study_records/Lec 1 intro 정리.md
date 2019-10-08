# Lecture 1

(**작성자**: 장형기)

([Youtube Link](https://www.youtube.com/watch?v=vT1JzLTH4G4&list=PLC1qU-LWwrF64f4QKQT-Vg5Wr4qEE1Zxk&index=1)를 참조하여 작성하였습니다)

Stanford CS231n 코스는 **Deep Learning을 사용한 Computer vision**에 대한 코스입니다.

딥러닝 비전은 최근에 들어서야 갑작스럽게 부상한 기술임에도 불구하고, 현재 (2019년) 컴퓨터 비전 관련 모든 연구의 약 70% 이상을 차지하고 있다고 봐도 과언이 아닐만큼 인기를 끌고 있는 기술입니다.

CS231n 코스를 통해 우리는 **딥러닝 비전의 작동 원리를 이해**하고 **직접 사용**해보며 기술을 익힐겁니다. 딥러닝 비전 공부의 방향성을 확실히 하기 위해 Lecture 1에서는 

- 컴퓨터 비전이란 무엇인지
- 컴퓨터 비전의 기원과 비-딥러닝 비전의 발전 과정
- 비-딥러닝 기법에서 딥러닝 기법으로 넘어가는 과정을 설명합니다.

이를 통해서 우리는 딥러닝 비전이 왜 효과적으로 컴퓨터 비전 문제를 풀 수 있는지, 또 왜 딥러닝 비전이 현재 컴퓨터 비전 학계에서 인기를 끌고 있는지 설명할 수 있습니다.

---

---

# 1. 컴퓨터 비전이란?

- 이미지와 영상을 분석해서 의미있는 정보를 얻어내는 방법
    - 학문마다 문제를 바라보는 관점이 다를 수 있습니다
        - 물리학 - 실제 세상을 이미지와 영상으로 표현하려면 어떻게 해야하는가? 카메라는 어떻게 빛을 받아드리고 디지털 처리를 할 수 있는가?
        - 생물학 - 생물의 눈과 시신경과 뇌는 어떻게 연결되어있고, 시각적 정보에서 어떻게 의미있는 정보를 추출하는가?
        - 심리학 - 시각적 정보에서 의미있는 정보를 추출할 때, 뇌는 어떤 정보를 더 우선시로 추출하는가?
        - 컴퓨터 공학 - 카메라가 받아드린 광학적 신호를 어떤 알고리즘과 시스템 아키텍처를 통해서 효율적으로 디지털 이미지/영상을 만들 수 있는가?
        - 수학 - 어떻게 해야 이미지/영상 등에서 패턴을 찾을 수 있을까? 이미지/영상에서 기하학적 정보를 추출하려면 어떻게 해야할까?
        - 공학 - 로봇이 주위 환경을 인식하게 하려면 어떻게 해야할까? 조명, 그림자, 움직임 등이 만드는 이미지/영상에 대한 변화는 어떻게 처리할까?
    - 이러한 **‘시각적 데이터를 처리하는 기법’을 아우르는 학문**이 컴퓨터 비전이 된다고 볼 수 있습니다.

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-72e15bd7-5905-4253-857f-a74a21e9ea95.png?raw=true)

# 2. 그래서 왜 컴퓨터 비전을 배워야하죠?

- 현대 세상에는 카메라가 굉장히 많이 발전하고 엄청나게 많은 곳에서 이미지/영상 데이터가 처리되고 있습니다.
    - 2015 CISCO 조사결과에 따르면, 웹 트래픽의 80%는 이미지/영상 데이터라고 합니다.
    - 통계에 따르면 매 초 Youtube에 업로드되는 비디오의 양은 5시간 분량의 비디오라고 합니다.
        - 생각해보면, 구글(유투브 소유)은 비디오의 내용을 분석해서 유저들의 관심사에 맞는 비디오를 추천해줘야 하는데, 사람이 직접 처리를 해야한다면 물리적으로 불가능한 수준입니다. 이를 해결하려면 **이미지/영상 데이터를 빠르게 처리할 똑똑한 컴퓨터 알고리즘**이 있어야겠죠. 어디서 뭔가 생각나지 않나요...? 아, 이게 컴퓨터 비전이려나요? 😉

# 3. 컴퓨터 비전의 기원

## 고대 동물

- 약 5.43억년 전, 고대 동물들은 시각적 정보를 처리할 수 있는 기관 (눈+시신경)이 없었습니다. 주위의 모든 것을 흡입하는 식, 또는 촉각 등이 발달하면서 먹이가 가까이 오면 흡입하는 식으로 영양분을 섭취했습니다.
- 약 5.40억년 전 쯤, 약 천만년의 기간 동안 점점 동물들에게서 눈이 생겨나는 것을 관찰할 수 있었습니다. 이것은 무엇을 의미할까요?
    - 눈이 있는 동물들은 이제 먹이를 발견해서 쫓을 수 있게 되었고, 또 위협을 감지하고 도망갈 수 있게 되었습니다. 시각적 정보는 생존에 있어 점점 필수적 요소가 되었습니다.
    - 이는 상황인지력과 판단력의 진화를 촉진시켰고, 지성이 있는 동물들의 출현에 큰 기여를 하게됩니다.
        - 실제로 사람의 두뇌의 50%정도는 시각적 정보를 처리하는데 사용됩니다.

## 시각적 정보에 대한 기초적 이해

- 동물의 눈을 따라서 만들다보니, 기본적인 **pinhole camera** 모델 형태의 기계식 ‘카메라’를 만들게되었습니다.
    - Pinhole Camera의 특성으로는 Pinhole → 빛을 모으는 역할, Plane → 이미지를 투영하는 plane이 있습니다.
    - 하지만 시각적 정보를 신호처리 하기 위한 처리 (e.g. 디지털화) 할 기술이 없었습니다.

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-0644ae32-9701-4cc4-a766-d2211fe78aa9.png?raw=true)

- 시간이 지나고 전극을 통한 전기적 신호처리가 가능해졌을 무렵, Hubel & Wiesel (1959) 연구가 진행되었습니다. “동물은 어떻게 시각적 정보를 처리하는가? 어떻게 형태를 인식하는가?”라는 문제에 초점을 두었습니다.
    - 고양이의 primary visual cortex에 전극을 꽂아 특정 모양의 물체에 뇌가 어떻게 반응하는지를 실험하였습니다.
        - 결과는 ‘simple structure (e.g. 방향성을 가진 edge)마다 특정 뇌의 부위가 반응한다’ 였고, 이를 통해 **뇌는 시각적 정보를 edge와 같은 간단한 구조적 정보들 부터 조합해서 계층구조적 인식을 한다**는 것을 알아내었습니다.
            - (필자의 개인적인 생각으로는) 어딘가 뉴럴네트워크와 굉장히 흡사하다고 생각됩니다.
- Hubel & Wiesel의 연구 후로, 이미지를 이해하기 위해서는 형태에 대한 계층구조적 사고가 필요하다는 것을 인지하고, ‘형태’를 표현하는 수학적 이론 연구가 시작되었습니다.
    - 물체의 형태를 수학적으로 표현하기 위해선 어떤 방법이 있을까요? Edge를 그리기? 점으로 표현하기?

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-c401e6f1-539e-447e-8482-e0725bff28d6.png?raw=true)

    - 이러한 고민을 하다가 나타난게 1966년 MIT Summer Vision Project입니다. 여름 프로젝트인데... 지금까지 이어지는... ㅜㅜ

## 컴퓨터 비전의 출현

- David Marr (1970s)의 책 - ‘VISION’에서 **컴퓨터 비전 문제에 대한 정의**와 **문제해결 파이프라인**을 제시했습니다.
    - 컴퓨터 비전 문제란? - Input Image에서 의미있는 정보를 추출하는 것
    - 파이프라인 - 계층구조적 정보를 추출하고 조합해가면서 의미있는 정보를 추출한다

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-f93bca16-aad0-4b82-a370-0790273eea7d.png?raw=true)

- 파이프라인이 정의되면서, 이미지 속 복잡한 형태의 객체 (e.g. 사람, 물체)를 인식하기 위해서는 어떤 하위 계층구조 정보가 필요한지에 대해 연구가 진행되었습니다.
    - 사람의 경우 skeleton화 하여 인식하는 방법, 물체의 경우 edge를 추출하는 방법 등이 제안되었습니다.

![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-ba79b4dc-e9fc-45b6-9859-c022640a7361.png?raw=true)

![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-b563bad7-b3e9-4219-8243-778435fd8d20.png?raw=true)

- 다만 컴퓨터의 성능이 충분하지 않아서, 추상화된 계산만이 가능하였습니다.

## 비-딥러닝 비전의 발전

- 1990년대부터 PC가 발전하면서 컴퓨터 비전 문제에 대해 추상화된 정보부터 하이레벨 계층구조적 계산까지 가능하게 되었습니다. 90년대부터 2012년 딥러닝이 나타나기까지에는 약 **3가지 큰 breakthrough**가 있었습니다.
- 첫째로는, SVM과 Boost와 같은 **머신러닝 기법들이 컴퓨터 비전에 적용**되기 시작했습니다. Viola (2001)의 연구에서는 AdaBoost를 사용해서 실시간 얼굴 검출이 가능해졌습니다.

    ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-57f6c2d9-279a-4d74-8ffe-5713d6caee9e.png?raw=true)

- 두번째로는, 단순히 이미지 속 객체를 읽어내는것을 뛰어넘어, **이미지 전체를 이해하려는 시도**들이 생겼습니다. 오늘날의 Segmentation 기법들이라고 생각하셔도 무방합니다.

    ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-ab90598d-52b2-4c21-88ff-f8ccec0c776e.png?raw=true)

- 세번째로, **특징점 기반 인식 (Feature-based recognition)**이 가능해졌습니다.
    - David Lowe(1999)의 SIFT를 포함하여, SURF, KAZE, AKAZE, FAST 등 여러가지 feature detection 기법들이 생겼습니다. 여기서 feature는, 물체에 대해 어떤 각도나 조명에서 봐도 감지할 수 있는 물체의 특징점을 뜻하며, 이는 이미지에 대해 이해하는데 엄청난 도움을 줍니다.

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-75c7ff52-cec6-4a67-9930-d5b3c8562071.png?raw=true)

## 복잡한 시각적 정보의 이해 + ImageNet Challenge

- Feature detection과 머신러닝을 합하게 되면서 사진 속 객체를 인식해내고 상황을 인지할 수 있게 되었습니다.

![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-82d737b5-50c2-4ec8-a3be-a8762ac52926.png?raw=true)

![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-d30da74c-bff9-4ff4-8ef6-f564ea60e25f.png?raw=true)

- 동시에, 카메라의 성능이 점점 더 좋아지고 더 많은 사람들이 카메라를 사용하게 되면서, **많은 양의 고품질 이미지 데이터가 축적**되었습니다.
    - 머신러닝의 특성 상 좋은 데이터가 많을 수록 더 정확해지는 점을 이용해서 **PASCAL VOC 2007** (Visual Object Challenge)라는 데이터셋이 생겨났고, 이를 이용해서 **'어떻게 더 좋은 이미지 분류 알고리즘을 만들 수 있는가?'에 대한 대회**가 열렸습니다.

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-711d8c57-9155-4bbc-ad7b-c2b7b413cf9d.png?raw=true)

    - 그 후, ImageNet Challenge 라는 더 큰 스케일 (22000개의 객체 카테고리, 1400만개 이미지)의 이미지 분류 대회가 열렸습니다. 더 많은 양질의 데이터를 통해, 훨씬 높은 정확도의 알고리즘이 나타났습니다.

## 딥러닝의 출현

- ImageNet Challenge에는 눈여겨볼 점이 있습니다. 바로 '딥러닝의 출현'입니다.
    - **2012년 Krizhevsky et al의 AlexNet을 필두로 심층신경망 (Deep Neural Network)를 사용한 방식들이 나타나기 시작했습니다.** 심층신경망을 사용한 방식들은, 이전의 방식들 보다 훨씬 적은 에러를 가지고 있었습니다. 2010~2011년에는 약 3%의 에러가 줄어든 반면, 2011~2012년에는 약 10%의 에러가 줄어든 것을 볼 수 있습니다.
    - 2014년에는 **VGG** 네트워크와 **GoogLeNet (Inception v1)**이 또 좋은 성적을 거두었습니다. 그 후 2015년 **ResNet** 네트워크는 **인간의 이미지 분류 성적을 뛰어넘게 되었습니다.**

        ![](https://github.com/ai-robotics-kr/CS231n_study/blob/master/images/lecture1/Untitled-5bde7a40-733f-4666-9af8-6616c94ce48a.png?raw=true)

# 4. CS231n에서는 무엇을 배우나?

CS231n에서는 이미지와 영상을 이해할 수 있는 뉴럴네트워크를 짜는 방법에 대해 공부합니다.  아래 syllabus를 참고하시기 바랍니다.

[Syllabus | CS 231N](http://cs231n.stanford.edu/syllabus.html)

---

---

# 참고하면 좋은 Stanford 수업들

- CS131, 컴퓨터 비전의 기초
- CS224n, 딥러닝 자연어처리
- CS231a, 고급 컴퓨터비전 - 영상처리, 3D 복원, segmentation, 객체인식, 환경인식
- cs231n, 이미지 분류를 위한 뉴럴네트워크 (이 코스입니다!)
- cs331, 자연어처리와 컴퓨터 비전의 융합
- cs431, 뇌과학과 컴퓨터 비전의 관계
