# Presidio란?
# PII(Personal Identifiable Information) entity
를 감지하고 이를 마스킹할 수 있게 해주는 모듈이다. 아래와 같은 방식으로 작동한다.
![[detection_flow.gif]]
- Regex: 정규표현식으로 패턴 분석 및 파악
- NER: NL을 분석하여 엔티티 파악(이름, 주소 등등)
- Checksum: 패턴 확인
- Context Words: 맥락을 파악해서 인식을 확실하게 함
- Anonymization: 파악된 민감한 개인정보를 cesnsoring함.
