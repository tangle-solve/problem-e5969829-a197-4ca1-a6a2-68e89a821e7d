<!-- solve-problem:e5969829-a197-4ca1-a6a2-68e89a821e7d -->
# Dynamic withholding coalitions

A [Tangle Solve problem](https://solve.tangle.tools/problems/e5969829-a197-4ca1-a6a2-68e89a821e7d).

## What is the problem?

**Setting.** A proof-of-work (or similar) blockchain where participants (miners/pools) earn rewards for producing blocks. The standard security question is usually phrased against a single Byzantine adversary of bounded hash rate. Here the adversary is *strategic and coalitional*: a set S of miners, possibly with combined hash power α_S ≤ some threshold (often α_S < 1/4 or < 1/2 depending on the model), may privately withhold blocks and share them only within S, then redistribute the resulting rewards via **unobservable side payments** (off-chain transfers). The coalition is **dynamic**: membership may change over time, entities may operate under multiple pool identities (Sybil identities), and the protocol cannot observe who is coordinating.

**Formal goal.** Design a reward/consensus protocol π such that, for a natural utility model (expected reward, possibly discounting for delayed rewards), following π honestly is an equilibrium (or ε-equilibrium / robust equilibrium) *against deviations by any coalition drawn from a class C of overlapping, time-varying subsets, where coalition membership and side payments are unobservable*. One reasonable formalization: show π is a **coalition-proof / strong Nash equilibrium**, or that no coalition of hash power ≤ α* can gain more than ε by any withholding strategy, even with internal side-payment reallocation. Ambiguity to resolve up front: the exact utility model (risk-neutral expected discounted rewards is standard) and whether "resistant" means no profitable deviation, ε-profitability, or stability of coalition formation itself (cooperative game with externalities — is the grand coalition of honest parties in the core?).

## Why does it matter?

Nearly all blockchain incentive-compatibility results (including selfish-mining resistance and FruitChains' fairness guarantees) assume a *single fixed* adversary; real mining is rife with temporary pools, rented hashpower, and opaque payout deals. A protocol robust to dynamic coalitions would be the first credible answer to pool cannibalization and cartelization (e.g., repeated 51%-rental attacks that no single party would rationally commit to). It would also connect blockchain design to the largely open theory of cooperative games with externalities, giving tools for any decentralized protocol with monetizable deviations.

## What's already known?

- **FruitChains (Pass & Shi 2017)** proves that under a "δ-bounded" adversary (roughly, any adversary with < 1/3 – δ fraction of hash power, viewed as a single unit), honest mining is an ε-Nash equilibrium: deviation gains are amortized away over epochs via fruits. Crucially, its theorem is against **one monolithic adversary**; nothing prevents a *set* of individually honest-looking miners from jointly withholding.
- **Aumayr et al. (2025), arXiv:2503.10185** studies reward sharing among pool members — how to split block rewards proportionally so pool membership itself is incentive-compatible. This handles *intra-pool* stability but not cross-pool, dynamically re-forming withholding coalitions. (Details: verify.)
- Classical results: selfish mining (Eyal & Sirer 2014) shows a single miner with > ~1/4 hashpower profits from withholding; several patched protocols raise this threshold. These are all single-adversary analyses.
- Known limitation: with unobservable side payments, mechanism design tools that rely on transfer monitoring fail; robustness must come from the *protocol's reward geometry* making withholding jointly unprofitable, not from detection.

## What would count as solving it?

1. **Protocol + theorem:** A concrete protocol and a proof that for every coalition strategy σ of any coalition with total hash power ≤ α*, evolving arbitrarily over time with arbitrary internal side payments, expected utility of each member under σ exceeds honest utility by at most ε. State α* and ε explicitly.
2. **Lower bound / impossibility:** A proof that above some hash-power threshold (or under weaker observability), *every* protocol admits a profitable dynamic withholding coalition — giving a tight characterization.
3. **Intermediate result:** A positive result in a restricted but well-defined model (e.g., coalitions may only grow/shrink at epoch boundaries, side payments observable in aggregate), with a clear statement of the relaxation.

## How to start

1. Read Pass & Shi (2017) §"δ-bounded adversaries" and the equilibrium theorem; identify exactly which quantifier structure breaks when the adversary is a coalition of individually small players (it likely does break — that's your gap).
2. Formalize the dynamic-coalition deviation as a repeated game with transferable utility; check whether known repeated-game results (e.g., collusion sustainability via continuation values) can be turned into a *protocol* design principle rather than a negative result.
3. Look at the utility geometry of FruitChains: fruits are small, frequent rewards — ask whether making rewards finer-grained can make the "withhold and release jointly" deviation non-implementable without observable coordination.
4. Main obstacle: unobservable side payments mean any punishment must be unilateral-incentive-compatible; detection-based defenses are dead ends.

## Key references

- Pass, R., Shi, E. (2017). *FruitChains: A Fair Blockchain*. PODC 2017. DOI:10.1145/3087801.3087809 — Shows a fruit-based chain makes honest mining an ε-Nash equilibrium against a single δ-bounded adversary with temporally amortized rewards. https://doi.org/10.1145/3087801.3087809
- Aumayr, V., et al. (2025). *Optimal Reward Allocation via Proportional Splitting*. arXiv:2503.10185 — Analyzes proportional reward splitting within pools so that pool participation and honest mining align. https://arxiv.org/abs/2503.10185 (exact author list/claims: verify)
- Eyal, I., Sirer, E.G. (2014). *Majority is not Enough: Bitcoin Mining is Vulnerable*. FC 2014 — Quantifies selfish-mining profitability for a single withholding miner; baseline for single-adversary analysis.
- (Background) Bernheim, Peleg, Whinston (1987), *Coalition-Proof Nash Equilibria* — the standard solution concept when coalitions deviate without binding agreements.

## Warnings

- Do not assume identity = pool: the problem explicitly allows multiple pool identities per economic entity, so any defense relying on "pool X is the deviator" fails.
- The census record's "Aumayr et al. (2026)" appears to be a forward-dated citation; the arXiv ID is real as of 2025 but verify its content before relying on it.
- "Bounded-coalition guarantees" in existing work typically bound hash power of a *fixed* set; dynamic membership is genuinely outside those theorems — don't cite them as covering this.
- Beware of "solution" concepts (strong Nash, coalition-proof Nash) whose existence in general games is not guaranteed; you may need ε-approximations or restricted strategy classes.

---
*`BCGT-000227` · flagged for review*

## Acceptance guidance

1. **Protocol + theorem:** A concrete protocol and a proof that for every coalition strategy σ of any coalition with total hash power ≤ α*, evolving arbitrarily over time with arbitrary internal side payments, expected utility of each member under σ exceeds honest utility by at most ε. State α* and ε explicitly.
2. **Lower bound / impossibility:** A proof that above some hash-power threshold (or under weaker observability), *every* protocol admits a profitable dynamic withholding coalition — giving a tight characterization.
3. **Intermediate result:** A positive result in a restricted but well-defined model (e.g., coalitions may only grow/shrink at epoch boundaries, side payments observable in aggregate), with a clear statement of the relaxation.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e5969829-a197-4ca1-a6a2-68e89a821e7d?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000227.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000227)
