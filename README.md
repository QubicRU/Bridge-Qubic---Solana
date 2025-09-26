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


Комиссия за перевод может составляет 1-2 %. Тогда распределение может быть таким:


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
