---
title: "Machine Learning에서 Prediction이란 무엇일까?" # Title of the blog post.
date: 2021-10-13T00:07:00+09:00 # Date of post creation.
description: "prediction, inference in machine learning and categories of prediction" # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: true # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
summary: Prediction과 혼용되는 용어들은 무엇이 있을까요? 또, Prediction에는 어떤 종류가 있을까요?
# menu: main
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
mathjax: true
categories:
  - Technology
tags:
  - Prediction
  - Inference
  - Serving
# comment: false # Disable comment if false.
---

머신러닝(Machine Learning, ML) 분야를 러프하게 정의하자면 '데이터를 통해 컴퓨터를 학습시키는 것'이라고 할 수 있습니다. 학습의 목적은 하나입니다. 사람 대신 기계가 예측(Predict)하도록 하는 것입니다. 머신러닝을 사용하면 기존에 사람이 부족해서 못하던 예측을 대신하거나, 사람이 보지 못한 특징들을 이용해 더 나은 예측을 내놓을 수 있습니다.

인간의 추론 능력은 경험(experience)을 통해 학습되는데, ML은 데이터셋이라는 수많은 예측 경험을 통해 기계가 사람처럼 학습하도록 합니다(*Machine learning is the study of computer algorithms that allow computer programs to automatically improve through experience. - Tom M. Mitchell*). 이처럼 ML은 "인간의 추론 과정을 모방"하여 "예측"을 하도록 하는 것이기에 인공지능의 하위분야로 간주됩니다. 그런데 여기에서 '추론(Inference)'이라는 단어와 '예측(Prediction)'이라는 단어는 정확히 어떤 의미를 가질까요?



# Prediction? Inference?

머신러닝을 공부하다보면 일견 비슷해보이는 수많은 단어들이 실제로는 다른 뜻을 지닌 경우가 있습니다. 입문자들도 자주 접했을 단어인 Prediction과 Inference는 비슷한 듯 하지만 차이가 있는 단어입니다. 머신러닝 분야가 비교적 최근에 많이 다뤄지는 분야라 표준이 없다 보니, 다양한 article과 프레임워크에서 두 용어가 혼용되곤 합니다. Prediction과 Inference는 어떤 점에서 같고, 어떤 점에서 다를까요?



## Inference, Estimation, Prediction

먼저 Inference를 위시한 ML의 많은 단어들이 통계학적 용어로부터 파생되었다는 것을 알 필요가 있습니다. 통계학적으로 Inference라는 용어는 아주 넓은 의미를 가지고 있는데, Wikipedia에서는 다음과 같이 설명하고 있습니다.

> *Inference is the act or process of deriving logical conclusions from premises known or assumed to be true.*
> ...
> *Statistical inference uses mathematics to draw conclusions in the presence of uncertainty.*

알려진 전제들 혹은 참이라고 추정되는 것들(*premises known or assumed to be true*)는 ML에서 곧 데이터셋에 해당합니다. 즉, Inference는 데이터셋을 이용해 끌어낸 conclusion들을 모두 일컫는 광범위한 말입니다. Inference의 하위집합(subset)으로는 Estimation과 Prediction이 있습니다.

통계학적으로 Estimation은 $f_*: X -> P(Y)$가 있을 때 $f \approx f_*$인 $f_*$를 찾는것을 의미합니다. 여기서 X를 집어넣으면 Y가 나오는 $f$는 Input과 Output의 정확한 관계를 의미합니다. ML에서는 이 자연상태(state of nature)의 관계를 명확히 알아내는 대신 근사치를 알아내는 정도로 만족합니다. 즉, 해당 관계를 잘 재현할 수 있는 모델 weight를 찾겠다는 의미입니다. **ML에서 일반적으로 말하는 Inference는 Estimation을 의미합니다.**

이와 달리 Prediction은 새로운 데이터인 $X_{new}$에 대하여 label $Y_{new}$를 찾는 것을 일컫습니다. 따라서 ML에서 Prediction의 목적은 모델의 weight를 찾는게 아니라, 정확한 결과값 $Y_{new}$를 찾는것입니다.



## ML에서의 공통점

- 데이터를 모델에 입력하여 output을 얻음

## ML에서의 차이점

- **Inference(Estimation)** : 입력과 출력의 관계를 잘 묘사하는 모델을 만들기 위해, 어떤 feature가 output에 큰 영향을 끼쳤는지 밝혀내는 것. 즉, **변수와 결과간의 관계를 규명하려고 함**
- **Prediction** : 만들어진 모델을 이용하여 지금까지 학습한 적 없던 새로운 데이터에 대하여 정답(label)을 찾아내는 것. 즉, **목적이 정답을 맞추는 것**



![prediction vs inference in machine learning & statistics](https://d33wubrfki0l68.cloudfront.net/478f2689f1b9903ce2feed61a1f5e9c9deb2bcc9/55b03/post/commentary/inference-vs-prediction_files/figure-html/unnamed-chunk-1-1.png)

따라서 Inference(Estimation)과 Prediction의 차이는 목적에 있습니다. 기존의 통계학적 관점에서는 가설을 검증하고 원인을 규명하기 위해 Inference를 Prediction보다 중요하게 여깁니다. 그러나 머신러닝/딥러닝에서는 Prediction을 Inference보다 강조하곤 합니다. 물론 모델이 정확한 예측을 내놓고, 예측의 근거에 대해 설명을 잘 할 수 있다면야 좋을 것입니다. 실제로 머신러닝에서 이러한 분야를 다루는 `Interpretable AI`,  `Explainable AI` 같은 연구 분야들이 존재합니다. 해당 연구분야에서는 Input이 Output으로 어떻게 연결되는지 확인하기 위해 모델이 주목하는 위치를 heatmap으로 찍어보는 [GradCAM](https://arxiv.org/pdf/1610.02391.pdf)같은 시도들을 수행합니다.

그러나 일반적으로 AI 서비스를 만들 때, 모델의 명확한 예측 근거보다는 (블랙박스 모형이더라도) 정확도가 중요한 경우가 더 많습니다. 예를 들어 넷플릭스에서 새로운 영화를 추천 받을 때, 지금까지 본 시청기록 하나하나를 짚어주며 추천의 근거를 알기 원하는 이용자는 별로 없을 것입니다. 그보다는 첫 화면에 좋아할법한 영화들을 가득 그러모아 보여주는 것이 더 매력적입니다.

다만 위에서도 언급했듯 다양한 자료에서 Inference와 Prediction을 혼용하고 있기 때문에, 무조건 다른 단어로 생각하기보다는 맥락 상 같은 단어로 사용하고 있는지 고민해볼 필요가 있습니다.



# Prediction의 종류

AI 서비스를 만들기 위해서는 학습된 모델을 이용하여 Prediction을 제공해야합니다. 다만 Prediction을 하는 방식은 서비스 이용자의 요구사항에 따라 달라집니다. 어떤 가치에 중점을 두고 서비스를 제공하느냐에 따라, 같은 모델이라도 다른 방식으로 사용할 수 있습니다. 따라서 서비스의 특징에 따라 적절한 Prediction 방식을 선택할 수 있어야 합니다.



아직까지 명확하게 업계에서 통용되는 분류법은 없지만, 다음과 같은 분류들이 쓰이곤 합니다.

## Offline Prediction

로컬 머신에서 Input 데이터를 받아 Prediction을 수행한 뒤, 이 결과를 csv같은 파일로 저장하는 방식입니다. 모델을 Serving할 필요도 없고, Prediction을 여러 번 수행할 필요도 없을 경우 사용합니다. 즉, 특정 이벤트에 대해서 새로운 모델을 만들어서 예측하고 폐기하는 일회성 task에 사용됩니다. 일반적으로 서비스에 자주 사용되는 방식은 아닙니다. 

- 재예측 할 일이 없는 경우
- 한번에 대량의 예측을 수행
- 한번에 하나의 머신만 가지고도 training과 predict 가능



### 사용 사례

선거 결과 예측, 스포츠 경기 결과 예측



## Batch Prediction

주어진 데이터에 대해 미리 연산해놓았다가, 유저의 요청이 들어오면 미리 연산해둔 결과를 DB에서 꺼내와서 보여줍니다. 서비스를 만드는 경우 오프라인에서 모델 예측을 수행할 때 일반적으로 이 방식으로 수행하므로, 이 방식을 Offline Prediction이라고 부르기도 합니다.

이 때, Prediction 결과는 Model의 학습이 어떻게 되었느냐, 데이터의 분포가 어떻느냐 등에 따라 달라질 수 있으므로, 시간이 지나면 자연스럽게 달라져야하는 값입니다(model drift, data drift). 따라서 주기적으로 데이터 포인트에 대해 일괄 예측을 수행하여 DB에 저장된 결과값을 갱신합니다. 이처럼 주기성으로 (전체 데이터를) 일괄 예측하므로 Batch Prediction이라는 이름이 붙었습니다.

이 방식을 사용하면, 유저의 요청에 응답하는 과정에서 실시간 Prediction이 필요하지 않고, DB에서 바로 꺼내서 가져오므로 시간이 크게 줄어듭니다. 다만, 이전 Prediction 이후부터 다음 Prediction까지는 결과값을 갱신할 수 없고, 정해진 몇몇 데이터의 경우에만 수행할 수 있다는 한계점을 가집니다.

- 직전까지의 User Action같은 실시간 온라인 정보가 필요하지 않은 경우
- 너무 긴 inference time이 드는 경우나, 극단적으로 낮은 latency가 필요한 경우

- 하나의 머신으로도 가능하고, 여러개의 머신으로도 가능



### 사용 사례

분기별 소득 예측, 마케팅을 위한 고객 타겟팅(마케팅 캠페인)



## Real-time(Online) Prediction

서비스 유저가 서버에 요청할 시, Online 데이터를 이용하여 즉시 Prediction를 수행하고 결과를 반환합니다. 요청할 때마다 Prediction이 새로 수행되므로 현재 컨텍스트가 즉시 반영되고, 서비스 유저는 실시간으로 변경되는 피드백을 받는 경험을 할 수 있습니다.

다만 Inference Time이 고려되므로, 이 방식의 경우 유저가 응답을 받는 데에 걸리는 latency를 최소화해야합니다. 이를 위해 모델 가속화(acceleration)나 모델 경량화같은 테크닉을 사용할수도 있습니다. 또, 유저 요청에 model을 실시간으로 prediction하는 동안은 머신을 training에 사용할 수 없으므로, training만을 위한 다른 머신이 필요합니다.

- 일반적으로 유저가 API 콜을 통하여 요청하며, 요청 즉시 예측
- 낮은 latency가 중요한 구현 과제
- Training과 Serving을 위한 서로 다른 머신이 있어야함



### 사용 사례

온라인 학습 피드백, 장바구니 물품을 기준으로 품목 추천하기



## Streaming Prediction

Streaming Prediciton은 Real-time Prediction과 거의 비슷하지만, 모델에 인입되는 데이터의 수와 수행해야하는 prediction의 빈도가 훨씬 큽니다. Real-Time Prediction이 하나의 request message를 받는다면, Streaming Prediction은 Input으로 Stream 형태의 지속적인 요청을 받습니다.

workload가 크면서도 정확한 prediction을 위해 데이터가 유실되면 안되므로, 일반적으로 kafka같은 메시지브로커를 사용하여 처리합니다.

음성메시지 같은 경우 Input을 Window 단위로 잘라서 일정 빈도로 지속적으로 모델에 입력하는 등, 데이터의 특징에 따라 다양한 구현방식이 존재합니다. latency가 좀 있을 수 있지만, on-demand이면서 대량 데이터를 처리한다는 점에서 독특한 특징을 가집니다. 



- Stream형 데이터를 처리하므로 Real-time에 비해 workload가 큼
- 데이터 유실을 막기 위해 kafka같은 메시지브로커를 사용하여 처리하는 것이 일반적
- on-demand면서 대량의 데이터를 처리



### 사용 사례

실시간 불량품 감지, 체온 감지



## 연산 시점에 따른 prediction 분류

연산시점에 따라서 새로운 요청으로 Input이 들어오기 전에 **미리 연산을 해두는 경우**는 `Offline Prediction`, 요청이 들어왔을 때 **on-demand하게 연산을 수행하는 경우**는 `Online Prediction`으로 분류하기도 합니다. 위에서 언급했던 분류 기준으로 보자면 Offline Prediction과 Batch Prediction은 Offline으로, Real-time Prediction과 Streaming Prediction은 Online으로 생각할 수 있습니다.



# Prediction과 Serving의 차이

Prediction은 (학습에 사용하지 않은) new datapoint를 모델의 input으로 하여 output을 얻어내는 행위를 의미한다면, **Model Serving**은 <u>**서비스 유저 입장에서 편리한 Prediction이 가능한 상태로 모델을 배포하는 것**</u>을 의미합니다. 여기에서 편리하다고 하는 것은, 일반적으로 통용되는 API Endpoint 형태로 두어 이를 처리하는 서버를 배포하는 방식으로 request만 보내면 모델을 쉽게 사용할 수 있도록 만들어두는 것을 말합니다.

예를 들어 Jupyter Notebook으로 코드를 짜둔 뒤, input으로 임의의 string을 입력하여 셀을 수행하면 output이 나오게 설계했다고 할 때, 보통은 이걸 serving이라고 부르지 않습니다. 유저가 코드레벨을 알아야 할 뿐더러, URI 요청이나 (쉽게는 버튼 클릭같은) 단순한 interaction으로 요청을 보낼 수 없는 구조이기 때문입니다. 

위에서 언급했던 Prediction 중 어느 것을 사용할 것인가에 따라 Model Serving을 해야하는지 여부를 결정할 수 있습니다. 예를 들어 사용자가 커머스에 들어온 후로 계속 주방식기와 관련된 페이지만 접근하고 있다고 생각해봅시다. 이용자의 페이지 접근 데이터(혹은 검색기록 등)를 이용해 광고효율을 극대화하려고 할 때, 최근의 접근기록을 활용하기 위해서는 Real-time Prediction을 수행해야합니다. 이 경우 몇초 전에 들어온 새로운 데이터들에 대하여 모델의 output을 구해야하기 때문에, 바로바로 request를 보내어 Prediction을 수행할 수 있도록 모델이 온라인 상에 Serving되고 있어야합니다. 그러나 기업의 분기별 매출액을 예상하는 모델은 각 분기마다 돌리면 되는데 굳이 Online Serving할 필요가 없겠죠.



## Offline Batch Serving이라는 용어도 있던데?

`Offline Batch Serving`이라는 용어는 BentoML 오픈소스에서 사용하고 있는 용어인데, docs를 보시면 다음과 같이 나와있습니다.

> *BentoML CLI allows you to load and run packaged models straight from the CLI without needing to start up a server to serve requests (hence offline batch serving)*

여기서 `Offline Serving`이라고 한 이유는 매번 request를 처리하는 서버를 deploy하지 않고도 편리한 request 형태로 model prediction을 지원하기 때문입니다. 코스트를 들여 Offline Serving 기능을 굳이 지원하지 않더라도 모델만 있으면 그냥 Offline Prediction을 할 수 있기 때문에, 프레임워크가 굳이 이를 지원할 필요성이 떨어집니다.

따라서 `Offline Serving`이라고 하는 것은 일반적으로는 잘 사용되지 않는 말입니다(로컬에서만 request를 받겠다는 건데, 이건 모델을 deploy하지 않으므로 불특정 다수의 유저에게 서비스를 제공하지 않는다는 말입니다). 어느정도 (BentoML만의) 마케팅적인 용어라고 생각할 수 있습니다. 



다만 `Batch Serving`은 이와 별개로 (다량의) request를 묶어서 일괄 예측한다는 점에서 Batch Prediction과 어느정도 맥락을 같이 하며, 실제로도 통용되는 용어입니다.



# Reference

https://towardsdatascience.com/navigating-ml-deployment-34e35a18d514
https://cloud.google.com/architecture/minimizing-predictive-serving-latency-in-machine-learning?hl=ko
https://www.coursera.org/lecture/managing-data-analysis/inference-vs-prediction-xKjFf
https://stats.stackexchange.com/questions/6/the-two-cultures-statistics-vs-machine-learning
https://stats.stackexchange.com/questions/17773/what-is-the-difference-between-estimation-and-prediction
https://stats.stackexchange.com/questions/130867/inference-vs-estimation