---
name: quantum-code-audit
description: >-
  Audit the rigor of quantum code, derivations, and claims before trusting
  them — the verification backbone the other quantum skills route through.
  Checks three things LLM quantum work fails at. First, internal mathematical
  consistency — unitarity, operator algebra, QUBO/Ising sign conventions,
  minimization vs. maximization, representation mappings. Second, source
  backing — every guarantee, number, or library-behavior claim confirmed
  against real literature/live docs, not asserted from confidence. Third,
  quantum logic — the algorithmic reasoning is valid, not merely that syntax
  compiles. Use WHENEVER reviewing, auditing, or verifying quantum results,
  whenever an LLM states an approximation guarantee or bound, when preparing a
  report or hackathon deliverable that must survive judges' questions, or to be
  sure a figure is not a hallucination. Trigger even without the word audit:
  any "review this", "is this right?", "check this", or "verify" over quantum
  content counts.
---

# Quantum code & logic audit

Act as a skeptical reviewer. An LLM's default bias is to make things *sound*
correct; your job here is the opposite — find where it sounds right but is
wrong. Do not approve anything on plausibility. Every technical claim passes
through one of two gates: **you verify it**, or **you mark it unverified**.
There is no third option.

## Golden rule

If you are about to state a figure, a theoretical guarantee, an approximation
bound, a library function name, or "algorithm X does Y", and you have not
confirmed it this session against a real source (web search, official docs, the
cited paper), **do not assert it as fact**. Write it as "unverified — pending"
and go verify. Model confidence is not evidence.

## The three audit layers

### Layer 1 — Internal mathematical consistency

These are the traps that most often slip into QAOA/Max-Cut work:

- **Sign and optimization sense.** Is the problem minimization or maximization?
  Max-Cut *maximizes* cut weight, but the QUBO and Ising forms are usually
  written to *minimize* energy. One flipped sign silently optimizes the wrong
  way while looking reasonable. Confirm the convention is written down and
  consistent across the whole pipeline (formulation → circuit → post-processing
  → ratio).
- **QUBO ↔ Ising map.** Standard substitution is `x_i = (1 − z_i)/2` with
  `x_i ∈ {0,1}`, `z_i ∈ {−1,+1}`. Check that constant, linear, and quadratic
  terms transform coherently and that the discarded constant offset does not
  contaminate the approximation ratio.
- **Unitarity.** Every `e^{-iHθ}` evolution is unitary; every gate must be. If
  someone builds a "cost" operator and applies it as a gate, verify the
  generator is Hermitian and that the exponential — not the raw Hamiltonian —
  is what gets implemented.
- **Approximation ratio.** `r = E_obtained / E_optimal`. The denominator must be
  the *true* optimum (brute force on small instances), not the best classical
  value found, and numerator and denominator must use the same objective and
  sign. For Max-Cut, `r ∈ [0,1]`; if it comes out >1 or <0, there is a sign or
  normalization error.
- **Parameter count.** Depth-`p` QAOA has exactly `2p` parameters (`p` γ + `p`
  β). A different count is a bug.

Method: reconstruct the derivation from scratch on a minimal case (2–3 nodes, a
graph you can cut by hand) and compare term by term. If it fails on the minimal
case, it fails.

### Layer 2 — Source contrast (anti-hallucination)

For each "guarantee", "bound", "the library does X", or figure:
1. Identify the exact claim and where it came from.
2. Find it in a primary source (paper, current official docs).
3. Check the **scope**, not just the number. This is the most common and most
   dangerous error: theoretical guarantees carry conditions.

**Verified anchor example (use as a mental pattern).** The QAOA p=1 bound
r ≥ 0.6924 (Farhi, Goldstone, Gutmann 2014, arXiv:1411.4028) is a guarantee
**specific to 3-regular graphs** — not arbitrary or weighted graphs. LLMs tend
to quote "0.6924" as if it applied to any instance. On a weighted, non-3-regular
grid graph that bound *does not apply*; what you report is the measured
empirical ratio. Goemans–Williamson (≈0.878, Goemans & Williamson 1995, JACM
42(6)) *is* a general Max-Cut guarantee via SDP relaxation. Mislabeling the
scope of these two numbers is a red flag a technical judge catches instantly.

Other typical checks in this domain:
- Library function signatures and behavior — APIs move; recalled snippets are
  often deprecated (see the `qiskit-qaoa-verify` and `guppy-h2` skills).
- "Quantum advantage" claims require a scaling comparison. Without measured
  scaling, the claim does not stand and must be flagged.

If you cannot verify a claim, **say so explicitly**: "no support found for X;
treat as unconfirmed." That is worth more than an invented citation.

### Layer 3 — Quantum logic (beyond syntax)

Code can compile and the reasoning still be wrong:
- **Does the measurement measure what's assumed?** Computational vs. other
  basis; bit ordering (endianness) between the SDK and the post-processing
  (Qiskit is little-endian; Guppy result streams follow their own tags).
- **Is the initial state right?** Standard QAOA starts from `|+⟩^n` (uniform
  superposition after Hadamards). A warm-start begins elsewhere; verify the
  initial state and mixer are mutually coherent — a warm-start with the plain
  X-mixer may not preserve the seeded solution (a subtle point the warm-start
  literature discusses; do not assume it "just works").
- **Is the statistical average honest?** One run of a variational algorithm is
  not a result. Require mean ± std over ≥5 different initializations. Reporting
  only the best run is cherry-picking.
- **Emulator vs. hardware?** A noiseless emulator number is not a hardware
  result and must be labeled as such.

## Audit output format

ALWAYS use this structure so the result is actionable:

```
## Audit

### ✅ Verified
- [claim] — confirmed against [source]

### ⚠️ Flagged (unverified / dubious scope)
- [claim] — [why]; missing [what verification]

### ❌ Incorrect
- [claim] — [concrete error]; fix: [what it should say]

### Suggested corrections
- [concrete code or text change]
```

If everything is clean, say so — but only after trying to break it on a minimal
case. An audit that found nothing because it did not look is not an audit.

## When to pull in a live source
Prefer web search / official docs over memory for: any numeric guarantee, any
library API path or signature, any "current best" claim, and any device
parameter (qubit counts, connectivity, native gates, noise). This is the same
discipline the other skills defer to; you are the enforcement point.

## Reference files
- `references/audit_checklists.md` — per-artifact checklists (QUBO, QAOA
  circuit, ratio computation, statistics, report claims).
- `references/claim_verification.md` — the verified-facts table for this project
  (bounds, scopes, device facts) to check claims against, with sources.

## Anchor references (verified)
- Farhi, Goldstone, Gutmann (2014), arXiv:1411.4028 — QAOA; 0.6924 is
  3-regular-specific.
- Goemans & Williamson (1995), JACM 42(6) — GW SDP, ≈0.878 (general guarantee).
- Lucas (2014), Front. Phys. 2:5 — Ising formulations of NP problems (canonical
  check for QUBO/Ising mappings).
