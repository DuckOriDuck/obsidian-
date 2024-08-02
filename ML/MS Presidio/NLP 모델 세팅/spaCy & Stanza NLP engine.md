
# Public pre-trained scaCy/Stanza model 사용

## pre-trained model 다운로드
- spaCy를 가진 모델 다운로드
```
python -m spacy download es_core_news_md
```
여기서는 스페인어를 위한 중간크기 모델을 다운로드함.

- Stanza를 가진 새로운 모델
```python
import stanza
stanza.download("en")
```

![[Pasted image 20240801143358.png]]
모델 트레이닝 관련 링크

- [Train your own spaCy model](https://spacy.io/usage/training).
- [Train your own Stanza model](https://stanfordnlp.github.io/stanza/training.html).
앱이 이미 존재하는 spaCy NLP 파이프라인을 로딩하고 있다면, presidio에서 다시 로드해서 재사용될 수 있다.