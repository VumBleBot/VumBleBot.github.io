---
title: "Emotion Detection(감성 분석) Overview" # Title of the blog post.
date: 2021-10-11T00:10:48+09:00 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: true # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
summary: 감정 분석(Sentiment analysis)은 주어진 텍스트를 이해하여 이에 내재된 작성자의 감정 방향(Polarity)을 식별해내는 작업입니다.
# menu: main
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
mathjax: true
categories:
  - Technology
tags:
  - Tag_name1
  - Tag_name2
# comment: false # Disable comment if false.
---

# Intro
감정 분석(Sentiment analysis)은 주어진 텍스트를 이해하여 이에 내재된 작성자의 감정 방향(Polarity)을 식별해내는 작업입니다. 감정 분석에는 여러가지 종류가 있을 수 있지만 우리가 일반적으로 알고 있는 감정 분석의 경우 대부분 이진 분류(긍정, 부정) 문제로 취급됩니다. 좀 더 어려운 문제로써 5개의 라벨(강한 긍정, 약한 긍정, 중립, 약한 부정, 강한 부정)을 사용할 수도 있습니다.

하지만 방금 말한 "어려운 문제"를 다룬다고 해도 5개 클래스의 면면을 살펴보면 아직도 이 task가 현실과는 동떨어져 보일 수 있습니다. 사람의 감정은 단순히 긍정과 부정으로 나눌 수 있는 것이 아니고 행복, 놀람, 슬픔, 분노, 우울 등의 다양한 형태로 존재하기 때문입니다. 

물론 지금 하는 이야기가 Sentiment analysis가 무용지물이라는 것은 아닙니다. Sentiment analysis도 그 존재에 대한 고유의 의도와 목적이 뚜렷이 존재합니다. 또한 이 문제를 푸는 데에 쓰이는 방법들은 클래스를 표현하는 단어들에서 풍기는 느낌과는 다르게 전혀 단순하지 않습니다.

다만 만약 우리가 앞서 언급한, 사람의 디테일한 "**감성**"을 다루고자 한다면 그 용어가 조금 달라지는데, 이 task는 **Emotion detection 혹은 Emotion classification**이라고 표현합니다. 사실 이게 정말 용어에 따라 task가 뒤바뀌는가라고 물으신다면 확답을 드리기는 어렵습니다. 단어가 혼용되는 경우도 상당히 많고 그렇다고 각 단어별로 여러 매체 정보들이 확실하게 구분되어 나타나는 것도 아니기 때문입니다. 

"감정(Sentiment)"과 "감성(Emotion)"의 그 미묘한 차이를 확실히는 모르겠지만, 중요한 것은 경향성만 따지자면 구글링을 하기에는(또한 여러 논문의 제목들도) Emotion이라는 키워드가 우리가 원하는 task에 더 적합한 키워드라는 점입니다. 아무튼 앞서 언급한 점들로 말미암아 지금부터는 "감성 인식(Emotion detection)" 문제를 푸는 방법과 관련 데이터셋에 대해 알아보도록 하겠습니다.


# Method
감정 분석(Sentiment analysis)에 대한 시도와 연구가 시작된지 매우 오래 되었기 때문에 솔루션도 그만큼 다양하게 존재합니다. Rule-based의 고전적인 방법부터 시작하여 Feature-based method(Logistic regression, SVM), Embedding-based method(FastText, Flair) 등이 존재합니다. 물론 대세는 Neural network(Deep learning) 기반 방법입니다.

애석하게도 감성 인식(Emotion detection) task는 자연어에 대한 연구보다는 이미지에 대한 연구, 특히 Real-time facial emotion detection에 대한 연구가 훨씬 많습니다. 보통 이미지에 대한 감성 인식 task의 이름은 Emotion recognition으로 통용됩니다.

task specific한 사전 지식 없이 생각해보았을때 감성 인식도 아마 어텐션 기반의 딥러닝 모델, 즉 BERT나 GPT 기반의 모델들이 잘 해낼 수 있을 것입니다. 일반적인 방법을 생각해본다면 역시 여타 다수의 NLP 태스크와 동일하게 앞단에 large corpus로 학습된 pre-trained model을 놓고 뒷단에 classifier head를 놓으면 됩니다. 

이제 Emotion detection을 주제로 다룬 논문 2가지만 간단하게 살펴보도록 하겠습니다. 

## Casting Multi-label Emotion Classification as Span-prediction
[Hassan Alhuzali, Sophia Ananiadou (EACL 2021)](https://aclanthology.org/2021.eacl-main.135.pdf)  
**한 문장에 하나가 아닌 여러 개의 emotion이 내재되어있다라는 것을 전제로 하고** 이에 적합하게 사용될 수 있는 model과 loss 함수를 제시한 논문입니다. 예를 들어 '내일이 정말 기대돼!'라는 문장에는 '행복'과 '설렘'이라는 두 가지 감성이 공존할 수 있을 것입니다.

제시된 모델(**SpanEmo**)은 아래와 같이 Input으로 클래스 전체와 Sentence를 함께 넣고 BERT와 Feed Forward Network(FFN)을 차례대로 통과시켜 각 position별로 single score를 도출합니다. 우리가 주목해야할 곳은 label의 position입니다. 해당 position에서 나온 output들에 sigmoid를 씌워 최종적으로 정답인 클래스들을 도출해냅니다. 

![](https://i.imgur.com/iudM7za.png)

식으로 쓰면 아래와 같습니다.

\$$
\text{H}_i = \text{Encoder}([\text{CLS}] + |\text{C}| + [\text{SEP}] + \mathrm{s}_i) \\
\hat{\mathrm{y}} = \text{sigmoid}(\text{FFN}(\mathrm{H}_i))
\$$

저자는 이와 같은 구조의 모델이 각 emotion과 sentece의 각 word간 연관 관계를 더 효과적으로 파악할 수 있다고 제시합니다.

한편 이 논문에서는 **Label-Correlation Aware (LCA) Loss**라는 새로운 구조의 loss도 제시합니다.

\$$
\mathcal{L} _\text{LCA} (\text{y}, \hat{\text{y}}) = \frac{1}{|\text{y} ^0|| \text{y} ^1|} \sum _{(p, q) \in \text{y} ^0 \times \text{y} ^1} \text{exp} (\hat{\text{y}} _p - \hat{\text{y}} _q)
\$$

> $\text{y}^0$은 negative label의 집합, $\text{y} ^1$은 positive label에 대한 집합을 나타냅니다. 예를 들어 혐오, 공포, 우울 등은 $\text{y}^0$에, 기쁨, 행복, 즐거움 등은 모두 $\text{y}^1$에 해당됩니다. $\hat{\text{y}}_k$는 prediction vector $\hat{\text{y}}$의 $k$번째 entry를 나타냅니다. 즉, $k$번째 class에 대한 확률값입니다. 

식을 보면 positive label들과 negative label간의 거리를 maximize하는 방향으로 학습이 이루어지는데, 다시 말해 **model이 공존(co-exist)할 수 없는 class를 동시에 맞다고 예측할 수 없도록 패널티를 부여**하는 식이라고 보면 됩니다. 

최종적인 loss 함수(objective)는 아래와 같습니다.

\$$
\mathcal{L} = (1 - \alpha) \mathcal{L} _{\text{BCE}} + \alpha \sum _{i=1} ^{M} \mathcal{L} _{\text{LCA}}
\$$

BCE는 Cross entropy loss이고 이를 LCA와 결합하여 사용합니다. $\alpha$는 가중치 값으로 논문에서는 실험을 통해 나온 최적값 $\alpha = 0.2$를 사용합니다. BCE는 풀어서 보면 binary cross entropy인데, 초기 목적이 이진 분류라 앞에 binary라는 말이 붙었으나 multi-label classification에도 이 함수를 사용할 수 있습니다. ([참고글](https://cvml.tistory.com/26))

## GoEmotions: A Dataset of Fine-Grained Emotions (2020)
[Demszky, Dorottya and Movshovitz-Attias, Dana and Ko, Jeongwoo and Cowen, Alan and Nemade, Gaurav and Ravi, Sujith (ACL 2020)](https://aclanthology.org/2020.acl-main.372.pdf)

Google에서 발표한 논문으로, 사실 모델보다는 새로운 dataset인 'GoEmotions'를 냈다는 데에 더 큰 의의가 있는 논문입니다. Reddit에서 크롤링한 문장으로 58,000 가량 되는 크기의 데이터셋을 만들었으며 class는 28개([아래 파트](#GoEmotions-Korean)에서 후술)입니다. 데이터 생성에 관한 내용, 즉 Annotation 방법과 데이터 가공 방법, 결과 데이터 분포에 대하여 여러 detail이 있지만 관련 내용은 생략하도록 하겠습니다.

여기서 사용하고 있는 모델의 특징은 multi-label classification 방법을 적용하고 있다는 점, bi-directional LSTM과 BERT-base를 사용하고 있다는 점입니다. 모델링 자체는 일반적인 BERT/LSTM 모델과 큰 차이가 없습니다.

# Metric
현재 다루고 있는 감성 인식도 일종의 분류(classification) 문제이기 때문에 만약 단순한 분류 문제로 간다면 **F1 score(macro)를** 사용해도 전혀 지장이 없을 것입니다. 우리가 절대 틀리지 말아야 할 클래스가 존재한다면 다른 metric을 고려해볼 수 있겠으나 현재로서 그런 클래스는 없을 것으로 예상됩니다. 또한 사용하는 dataset 내에 class imbalancing 문제가 크게 두드러지지 않는다면 F1-score의 대안으로써 **Accuracy**를 고를 수 있습니다.

다만 앞서 제시한 논문들처럼 **multi-label classification으로 가게 된다면** 여러 metric이 존재할 수 있는데 만약 저희가 이 방향으로 프로젝트를 진행할 경우 metric에 대한 글을 따로 작성하도록 하겠습니다. 

# 한국어 감성 인식 데이터셋
감성 인식을 한다고 해도 감정 분석(Sentiment analysis) 데이터셋을 사용할 수도 있기 때문에 이를 글에서 아예 배제하지는 않았습니다. 물론 후술할 감정 분석 데이터셋들도 주로 긍정/부정 분류를 목적으로 하는 데이터셋들이고, 혐오 표현 분류 등 저희와 연관이 크게 없는 데이터셋들은 담지 않았습니다.

## [Naver Sentiment Movie Corpus(NSMC)](https://github.com/e9t/nsmc/)

<!-- ![](https://i.imgur.com/xox9AAT.png) -->
### 특징
영화당 100개의 리뷰, 총 200,000개의 리뷰

### 클래스
긍정(0), 부정(1)

### 기타
- 공개 배포
- 분류하고자 하는 감정이 더욱 구체적인 감정(웃김, 신기함, 화남, 귀찮음 등)이라면 한계가 명확한 데이터셋
- (참고) [쇼핑 리뷰 기반의 자매품](https://github.com/bab2min/corpus/tree/master/sentiment)도 존재합니다. 개인적인 생각으로는 영화 리뷰 데이터셋보다 활용성이 떨어진다고 생각하여 따로 소개하지는 않겠습니다.

## [감성 분석 말뭉치 2020(모두의 말뭉치)](https://corpus.korean.go.kr/main.do#down)

### 특징
총 2,081개의 리뷰 데이터(제품, 영화, 여행), 긍정/부정으로 나누지만 앞서 NSMC와 다르게 강한 긍정, 강한 부정, 중립 등이 존재

### 클래스
강한 부정(-2), 부정(-1), 중립(0), 긍정(1), 강한 긍정(2)

### 기타
- 신청 필요 / 무료 배포
- 긍정/부정의 세기를 평가하기 위한 데이터셋, NSMC에서와 마찬가지로 다른 여러 감정을 분류하기에는 한계가 있는 데이터셋입니다.

## [한국어 감정 정보가 포함된 단발성 대화 데이터셋(AIHub 외부)](https://aihub.or.kr/opendata/keti-data/recognition-laguage/KETI-02-009)

### 특징
SNS 및 온라인 댓글에 대한 크롤링으로 데이터 수집, 다분류 감정에 대한 데이터셋. 총 38,594문장, 문장 당 글자수는 약 10~40자

### 클래스
기쁨, 슬픔, 놀람, 분노, 공포, 혐오, 중립

### 공개 여부 및 기타
- 신청 필요 / 연구 용도에 한하여 공개, 상업적 용도는 계약 필요
- (참고) 같은 클래스를 가진 [음성 데이터](https://aihub.or.kr/opendata/keti-data/recognition-laguage/KETI-02-002)도 존재합니다.

## [감성 대화 말뭉치(AIHub 개방)](https://aihub.or.kr/aidata/7978)

### 특징
대화 기반의 감성 인식, 60개의 감정 클래스. 인물의 페르소나(인물 정보), 상황, 질병 유무 등 다양한 상황적 배경에 대화 데이터가 들어감. 음성 15,700문장 + 코퍼스 270,000문장

### 클래스
![](https://i.imgur.com/cxFFfQo.png)
데이터셋 구축의 목적이 우울증 및 정신건강 관련 내용이라 부정적인 감정의 라벨이 다소 많습니다.

### 기타
- 신청 필요 / 무료 배포
- 순수한 감정 분석을 위해 만들어진 데이터셋은 아니므로 2차 가공이 필요합니다.
- 데이터 형태가 다소 복잡하므로 들어가서 직접 문서를 보는 것을 권장합니다.
- 예시
![](https://i.imgur.com/vbAc5KD.png)

## [GoEmotions-Korean](https://github.com/monologg/GoEmotions-Korean)

### 특징
앞서 언급한 [GoEmotions](https://github.com/google-research/google-research/tree/master/goemotions)을 한국어로 번역한 데이터셋입니다. 원본 데이터셋은 Reddit comments를 가공한 것으로 클래스는 28개(27+Neutral), 총 데이터 수는 58,009개입니다.

### 클래스
admiration, amusement, anger, annoyance, approval, caring, confusion, curiosity, desire, disappointment, disapproval, disgust, embarrassment, excitement, fear, gratitude, grief, joy, love, nervousness, optimism, pride, realization, relief, remorse, sadness, surprise, neutral

### 기타
- 무료 배포
- 영어를 한국어로 번역하여 사용하는 데이터이기 때문에 실제 사람들이 쓰는 한국어와 다를 수 있습니다.
- README에도 데이터셋의 품질이 낮을 수 있다는 경고가 쓰여있습니다.


<!-- 

감정분석(Sentiment analysis) 총정리
https://github.com/declare-lab/awesome-sentiment-analysis


-->