---
name: error-mitigation-qaoa
description: Apply and honestly report quantum error mitigation and error-correction tradeoffs for QAOA runs on the Quantinuum H2 emulator/hardware — ZNE, Pauli twirling, readout mitigation, and the Iceberg QEC code with its depth cost. Use whenever the task involves running QAOA on a noisy emulator/device and wanting trustworthy numbers, comparing encoded vs. unencoded results, or deciding between error mitigation and error correction for the Quantathon CR 2026 project's optional-extensions criterion. Trigger on "ZNE", "zero-noise extrapolation", "Pauli twirling", "readout mitigation", "Iceberg", "QEC", "noise mitigation", or "error bars on hardware results". Emphasize honest reporting of tradeoffs over claiming improvement.
---

# Error mitigation & QEC for QAOA on H2

For a shallow variational algorithm like QAOA, the right move is usually **error mitigation, not error correction** — and the honest reporting of what it did (and did not) buy you is what scores, not a claim of improvement. The rubric explicitly warns against "hardware results without noise analysis" as a red flag. When citing a technique's behavior, confirm against a live source; noise-model details change per device.

## Decision: mitigation vs. correction

- **Error mitigation** (ZNE, twirling, readout correction): post-processing / circuit-level tricks that reduce bias in expectation values without extra logical qubits. Low overhead, well-suited to QAOA. **Start here.**
- **Error correction (QEC)** e.g. **Iceberg** (Quantinuum): encodes logical qubits, but **adds circuit depth**. For a shallow QAOA circuit, that added depth can *degrade* results rather than improve them. The project brief says exactly this. If you use QEC, you must **report both encoded and unencoded** results and discuss the tradeoff quantitatively — do not assume encoding helps.

## Techniques (what each does, when it helps)

- **Zero-Noise Extrapolation (ZNE).** Run the circuit at several artificially amplified noise levels (e.g., gate folding), then extrapolate the expectation value to the zero-noise limit. Helps for QAOA's ⟨H_C⟩ estimates. Pitfall: the extrapolation model (linear/exponential) is a modeling assumption — report which you used and its fit quality; a bad extrapolation invents confidence.
- **Pauli twirling / randomized compiling.** Insert random Pauli gates that average coherent errors into a stochastic (more benign) channel, making noise better-behaved for mitigation. Improves the *reliability* of ZNE more than raw accuracy; combine with it.
- **Readout / measurement mitigation.** Characterize the assignment-error matrix and invert it to correct measured probabilities. Cheap, almost always worth doing on real/emulated hardware. Trapped-ion SPAM errors are relatively low, so the correction is modest but honest.

## H2-specific notes
- The H2 emulator can run noiseless (exact statevector, ~26-qubit ceiling) **or** with a device-matched noise model. Report both: noiseless establishes the algorithm is correct; noisy establishes what the device would see. Never present a noiseless number as a hardware result.
- H2's native ZZ gate and all-to-all connectivity keep QAOA cost layers shallow — a reason mitigation is preferable to depth-adding QEC here.
- Quantinuum's stack (Selene error models; Nexus emulator configs) supplies the noise model; the `guppy-h2` skill covers submission.

## Honest reporting template (required)
For every mitigated result, report:
1. Unmitigated (noisy) value.
2. Mitigated value + the technique and its assumptions (extrapolation model, twirling count, calibration matrix source).
3. Noiseless reference (statevector) for comparison.
4. Whether mitigation *helped, hurt, or was neutral* — and if QEC was tried, the encoded-vs-unencoded comparison with the depth cost.

Claiming an improvement without the unmitigated and noiseless anchors is unverifiable — a Layer-4 audit failure. Route results through `quantum-code-audit`.

## References (verify current details)
Cai et al. (2023), arXiv:2210.00921 (mitigation review); Temme, Bravyi, Gambetta (2017), PRL 119, 180509 (ZNE/PEC foundations); Wallman & Emerson (2016), PRA 94, 052325 (Pauli twirling); Jin et al. (2025), arXiv:2504.21172 (Iceberg); LaRose et al. (2022), Quantum 6, 774 (Mitiq).
