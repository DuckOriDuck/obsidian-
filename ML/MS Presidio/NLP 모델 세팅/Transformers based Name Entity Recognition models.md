Presidio의 `TransformersNlpEngine`은 spaCy NER 컴포넌트 대신 Huggingface Transformers model를 캡슐레이팅하는 spaCy 파이프라인으로 구성돼있다.

Presidio를 활용한 NER result
![[Pasted image 20240801144158.png]]
## 새로운 모델 추가
underlying transformer 모델임에서 커스텀 모델 vs 공개된 pretrained 모델중에서 고를 수 있음

### pre-trained model 다운로드

우선 원하는 NER 모델을 허깅페이스에서 다운로드받는다.
```python
import transformers 
from huggingface_hub import snapshot_download 
from transformers import AutoTokenizer, AutoModelForTokenClassification 
transformers_model = <PATH_TO_MODEL> # e.g. "obi/deid_roberta_i2b2"

snapshot_download(repo_id=transformers_model)

# Instantiate to make sure it's downloaded during installation and not runtime
AutoTokenizer.from_pretrained(transformers_model) 
AutoModelForTokenClassification.from_pretrained(transformers_model)
```
그리고, spaCy pipeline/model을 다운받는다.
```python
python -m spacy download en_core_web_sm
```
**configuration 파일 생성**
모델이 다운로드 됐으면 YAML config파일을 생성한다. configuration은 `spaCy` 파이프라인 이름이나 transformer 모델 이름을 보유하고 있어야한다. 추가적으로, 다른 결과를 parsing하는 트랜스포머 모델이 추가될 수 있다.
```python
nlp_engine_name: transformers
models:
  -
    lang_code: en
    model_name:
      spacy: en_core_web_sm
      transformers: StanfordAIMI/stanford-deidentifier-base

ner_model_configuration:
  labels_to_ignore:
  - O
  aggregation_strategy: simple # "simple", "first", "average", "max"
  stride: 16
  alignment_mode: strict # "strict", "contract", "expand"
  model_to_presidio_entity_mapping:
    PER: PERSON
    LOC: LOCATION
    ORG: ORGANIZATION
    AGE: AGE
    ID: ID
    EMAIL: EMAIL
    PATIENT: PERSON
    STAFF: PERSON
    HOSP: ORGANIZATION
    PATORG: ORGANIZATION
    DATE: DATE_TIME
    PHONE: PHONE_NUMBER
    HCW: PERSON
    HOSPITAL: ORGANIZATION

  low_confidence_score_multiplier: 0.4
  low_score_entity_names:
  - ID
```
- `model_name.spacy` : transformers NER model를 래핑할 spaCy model/pipeline 이름이다. For example, `en_core_web_sm`.
- `model_name.transformers` : huggingface model에 대한 full path이다.  [HuggingFace Models Hub](https://huggingface.co/models?pipeline_tag=token-classification) 에서 `obi/deid_roberta_i2b2`와 같은 모델을 찾을 수 있다  

The `ner_model_configuration` section contains the following parameters:

- `labels_to_ignore`: A list of labels to ignore. For example, `O` (no entity) or entities you are not interested in returning.
- `aggregation_strategy`: The strategy to use when aggregating the results of the transformers model.
- `stride`: The value is the length of the window overlap in transformer tokenizer tokens.
- `alignment_mode`: The strategy to use when aligning the results of the transformers model to the original text.
- `model_to_presidio_entity_mapping`: A mapping between the transformers model labels and the Presidio entity types.
- `low_confidence_score_multiplier`: A multiplier to apply to the score of entities with low confidence.
- `low_score_entity_names`: A list of entity types to apply the low confidence score multiplier to.