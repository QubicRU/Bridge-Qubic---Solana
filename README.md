# Qubic - Solana. 
This bridge connects the Qubic network with the Solana network, allowing users to transfer tokens between the two networks. 


## 1 Project Scope‬
The scope of this project covers two main phases: 

1. The design and development of the Qubic–Solana‬ bridge  
2. Its operation and long-term sustainability.‬
‭
‬
### 2‬‭ Development phase‬
‭●
‬‭ Architecture & Design:‬‭ Designing a secure, modular,‬‭ and extensible bridge architecture that‬ supports transfer of tokens between Qubic and Solana. 

●
‬‭ Smart Contract Development:‬‭ Implementing and thoroughly‬‭ testing all necessary smart‬ contracts for both Qubic and Solana, ensuring compliance with each network’s standards.‬ Contracts must include proper access controls, event logging, and mechanisms for upgradeability‬ if required.‬

●
‬‭ Backend & Off-chain Services:‬‭ Developing any required‬‭ off-chain components such as‬ relayers, oracles, monitoring systems, and service backends to ensure correct and efficient‬ bridge operation.‬

●
‬‭ User Interfaces & APIs:‬‭ Providing user-facing components‬‭ (web interface, dashboards, or‬ widgets) and public APIs for interaction with the bridge, transaction monitoring, and integration by‬ third-party applications.‬

●
‬‭ Initial Liquidity Provisioning (where applicable):‬‭ A critical requirement is that the provider‬ must‬‭ design and implement a solution for ensuring‬‭ sufficient liquidity on both sides of the‬ bridge at launch‬‭ . This may involve deploying and bootstrapping‬‭ liquidity pools, setting up‬ custodial reserves, or leveraging automated market-making mechanisms. 

●
‬‭ Documentation:‬‭ Supplying comprehensive technical documentation,‬‭ including integration‬ guides, API references, and operational manuals.‬

●
‬‭ Testing & Auditing:‬‭ Supporting extensive internal‬‭ and external testing (including testnet‬ deployments on both chains), and participating fully in all required audit and review processes as‬ specified in the Security Requirements.‬

●
‬‭ Deployment:‬‭ Deploying the bridge to both Qubic and‬‭ Solana mainnets, with coordinated support‬ for testnet and production rollout.‬
‭
‭
# 2. Блок-схема архитектуры моста Qubic ↔ Solana:

Пользователь → кошельки и смарт-контракты (Qubic / Solana)

Мониторинг событий → Релейеры / Валидаторы

Центральная база и система мониторинга фиксируют все транзакции.

‭<img width="501" height="349" alt="Снимок экрана 2025-09-25 в 15 09 23" src="https://github.com/user-attachments/assets/700c89e4-962a-4f6a-9da0-fe2d7a34479d" />


## Liquidity Pool Model

Как работает:
На обеих сетях заранее создаются ликвидити-пулы (Qubic ↔ wQubic).
Пользователь кладёт токен в пул одной сети и мгновенно получает эквивалент из пула другой сети.
Баланс поддерживается за счёт автоматического ребалансинга и комиссий.

Плюсы:
- Мгновенные переводы без ожидания верификации.
- Удобнее для пользователей с точки зрения UX.
- Возможность вознаграждать провайдеров ликвидности комиссиями.

Минусы:
- Требует постоянной поддержки ликвидности в пулах.
- Риск дефицита ликвидности при больших перекосах.
- Более сложное управление резервами.

Liquidity Pool = скорость и удобство.
‭
Блок-схеме моста Qubic ↔ Solana (Liquidity Pool):
Transfer Request, Sign & Submit Tx, Deposit Tokens — от пользователя до пула Qubic.
Swap Event, Release Tokens, Receive Tokens, Confirmation — поток через релейеров к Solana.
Log Events, Tx Records, Pool Balance — параллельная запись и мониторинг.

‭
<img width="574" height="347" alt="Снимок экрана 2025-09-26 в 12 39 44" src="https://github.com/user-attachments/assets/24e6e309-6beb-47d3-aa5d-ba41af47cb04" />

## Liquidity Maintenance: Creation and Implementation of the Mechanism

The liquidity management mechanism must ensure:
Sufficient reserves on both sides of the bridge to handle user transactions without delays.
Risk minimization related to volatility, liquidity drain, and malicious behavior.
Sustained user trust through transparent operations and reliable execution of transfers.


## Liquidity Provisioning (Initial Setup)

Dual-Side Pools: Create liquidity pools on both chains (e.g., QUBIC pool and wQUBIC pool on Solana).
Bootstrapping Liquidity: Initial liquidity provided by the bridge operator and early backers.
Incentive programs for external liquidity providers (LPs), e.g., yield farming rewards or share of transaction fees.

ПЕРЕВОД:
Обеспечение ликвидности (первоначальная настройка)
Двусторонние пулы: создайте пулы ликвидности на обеих цепочках (например, пул QUBIC и пул wQUBIC на Solana).
Повышение ликвидности: Первоначальная ликвидность была предоставлена ​​оператором моста и ранними спонсорами.
Программы стимулирования для внешних поставщиков ликвидности (ВП), например, вознаграждения за выращивание урожая или доля комиссий за транзакции.

# 3. План работы моста Qubic ↔ Solana для трансфера токенов. 
на примере модели Liquidity Pool

<img width="709" height="458" alt="Снимок экрана 2025-09-25 в 20 29 08" src="https://github.com/user-attachments/assets/0ff01598-b716-47af-955d-343f82e6389c" />

### Основные процессы в работе моста

1. **Инициация → депозит токенов в пул.**
2. **Мониторинг события → передача валидаторам.**
3. **Подтверждение валидаторами → формирование кросс-чейн сообщения.**
4. **Выдача токенов из пула целевой сети.**
5. **Подтверждение пользователю → обновление статуса.**
6. **Логирование и мониторинг для безопасности и аудита.**

---

### Важные моменты

* Поддержка достаточной ликвидности на обеих сторонах.
* Защита от double-spend и re-play атак.
* Надёжная работа валидаторов (мультисиг, защита ключей).
* Постоянный мониторинг пула и системы.
---

### 1. **Инициация пользователем**

* Пользователь открывает **Frontend/UI** (веб-интерфейс или dApp).
* Подключает свой кошелёк: **Qubic Wallet** и/или **Solana Wallet**.
* Выбирает:

  * источник (Qubic или Solana),
  * количество токенов,
  * модель моста (**Liquidity Pool**),
  * адрес назначения.
* Система отображает **комиссии**, **баланс пулов** и **примерное время перевода**.

---

### 2. **Отправка транзакции**

* Пользователь подтверждает операцию в своём кошельке (подписывает транзакцию).
* Токены отправляются в **Liquidity Pool на исходной сети** (например, Qubic).
* Смарт-контракт пула фиксирует депозит и генерирует событие о пополнении.

---

### 3. **Мониторинг и верификация**

* **Event Monitor** на стороне Qubic фиксирует событие депозита.
* Информация передаётся в **Relayer / Validators** (сетевой слой мостов).
* Валидаторы проверяют корректность транзакции:

  * подписи пользователя,
  * уникальность nonce (чтобы не было повторного исполнения),
  * баланс пула.

---

### 4. **Кросс-чейн координация**

* После подтверждения валидаторы создают сообщение для противоположной сети (Solana).
* Это сообщение включает:

  * ID транзакции,
  * сумму и токен,
  * адрес получателя.
* Валидаторы подписывают сообщение (мультисиг) и отправляют его в **Liquidity Pool на Solana**.

---

### 5. **Выдача средств на целевой сети**

* **Liquidity Pool (Solana)** получает инструкцию от валидаторов.
* Смарт-контракт проверяет подписи и данные.
* Из пула Solana списываются соответствующие токены (например, wQUBIC → QUBIC, или наоборот).
* Средства переводятся на **Solana Wallet получателя**.

---

### 6. **Подтверждение пользователю**

* **Frontend/UI** получает от Solana сети статус "Transaction Completed".
* Пользователь видит:

  * хэш транзакции,
  * подтверждение получения,
  * обновлённый баланс в кошельке.

---

### 7. **Мониторинг и отчётность**

* Все события (депозит, релиз, статус) логируются в **Database & Monitoring**.
* Система мониторинга:

  * отслеживает баланс пулов,
  * фиксирует ошибки, таймауты, повторные попытки,
  * уведомляет о проблемах (например, недостаток ликвидности).
* В случае сбоя:

  * транзакция может быть отменена,
  * токены возвращены на исходный кошелёк.

---



---
# 4. Bridge with a набором валидаторов/релееров и пулом ликвидности:

| Источник дохода                             | Как формируется                                                                                                                  | Распределение / доли                                                                                    | Замечания                                                                            |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Транзакционные комиссии (bridge fee)        | Пользователь платит комиссию за перевод (фиксированная или процент)                                                              | Например: 50 % → валидаторам / релеерам / операторам, 30 % → резерв безопасности, 20 % → DAO / развитие | Комиссия может быть дифференцированной (приоритетный перевод, большая сумма и т. д.) |
| Доход с ликвидности / блокированных активов | Активы, заблокированные в мостовых пулах, могут участвовать в стейкинговых или yield-стратегиях                                  | Часть дохода идёт мосту (например 20-30 %), остальное — поставщикам ликвидности (LP)                    | Нужно выбирать безопасные стратегии, не подвергающие актив мосту большому риску      |
| Плата за подписку / сервис интеграции       | Проекты, желающие интегрировать свой токен или услугу через мост, платят за API / SDK / обслуживание                             | 100 % → мост / DAO                                                                                      | Стабильный источник дохода вне зависимости от трафика пользователей                  |
| Комиссия за relay / сообщение               | Если мост поддерживает не просто токены, а произвольные сообщения / контракты, релееры взимают плату                             | Например: фиксированная плата + переменная часть                                                        | Особенно актуально в GMP-мостах                                                      |
| Часть от операций внутри моста / свопов     | Если мост встроен с DEX / обменом внутри цепочек, часть спреда / платы идет мосту                                                | Можно брать, скажем, 0.1-0.3 % от суммы свопа                                                           | Требует интеграции с DEX-механизмами                                                 |
| Резерв безопасности / штрафы                | При ошибках, злоупотреблениях или деривативах, часть штрафов или комиссий остаётся в резерве, который покрывает возможные убытки | Фонд накапливается до определённого порога                                                              | Это снижает риски и повышает доверие                                                 |


| Revenue Source                              | How It Is Generated                                                                                                                | Distribution / Shares                                                                             | Notes                                                                             |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Transaction Fees (bridge fee)               | Users pay a fee for transfers (fixed or percentage-based)                                                                          | Example: 50% → validators / relayers / operators, 30% → security reserve, 20% → DAO / development | Fees can be differentiated (priority transfer, large amount, etc.)                |
| Income from Liquidity / Locked Assets       | Assets locked in bridge pools can participate in staking or yield strategies                                                       | Part of the income goes to the bridge (e.g., 20–30%), the rest to liquidity providers (LPs)       | Safe strategies must be chosen to avoid exposing the bridge’s assets to high risk |
| Subscription Fee / Integration Service      | Projects that want to integrate their token or service through the bridge pay for API / SDK / maintenance                          | 100% → bridge / DAO                                                                               | A stable source of revenue regardless of user traffic                             |
| Relay / Message Fee                         | If the bridge supports not just tokens but arbitrary messages/contracts, relayers charge a fee                                     | For example: fixed fee + variable component                                                       | Particularly relevant in GMP-bridges                                              |
| Share from Internal Bridge Operations/Swaps | If the bridge is integrated with a DEX / cross-chain swap, part of the spread/fee goes to the bridge                               | For example: 0.1–0.3% of the swap amount                                                          | Requires integration with DEX mechanisms                                          |
| Security Reserve / Penalties                | In case of errors, abuse, or misbehavior, part of the penalties or fees remains in the reserve fund, which covers potential losses | The fund accumulates up to a defined threshold                                                    | Reduces risks and increases trust                                                 |


Комиссия за перевод может составляет 1-2 %. 

# Модель безопасной работы моста Qubic — Solana с использованием ИИ

Краткая идея

Гибридная архитектура моста (валидаторы + light-client) дополняется слоями интеллектa, которые автоматически:
мониторят и обнаруживают аномалии и атаки в реальном времени;
оптимизируют комиссии и стейкинг/инцентивы для поддержания ликвидности;
помогают принимать решения по аварийным меркам (pausing, slashing, топ-апы);
прогнозируют потоки (demand forecasting) и планируют пополнения;
анализируют поведение валидаторов/релееров и выявляют рискованные/зловредные узлы.

ИИ — не «единый оракул», а набор специализированных моделей с контролем, explainability и «человеко-в-петле» (human-in-the-loop) для критичных решений.

Компоненты системы (высокоуровневая архитектура)

On-chain слой

Смарт-контракты моста (Qubic и Solana): lock/mint, pool accounting, fee collector, pausе, slashing, dispute.

Oracle интерфейс для получения «решений»/сигналов от офф-чейн ML-сервиса (см. далее).

Модули аудита и proof-store (для light client и доказательств).

Off-chain инфраструктура (оперативный бекенд)

Validator Network (N валидаторов, multisig/threshold signatures).

Relayers — служба передачи сообщений.

Monitoring & Data Collector — собирает данные из обеих цепочек, логов валидаторов, релееров, ордеров, API, ордингов, агрегаторов цен.

AI Engine — набор ML моделей (см. раздел ниже).

Decision Service (Policy Engine) — business rules + ML signals → действия (например, raise fee, pause bridge, flag validator). Любое автоматическое действие критического уровня проходит через проверку политиками и, при необходимости, human approval.

Dashboard & Incident Console — UI для операторов/DAO с explainability сигналов ИИ.

Governance / Treasury / Insurance

DAO voting, treasury для топ-апов, insurance pool, bug bounty, slashing rules.

Роли ИИ (где и зачем)

Anomaly Detection / Fraud Detection

Цель: обнаружить аномальные события (всплески вывода, синхронные транзакции по схеме «вымывания», подозрительная активность валидатора/релера).

Модели: unsupervised (Isolation Forest, Autoencoder), графовые (GNN) для выявления нетипичных цепочек транзакций/взаимодействий.

Действия: raise alert, временная приостановка операций высокого риска, запускается dispute/forensic pipeline.

Validator / Relayer Risk Scoring

Цель: профилирование узлов по надежности.

Модели: supervised (если метки есть) + unsupervised clustering + time-series features (uptime, latency, signature patterns, IP география, связность).

Действия: динамическое изменение порога доверия, слэшинг/ремув, рекомендации для DAO по смене состава.

Liquidity Forecasting & Dynamic Fee Optimization

Цель: прогноз спроса (по направлению), оптимизация комиссии/инцентивов, предотвращение дефицита ликвидности.

Модели: time-series (Prophet, LSTM), causal models + Reinforcement Learning агент (policy) для динамической настройки fee и LP rewards.

Reward в RL: комбинирует доходность, вероятность резкого оттока, удовлетворённость пользователей и резервный фонд уровни.

Automated Incident Response Assistant

Цель: выработать и предложить ближайшие шаги при инциденте (pausing, rollback, emergency top-up).

Модели: decision trees + rule engine + similarity search (по истории инцидентов) → rank действий и ожидаемый эффект.

Human-in-loop обязательный при крупных суммах.

Proof Verification Optimization

Цель: оптимизировать формирование, пакетирование и проверку Merkle proofs / light client sync для снижения затрат.

Модель: planner + cost model → решает, когда формировать батчи, какие транзакции требовать обязательной проверкой.

Explainability & Audit Logs

Цель: все ML решения логируются, с причинно-следственными объяснениями (SHAP/Anchors для tabular, attention-maps для sequence models).

Подробно: ключевые модели и алгоритмы
1) Аномалия (real-time) — схема

Входные данные: поток транзакций, таймстемпы, суммы, адреса, профили валидаторов, IP/LOC, on-chain balances, mempool patterns.

Фичи: rate per address, velocity (USD/sec), cluster membership, entropy of destinations, proportion of transfers > X, changes in nonce patterns.

Модель: Ensemble of:

Streaming IsolationForest (detect point anomalies).

Sequence Autoencoder (LSTM/Transformer) для multivariate series (выявляет аномальные паттерны).

Graph Anomaly Detector (for coordinated addresses).

Порог: multi-signal voting: если ≥2 моделей триггерят → high alert.

Action: immediate throttle on direction (soft block) + notify operators + mark related validator signatures for review.

2) Validator Risk Scoring — схема

Вход: uptime, avg latency, signature timing distribution, stake history, on-chain tx patterns, infra metrics.

Модель: Gradient Boosted Trees (XGBoost) + temporal decay; periodic re-training.

Output: score ∈ [0..1]. Политики:

score < 0.3 → temporary isolation, reduce weight in multisig quorum.

0.3–0.6 → monitoring + require additional proof for their signatures.

0.6 → normal.

3) Dynamic Fee Optimization (RL agent) — схема

State: pool balances (both sides), time, last K transfer volumes per direction, current fee, LP available, reserve level, price volatility.

Action: set fee multiplier for direction(s), set LP bonus.

Reward: R = α * fee_revenue - β * user_drop_rate - γ * reserve_depletion_penalty - δ * frictions_cost

coefficients tuned by governance.

Alg: PPO (or DQN for discrete grid) with safety constraints: actions that would reduce reserve below min_threshold are masked.

Safety: conservative exploration, start in simulated environment, shadow deploy with human approval.

4) Proof Batching & Cost Planner

Decide: per tx proof generation vs batched zk/merkle proof based on cost model (gas costs on Solana vs Qubic), priority flag, and risk score.

If tx flagged high risk → immediate individual proof mandatory.

Batch lower risk txs periodically to reduce costs.

Механизмы интеграции ИИ → on-chain действия

Oracle Bridge: ML Decision Service подписывает (oracle) structured signals (signed) и отправляет в контракт-receiver. Контракт имеет набор заранее разрешённых команд: setFeeMultiplier, pauseDirection, requireProofForTxsAbove(N), markValidator(validatorId, mode). Любая критичная команда требует multisig from operators or DAO approval (policy).

Policy Engine: сочетает ML score и business rules:

Если ML_alert == low → only create ticket.

Если ML_alert == medium → soft mitigation (increase fee, throttle).

Если ML_alert == high and predicted loss > threshold → auto action with operator confirmation or immediate pause if emergency_flag.

Безопасность ML (защита моделей и данных)

Защита от data poisoning

Использовать robust statistics; репутационные временные веса; откат к “baseline” при подозрении на poisoning.

Канарные модели и holdout validation streams.

Защита от model evasion

Ensemble моделей, adversarial training (simulate evasion patterns).

Monitor drift: sudden change в распределении признаков → trigger retraining and human review.

Конфиденциальность и безопасность данных

Все приватные логи шифруются. Модели не разглашают PII.

Доступ к моделям через RBAC, секреты в HSM/Secret Manager.

Explainability / Audit

Каждое ML-решение сопровождается explainability payload (top contributing features).

Логи хранятся для аудита на-chain hashes for tamper-evidence.

Операционные процессы (playbook)

Normal ops

ML мониторинг в реальном времени → alerts dashboard.

Weekly retrain: validator scoring, demand forecasting.

Monthly governance proposals: adjust RL reward weights, reserve policy.

Incident (suspicious outflow detected)

Stage 1 (automated): soft throttling on direction + create incident ticket.

Stage 2 (if persistence or high predicted loss): elevated actions (increase fee, request manual approval for pause).

Stage 3 (confirmed exploit): pause relevant contract functions, initiate slashing, activate reserve compensation, open forensic pipeline.

All steps логируются и публикуются в transparency dashboard.

Human in loop

Operator confirmation required for pausing major bridge functions and for slashing validators beyond soft measures.

DAO can override operator decisions via governance.

Меры доверия и прозрачности пользователям

Real-time Proofs & PoR (proof-of-reserves) публикуются (Merkle commitments) — автоматизированный feed.

Explainable Alerts: пользователи видят состояние liquidity pools, why fee increased (aggregate reason) и expected time to normal.

Audit & Bug Bounty: публикуются ML models high-level design и code for review; external security audit of both ML infra and smart contracts.

Валидация, тесты и метрики успеха
Метрики (KPI)

Mean Time To Detect (MTTD) аномалий. Цель: < 1 min for high-severity.

False Positive Rate (FPR) на anomaly detection — удерживать < X% (например <5% для high alerts).

% of transfers processed instant via validators.

Liquidity reserve ratio (min_target e.g. 10% of 30-day avg flow).

Revenue vs OpEx (самоокупаемость): fee revenue >= running cost within T месяцев.

User satisfaction / failed transfer rate.

Тестирование

Backtest ML models on historic data + synthetic attacks (reentrancy, mass withdrawals, validator collusion).

Red team (penetration test) с ML adversary scenarios.

Shadow rollouts: RL agent действует в shadow mode (предлагает fees) без on-chain effect, сравнение с baseline.

Риски и способы их снижения

Over-automation risk: автоматические паузы → пользовательский ущерб.
→ Решение: conservative policy, human approval for critical actions, gradual throttling.

ML model manipulation (poisoning/evasion):
→ Решение: data provenance, anomaly ensembles, adversarial training, canary models.

Cost explosion (proof generation, light client upkeep):
→ Решение: batching, adaptive verification (разные уровни для разного размера транзакций), fee allocation.

Governance attack / corrupted DAO decision:
→ Решение: strata of checks — operator multisig + time locks + insurance fund.

Примерные технические спецификации и стек

Data ingest & streaming: Kafka / Kinesis, change data capture from nodes.

Feature store: Feast / custom store.

Models: PyTorch / TensorFlow для sequence/GNN; scikit-learn/XGBoost для таблички.

Serving: Triton / TorchServe / FastAPI endpoints. Low latency for anomaly detection (<200ms).

Orchestration: Kubernetes, ArgoCD for infra.

Secrets / Keys: HSM for signing ML oracle outputs.

On-chain integration: Oracle signer uses threshold signature (to avoid single key compromise).

Logging & Audit: Immutable logs in object storage + merkleized on-chain anchors.

Псевдокод: простая аномалия + action flow (упрощённо)
# streaming handler (simplified)
for tx in tx_stream:
    features = featurize(tx)
    score_if = isolation_forest.score(features)
    score_ae = autoencoder.reconstruction_error(features)
    graph_anom = gnn.detect(features)

    alerts = 0
    if score_if > IF_THRESHOLD: alerts += 1
    if score_ae > AE_THRESHOLD: alerts += 1
    if graph_anom: alerts += 1

    if alerts >= 2:
        # create incident
        incident = create_incident(tx, scores=[score_if, score_ae], explain=explain(features))
        # policy engine
        policy = policy_engine.evaluate(incident)
        if policy.action == "soft_throttle":
            apply_throttle(direction=tx.direction, factor=policy.factor)
        elif policy.action == "pause" and operator_confirms():
            onchain.pause(direction=tx.direction)
        notify_dashboard(incident)

Roadmap внедрения (эпохи)

Phase 0 (0–2 мес): data collection infra, baseline monitoring, simple rule engine, basic anomaly detector.

Phase 1 (2–6 мес): deploy validator scoring, demand forecasting, basic RL in shadow; integrate with dashboard.

Phase 2 (6–12 мес): RL live with conservative constraints, batching proof planner, automated incident assistant (human approval).

Phase 3 (12+ мес): расширенная автоматизация, DAO-governed policies, full explainability + audits.

Заключение — почему это безопасно и жизнеспособно

Гибридный подход (валидаторы + light client) даёт нужный компромисс между UX и безопасностью.

ИИ-слой делает систему проактивной: обнаруживает ранние признаки атак, прогнозирует ликвидность, оптимизирует комиссии, но не заменяет человеческую ответственность — критичные решения проходят через policy + multisig/DAO.

Защита ML от атак, explainability и аудитируемость обеспечивают прозрачность и приемлемую операционную модель.

Модель масштабируема: можно начать с базовых ML инструментов и постепенно вводить более рискованные автоматизации по мере накопления данных и доверия.
---

# 5. Detailed Development Plan (10 Months)

<img width="667" height="342" alt="Снимок экрана 2025-09-24 в 19 05 45" src="https://github.com/user-attachments/assets/c40c12cf-58e9-4e90-b661-70beae041f76" />



### **Phase 1 – Foundations (Months 1–2)**

* Requirements gathering with Qubic Core-tech team.
* Decide bridge model (Lock/Mint vs Liquidity Pool).
* Select security model (validator, light client, hybrid).
* Draft architecture diagrams, threat models, governance design.
* Develop initial prototypes of smart contracts.
* UX/UI mockups for wallet integration and transfer flows.

**Deliverables:** Architecture documents, prototype contracts, draft UI, API design.

---

### **Phase 2 – Core Development (Months 3–4)**

* Full implementation of smart contracts on Qubic and Solana.
* Backend/off-chain components: relayers, event monitoring, proof relays, nonce management.
* Frontend: wallet integrations (Phantom, Solflare, Qubic Wallet), transfer dashboard, fee/slippage options.
* Setup of liquidity provisioning model for testnet.
* Integration of database for transaction state management.

**Deliverables:** Testnet-ready bridge (contracts, backend, frontend).

---

### **Phase 3 – Testing & Audit Preparation (Months 5–6)**

* Unit, integration, and load testing.
* Deploy to testnets with simulated liquidity flows.
* Prepare for external audit: documentation, coverage reports, threat analysis.
* Participate in external smart contract audit (funded by Qubic).
* Fix issues and resubmit for approval by Qubic Core-tech.

**Deliverables:** Audited and QA-approved contracts, testnet bridge validated.

---

### **Phase 4 – Deployment & Stabilization (Months 7–8)**

* Deploy bridge on Qubic and Solana mainnets.
* Bootstrap liquidity pools or reserves.
* Activate monitoring systems (dashboards, alerts, logs).
* Launch support channels for users and developers.
* Incident response playbook in place.

**Deliverables:** Fully operational mainnet bridge with liquidity and monitoring.

---

### **Phase 5 – Optimization & Governance (Months 9–10)**

* Optimize performance, latency, and transaction costs.
* Bug fixes, upgrades, liquidity rebalancing.
* Establish governance framework (upgrades, emergency patching, multi-sig).
* Final documentation: API references, integration guides, operational manuals.
* Knowledge transfer and handover to Qubic team.

**Deliverables:** Optimized production bridge with governance and documentation.

---

# Work Schedule 

| Months | Activities                                                        |
| ------ | ----------------------------------------------------------------- |
| 1–2    | Foundations: Architecture, design, prototyping                    |
| 3–4    | Core Development: Contracts, backend, frontend, testnet liquidity |
| 5–6    | Testing & Audit: QA, audit prep, external review                  |
| 7–8    | Deployment & Stabilization: Mainnet launch, liquidity, monitoring |
| 9–10   | Optimization & Governance: Performance, upgrades, handover        |

---



# 6. Budget Allocation $90,000

| Month     | Focus                                                        | % of Budget | Amount (USD) |
| --------- | ------------------------------------------------------------ | ----------- | ------------ |
| **1**     | Architecture, design, initial contracts, backend setup       | 20%         | $18,000      |
| **2**     | Core development (contracts, backend, UI, liquidity testnet) | 25%         | $22,500      |
| **3**     | Testing, QA, documentation, audit preparation                | 20%         | $18,000      |
| **4**     | Audit fixes, mainnet deployment, liquidity provisioning      | 20%         | $18,000      |
| **5**     | Stabilization, monitoring, support, handover                 | 15%         | $13,500      |
| **Total** |                                                              | 100%        | **$90,000**  |


Alignment with RFP:

First milestone ≤ 20% (we allocated exactly 20%).
Final milestone ≥ 30% (combined Month 4 + 5 payments = 35%)
Ensures compliance with QA and milestone-based payments



# ‭7.Каналы поддержки
 Настроим саппорт через Дискорд и телеграм  

# 8. Team. Master List of Specialists Needed

### **Leadership & Oversight**

* **Project Manager** – oversee timeline, milestones, and communications.
* **Solution Architect / Team Lead** – design bridge architecture, ensure compliance with security and interoperability requirements.

### **Development**

* **Smart Contract Developers (2–3)** – build and test Qubic & Solana contracts (Lock/Mint, Liquidity Pool).
* **Backend Developers (2)** – event monitoring, relayers, proofs, APIs, database.
* **Frontend Developers (1–2)** – wallet integration, transfer UI, dashboards.

### **Infrastructure & QA**

* **DevOps Engineer (1)** – CI/CD, deployments, monitoring setup.
* **QA/Test Engineer (1)** – unit, integration, and stress testing, regression testing.

### **Security**

* **Security Engineer (1)** – internal audits, incident response, coordinate external audits.

### **Operations & Support**

* **Liquidity/Operations Specialist (1)** – manage liquidity provisioning and balancing.
* **Support & Community Manager (1)** – user support channels, FAQs, documentation.

‭
