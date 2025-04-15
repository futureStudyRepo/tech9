# 📘 자연어 처리 분류 모델의 발전사

자연어 처리는 인간 언어를 컴퓨터가 이해할 수 있도록 하는 기술입니다. 이 노트북은 감정 분석, 뉴스 분류, 댓글 분석 등 텍스트 분류를 중심으로 역사적 흐름과 실제 구현 예제를 담고 있습니다.

---

## 🧭 개요

> 예시:  
> - "와 너무 행복해" → **기쁨**  
> - "진짜 짜증난다" → **분노**

텍스트 분류는 초기 규칙 기반에서 시작하여 딥러닝, Transformer로 발전해왔습니다.


## 🧩 1. 원핫 인코딩 / BoW / TF-IDF

### 📖 이론
- **원핫 인코딩**: 단어 하나를 고유한 벡터로 표현 (1 외 나머지는 0)
- **BoW (Bag of Words)**: 단어의 존재 여부 또는 등장 빈도 벡터화
- **TF-IDF**: 단어 빈도에 역문서 빈도를 곱하여 중요 단어 강조

> ❌ 단점: 의미 정보 손실, 차원 수가 많음


## 🧬 2. Word2Vec / GloVe 임베딩

### 📖 이론
- **Word2Vec** (Mikolov, 2013): 의미 기반 임베딩
- CBOW / Skip-gram 구조
- 유사한 의미 → 유사한 벡터

> ✅ 장점: 의미 반영 가능  
> ❌ 단점: 문장 전체 표현 불가


## 🧠 3. LSTM 기반 분류기 (간단 구조)

> - 단어 순서 반영  
> - 문맥 이해 가능  
> - 긴 문장에서 gradient 소실 문제 있음

(여기선 생략 또는 PyTorch로 구성 가능)


## 🔮 4. Transformer 기반 분류 (BERT)

### 📖 이론
- BERT: 양방향 문맥 고려
- 사전학습 + 파인튜닝 구조
- 다양한 NLP task에 사용 가능

> ✅ 성능 매우 우수  
> ❌ GPU 자원 필요


## 🧠 5. 한국어 분류 모델 (KcELECTRA 등)

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, pipeline

model_name = "beomi/KcELECTRA-base"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=3)

text_classifier = pipeline("text-classification", model=model, tokenizer=tokenizer)
text = "진짜 이 영상 너무 웃겨요 ㅋㅋ"
print(text_classifier(text))
```

> Hugging Face 모델 허브에서 다양한 한국어 모델 사용 가능


## 📚 정리표

| 세대 | 기술 | 장점 | 단점 |
|------|------|------|------|
| 1세대 | BoW, TF-IDF | 단순하고 빠름 | 의미 반영 불가 |
| 2세대 | Word2Vec | 의미 반영 | 문장 표현 어려움 |
| 3세대 | LSTM | 문맥 처리 | 순차 처리 느림 |
| 4세대 | BERT | 최고 정확도 | GPU 자원 필요 |


