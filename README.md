# Qubic - Solana. 
This bridge connects the Qubic network with the Solana network, allowing users to transfer tokens between the two networks. 


# 1 Project Scope‬
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



# 2. Qubic ↔ Solana bridge architecture block diagram:

User → Wallets and Smart Contracts (Qubic / Solana)

Event Monitoring → Relayers / Validators

The central database and monitoring system records all transactions.

‭<img width="501" height="349" alt="Снимок экрана 2025-09-25 в 15 09 23" src="https://github.com/user-attachments/assets/700c89e4-962a-4f6a-9da0-fe2d7a34479d" />

---
## Liquidity Pool Model

How it works:
Liquidity pools are created in advance on both networks (Qubic ↔ wQubic).
A user deposits a token into a pool on one network and instantly receives the equivalent from a pool on the other network.
The balance is maintained through automatic rebalancing and fees.

Pros:
- Instant transfers with no verification wait.
- More user-friendly.
- Ability to reward liquidity providers with fees.

Cons:
- Requires constant liquidity maintenance in pools.
- Risk of liquidity shortages during large imbalances.
- More complex reserve management.
 
 ---
 
Liquidity Pool = speed and convenience.
‭
Qubic ↔ Solana (Liquidity Pool) bridge flowchart:
Transfer Request, Sign & Submit Tx, Deposit Tokens — from the user to the Qubic pool.
Swap Event, Release Tokens, Receive Tokens, Confirmation — flow through relayers to Solana.
Log Events, Tx Records, Pool Balance — parallel recording and monitoring.

‭
<img width="574" height="347" alt="Снимок экрана 2025-09-26 в 12 39 44" src="https://github.com/user-attachments/assets/24e6e309-6beb-47d3-aa5d-ba41af47cb04" />

## Liquidity Maintenance: Creation and Implementation of the Mechanism

The liquidity management mechanism must ensure:
Sufficient reserves on both sides of the bridge to handle user transactions without delays.
Risk minimization related to volatility, liquidity drain, and malicious behavior.
Sustained user trust through transparent operations and reliable execution of transfers.

---
## Liquidity Provisioning (Initial Setup)

Dual-Side Pools: Create liquidity pools on both chains (e.g., QUBIC pool and wQUBIC pool on Solana).
Bootstrapping Liquidity: Initial liquidity provided by the bridge operator and early backers.
Incentive programs for external liquidity providers (LPs), e.g., yield farming rewards or share of transaction fees.

---




# 3. Qubic ↔ Solana token transfer bridge workflow. 
using the Liquidity Pool model as an example

<img width="709" height="458" alt="Снимок экрана 2025-09-25 в 20 29 08" src="https://github.com/user-attachments/assets/0ff01598-b716-47af-955d-343f82e6389c" />

### Main processes in the operation of the bridge

1. **Initiation → token deposit into the pool.**
2. **Event monitoring → transmission to validators.**
3. **Validator confirmation → generation of a cross-chain message.**
4. **Issue tokens from the target network pool.**
5. **User confirmation → status update.**
6. **Logging and monitoring for security and auditing.**

---

### Important points

* Maintaining sufficient liquidity on both sides.
* Protection against double-spend and replay attacks.
* Reliable validator operation (multisig, key protection).
* Continuous pool and system monitoring.
---

### 1. **User initiation**

* The user opens the **Frontend/UI** (web interface or dApp).
* Connects their wallet: **Qubic Wallet** and/or **Solana Wallet**.
* Selects:

 * source (Qubic or Solana),
* number of tokens,
* bridge model (**Liquidity Pool**),
* destination address.
* The system displays **fees**, **pool balance**, and **estimated transfer time**.

---

### 2. **Transaction Submission**

* The user confirms the operation in their wallet (signs the transaction).
* Tokens are sent to the **Liquidity Pool on the source network** (e.g., Qubic).
* The pool’s smart contract records the deposit and generates a deposit event.

---

### 3. **Monitoring and Verification**

* The **Event Monitor** on the Qubic side captures the deposit event.
* Information is forwarded to the **Relayer / Validators** (the bridge network layer).
* Validators check the correctness of the transaction:

  * user signatures,
  * nonce uniqueness (to prevent replay execution),
  * pool balance.

---

### 4. **Cross-Chain Coordination**

* After confirmation, validators create a message for the target network (Solana).
* This message includes:

  * transaction ID,
  * amount and token,
  * recipient address.
* Validators sign the message (multisig) and send it to the **Liquidity Pool on Solana**.

---

### 5. **Funds Release on the Target Network**

* The **Liquidity Pool (Solana)** receives the instruction from the validators.
* The smart contract verifies the signatures and data.
* The corresponding tokens are deducted from the Solana pool (e.g., wQUBIC → QUBIC, or vice versa).
* Funds are transferred to the **recipient’s Solana Wallet**.

---

### 6. **User Confirmation**

* The **Frontend/UI** receives a “Transaction Completed” status from the Solana network.
* The user sees:

  * transaction hash,
  * confirmation of receipt,
  * updated wallet balance.

---

### 7. **Monitoring and Reporting**

* All events (deposit, release, status) are logged in the **Database & Monitoring** system.
* The monitoring system:

  * tracks pool balances,
  * logs errors, timeouts, retries,
  * alerts about issues (e.g., liquidity shortage).
* In case of failure:

  * the transaction may be rolled back,
  * tokens returned to the source wallet.
---



# 4. Bridge with a set of validators/relays and a liquidity pool:                                              


| Revenue Source                              | How It Is Generated                                                                                                                | Distribution / Shares                                                                             | Notes                                                                             |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Transaction Fees (bridge fee)               | Users pay a fee for transfers (fixed or percentage-based)                                                                          | Example: 50% → validators / relayers / operators, 30% → security reserve, 20% → DAO / development | Fees can be differentiated (priority transfer, large amount, etc.)                |
| Income from Liquidity / Locked Assets       | Assets locked in bridge pools can participate in staking or yield strategies                                                       | Part of the income goes to the bridge (e.g., 20–30%), the rest to liquidity providers (LPs)       | Safe strategies must be chosen to avoid exposing the bridge’s assets to high risk |
| Subscription Fee / Integration Service      | Projects that want to integrate their token or service through the bridge pay for API / SDK / maintenance                          | 100% → bridge / DAO                                                                               | A stable source of revenue regardless of user traffic                             |
| Relay / Message Fee                         | If the bridge supports not just tokens but arbitrary messages/contracts, relayers charge a fee                                     | For example: fixed fee + variable component                                                       | Particularly relevant in GMP-bridges                                              |
| Share from Internal Bridge Operations/Swaps | If the bridge is integrated with a DEX / cross-chain swap, part of the spread/fee goes to the bridge                               | For example: 0.1–0.3% of the swap amount                                                          | Requires integration with DEX mechanisms                                          |
| Security Reserve / Penalties                | In case of errors, abuse, or misbehavior, part of the penalties or fees remains in the reserve fund, which covers potential losses | The fund accumulates up to a defined threshold                                                    | Reduces risks and increases trust                                                 |


The transfer fee may be 1-2%.

---

# 5. Secure Hybrid Qubic ↔ Solana Bridge with AI-based Verification & Validation

## Executive summary

A Hybrid security architecture combining:

* **External validator network** (fast, validator-first execution with threshold signatures), and
* **Cryptographic verification via light clients** (trust-minimizing checks),

augmented with an **AI layer** that performs real-time monitoring, anomaly detection, validator risk scoring, liquidity forecasting, dynamic fee optimization, and decision support. AI assists validation and verification: validator actions remain primary for latency-sensitive flows, while AI + light-client proofs provide a cryptographic safety net and automated risk mitigation.

Key goals:

* fast, UX-friendly transfers;
* robust, multi-layered security;
* automated detection/response to attacks and anomalies;
* self-sustaining operations via fee optimization and liquidity management;
* auditable, explainable AI decisioning with human-in-the-loop controls for critical actions.

---

## High-level architecture (components & responsibilities)

### On-chain components

* **Qubic Bridge Contract** (on Qubic):

  * lock/unlock, pool accounting, events emitter, pause/slash hooks, upgradeability.
* **Solana Bridge Contract** (on Solana):

  * mint/burn wQUBIC, pool accounting, dispute handling.
* **Light-Clients / Proof-Verification Modules:**

  * simplified light-client components deployed on each chain capable of verifying headers / Merkle proofs from the opposite chain.
* **Fee & Reserve Contracts:**

  * collectors for fee shares, reserve fund contract, staking/slashing interfaces.

---

### Off-chain components

* **Validator Network (N nodes)**:

  * monitors on-chain events; signs cross-chain messages; participates in threshold multisig for finalization.
* **Relayers**:

  * transport signed messages and proofs between chains.
* **Data Collector & Monitoring**:

  * streaming ingestion of on-chain events, mempool, validator metrics, infra telemetry, price oracles.
* **AI Engine (Off-chain)**:

  * real-time anomaly detection, validator risk scoring, demand forecasting, RL-based fee optimizer, proof-batching optimizer.
* **Policy / Decision Service**:

  * business rules + ML signals → recommended or automated actions. Enforces human-in-the-loop policy for critical actions.
* **Operator Dashboard & Incident Console**:

  * incident triage, ML explainability, dashboards, audit logs.
* **Governance & Treasury**:

  * DAO proposals, treasury to top-up liquidity/reserves, manage policy params.

---

### Integration layer

* **Oracle / Signed Decisions**:

  * Decision Service signs structured signals (signed messages) and relayers pass them to on-chain contracts to execute allowed actions (set fee, enable emergency pause, request proof enforcement).
* **Logging & Audit Anchoring**:

  * immutable audit entries (hashes) stored on-chain periodically for tamper-evidence.

---

## Data flows and typical transaction lifecycle

### Example: QUBIC → wQUBIC (Qubic → Solana)

1. **User** initiates lock on Qubic bridge contract (lock event emitted).
2. **Validator network** observes event and performs off-chain consensus. When threshold reached (e.g., 4/7), validators produce a threshold signature and submit the signed message to relayers.
3. **Relayers** forward signed message to Solana bridge contract.
4. **Solana bridge contract** mints wQUBIC to user (near-instant UX).
5. **Parallel**: Data Collector feeds the event to AI Engine which:

   * generates a proof verification job (light-client / Merkle proof) or schedules a zk-batch proof verification.
   * computes risk/ anomaly score for the specific event or validator signature pattern.
6. **Light client or proof** is submitted within a configured window and verified on Solana. If verification fails or AI flags fraud, dispute process is triggered; slashing/top-up/rescue is executed per policy.

---

## Roles of AI — concrete functions

### 1. Real-time Anomaly Detection

* Purpose: detect unusual withdrawal patterns, coordinated mass-exit, mempool manipulation, bot attacks.
* Inputs: per-tx features (amount, frequency, origin cluster), pool balances, validator signature timing, IP/geo telemetry, price feeds.
* Models: ensemble (streaming Isolation Forest, sequence autoencoders / LSTM, graph anomaly detector for address clusters).
* Outputs: alert level (low/medium/high), affected txs, explainability features.

### 2. Validator & Relayer Risk Scoring

* Purpose: score validators by reliability and risk (uptime, signature timing anomalies, infra indicators, stake history).
* Models: gradient-boosted trees (XGBoost) with temporal features and exponential decay; periodic profile clustering.
* Outputs: per-validator risk score ∈ [0..1], suggestions to reduce weight in multisig or require secondary proof for a node’s signatures.

### 3. Liquidity Forecasting & Dynamic Fee Optimization (RL)

* Purpose: predict direction-specific demand and set fees to maintain balanced pools and cover costs.
* Modules:

  * Demand forecasting: Prophet / LSTM time-series for per-direction volumes.
  * RL fee optimizer: constrained RL (PPO) that sets fee multipliers, LP incentives, or temporary caps.
* Rewards blend revenue vs. risk (reserve depletion penalty, user drop, slippage cost).

### 4. Proof-Batching / Proof Planner

* Purpose: decide when to generate/verfiy proofs individually or batched (cost vs. risk tradeoff).
* Heuristic + learned cost model that minimizes verification cost subject to risk constraints (flagged txs forced to immediate proof).

### 5. Incident Response Assistant & Playbook Ranking

* Purpose: rank recommended mitigations (soft-throttle, raise-fee, pause-direction, slashing) using historical outcomes and model-based simulation.
* Ensures human operator / DAO sees ranked options with expected impacts.

### 6. Explainability & Audit Trails

* All AI decisions must produce explainability payloads (e.g., SHAP values, top features) and a signed audit log for governance and external auditors.

---

## Verification & validation lifecycle (AI + cryptography)

1. **Primary fast path** — validator threshold signatures finalize transfers quickly for UX.
2. **Secondary verification** — either:

   * Light-client verification of chain headers + Merkle proofs, OR
   * Batched zk- or succinct proofs when feasible (costly but strong).
3. **AI verification** adds:

   * anomaly detection at time of event,
   * validator score check (if validator low score → force proof or reject),
   * scheduling of proof verification and marking for dispute if mismatch.
4. **Dispute resolution**: if proof invalid or AI signals high fraud probability:

   * freeze affected assets on destination chain (via emergency pause),
   * activate slashing of malicious validators,
   * use reserve fund to protect users while forensic analysis proceeds,
   * propose DAO governance action if required.

---

## Key policies & safety constraints

* **Human-in-the-loop for critical actions**: any automatic pause, large slashing, or reserve usage requires operator sign-off or multi-sig + DAO time-lock depending on thresholds.
* **Action tiers**:

  * Tier 1 (low-risk): AI suggests, auto-applies soft mitigations (increase fee, soft throttle).
  * Tier 2 (medium-risk): AI suggests and requires single operator approval (or multisig) to execute.
  * Tier 3 (high-risk / emergency): automated safeguards (auto-pause) enabled if immediate loss predicted > emergency threshold — but always followed by emergency human review and DAO notification.
* **Fail-safe defaults**: in the event of AI failure, system reverts to validator-only mode with conservative fees and increased proof requirements.
* **Immutable logging & transparency**: all AI signals and actions are logged, and on-chain commitments of logs are made periodically.

---

## Governance & economics

* **Fee split**: part to validator rewards, part to LP incentives, part reserved for insurance/reserve, part to DAO/development.
* **Reserve fund**: sourced from a percentage of fees; used to reimburse in validated exploits; minimum reserve threshold defined (e.g., 10% of 30-day average flow).
* **Staking & Slashing**:

  * Validators stake bonds; slashing rules tied to malicious signatures, excessively anomalous behavior validated by AI + proofs.
  * Dispute and appeal windows are established to prevent false positives.
* **DAO controls**: parameter changes to ML thresholds, fee multipliers, validator set size, emergency thresholds happen through DAO proposals.

---

## ML security & trustworthiness

* **Data integrity & provenance**: signed data feeds, tamper-evident ingestion pipelines.
* **Protect against data poisoning**:

  * Use holdout streams, train on multiple windows, canary models, and robust estimators.
  * Use ensemble voting; require ≥2 independent signals for high-severity decisions.
* **Adversarial resilience**:

  * Periodic adversarial training and simulated attack scenarios.
  * Monitoring for distribution drift triggers human review and rollback to baseline models.
* **Access control & secrets**:

  * All model endpoints behind RBAC; oracle signing keys in HSM; output signatures thresholded to avoid single point compromise.
* **Explainability**:

  * Provide interpretable reasoning (top features, historical precedents) for any decision that affects on-chain state.

---



# 7. APIs & message formats (examples)

### 1. Decision Service → On-chain (signed message)

```json
{
  "action": "increase_fee",
  "direction": "Qubic->Solana",
  "multiplier": 1.5,
  "reason": "liquidity_imbalance",
  "evidence_hash": "0xabc...",
  "timestamp": 1712345678
}
```

* Signed by Decision Service (HSM) and optionally by a threshold of operator keys.
* Contracts accept only pre-authorized action types and require multisig/time-lock for critical actions.

### 2. Validator multisig message (threshold signature)

```json
{
  "type": "finalize_transfer",
  "tx_id": "QUBIC:0x123...",
  "amount": 5000,
  "recipient": "Solana:4Gk...",
  "validators_signatures": ["sig1", "sig2", "sig3", "sig4"],
  "timestamp": 1712345678
}
```

### 3. AI alert webhook (for operator dashboard)

```json
{
  "alert_id": "ALERT-2025-0001",
  "severity": "high",
  "description": "Unusual coordinated outflow from 12 addresses",
  "affected_txs": ["QUBIC:0x...","QUBIC:0x..."],
  "explainability": {"top_features": [{"name":"velocity","value":0.98},{"name":"new_address_count","value":0.93}]},
  "recommendation": {"action":"soft_throttle", "params": {"direction":"Qubic->Solana", "factor":0.5}}
}
```

---

## Metrics / KPIs

* **MTTD (Mean time to detect)** anomalies — target < 1 min for high-severity.
* **MTTR (Mean time to respond)** for critical incidents — target < 30 minutes operator triage.
* **False positive rate** for high-severity alerts — target < 5%.
* **% transactions processed instant via validators** (fast path) — target > 95%.
* **Reserve coverage ratio** (reserve / 30-day avg outflow) — target ≥ 10–20%.
* **Liquidity imbalance frequency** — targeted reduction with RL fee optimizer.
* **System availability** — > 99.9% (excluding emergency pauses).

---

## Incident response playbook (example)

1. **AI raises high alert** → soft-throttle applied automatically (per policy).
2. **Operator receives alert in dashboard** with evidence & explainability.
3. **Decision**:

   * if evidence weight high => operator approves emergency pause + multisig executed.
   * else => increase fees + request proof verification for flagged txs.
4. **Forensic pipeline** retrieves full history, light-client proofs verified, validators reviewed.
5. **If confirmed exploit**:

   * Slash misbehaving validators (per protocol rules).
   * Use reserve to reimburse legitimate victims.
   * Publish incident report and DAO proposal for further actions.

---

## Deployment roadmap & phased rollout

**Phase 0 — foundational**

* Deploy basic validator network & relayers.
* Implement bridge contracts (lock/mint) and reserve contract.
* Deploy monitoring + simple rule-based alerts.

**Phase 1 — ML pilot & light client**

* Implement Data Collector, simple anomaly detection, validator scoring baseline.
* Deploy light-client proof verification module (basic).
* Policy Engine for recommended actions; human-in-loop required.

**Phase 2 — optimization & automation**

* RL fee optimizer in shadow mode; then conservative live mode.
* Proof-batching optimizer & selective proof enforcement.
* Automated incident assistant (operator approval still required for critical actions).

**Phase 3 — maturity**

* Full automation pathways with strict safeguards, explainability guarantees, DAO-managed policy updates.
* Extensive external audits and public PoR feeds; community bug-bounty.

---

## Implementation considerations & recommended tech stack

* **Streaming & ingestion**: Kafka or Kinesis for real-time event streams.
* **Feature store**: Feast or custom store for model features.
* **ML frameworks**: PyTorch / TensorFlow for sequence/graph models; scikit-learn / XGBoost for tabular scores.
* **Serving**: Triton / TorchServe / FastAPI for low-latency model inference.
* **Orchestration**: Kubernetes, ArgoCD.
* **Secrets & signing**: HSM for signing Decision Service outputs; threshold signing for validator keys.
* **Logging & audit**: Immutable S3 + periodic on-chain anchors for tamper-evidence.
* **Dashboards**: Grafana + custom UI for explainability and incident workflows.

---

## Example pseudocode (simplified) — streaming anomaly + policy decision

```python
for event in transaction_stream:
    features = featurize(event)
    anom_score = ensemble_anomaly.score(features)
    validator_scores = get_validator_scores(event.validators)
    proof_required = False

    if anom_score > HIGH_THRESHOLD:
        create_alert(event, severity="high")
        policy = policy_engine.evaluate(alert)
        if policy.action == "soft_throttle":
            apply_throttle(direction=event.direction, factor=policy.factor)
        elif policy.action == "pause":
            if operator_approve():
                onchain.pause(direction=event.direction)
        proof_required = True

    if any(v.score < VALIDATOR_SCORE_MIN for v in event.validators):
        proof_required = True

    if proof_required:
        schedule_immediate_proof_verification(event)
```

---

## Closing notes

This design offers a pragmatic, layered, and auditable approach:

* **Validators** give the speed users expect.
* **Light clients + proof verification** supply cryptographic guarantees.
* **AI** provides early detection, intelligent orchestration, dynamic economics, and human-guided automated response — improving safety and economic sustainability while preserving human oversight for high-impact decisions.
---




# 6. Detailed Development Plan (10 Months)

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



# 8. Budget Allocation $90,000

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

---



# 9. Support Channels
We'll set up support via Discord and Telegram and others.

---



# 10. Team. Master List of Specialists Needed

## Information about the team and work experience https://lab.tenzorro.com/

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
