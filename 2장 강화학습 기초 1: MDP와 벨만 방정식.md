## 2장 강화학습 기초 1: MDP와 벨만 방정식

사람과는 달리, 강화학습에서는 사용자가 문제를 정의해햐 한다.

문제를 잘못 정의하면 에이전트가 학습을 못 할 수도 있다.

<br>

### MDP
- **상태**: 에이전트가 관찰 가능한 상태의 집합 S

- **행동**: 에이전트가 상태 S<sub>t</sub>에서 할 수 있는 가능한 행동의 집합 A

- **보상함수**: 시간 t일 때 상태가 S<sub>t</sub> = s이고 그 상태에서 행동 A<sub>t</sub> = a를 했을 경우에 받을 보상에 대한 기댓값 E
(보상은 에이전트가 학습할 수 있는 유일한 정보로서 환경이 에이전트에게 주는 정보)
r(s, a) = E[r<sub>t+1</sub> | S<sub>t</sub> = s, A<sub>t</sub> = a]

- **상태 변환 확률**: 상태 s에서 행동 a를 취했을 때 다른 상태 s'에 도달할 학률
(에이전트가 아닌 환경의 일부이다.) (환경의 모델 model이라고도 부른다.)
P<sup>a</sup><sub>ss'</sub> = P[S<sub>t+1</sub> = s' | S<sub>t</sub> = s, A<sub>t</sub> = a]

- **할인율**: 미래의 가치를 시간에 따라 할인하는 비율 γ

- **정책**: 시간 t에 S<sub>t</sub> = s에 에이전트가 있을 때, 가능한 행동 중에서 A<sub>t</sub> = a를 할 확률 π
상태가 입력으로 들어오면 행동을 출력으로 내보내는 일종의 함수라고 생각해도 좋다.
π(a | s) = P[A<sub>t</sub> = a | S<sub>t</sub> = s]

<br>

### 가치함수
- **반환값**: 에이전트가 실제로 환경을 탐험하며 받은 보상의 합.
G<sub>t</sub> = r<sub>t+1</sub> + γ r<sub>t+2</sub> + γ<sup>2</sup> r<sub>t+3</sub> + ...

- **가치함수**: 상태 S<sub>t</sub>에서의 반환값에 대한 기댓값. 
(각 타임스텝마다 받는 보사이 모두 확률적이고 반환값이 그 보상들의 합이므로 반환값은 확률변수이다.) (상태 가치함수)
v(s) = E[G<sub>t</sub> | S<sub>t</sub> = s] = E[r<sub>t+1</sub> + γ v(S<sub>t+1</sub>) | S<sub>t</sub> = s]

- **벨만 기대 방정식**: 현재 상태의 가치함수 v<sub>π</sub>(s)와 다음 상태의 가치함수 v<sub>π</sub>(S<sub>t+1</sub>) 사이의 관계를 말해주는 방정식
v<sub>π</sub>(s) = E<sub>π</sub>[r<sub>t+1</sub> + γ v<sub>π</sub>(S<sub>t+1</sub>) | S<sub>t</sub> = s] (정책을 고려한 가치함수의 표현)

<br>

### 큐함수
- **큐함수**: 어떤 상태에서 어떤 행동이 얼마나 좋은지 알려주는 함수 (행동 가치함수)
q<sub>π</sub>(s, a) = E<sub>π</sub>[r<sub>t+1</sub> + γ q<sub>π</sub>(S<sub>t+1</sub>, A<sub>t+1</sub>) | S<sub>t</sub> = s, A<sub>t</sub> = a]

- **가치함수와 큐함수 사이의 관계식**
v<sub>π</sub>(s) = Σ<sub>a∈A</sub> π(a | s) q<sub>π</sub>(s, a)

<br>

### 벨만 방정식
- **벨만 기대 방정식**: 정책을 반영한 가치함수
v<sub>π</sub>(s) = E<sub>π</sub>[r<sub>t+1</sub> + γ v<sub>π</sub>(S<sub>t+1</sub>) | S<sub>t</sub> = s]

- **계산 가능한 벨만 방정식**
v<sub>π</sub>(s) = Σ<sub>a∈A</sub> π(a | s) ( r(s, a) + γ Σ<sub>s'∈S</sub> P<sup>a</sup><sub>ss'</sub> v<sub>π</sub>(s') )

  - 벨반 기대 방정식을 이용해 현재의 가치함수를 계속 업데이트하다 보면 참값을 구할 수 있다
(현재의 정책π에 대한 참 가치함수)

- **최적 가치함수**: 최적 정책을 따라갔을 때 받을 보상의 합
최적의 큐함수 중에서 max를 취하는 것이 최적의 가치함수가 된다.

- **벨만 최적 방정식**: 최적의 정책에서 가치함수 사이의 관계식
v*<sub>π</sub>(s) = max<sub>a</sub> E<sub>π</sub>[r<sub>t+1</sub> + γ v*(S<sub>t+1</sub>) | S<sub>t</sub> = s]
