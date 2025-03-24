# 선형 회귀, 손실 함수, 최적화, 편미분 상세 설명

---

## 1. 선형 회귀란?

선형 회귀(Linear Regression)는 **입력 변수 x에 대해 출력 y를 예측하는 직선 형태의 모델**입니다. 

수식으로 표현하면:

![예측식](https://latex.codecogs.com/svg.image?%5Chat%7By%7D%20%3D%20wx%20%2B%20b)

- \( w \): 기울기 (weight)
- \( b \): 절편 (bias)
- ![yhat](https://latex.codecogs.com/svg.image?\hat{y}): 예측값

이 직선이 데이터의 분포를 가장 잘 설명하도록 만드는 것이 학습의 목표입니다.

---

## 2. 손실 함수 (Loss Function)

### 왜 필요한가?

모델이 예측한 값  ![yhat](https://latex.codecogs.com/svg.image?\hat{y}) 가 실제 값 \( y \)와 얼마나 차이가 나는지를 수치로 표현해야 합니다. 그 차이를 측정하는 함수가 손실 함수입니다.

### 평균 제곱 오차 (MSE, Mean Squared Error)

가장 대표적인 손실 함수는 MSE입니다:

![손실함수](https://latex.codecogs.com/svg.image?%5Cmathcal%7BL%7D(w%2C%20b)%20%3D%20%5Cfrac%7B1%7D%7Bn%7D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20(y_i%20-%20%5Chat%7By%7D_i)%5E2%20%3D%20%5Cfrac%7B1%7D%7Bn%7D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20(y_i%20-%20(wx_i%20%2B%20b))%5E2)

- 각 데이터의 예측 오차를 제곱해 모두 더한 뒤 평균을 내는 방식입니다.
- 예측이 정확할수록 손실은 작아집니다.

---

## 3. 최적화 함수 (Optimization)

### 목적


손실 함수 ![손실함수](https://latex.codecogs.com/svg.image?\\mathcal{L}) (w, b) 를 가장 작게 만드는 \( w \), \( b \) 값을 찾는 것.

### 경사 하강법 (Gradient Descent)

가장 널리 사용되는 최적화 알고리즘입니다. 작동 방식:

1. 초기값 \( w, b \) 설정
2. 손실 함수의 기울기(gradient)를 계산
3. 기울기의 반대 방향으로 조금씩 이동 (업데이트)
4. 손실이 줄어들 때까지 반복

업데이트 식:

![업데이트식](https://latex.codecogs.com/svg.image?w%20%3A%3D%20w%20-%20%5Calpha%20%5Ccdot%20%5Cfrac%7B%5Cpartial%20%5Cmathcal%7BL%7D%7D%7B%5Cpartial%20w%7D)

- \( \alpha \): 학습률 (learning rate), 이동 거리의 크기를 조절

---

## 4. 편미분 (Partial Derivative)의 의미

경사 하강법에서는 손실 함수의 기울기를 계산해야 합니다. 
이때 사용하는 것이 **편미분**입니다.

### 편미분이란?

여러 변수 중 **하나의 변수만 변할 때 함수가 얼마나 변하는지**를 나타내는 도구입니다.

![수식 보기](https://latex.codecogs.com/svg.image?\frac{\partial\mathcal{L}}{\partial{w}}): \( w \)가 바뀔 때 손실이 얼마나 바뀌는가

### 예: \( w \)에 대한 편미분 수식

![편미분_w](https://latex.codecogs.com/svg.image?%5Cfrac%7B%5Cpartial%20%5Cmathcal%7BL%7D%7D%7B%5Cpartial%20w%7D%20%3D%20-%5Cfrac%7B2%7D%7Bn%7D%20%5Csum%20x_i%20(y_i%20-%20%5Chat%7By%7D_i))

이 수치는 \( w \)를 증가시켰을 때 손실이 얼마나 증가하거나 감소하는지를 보여줍니다.

---

## 5. 왜 "양수면 줄이고, 음수면 키우는가?"

업데이트 식 다시 보기:

```text
w := w - alpha * (∂L/∂w)
```

- ![편미분](https://latex.codecogs.com/svg.image?\frac{\partial%20\mathcal{L}}{\partial%20w}) 가 **양수**: 현재 위치가 오르막 → 더 내려가려면 **w를 줄여야** 함
- ![편미분](https://latex.codecogs.com/svg.image?\frac{\partial%20\mathcal{L}}{\partial%20w}) 가 **음수**: 현재 위치가 내리막 → 더 내려가려면 **w를 키워야** 함

이렇게 기울기의 부호를 기준으로 **손실을 줄이는 방향**으로 이동합니다.

| 기울기 | 이동 방향 | 이유 |
|--------|------------|------|
| 양수  | 감소 (왼쪽으로) | 손실 증가 방향이므로 반대로 이동 |
| 음수  | 증가 (오른쪽으로) | 손실 감소 방향이므로 그대로 이동 |

---

## 6. 전체 정리

| 개념 | 설명 |
|------|------|
| **선형 회귀** | 입력에 대해 직선 형태로 예측하는 모델 |
| **손실 함수** | 예측이 얼마나 틀렸는지 수치화 |
| **최적화 함수** | 손실을 줄이는 파라미터를 찾는 과정 |
| **편미분** | 각 파라미터가 손실에 미치는 영향 분석 |
| **경사 하강법** | 기울기 방향을 따라 손실을 최소화 |
| **기울기의 부호 해석** | 양수면 감소, 음수면 증가 방향으로 이동하여 손실 최소화 |

---

✔ 이 내용을 GitHub README에 그대로 붙이면, 수식이 **SVG 이미지**로 예쁘게 보입니다.

✔ `.md`로 저장해서 프로젝트 문서화에 바로 활용 가능합니다.

✔ 필요시 Python 코드나 시각화 예시도 추가해드릴게요!

