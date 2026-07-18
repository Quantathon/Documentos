# Per-artifact audit checklists

Run the checklist for whichever artifact you are auditing. A box is only checked
once you have actually verified it — not because it looks fine.

## QUBO / Ising formulation
- [ ] Optimization sense stated in writing (min vs. max) and consistent everywhere.
- [ ] `x_i ∈ {0,1}` meaning documented (which side of the partition).
- [ ] Ising map `x_i=(1−z_i)/2` applied consistently; constant offset tracked.
- [ ] Each Q-matrix entry traceable to an edge/weight; no stray factors of 2.
- [ ] Brute-forced on a 6–8 node instance; QUBO energy ordering matches direct
      cut-value ordering (up to offset).

## QAOA circuit
- [ ] `p` layers → exactly `2p` variational parameters.
- [ ] Initial state = uniform superposition `|+⟩^n` (Hadamards) for standard QAOA.
- [ ] Cost layer implements `e^{-iγH_C}` (per-edge ZZ / RZZ), not the raw H_C.
- [ ] Mixer implements `e^{-iβH_B}` = RX(2β) per qubit for the standard mixer.
- [ ] If warm-start: initial state AND mixer changed coherently; documented.
- [ ] Endianness between bitstrings and node indices spot-checked on one known cut.

## Approximation ratio
- [ ] `r = E_obtained / E_optimal`, same objective & sign in both.
- [ ] `E_optimal` from brute force where feasible (not "best found").
- [ ] `r ∈ [0,1]`; anything outside signals a sign/normalization bug.
- [ ] Ratio reported, never raw cost.

## Statistics
- [ ] ≥5 seeds per `p`, different initializations.
- [ ] Mean ± std reported (not best-of-N).
- [ ] Plot has error bars; GW and greedy shown as reference lines.
- [ ] Random seeds fixed and logged for reproducibility.

## Classical baselines
- [ ] GW, greedy, brute force on the SAME instance file as QAOA.
- [ ] GW ratio is measured by your implementation, not the literature 0.878
      quoted in place of a measurement.
- [ ] Stochastic baselines (GW rounding, SA) reported with mean ± std.

## Report claims (final pass)
- [ ] No approximation guarantee stated without its scope (esp. 0.6924 = 3-regular).
- [ ] No "quantum advantage" claim without scaling evidence.
- [ ] Emulator results labeled as emulator, never as hardware.
- [ ] Mandatory honest-limitations section present (QAOA<GW gap with error bars,
      emulator≠hardware, aggregated-instance caveats).
- [ ] Every library API path confirmed against current release notes/docs.
- [ ] Single entry point regenerates every figure and number from fixed seeds.
