## [원본 링크](https://aws.amazon.com/ko/what-is/transformers-in-artificial-intelligence/#:~:text=Transformers%20are%20a%20type%20of,tracking%20relationships%20between%20sequence%20components.)
# Transformer?
트랜스포머란 NN 아키텍쳐에서 입력 스퀸스를 출력 시퀸스로 변환하거나 변경하는 일종의 신경망 아키텍쳐임

## 왜 쓰는가?
NLP 작업에서 광범위하게 초점을 맞춘 것이 딥러닝 모델임. 초기 모델은 이전 단어를 기반으로 다음 단어를 순서대로 추측했었음.

초기 기술에서는 특정 입력 길이 이상 컨텍스트를 유지할수가 없었음.  이전 단어만을 기반으로 다음 단어를 추측하다보니 이전에 나왔던 단어들의 관계와 연관성을 기억했어야 했기 때문. I am fine 을 생성한다면 am 다음에 fine을 생성하는데, fine day isn't it?을 생성하는 알고리즘도 있으면 I am fine day isn't it ? 이런식으로(극단적인 예시이다) 맥락이라는게 존재하지 않는 글이 사용되니깐 문제였음

transformer 모델은 이런 장거리 종속성을 처리할 수 있도록 함으로써 NLP 기술을 근본적으로 변환시켰음.

### 대규모 모델 지원
transformer는 병렬 연산을 통해 긴 시퀸스 전체를 처리하므로 훈련 시간과 처리 시간이 크게 단축됨. 이를 통해 복잡한 언어 표현을 학습할 수 잇는 GPT & BERT와 같은 LLM의 학습이 가능해짐.

# 작동방식
- 데이터 시퀸스를 처리하는 기존의 신경망은 인코더/디코더 아키텍쳐 패턴을 사용하는 경우가 많음.
	- 인코더는 영어 문장과 같은 전체 입력 데이터 시퀸스를 읽고 처리하여 간결한 수학적 표현으로 변환
	- 입력의 본질을 포착하는 요약임
	- 