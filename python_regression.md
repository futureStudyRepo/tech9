# 📐 최소제곱법(Least Squares Method)으로 선형 회귀 구하기 (공식으로)

주어진 데이터 x (예: 공부 시간)와 y (예: 시험 점수)에 대해 선형 회귀 직선을 다음과 같이 구할 수 있습니다:

회귀식의 형태는 다음과 같습니다:

```
y = ax + b
```

여기서:

- `a`: 기울기 (slope)
- `b`: 절편 (intercept)

---

## 🔢 최소제곱법 공식

기울기 `a` 와 절편 `b`는 다음 공식을 통해 계산합니다:

```
a = (n * Σxy - Σx * Σy) / (n * Σx² - (Σx)²)
b = (Σy - a * Σx) / n
```

각 항목 설명:

- `n`: 데이터의 개수
- `Σx`: x 값들의 합
- `Σy`: y 값들의 합
- `Σx²`: x 값들의 제곱 합
- `Σxy`: x와 y의 각 쌍의 곱의 합

---

## 🧮 Python 코드 예시

```python
x = [2, 4, 6, 8]
y = [81, 93, 91, 97]
n = len(x)

sum_x = sum(x)                               # Σx
sum_y = sum(y)                               # Σy
sum_xx = sum(i**2 for i in x)                # Σx²
sum_xy = sum(x[i] * y[i] for i in range(n))  # Σxy

a = (n * sum_xy - sum_x * sum_y) / (n * sum_xx - sum_x ** 2)
b = (sum_y - a * sum_x) / n

print(f"기울기 a: {a:.2f}")
print(f"절편 b: {b:.2f}")
print(f"회귀식: y = {a:.2f}x + {b:.2f}")
```

---

# 🤖 머신러닝과 선형 회귀

머신러닝에서는 선형 회귀를 수학적으로 풀기보다, **가설 설정 → 손실 함수 정의 → 옵티마이저로 최소화** 하는 방식으로 접근합니다.

## 🧪 1. 가설 (Hypothesis)

모델이 데이터를 통해 예측하고자 하는 함수입니다. 선형 회귀에서는 다음과 같은 형태를 가집니다:

```
y_hat = wx + b
```

- `w`: 가중치 (weight)
- `b`: 편향 (bias)

## 💥 2. 손실 함수 (Loss Function)

모델이 예측한 값과 실제 값 사이의 오차를 수치화한 함수입니다.  
선형 회귀에서는 주로 **평균 제곱 오차(MSE)** 를 사용합니다:

```
MSE = (1/n) * Σ(y - y_hat)²
```

MSE가 작을수록 모델의 예측이 실제 값과 가까움을 의미합니다.

## 🛠️ 3. 옵티마이저 (Optimizer)

손실 함수 값을 최소화하기 위해 `w`와 `b`를 조정하는 알고리즘입니다.  
대표적인 방법은 **경사 하강법 (Gradient Descent)** 입니다:

```python
# 경사 하강법 한 스텝 (예시)
w = w - learning_rate * gradient_w
b = b - learning_rate * gradient_b
```

- learning_rate: 학습률 (한 번에 얼마나 이동할지)
- gradient: 손실 함수에 대한 편미분값

---

## 📈 참고

- `matplotlib`을 사용하면 회귀선을 시각화할 수 있습니다.
- 머신러닝에서는 정규화 (Regularization: L1, L2 등) 도 자주 사용됩니다.
- 사이킷런(sklearn)에서는 `LinearRegression` 클래스로 간편하게 구현할 수 있습니다.



---
# 📊 선형 회귀의 종류

머신러닝 또는 통계에서 회귀(Regression)는 연속적인 값을 예측하기 위한 지도 학습 방식입니다.  
회귀에는 입력 데이터의 형태나 복잡도에 따라 다양한 종류가 있으며, 주요 회귀 방식은 다음과 같습니다:

---

## 1️⃣ 단순 선형 회귀 (Simple Linear Regression)

- **특징**: 입력 변수가 1개인 선형 회귀
- **식 형태**:  
  ```
  y = ax + b
  ```
- **예시**: 공부 시간(x)으로 시험 점수(y)를 예측

---

## 2️⃣ 다중 선형 회귀 (Multiple Linear Regression)

- **특징**: 두 개 이상의 입력 변수 사용
- **식 형태**:  
  ```
  y = a₁x₁ + a₂x₂ + ... + aₙxₙ + b
  ```
- **예시**: 공부 시간(x₁), 수면 시간(x₂), 식사량(x₃) 등을 기반으로 시험 점수(y) 예측

---

## 3️⃣ 비선형 회귀 (Nonlinear Regression)

- **특징**: 데이터의 관계가 선형이 아닌 경우 사용
- **식 형태 예시**:
  ```
  y = a * e^(bx)
  y = 1 / (1 + e^(-x))  (시그모이드 곡선)
  y = a * ln(x) + b
  ```
- **예시**: 바이러스 확산 속도, 약효 반응, 포화 현상 등

---

## 4️⃣ 다항 회귀 (Polynomial Regression)

- **특징**: 입력 변수는 하나지만, 다항식으로 확장한 형태의 선형 회귀
- **식 형태**:
  ```
  y = a₀ + a₁x + a₂x² + a₃x³ + ... + aₙxⁿ
  ```
- **예시**: 자동차 속도에 따른 제동 거리, 온도 변화에 따른 물질 반응

> 💡 참고: 다항 회귀는 여전히 **선형 회귀**에 속합니다. 이유는 회귀 계수에 대해 **선형적**이기 때문입니다.

---

## 📌 요약 비교

| 종류             | 입력 변수 수 | 식 형태             | 비선형 여부 |
|------------------|---------------|----------------------|-------------|
| 단순 선형 회귀   | 1개            | y = ax + b           | ❌ 선형     |
| 다중 선형 회귀   | 2개 이상       | y = a₁x₁ + ... + b   | ❌ 선형     |
| 다항 회귀        | 1개 (하지만 제곱 등 다항) | y = a₀ + a₁x + a₂x² + ... | ❌ 선형 (형태는 곡선) |
| 비선형 회귀      | 제한 없음      | 지수/로그/시그모이드 등 | ✅ 비선형  |

---

## 🧠 어떤 회귀를 선택해야 할까?

- **데이터가 직선 형태에 가깝다** → 단순 또는 다중 선형 회귀
- **입력이 많고 해석이 중요** → 다중 선형 회귀
- **곡선 형태로 증가/감소** → 다항 회귀
- **형태가 명확히 비선형** → 비선형 회귀 (모델링 복잡 ↑)
