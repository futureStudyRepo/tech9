# 📘 자연어 처리 분류 모델의 발전사

자연어 처리는 인간 언어를 컴퓨터가 이해할 수 있도록 하는 기술입니다.

---

## 🧭 개요

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


물론입니다! 아래는 각 기술별로 간단한 **예시 코드**만 따로 정리한 것입니다.  

---

## 🧩 1. BoW / TF-IDF 예시 (scikit-learn)

```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

texts = ["오늘 날씨가 정말 좋다", "오늘은 기분이 너무 좋아", "비가 와서 우울해"]

# BoW
bow_vectorizer = CountVectorizer()
bow = bow_vectorizer.fit_transform(texts)
print("BoW:\n", bow.toarray())

# TF-IDF
tfidf_vectorizer = TfidfVectorizer()
tfidf = tfidf_vectorizer.fit_transform(texts)
print("TF-IDF:\n", tfidf.toarray())
```

---

## 🧬 2. Word2Vec 임베딩 예시 (gensim)

```python
from gensim.models import Word2Vec

sentences = [["오늘", "날씨", "좋다"], ["기분", "좋아"], ["비", "우울"]]
model = Word2Vec(sentences, vector_size=100, window=2, min_count=1, sg=0)  # CBOW

print("기분 벡터:\n", model.wv["기분"])
print("유사도(기분, 좋아):", model.wv.similarity("기분", "좋아"))
```

---

## 🧠 3. LSTM 분류기 예시 (PyTorch)

```python
import torch.nn as nn

class SimpleLSTMClassifier(nn.Module):
    def __init__(self, vocab_size, embedding_dim, hidden_dim, output_dim):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, embedding_dim)
        self.lstm = nn.LSTM(embedding_dim, hidden_dim, batch_first=True)
        self.fc = nn.Linear(hidden_dim, output_dim)

    def forward(self, x):
        embedded = self.embedding(x)
        _, (hidden, _) = self.lstm(embedded)
        return self.fc(hidden[-1])
```

> ※ 학습용 데이터와 토크나이징은 생략

---

## 🔮 4. BERT 분류 예시 (transformers)

```python
from transformers import BertTokenizer, BertForSequenceClassification
from transformers import pipeline

model_name = "bert-base-multilingual-cased"
tokenizer = BertTokenizer.from_pretrained(model_name)
model = BertForSequenceClassification.from_pretrained(model_name, num_labels=2)

classifier = pipeline("text-classification", model=model, tokenizer=tokenizer)
print(classifier("오늘 정말 기분이 좋다!"))
```

---

## 🧠 5. 한국어 분류 모델 예시 (KcELECTRA)

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, pipeline

model_name = "beomi/KcELECTRA-base"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=3)

text_classifier = pipeline("text-classification", model=model, tokenizer=tokenizer)
print(text_classifier("진짜 이 영상 너무 웃겨요 ㅋㅋ"))
```

---




