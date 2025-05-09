
# 📘 1. 강화학습 개요 (Reinforcement Learning Overview)

## 💡 강화학습(RL, Reinforcement Learning)이란?

강화학습은 **에이전트(Agent)** 가 **환경(Environment)** 과 상호작용하며,
최종적으로 **누적 보상(total reward)** 을 최대화하도록 학습하는 기계 학습 방법입니다.

> 지도학습(Supervised Learning)과 달리, 정답을 명시적으로 제공하지 않고
> 오직 보상을 통해 행동을 유도합니다.

---

## 🧩 주요 특징

- **Trial & Error 학습**: 에이전트는 시행착오를 통해 학습함
- **Delayed Reward**: 보상은 즉시 주어지지 않고, 행동의 결과가 미래에 반영됨
- **탐험 vs 활용 문제 (Exploration vs Exploitation)**: 모르는 행동을 탐색할지, 현재 가장 좋은 행동을 선택할지의 균형이 필요
- **마르코프 결정 과정 (Markov Decision Process, MDP)** 를 기반으로 정의됨

---

## 🧠 에이전트의 학습 구조

```text
[상태(State)] --(정책 π)--> [행동(Action)] --> [환경(Environment)] --> [보상(Reward), 다음 상태(State')]
```

1. 현재 상태(state)를 관찰
2. 정책(policy)에 따라 행동(action)을 선택
3. 환경은 해당 행동의 결과로 다음 상태와 보상 제공
4. 에이전트는 이 정보를 바탕으로 정책을 업데이트

---

## 🔁 주요 구성 요소

| 구성 요소     | 설명 |
|--------------|------|
| **Agent**     | 학습하는 주체. 정책을 통해 행동을 결정 |
| **Environment** | 에이전트가 상호작용하는 세계 |
| **State (s)** | 현재 환경의 상태를 나타내는 정보 |
| **Action (a)** | 에이전트가 취할 수 있는 동작 |
| **Reward (r)** | 행동 후 환경이 에이전트에게 주는 피드백 |
| **Policy (π)** | 상태에 따라 행동을 선택하는 전략 |
| **Value Function (V(s))** | 상태에서 받을 수 있는 기대 보상의 합 |
| **Q Function (Q(s, a))** | 상태-행동 쌍에서 기대되는 누적 보상 |

---

## 🧾 강화학습 vs 지도/비지도학습 비교

| 항목 | 지도학습 | 비지도학습 | 강화학습 |
|------|-----------|--------------|-------------|
| 입력 | 데이터 + 정답 | 데이터만 | 상태 |
| 출력 | 예측값 | 군집, 패턴 | 행동 |
| 목표 | 정답 예측 | 구조 발견 | 보상 최대화 |
| 학습 방법 | 오차 최소화 | 패턴 분석 | Trial & Error |

---

## 📌 강화학습이 사용되는 분야

- 게임 (예: 알파고, DQN)
- 로봇 제어
- 자율주행
- 금융 트레이딩
- 추천 시스템
- 스마트 팩토리 및 제어 시스템

---

## 📚 참고 자료

- Sutton & Barto, *Reinforcement Learning: An Introduction*
- David Silver’s RL 강의 (DeepMind)
- OpenAI Spinning Up

