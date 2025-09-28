# Web3 Learning Lab  
Smart Contracts (Solidity) + On‑chain Data Analytics 90‑Day Portfolio Project

---

## 0. TL;DR (Elevator Summary)
This repository documents a structured 90‑day journey to build:
- A production‑style DeFi staking protocol (ERC20 → AMM → Multi‑pool Staking + Rewards)
- A reproducible on‑chain analytics pipeline (Uniswap pair metrics, slippage distributions, naive sandwich attack heuristic)
- A security & gas efficiency practice framework (unit tests, fuzzing, invariants, self‑audit, gas reports)
- Weekly public artifacts (blogs, reports, dashboards) for remote‑friendly credibility

If you skim only one section, read: [Roadmap & Milestones](#2-roadmap--milestones)

---

## 1. Objectives

| Axis | Goal | Evidence Artifact |
|------|------|-------------------|
| Smart Contracts | Implement & harden core DeFi patterns | `contracts/` + tests + gas reports |
| Security Mindset | Identify + mitigate common vulnerability classes | `security/checklist.md` + Ethernaut writeups |
| Data Engineering | Build reproducible, parameterized ETL & metric computation | `data/` (raw → processed → notebooks) |
| Quant / Analytics | Generate slippage & concentration insights + heuristic detection | `notebooks/*.ipynb`, `reports/onchain_analysis_final.md` |
| Engineering Hygiene | Versioned artifacts & CI pipeline | Git tags, Actions (optional), structured commits |
| Communication | Weekly log + 6–12 English blogs | `docs/blogs/`, `WEEKLY_LOG.md` |

Success Definition (Day 90):
- Contract suite: ERC20, AMM, multi‑pool staking with rewards, pause/emergency, tests (unit + fuzz + ≥3 invariants)
- Analytics: >5 metrics (tx_count, volume, unique_traders, slippage stats, topN concentration, sandwich suspects)
- Reports: Gas before/after; self‑audit v1; final analytics markdown
- Portfolio: Consolidated README + blog index + milestone tags (`v0.1-m1`, `v0.2-m2`, `v1.0-m3`)

---

## 2. Roadmap & Milestones

| Milestone | Target Day | Core Deliverables | Tag |
|-----------|-----------:|-------------------|-----|
| M1 – Foundations | 30 | ERC20 + AMM + Staking V1 (rewards), initial metrics + slippage plot, 2 blogs | `v0.1-m1` |
| M2 – Integration | 60 | Multi‑pool staking + pause/emergency + gas opt #1 + sandwich heuristic v0 + ≥5 metrics | `v0.2-m2` |
| M3 – Production Ready | 90 | Full tests (unit/fuzz/invariant), audit report, analytics final, gas delta tables, portfolio | `v1.0-m3` |

Visual Progress (checklist mirrored in `打卡.md`)

---

## 3. Repository Structure

```
contracts/
  erc20/        # Minimal ERC20
  amm/          # Constant product AMM
  staking/      # Multi-pool staking & reward accrual
test/           # Foundry test suite (unit/fuzz/invariant)
script/         # Deployment / utility scripts
security/
  checklist.md  # Evolving security checklist
  writeups/     # Ethernaut & CTF notes
data/
  raw/          # Unprocessed logs (CSV / JSON)
  processed/    # Derived metrics tables
  notebooks/    # Jupyter analyses
docs/
  blogs/        # Weekly/biweekly blog markdown
  math/         # Reward math, slippage formulas
  design/       # Architectural notes & diagrams
reports/
  gas/          # gas_wX.md snapshots
  audit/        # audit_report_v1.md
  final/        # onchain_analysis_final.md
tools/          # Helpers (decoders, metric runners)
WEEKLY_LOG.md   # Week-by-week progress reflections
打卡.md           # 90-day task checklist (Chinese)
README.md
```

---

## 4. Setup & Tooling

### 4.1 Prerequisites
| Component | Version (suggested) | Use |
|-----------|---------------------|-----|
| Foundry   | latest nightly       | Solidity build + test + fuzz |
| Node.js   | LTS (optional)       | Front-end / scripting (future) |
| Python    | 3.11+                | Data pipeline & analysis |
| web3.py   | ≥6.x                 | RPC access |
| pandas    | ≥2.x                 | Metrics calc |
| plotly/matplotlib | latest       | Visualization |

### 4.2 Install Foundry
```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

### 4.3 Python Environment
```bash
python -m venv venv
source venv/bin/activate            # Windows: venv\Scripts\activate
pip install web3 pandas matplotlib plotly
```

### 4.4 Environment Variables (optional)
Create `.env` (not committed):
```
RPC_URL=https://mainnet.infura.io/v3/<key>
START_BLOCK=...
END_BLOCK=...
PAIR_ADDRESS=0x...
```

---

## 5. Development Workflow

| Phase | Action | Command / Practice |
|-------|--------|--------------------|
| Pull latest | Sync remote | `git pull` |
| Implement | Write or modify contract | Edit under `contracts/` |
| Test | Run unit + gas | `forge test --gas-report` |
| Fuzz | Run fuzz subset | `forge test --match-test Fuzz` |
| Data Extract | Pull logs | `python tools/fetch_swaps.py` |
| Metrics | Aggregate | `python tools/compute_metrics.py` |
| Commit | Stage & commit | `git add . && git commit -m "feat(staking): add claim()"` |
| Push | Share | `git push` |
| Document | Update README / blog | Add in `docs/blogs/` |
| Tag (milestone) | Freeze snapshot | `git tag -a v0.1-m1 -m "..."; git push origin v0.1-m1` |

---

## 6. Week-by-Week Summary (Condensed)

| Week | Contracts Focus | Data Focus | Security/Test Focus | Key Output |
|------|-----------------|-----------|---------------------|------------|
| 1 | ERC20 basics | Block & timestamp fetch | Ethernaut 1–2 | Blog#1 |
| 2 | AMM (swap logic) | Swap event decode | Ethernaut 3–5 | Blog#2 |
| 3 | Staking V0 | Core metrics (tx, volume, unique) | Ethernaut 6–7 | Blog#3 |
| 4 | Staking rewards V1 | Slippage distribution | Fuzz init | Blog#4 + M1 review |
| 5 | Multi-pool + alloc | Concentration metrics | Invariant base | Blog#5 |
| 6 | Gas Opt #1 | Time-series metrics | Checklist v1 | Blog#6 |
| 7 | Refactor + natspec | Sandwich heuristic v0 | Invariant expand | Blog#7 |
| 8 | Pause & emergency | Outlier filtering | Checklist v2 | Blog#8 + M2 review |
| 9 | Gas Opt #2 (packing) | Interactive dashboard | Invariant (rewards bound) | Blog#9 |
| 10 | View funcs + upgrade slots | Sandwich stats | Self-audit draft | Blog#10 |
| 11 | Polish + scripts | Final notebook assembly | Fuzz breadth | Blog#11 |
| 12 | Tag + packaging | Summary JSON | Reg test + portfolio | Blog#12 + M3 |

Full granular tasks: see `打卡.md`

---

## 7. Running Smart Contract Tests

| Task | Command | Notes |
|------|---------|-------|
| All tests | `forge test` | Default run |
| Gas report | `forge test --gas-report` | Save snapshots in `reports/gas/` |
| Specific file | `forge test --match-path test/Staking.t.sol` | Narrow run |
| Specific test | `forge test --match-test testClaim` | Single case |
| Fuzz tests | Annotate with `forge-config` or fuzz attributes | Add invariants in dedicated test file |
| Invariant tests | `forge test --match-test Invariant` | Use harness structure |

---

## 8. Data Pipeline (Conceptual Flow)

```
          ┌──────────┐
          │  RPC/API │
          └────┬─────┘
               │ (eth_getLogs)
        raw logs (JSON / topics)
               │ decode ABI
          ┌────▼─────┐
          │ raw CSV  │  data/raw/
          └────┬─────┘
        clean / normalize
               │ pandas
          ┌────▼────────┐
          │ processed    │ data/processed/
          └────┬────────┘
         metrics calc (tx_count, volume,
         unique_traders, slippage stats,
         concentration, sandwich suspects)
               │
          ┌────▼────────┐
          │ notebooks/   │ analysis + viz
          └────┬────────┘
               │ export
          ┌────▼────────┐
          │ reports/     │ final markdown & HTML
          └─────────────┘
```

### Primary Metrics
| Metric | Description | Formula / Method |
|--------|-------------|------------------|
| tx_count | Swap events count | row count |
| unique_traders | Distinct addresses | `nunique(address)` |
| volume_token0/1 | Total in/out amounts | sum fields |
| avg_slippage | Mean price impact | `(expected - actual)/expected` |
| p95_slippage | Tail risk | quantile(0.95) |
| top10_share | Top 10 volume / total | sorted cumulative |
| sandwich_suspects | A–V–B pattern count | heuristic window scan |

---

## 9. Reward Accrual Math (Summary)

State:
```
accRewardPerShare (scaled by ACC_PRECISION)
user.rewardDebt = user.amount * accRewardPerShare / ACC_PRECISION
pending(user) = user.amount * accRewardPerShare / ACC_PRECISION - user.rewardDebt
```
Update on pool emission:
```
delta = (blocksElapsed * rewardPerBlock * pool.allocPoint / totalAllocPoint)
accRewardPerShare += delta * ACC_PRECISION / pool.totalStake
```
(Full derivation: `docs/math/reward_accrual.md`)

---

## 10. Security Practice (Surface Overview)

| Category | Mitigation Implemented |
|----------|------------------------|
| Reentrancy | Checks-Effects-Interactions + ReentrancyGuard (if needed) |
| Authorization | Ownable / role gating for admin ops |
| Math Precision | `ACC_PRECISION` scaling + integer arithmetic |
| Pause / Emergency | `pause()` / `emergencyWithdraw()` |
| Reward Integrity | Invariants ensure no “mint from thin air” |
| Gas Review | Document high-cost hotspots & remediation |
| Event Coverage | Emit on state transitions for off-chain indexing |
| Storage Layout | Notes for potential upgrade placeholder slots |
| Sandwich Heuristic | Data-only component (non-intrusive) |

Checklist evolves in `security/checklist.md`.

---

## 11. Gas Optimization Methodology

1. Baseline snapshot (`gas_wX.md`)
2. Identify top N functions (gas report)
3. Apply micro-optimizations:
   - Cache storage reads
   - Pack struct members (descending size)
   - Use `unchecked` for safe arithmetic segments
   - Minimize event fields
   - Replace repeated division with precomputed factors
4. Re-measure & record delta percentage
5. Document trade-offs (readability vs savings)

Comparison Table Example:
| Function | Before | After | Δ Gas | Δ % | Notes |
|----------|--------|-------|------:|----:|-------|
| deposit  | 52,111 | 47,003 | -5,108 | -9.8% | Cached pool vars |
| claim    | 61,420 | 55,770 | -5,650 | -9.2% | Consolidated math |

---

## 12. Commit & Branch Conventions

| Branch Type | Pattern | Example |
|-------------|---------|---------|
| Feature | `feat/<area>-<short>` | `feat/staking-rewards` |
| Fix | `fix/<area>-<issue>` | `fix/amm-reserve-order` |
| Performance | `perf/<area>-<focus>` | `perf/staking-gas-claim` |
| Data | `data/<metric>` | `data/slippage-p95` |
| Docs | `docs/<topic>` | `docs/gas-report-w6` |

Commit format (Conventional Commits dialect):
```
type(scope): concise message
```
Examples:
```
feat(staking): add multi-pool reward allocation
perf(staking): pack PoolInfo struct to save gas
data(metrics): compute top10 concentration
docs(security): expand checklist with pause handling
```

---

## 13. Self-Audit Report Outline (Planned)

```
1. Scope & Contracts
2. Architecture Summary
3. Critical Invariants
4. Findings (severity ranked)
5. Gas Observations
6. Testing Coverage (unit/fuzz/invariant notes)
7. Upgrade & Operational Risks
8. Recommendations
9. Appendix (function matrix, roles, events)
```

---

## 14. Tags & Releases

| Tag | When | Content |
|-----|------|---------|
| `v0.1-m1` | End Week 4 | Baseline & first reward model |
| `v0.2-m2` | End Week 8 | Multi-pool + gas opt #1 + heuristics |
| `v1.0-m3` | End Week 12 | Full suite + final analytics |

Create & push:
```bash
git tag -a v0.1-m1 -m "Milestone 1"
git push origin v0.1-m1
```

---

## 15. CI (Planned Optional)

| Stage | Job | Purpose |
|-------|-----|---------|
| solidity-tests | `forge test --gas-report` | Ensure correctness & measurable gas |
| python-lint | compile / syntax check | Prevent runtime ETL errors |

Workflow file: `.github/workflows/ci.yml` (added once core tests exist).

---

## 16. Blog / Documentation Strategy

| Blog # | Working Title | Core Theme |
|--------|---------------|-----------|
| 1 | Building a Minimal ERC20 | Fundamentals / Gas baseline |
| 2 | Implementing a Constant Product AMM | Swap logic & events |
| 3 | Designing a Basic Staking Contract | Deposit/withdraw architecture |
| 4 | Reward Accrual Math Explained | accRewardPerShare deep dive |
| 5 | Multi-Pool Allocation Mechanics | allocPoint weighting |
| 6 | First Gas Optimization Pass | Method & wins |
| 7 | Naive Sandwich Detection Heuristic | Data heuristics |
| 8 | Pause & Emergency Patterns | Operational safety |
| 9 | Struct Packing Trade-offs | Gas vs maintainability |
| 10 | From Developer to Self-Auditor | Risk framing |
| 11 | Final On-chain Analytics Insights | Metrics synthesis |
| 12 | 90 Days Retrospective | Outcomes & next steps |

---

## 17. Learning Artifacts & Metrics

| Metric | Source | Tracking File |
|--------|--------|---------------|
| Test count | `forge test` | WEEKLY_LOG.md |
| Coverage (qualitative) | manual | WEEKLY_LOG.md |
| Gas Δ summary | reports/gas/*.md | Gas table |
| Data rows processed | CSV line counts | metrics JSON |
| Sandwich suspects | heuristic CSV | final report |
| Blogs published | docs/blogs/ | README tally |
| Invariants | test files | checklist.md |

---

## 18. Future Extensions (Post Day 90)

| Direction | Rationale | First Step Stub |
|-----------|-----------|------------------|
| Light client / protocol internals | Deeper infra credibility | Annotate geth `core/blockchain.go` |
| ZK Circuit benchmarking | Algebraic performance exploration | Poseidon vs Keccak circuit constraints |
| Advanced MEV classification | Beyond heuristic | Integrate mempool sample stream |
| Rust indexer re‑implementation | Performance scaling | Prototype async fetch/gather |
| Formal verification pilot | Strong safety signal | Add simple Certora / Scribble spec (later) |

---

## 19. Reference Material

| Topic | Resource |
|-------|----------|
| Solidity Docs | https://docs.soliditylang.org |
| Foundry Book | https://book.getfoundry.sh |
| OpenZeppelin Contracts | https://github.com/OpenZeppelin/openzeppelin-contracts |
| Uniswap V2 Whitepaper | https://uniswap.org/whitepaper.pdf |
| web3.py Docs | https://web3py.readthedocs.io |
| Ethernaut | https://ethernaut.openzeppelin.com |
| Paradigm CTF (security) | https://github.com/paradigmxyz/paradigm-ctf-2022 |
| Gas Optimization Patterns | Community writeups (curated in `docs/gas_resources.md`) |

---

## 20. Contributing (Internal Discipline)
Even as a solo learner, use PRs to:
1. Clarify intent (PR description = “WHAT + WHY + TESTS”)
2. Encourage structured changes (small diff / atomic)
3. Maintain documented evolution (link commit → blog reference)

PR Template (suggested `docs/pr_template.md`):
```
### What Changed
### Why
### Tests / Evidence
### Gas Impact (if applicable)
### Security Considerations
```

---

## 21. License
MIT (Add `LICENSE` file if publishing broadly)

---

## 22. Contact / About
Author: (Add your name / handle)  
GitHub: (link)  
Purpose: Public learning artifact toward remote smart contract + data / research engineer role.

---

## 23. Quick Start Cheat Sheet

| Task | Command |
|------|---------|
| Install deps | `foundryup` / `pip install -r requirements.txt` (if created) |
| Run tests | `forge test` |
| Gas report | `forge test --gas-report > reports/gas_wX.md` |
| Fetch swaps | `python tools/fetch_swaps.py --pair <addr> --start <block>` |
| Compute metrics | `python tools/compute_metrics.py` |
| Launch notebook | `jupyter notebook` |
| Lint Python (optional) | `python -m py_compile $(git ls-files '*.py')` |
| Tag milestone | `git tag -a v0.1-m1 -m "..."; git push origin v0.1-m1` |

---

> Continuous Principle: “No week without a verifiable artifact.”  
If something isn’t in Git, it effectively does not exist for evaluation.

(End of README)