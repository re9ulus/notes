-- Конспект лекций по RL с мыслями и заметками

# RL

- Learning optimal strategy by trial and error
- Needs feedback on agents own actions
- Agent can affect it's own observations

- Bandit
Env --[observation]--> Agent --[action]--> Feedback

Процесс принятия решений
Диаграмма со средой, агентом action/observation

Агент в нашем распоряжении
Среда в худшем случае - черная коробка

ключевые идеи:
- возможность самому выбирать действия и исследовать среду
- недифференцируемая награда, которая происходит в следствии общения со средой (лайки в соцсети)

---
### MDP process

Марковский процесс принятия решений

Марковское предположение: текущие состояние зависит только от предыдущего и не зависит от всех предшествующих.
P(s\_{t+1} | s\_t, a\_t, s\_{t-1}, a\_{t-1}) = P(s\_{t+1} | s\_t, a\_t)

- Env states s \in S
- Agent actions a \in A
- Rewards r \in R

- Dynamics P(s\_{t+1} | s\_t, a\_t)

### Rewards

Total reward per session:

R = \sum r\_t

Agents policy (политика, стратегия) - определяет вероятностное распределение

\pi (a | s) = P(take action a | in state s)

Problem: Find policy with highest reward: \pi (a | s) : E\_{\pi}[R] -> max

### Cross Entropy Method

General idea:
- Play few sessions  ; Sample N[100] sessions
- Update your policy ; Pick M[25] best sessions, called elite sessions
- Repeat             ; Change policy so that it prioritizes actions from elite sessions

### Tabular crossentropy method

- Policy matrix
\pi(a | s) = A\_{s, a}
- Sample N games with that policy
- Get M best sessions (elites)
Elite = [(s\_0, a\_0), (s\_1, a\_1) ... (s\_k, a\_k)]
- Aggregate by states
\pi (a | s) = (took a at s) / (was at s)  <- In M best games

- Алгоритм очень нестабилен при малых выборках. Пусть в одном из состояний мы оказались 1 раз. Тогда всегда попадая в это состояние мы будем идти в одну сторону.
Для починки можно использовать сглаживание или другие костыли

- Алгоритм очень нестабилен при малых выборках. Пусть в одном из состояний мы оказались 1 раз. Тогда всегда попадая в это состояние мы будем идти в одну сторону.
Для починки можно использовать сглаживание, экспоненциальное скользящее среднее или другие костыли.
- Если в среде есть элемент случайности - алгоритм отберет в качестве лучших те исполнения, где ему повезло. Алгоритм никогда не научиться правильно обрабатывать неудачные для него среды
- Реворд мы получаем в конце сессии => чтобы хоть чему-то научиться - нужно закончить игру => теряем кучу информации в процессе. Если процесс бесконечный или длинный - можно сделать лучше
- Нужно много итераций

А если таблица слишком большая и не лезет в память... то просто возьмем любой алгоритм машинного обучения возвращающий вероятности

### Approxymate CrossEntropy Method
- Policy is approxymation
  - Neural network predicts \pi\_w (a | s) given s
  - Linear model, Random forest, etc.

Can't set \pi (a | s) explicitly

All state-action pairs from M best sessions

Elite = [(s\_0, a\_0)yy, (s\_1, a\_1), ... (s\_k, a\_k)]

Maximize likelihood of actions in "best" games

\pi = argmax\_{\pi} \sum\_{a, s \in Elite} log \pi(a\_i, s\_i)

Algorithm:
- Initialize NN weights W\_0 <- random
- Loop:
  - Sample N Sessions
  - Elite = [(s\_0, a\_0)yy, (s\_1, a\_1), ... (s\_k, a\_k)]
  - W\_{i+1} = W\_i + alpha \grad [\sum\_{a\_i, s\_i} \in Elite} log \pi\_{w\_i} (a\_i | s\_i)]

# Метод можно отнести к DFO (Derivative-free-optimization) / Evolution optimization
[Видео](https://www.youtube.com/watch?v=aUrX-rP_ss4&list=PLCTc_C7itk-GaAMxmlChrkPnGKtjz8hv1)

In eqch episode initial state is sampled from \mu and the process proceed until the terminal state is reached. For example:
- Taxi robot reaches it's destination (termination = good)
- Walking robot falls over (termniation = bad)

Types of parametrized policies:
- Deterministic a = \pi(s, theta)
- Stochastic a = \pi(a|s, theta)

- Parameterized policies \pi\_{\Theta}

Objective: maximize \nu(\pi) = E [r\_0, .. r\_{t-1} | \pi]

# Методы монте-карло
[Видео](https://yadi.sk/i/5yf_4oGI3EDJhJ)

Crossentropy Method
- Stochastic optimization

Monte Carlo Policy Gradient

Источники:
- [Курс ШАД по RL](https://github.com/yandexdataschool/Practical_RL)
