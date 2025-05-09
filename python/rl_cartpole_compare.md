
# 🎯 CartPole 강화학습 접근 방식 비교

강화학습의 대표 환경인 **CartPole-v1**를 대상으로,
다양한 방식의 접근을 비교 정리합니다.

---

## ✅ 환경 정보

- 목표: 막대가 쓰러지지 않도록 카트를 좌우로 움직임
- 상태(state): 총 4개 요소 (위치, 속도, 각도, 각속도)
- 행동(action): 0 (왼쪽), 1 (오른쪽)

---

## 1. 🧮 조건문 기반 규칙형(Heuristic)

간단한 수식을 통한 rule-based 접근 (학습 없음)

```python
def heuristic_policy(state):
    angle = state[2]
    return 1 if angle > 0 else 0
```

> 각도가 오른쪽으로 기울면 오른쪽으로 밀고, 왼쪽이면 왼쪽으로

---

## 2. 🧠 신경망 기반 DQN (Deep Q-Network)

딥러닝을 활용하여 상태 → Q값 예측

```python
import tensorflow as tf
from tensorflow.keras import layers

model = tf.keras.Sequential([
    layers.Dense(24, activation='relu', input_shape=(4,)),
    layers.Dense(24, activation='relu'),
    layers.Dense(2, activation='linear')  # 행동 0, 1에 대한 Q값
])
```

- 정책: ε-greedy
- 리플레이 메모리 사용
- 타겟 Q 계산: `r + γ * max(Q(s', a'))`

> 중간 크기의 환경에 적합하며, 표준적인 DQN 구조

---

## 3. 🚀 최근 방법: PPO (Proximal Policy Optimization)

Actor-Critic 구조 기반의 최신 정책 기반 알고리즘

```python
from stable_baselines3 import PPO
from stable_baselines3.common.env_util import make_vec_env

env = make_vec_env("CartPole-v1", n_envs=1)
model = PPO("MlpPolicy", env, verbose=1)
model.learn(total_timesteps=10000)
```

- Policy gradient 기반으로 직접 정책을 업데이트
- 신경망이 정책과 가치함수를 모두 예측
- 안정적이고 빠른 수렴으로 다양한 실전 문제에 활용

---

## 🧾 비교 요약

| 접근 방식 | 특징 | 학습 여부 | 구현 난이도 | 실제 적용 |
|-----------|------|------------|--------------|-------------|
| Heuristic (조건문) | 각도 기반 규칙 | ❌ | 매우 쉬움 | 학습 필요 없는 간단한 상황 |
| DQN | Q-value 학습 | ✅ | 중간 | 상태 공간이 작을 때 |
| PPO | 정책 직접 학습 | ✅✅ | 높음 | 복잡한 환경, 실전 사용 |

---

## 📌 참고 자료

- [Gym CartPole-v1 설명](https://www.gymlibrary.dev/environments/classic_control/cart_pole/)
- [Stable-Baselines3 공식 문서](https://stable-baselines3.readthedocs.io/)
- Sutton & Barto, *Reinforcement Learning: An Introduction*

