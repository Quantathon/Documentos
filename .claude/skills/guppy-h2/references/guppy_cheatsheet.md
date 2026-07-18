# Guppy cheat-sheet (verify against docs.quantinuum.com/guppy for the installed version)

## Import map
```python
from guppylang import guppy
from guppylang.std.builtins import array, result, comptime
from guppylang.std.quantum import qubit, measure, measure_array, discard, discard_array
from guppylang.std.quantum import h, x, y, z, cx, cz, rz, rx, ry, t, s, toffoli
# functional variants (return the qubit) live under:
from guppylang.std.quantum.functional import h as h_f
```

## Lifecycle
```python
prog.check()             # type-check (ownership / linear types) — do this first
hugr = prog.compile()    # -> HUGR binary
hugr = prog.compile(opt_level=...)  # TKET optimization level
```

## Ownership rules (the type checker enforces these)
- A `qubit()` is linear: use it exactly once per "consuming" operation path.
- Gates borrow qubits and return them to scope; `measure(q)` consumes `q` and yields a classical bit.
- `measure_array(arr)` consumes an array of qubits, returns an array of bits.
- Unused qubit at end of scope → compile error. Discard intentionally with `discard`/`discard_array`.
- No operation duplicates a qubit (no-cloning is a type error, not a runtime surprise).

## Results
- `result("tag", value)` streams a labeled result. Retrieve with `QsysResult(...)`, then `.collated_counts()` or per-shot access.

## Max-Cut QAOA cost layer sketch (edges as (i, j, w))
```python
from guppylang.std.quantum import rzz  # arbitrary-angle ZZ, native on H2 (confirm name/signature in docs)

@guppy
def cost_layer(qs: "array[qubit, N]", gamma: float) -> None:
    # for each weighted edge, apply exp(-i * gamma * w * Z_i Z_j)
    # rzz(angle) with angle = 2 * gamma * w  (verify the factor of 2 against your Ising derivation)
    ...
```
Because H2 is all-to-all with a native ZZ gate, express each edge as a single RZZ instead of a CX–RZ–CX decomposition where the API allows it. Verify the exact gate name/signature and angle convention in the installed guppylang version before relying on it — do not assume from memory.

## Selene backends
- `Quest()` — statevector (exact, noiseless), good for verification.
- `Stim()` — Clifford-only, fast for stabilizer circuits.
- error-model plugins — noisy emulation approximating device behavior.
