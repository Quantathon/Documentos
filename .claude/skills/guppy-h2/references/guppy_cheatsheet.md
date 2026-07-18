# Guppy cheat-sheet (verified against guppylang 0.21.16, 2026-07; re-verify on version bumps at docs.quantinuum.com/guppy)

## Import map
```python
from guppylang import guppy
from guppylang.std.builtins import array, result, comptime
from guppylang.std.quantum import qubit, measure, measure_array, discard, discard_array
from guppylang.std.quantum import h, x, y, z, cx, cz, rz, rx, ry, t, s, toffoli
from guppylang.std.quantum import angle, pi          # rotation-angle type + constant
from guppylang.std.qsystem import zz_phase, zz_max, phased_x  # H2-native gates
# functional variants (return the qubit) live under:
from guppylang.std.quantum.functional import h as h_f
```

## Angles (verified)
- Rotation gates (`rz`, `rx`, `ry`, `crz`, `zz_phase`, ...) take the `angle` type, **not** `float` — passing a float is a type error.
- `angle` counts **half-turns**: `angle(x)` = x·π radians. Convert radians with `angle(theta / math.pi)` (do the division in Python, outside the `@guppy` body, or via `comptime`). `pi == angle(1)`.
- Angles do not wrap: `angle(2) != angle(0)`.

## Scope rule (verified)
Module-level Python constants are **not** visible inside a `@guppy` body: `range(N)` with module-level `N` fails with "Variable not defined". Hardcode the literal or wrap every reference in `comptime(N)`.

## Source rule (verified)
`@guppy` functions must be defined in a real `.py` file. `python -c` one-liners and bare REPL definitions fail at `.check()` with `OSError: could not get source code` (Guppy re-parses source via `inspect`).

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

## Max-Cut QAOA cost layer (verified pattern)
```python
import math
from guppylang.std.qsystem import zz_phase   # arbitrary-angle ZZ, native on H2
from guppylang.std.quantum import angle

# ZZPhase(theta) = exp(-i * theta/2 * Z tensor Z)   [docstring, guppylang 0.21.16]
# QAOA cost edge exp(-i * gamma * w * Z_i Z_j)  =>  theta = 2 * gamma * w
# angle is in half-turns  =>  pass angle(theta / math.pi)

GAMMA, W = 0.7, 1.0
THETA_HT = 2 * GAMMA * W / math.pi   # compute in Python scope; literals only inside @guppy

@guppy
def cost_layer() -> None:
    ...
    zz_phase(qs[0], qs[1], angle(0.4456338406573069))  # = 2*0.7*1.0 rad in half-turns
    ...
```
Because H2 is all-to-all with a native ZZ gate, one `zz_phase` per edge replaces a CX–RZ–CX decomposition (3 gates → 1). Remember the two scope rules above: the half-turn arithmetic happens in Python (or `comptime`), and the angle argument must be an `angle`, not a float.

## Selene backends
- `Quest()` — statevector (exact, noiseless), good for verification.
- `Stim()` — Clifford-only, fast for stabilizer circuits.
- error-model plugins — noisy emulation approximating device behavior.
