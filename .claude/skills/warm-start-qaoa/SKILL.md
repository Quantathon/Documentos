---
name: warm-start-qaoa
description: Implement warm-start QAOA correctly (classically-seeded initial state + rotated mixer, following Egger-Marecek-Woerner 2021) and frame its results honestly as a gap-narrowing experiment, never as "beating Goemans-Williamson". Use whenever the task involves warm-starting QAOA, seeding QAOA from a greedy/spectral/GW/SDP solution, custom or rotated mixers, comparing warm-start vs standard initialization, or writing up warm-start results for the Quantathon CR 2026 entry — this is the proposal's central Layer-1 differentiator and its framing is a scoring risk if done wrong. Trigger on "warm-start", "warm start QAOA", "WS-QAOA", "seeded QAOA", "rotated mixer", "Egger", "Tate", or any claim that QAOA can outperform GW.
---

# Warm-start QAOA: implementation + honest framing

Warm-start QAOA is this project's differentiator, and also its biggest honesty trap. The official brief states the jury's position outright: *"Currently, QAOA does not beat Goemans-Williamson for Max-Cut on any graph instance"* and lists quantum-advantage claims as a red flag. Meanwhile Tate et al. (2023) prove theoretical low-depth results for warm-started variants. Both are real; the winning move is to run warm-start as a **controlled experiment** and report it in the jury's language.

## The framing rule (decide before running anything)

The research question is: **"How much does the QAOA-GW gap narrow at fixed depth p when QAOA is initialized from a classical solution?"**

- Never present warm-start as a route to beat GW. Cite Tate et al. (Quantum 7, 1121, 2023) as theoretical context for why the question is interesting — with its assumptions — not as a promise.
- Pre-commit to reporting the effect **whether or not it helps**. "Warm-start did not improve over standard QAOA on our instance" is a publishable, rubric-scoring result. Cherry-picking the comparison is the same red flag as cherry-picking seeds.
- The mandatory limitations section already concedes QAOA < GW; a warm-start section that contradicts it torpedoes the report's internal consistency (a thing judges check).

## Mechanics (Egger, Marecek & Woerner, Quantum 5, 479, 2021)

Verify the details against the paper / live docs before coding — this is the recalled shape, not gospel:

1. **Get a classical seed** for the same instance: greedy solution, spectral relaxation, or the GW SDP solution. For a continuous relaxation, each node gets c*_i ∈ [0, 1]; for a binary solution, c*_i ∈ {0, 1}.
2. **Replace the uniform-superposition initial state** with a product state encoding the seed: qubit i prepared as RY(θ_i)|0⟩ with θ_i = 2·arcsin(√(c*_i)).
3. **Regularize binary seeds.** If c*_i is exactly 0 or 1 the mixer cannot move that qubit, so clamp with ε (paper uses ε ≈ 0.25): c*_i ← ε if c*_i < ε, and 1−ε if c*_i > 1−ε. Report the ε used.
4. **Rotate the mixer** so its ground state is the new initial state (the WS-QAOA mixer replaces RX with single-qubit rotations built from θ_i). Skipping this step and keeping the plain X mixer is a *different* algorithm — label which variant you ran.
5. Everything else (cost layer, optimizer, statistics) is identical to standard QAOA — reuse the `maxcut-qubo-qaoa` pipeline unchanged so the comparison is clean.

## Experimental design (what makes it a controlled experiment)

- **Same instance, same p values, same optimizer, same shot budget, same ≥5-seed statistics** for both arms (standard init vs warm-start). Only the initialization/mixer differs.
- Report per arm: mean ± std of approximation ratio r at each p, on the same r-vs-p plot, with GW and greedy as horizontal reference lines.
- Headline metric: **gap narrowing** Δ(p) = r_warmstart(p) − r_standard(p), with its uncertainty. Also report where each arm sits relative to GW's ratio on the instance.
- Watch for the known failure mode: a warm-start seeded too close to a local optimum can *freeze* (the regularization ε exists to prevent this). If you see it, report it — it is an interesting, honest finding.

## Write-up checklist
- [ ] Variant labeled precisely (seed type, ε, mixer rotated or not).
- [ ] Both arms run under identical budgets; statistics per `maxcut-qubo-qaoa` rules.
- [ ] Δ(p) reported win or lose, with error bars.
- [ ] Tate et al. cited as context with assumptions; no "beats GW" language anywhere.
- [ ] Consistent with the limitations section (QAOA < GW stated).

References: Egger, Marecek, Woerner, "Warm-starting quantum optimization", Quantum 5, 479 (2021). Tate et al., Quantum 7, 1121 (2023). Route final numbers and claims through `quantum-code-audit`.
