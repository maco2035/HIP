# HIP-150: Amendment to HIP-149 — Algorithmic Utility Realignment & Dynamic Supply

- **Author(s):** maco2035
- **Start Date:** 2026-06-09
- **Category:** Economic, Technical
- **Original HIP PR:** *(pending)*
- **Tracking Issue:** *(pending)*
- **Status:** Draft
- **Amends:** HIP-149 (pending)

---

## Summary

This proposal amends HIP-149 to establish a sustainable, permanent funding mechanism for network utility and core operations.

HIP-150 adopts the core utility and oversight upgrades proposed in HIP-149 — retiring Proof-of-Coverage (PoC), establishing a Deployer Earnings Floor, and seating an Advisory Council — but completely replaces the funding mechanism and council structure. The 141M HNT one-time capitalization and the maximum supply cap are replaced with an algorithmic, dynamic supply model analogous to Ethereum's post-Merge emission design.

By implementing an uncapped total supply with a fixed daily emission rate targeting **5% total annualized inflation**, this HIP guarantees perpetual funding for both deployers and network operations. Excess emissions fuel subDAO ecosystem growth, while the Burn-and-Mint Equilibrium (BME) programmatically eliminates inflationary death spirals through a hard daily mint ceiling.

---

## Motivation

HIP-149 correctly identifies a critical flaw in the current network trajectory: deployers routing real data must be rewarded for verifiable utility rather than mere coverage existence via PoC, and Nova and core network operations require sustained, predictable funding.

However, the current HIP-149 draft proposes solving the operations funding problem by minting a large, front-loaded lump sum (~141M HNT) into a centralized treasury managed by a human Advisory Council, while simultaneously extending a hard cap on total supply. This approach has two structural weaknesses:

1. **It is a bailout, not a model.** A one-time treasury injection provides finite runway. Once that supply is consumed, the network faces the same funding crisis it faces today. There is no permanent mechanism to fund Nova or any future operational need without another community vote and another dilutive mint event.

2. **Hard caps constrain DePIN networks.** Physical infrastructure networks require continuous economic incentives to attract deployers as coverage expands. A fixed supply curve that declines to zero eventually starves the network of the capital needed to fund that growth, unless real-world burn at sufficient scale has already taken over — a bet that is difficult to guarantee.

For a decentralized physical infrastructure network (DePIN), a sustainable model requires an emission mechanism with **infinite runway**. A dynamic supply with a fixed daily minting allowance provides exactly that. It ensures that as data demand fluctuates, the network continuously injects capital into deployer incentives and operations, while the BME's burn mechanics naturally drive the network toward net-deflation as carrier utility scales.

---

## Stakeholders

- **Hotspot Deployers (Mobile & IoT):** Transition from PoC-based earnings to utility-based earnings with a USD-anchored floor during normal market conditions, plus a proportional share of excess emissions from the deployer stream.
- **HNT Token Holders:** Transition from a nominally fixed-supply asset to a dynamic-supply asset. Holders benefit from massive, structural deflationary pressure whenever the network experiences high carrier burn or an HNT price crash triggers the failsafe.
- **Service Providers & Carriers:** Benefit from a continuously and predictably funded deployer network without the risk of incentive starvation.
- **Nova & Core Network Builders:** Receive a perpetual, programmatic stream of operational funding — the 2% Operations Stream — rather than a finite lump-sum treasury that must be re-authorized by governance each time it is depleted.
- **Mobile & IoT subDAOs:** Receive surplus tokens from the 3% Deployer Stream proportional to their utility scores whenever carrier demand is insufficient to absorb the full daily deployer mint.

---

## Detailed Explanation

This proposal amends HIP-149 in two areas: (1) the funding mechanism, and (2) the Advisory Council structure. All other provisions of HIP-149 — particularly the transition away from PoC and the implementation of utility-based rewards — are retained as written.

### 1. Abolishing the Maximum Supply Cap

The current maximum supply cap (historically 223M HNT) is permanently removed. The network transitions to an uncapped, dynamic supply model to ensure perpetual operational funding regardless of the network's age, token price, or total circulating supply at any given time.

This is not unprecedented in major networks. Ethereum removed its supply cap in favor of a dynamic emission model and subsequently became a net-deflationary asset as network usage scaled. The Helium network can achieve the same outcome through the BME mechanics described below.

### 2. Fixed Algorithmic Emission Target (5% Annualized)

To fund the Deployer Earnings Floor, fuel the ecosystem, and provide permanent operations funding, the protocol transitions to a fixed emission schedule targeting **5% total annualized inflation** of the circulating supply.

#### The Split

The 5% is divided into two distinct daily streams:

| Stream | Annualized Rate | Purpose |
|---|---|---|
| Deployer & Ecosystem Stream | 3% | Funds Deployer Earnings Floor; surplus to subDAO treasuries |
| Operations Stream | 2% | Funds Nova and core network development via Advisory Council |

#### Daily Epoch Mint

The annual targets are computed against the **rolling 12-month circulating supply** and divided by 365. These amounts are minted at every daily epoch with no discretionary override.

**Formula:**
```
Total Daily Mint  = (Circulating Supply × 0.05) / 365
Deployer Daily    = (Circulating Supply × 0.03) / 365
Operations Daily  = (Circulating Supply × 0.02) / 365
```

**Example** (Circulating Supply = 200,000,000 HNT):

| Metric | Calculation | Result |
|---|---|---|
| Total Daily Mint | (200M × 0.05) / 365 | ~27,397 HNT |
| Deployer Stream (3%) | (200M × 0.03) / 365 | ~16,438 HNT |
| Operations Stream (2%) | (200M × 0.02) / 365 | ~10,959 HNT |

### 3. Deployer Earnings Floor & Surplus Distribution (The 3% Stream)

Deployers earn HNT pro-rata of rewardable bytes transferred. A USD-anchored backstop supplements that baseline earnings whenever it falls below **50% of the carrier-paid burn rate** — for example, guaranteeing $0.05 of HNT to the deployer for every $0.10 burned by the carrier.

The protocol routes the 3% daily deployer mint according to one of two states:

#### State A: Normal Operations (Surplus)

The protocol allocates the minimum HNT necessary from the 3% stream to fulfill the Deployer Earnings Floor. All remaining surplus HNT from the 3% stream is distributed to the Mobile and IoT subDAO treasuries, proportional to their current utility scores. This injects ecosystem capital during periods of healthy network activity.

#### State B: Death Spiral Failsafe (Deficit)

If the price of HNT falls severely such that fulfilling the USD-pegged deployer floor would require more HNT than the protocol's fixed daily 3% allowance, the failsafe activates:

- The **entire** 3% daily deployer mint is distributed to deployers.
- The deployer earnings floor **temporarily breaks** — deployers receive less than the $0.05 target, but are not left with nothing.
- No additional HNT is minted beyond the fixed daily cap under any circumstances.
- No surplus flows to subDAOs during a failsafe state.

The failsafe enforces a strict priority: **survival of the currency over the USD deployer guarantee.** Because an HNT price crash forces carriers to burn vastly more tokens to acquire the same USD value of Data Credits, the net effect is extreme, structural deflation — exactly when it is most needed to restore price equilibrium.

### 4. Continuous Operations Funding & The Advisory Council (The 2% Stream)

Unlike the 141M HNT lump sum proposed in HIP-149 — which is finite by design — this proposal provides **perpetual operations funding** through a dedicated daily algorithmic stream.

#### Operations Vault

The 2% daily mint is deposited directly into a Squads multisig vault each epoch, without any discretionary gate. Nova and core development teams are never dependent on a governance vote to maintain baseline funding.

#### Authorized Use of Funds

The Advisory Council is authorized to direct the Operations Stream toward the following purposes:

- Nova Labs engineering and protocol development
- International carrier expansion and business development
- Deployer growth programs and subsidies
- Regulatory and legal costs
- Ecosystem grants to builders advancing network utility
- Core operating costs (infrastructure, security audits, etc.)

#### Advisory Council Structure

A **7-seat Advisory Council** maintains standing oversight of the Operations Vault. This structure amends the council composition described in the original HIP-149 draft as follows:

| Seat(s) | Selection Method | Count |
|---|---|---|
| Nova-designated | Appointed directly by Nova | 2 |
| Mobile Working Group | Designated by Mobile WG / Board | 1 |
| IoT Working Group | Designated by IoT WG / Board | 1 |
| Community-elected | veHNT-weighted election, staggered 3-year terms | 3 |
| **Total** | | **7** |

The Council holds signing authority on the Squads multisig and is responsible for publishing quarterly transparency reports to the community. Major expenditures above a defined threshold (to be set in the implementation specification) must be escalated to a community governance vote.

> **Note for implementation:** Exact parameters for the community seat nomination threshold (minimum veHNT), election cadence, replacement election mechanics, and Council compensation (if any) are to be finalized prior to formal submission. These are flagged as open items.

### 5. Burn-and-Mint Equilibrium & Deflationary Mechanics

Under this model, BME is determined by the balance between a **variable burn** (driven by carrier Data Credit purchases) and a **fixed 5% mint**.

When a carrier burns HNT to acquire Data Credits (DC), that HNT is permanently destroyed. It is never recycled. Because the protocol mints a constant daily amount regardless of burn activity, the network achieves net deflation at the moment carrier demand destroys more HNT per day than the fixed daily mint produces. There is no minimum burn threshold required to trigger deflation — any single day where carrier burn exceeds ~27,397 HNT (in the 200M example) is a net-deflationary day.

This creates a structural incentive for Nova and service providers to aggressively drive carrier adoption: every marginal carrier burned dollar beyond the daily mint permanently reduces total supply.

### 6. Worked Examples

Assume: Circulating supply = 200,000,000 HNT. Fixed daily epoch mint = ~27,397 HNT (16,438 deployer + 10,959 operations). Carrier burn rate = $0.10/GB; Deployer floor target = $0.05/GB.

#### Case A: Normal Operations — HNT at $10.00

Carriers purchase and burn $100,000 USD worth of data routing in a single day.

| Stage | USD Value | HNT Equivalent | Protocol Outcome |
|---|---|---|---|
| 1. Carrier Burn | $100,000 | 10,000 HNT | Permanently burned |
| 2. Operations Mint (2%) | — | 10,959 HNT | Deposited to Council vault |
| 3. Deployer Mint (3%) | — | 16,438 HNT | 3% deployer stream minted |
| 4. Deployer Floor Payout | $50,000 | 5,000 HNT | Floor fulfilled (< 16,438 cap) |
| 5. Ecosystem Surplus | ~$114,380 | ~11,438 HNT | Distributed to subDAO treasuries |
| **Net Daily Supply Change** | | **+17,397 HNT** | **Mildly inflationary** |

Deployers receive their full floor. Nova and operations are funded. The remaining 11,438 HNT from the deployer stream flows to subDAOs. The network operates well within its 5% design parameters.

#### Case B: Crash Scenario — HNT at $0.01

Carriers still purchase $100,000 USD worth of data. HNT has flash-crashed.

| Stage | USD Value | HNT Equivalent | Protocol Outcome |
|---|---|---|---|
| 1. Carrier Burn | $100,000 | 10,000,000 HNT | Permanently burned |
| 2. Operations Mint (2%) | — | 10,959 HNT | Deposited to Council vault |
| 3. Deployer Mint (3%) | — | 16,438 HNT | 3% deployer stream minted |
| 4. Deployer Floor Target | $50,000 | 5,000,000 HNT | **Blocked by failsafe** |
| 5. Deployer Actual Payout | ~$164 | 16,438 HNT | All available deployer HNT paid out |
| **Net Daily Supply Change** | | **−9,972,603 HNT** | **Hyper-deflationary** |

Nova's operations are funded regardless of price. Deployers split the maximum available HNT from the deployer stream. The extreme deflation — 10M tokens burned vs. 27K minted — creates overwhelming buy-side pressure to restore equilibrium.

### 7. Retiring Proof-of-Coverage

As proposed in HIP-149, PoC rewards are fully retired across the network. All emissions shift to verifiable utility and data transfer. This proposal makes no changes to HIP-149's PoC retirement provisions and mechanics.

---

## Drawbacks

**Psychological shift.** Moving away from a hard-capped token is a paradigm shift for investors accustomed to fixed-supply "digital gold" economics. Community education will be required to communicate the net-deflationary mechanics of the BME and the role of the failsafe.

**Continuous dilution during low-utility periods.** If carrier usage remains insufficient to offset the daily mint for an extended period, the fixed 5% emission will continuously inflate the circulating supply, placing downward price pressure on HNT.

**Deployer floor vulnerability during crashes.** In a catastrophic HNT price crash, deployers will not receive their full USD-pegged floor. The fixed daily mint is a hard ceiling. Deployers temporarily absorb the economic risk to protect the broader token economy and, by extension, their own long-term earning potential.

**Implementation complexity.** Replacing the existing emissions curve with a rolling 12-month circulating supply calculation requires non-trivial smart contract work on Solana. This must be coordinated with core developers to assess feasibility and timeline.

---

## Rationale and Alternatives

**Why not the currently proposed HIP-149?**
The pending HIP-149 draft centralizes ~141M HNT into a multisig wallet requiring ongoing human governance. It acts as a multi-year bailout. Once those funds are consumed, the network faces the same operational funding crisis it faces today — requiring another community vote, another dilutive mint event, and another governance cycle. This proposal eliminates that recurring problem entirely with perpetual, algorithmic emission.

**Why not uncap emissions entirely to guarantee the deployer floor at all times?**
If the protocol were forced to always meet the USD floor without an emission ceiling, an HNT price crash would trigger hyperinflation: the protocol would mint billions of tokens attempting to fulfill a USD guarantee denominated in a collapsing asset, accelerating the collapse in a feedback loop. The fixed daily mint cap is the mechanism that breaks this feedback loop. Destroying the currency to save the deployer guarantee destroys the deployer guarantee as well.

**Why 5% (3% + 2%)?**
The 5% split was chosen to provide meaningful deployer incentives while ensuring operations funding sufficient for Nova and core teams at current treasury valuations. The 3/2 split reflects that deployer activity is the primary driver of carrier burn and network growth — and therefore deserves the larger stream — while the 2% operations stream is sized to sustain Nova and network development costs. These parameters should be stress-tested by the community prior to formal vote. A 4% target (2.5% / 1.5%) is an alternative worth modeling if the community has a lower inflation tolerance.

---

## Deployment Strategy

Implementing this HIP requires coordinating smart contract upgrades with the Helium core developers to accomplish the following:

1. **Remove the maximum total supply variable** from chain state.
2. **Implement the fixed daily mint formula** (5% total, split 3% / 2%) based on a rolling 12-month circulating supply calculation.
3. **Deploy routing logic** for:
   - Deployer Earnings Floor (50% of carrier burn rate as USD peg)
   - Surplus token distribution to Mobile/IoT subDAOs
   - Operations Vault deposits (Squads multisig)
   - Death Spiral Failsafe logic and floor-break handling
4. **Deprecate PoC emission logic** in coordination with HIP-149.
5. **Seat the Advisory Council** via the specified election and appointment processes before the Operations Vault begins receiving funds.

A phased rollout with a shadow-mode observation period (where the new mint formula runs in parallel without affecting actual emissions) is recommended before full activation.

---

## Success Metrics

- Successful, clean transition of deployer rewards from PoC to data utility with no coverage gaps.
- Consistent surplus token distribution to Mobile/IoT subDAOs during healthy network states.
- Perpetual, uninterrupted funding of Nova and core operations via the 2% stream without requiring additional governance votes.
- Zero future occurrences of emergency treasury governance votes for operational funding.
- Net-deflationary supply during periods of high carrier burn, demonstrable on-chain.

