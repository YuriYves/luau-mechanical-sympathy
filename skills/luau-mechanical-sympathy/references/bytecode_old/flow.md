*   `Tar.R.`: Target register
*   `Src.R.`: Source register

| INST.         | OP.       | DESC.         | COST  |
| :----------   | :------   | :----------   | ---: |
| `JUMP`        | `D`: Jump offset (-32768..32767; 0 means "next instruction", i.e., "do not jump") | Jumps to target offset | 1 |
| `JUMPBACK`    | `D`: Jump offset (-32768..32767; -) | Jumps to target offset. This is equivalent to `JUMP` but is used as a safepoint to be able to interrupt `while`/`repeat` loops | 1 |
| `JUMPIF`      | `A`: Src.R.<br>`D`: Jump offset (-32768..32767; -) | Jumps to target offset if register is not `nil`/`false` | 2 |
| `JUMPIFNOT`   | `A`: Src.R.<br>`D`: Jump offset (-32768..32767; -) | Opposite of `JUMPIF` | 2 |
| `JUMPIFEQ`    | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; 1 means "next instruction", i.e., "do not jump")<br>`AUX`: Src.R.(2) | Jumps to target offset if the comparison is `true` (or `false`, for `NOT` variants) | 2 |
| `JUMPIFLE`    | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Src.R.(2) | - | 2 |
| `JUMPIFLT`    | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Src.R.(2) | - | 2 |
| `JUMPIFNOTEQ` | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Src.R.(2) | - | 2 |
| `JUMPIFNOTLE` | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Src.R.(2) | - | 2 |
| `JUMPIFNOTLT` | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Src.R.(2) | - | 2 |
| `JUMPX`       | `E`: Jump offset (-2^23..2^23; 0 means "next instruction", i.e, "do not jump") | Jumps to the target offset; like `JUMPBACK`, supports interruption | 2 |
| `JUMPXEQKNIL` | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; 1 means "next instruction", i.e, "do not jump")<br>`AUX`: Constant value (for `boolean`) in low bit, `NOT` flag (that flips comparison result) in high bit | Jumps to target offset if the comparison with constant is `true` (or `false`, see `AUX`) | 3 |
| `JUMPXEQKB`   | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -) | - | 3 |
| `JUMPXEQKN`   | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -)<br>`AUX`: Constant table index in low 24 bits, `NOT` flag (that flips comparison result) in high bit | - | 3 |
| `JUMPXEQKS`   | `A`: Src.R. (1)<br>`D`: Jump offset (-32768..32767; -); `AUX`: - | - | 3 |
| `FORNPREP`    | `A`: Tar.R. (Numeric for loops assume a register layout `[limit, step, index, variable]`)<br>`D`: Jump offset (-32768..32767) | Prepare a numeric for loop, jump over the loop if first iteration does not need to run; limit/step are immutable, index isn't visible to user code since it's copied into variable| 1 |
| `FORGPREP`    | `A`: Tar.R. (Generic for loops assume a register layout `[generator, state, index, variables...]`)<br>`D`: Jump offset (-32768..32767) | Prepare loop variables for a generic for loop, jump to the loop backedge unconditionally | 2 |
| `FORNLOOP`    | `A`: Tar.R. (see `FORNPREP` for register layout)<br>`D`: Jump offset (-32768..32767) | Adjust loop variables for one iteration, jump back to the loop header if loop needs to continue | 1 |
| `FORGLOOP`    | `A`: Tar.R. (Generic for loops assume a register layout `[generator, state, index, variables...]`)<br>`D`: Jump offset (-32768..32767)<br>`AUX`: variable count (1..255) in the low 8 bits, high bit indicates whether to use ipairs-style traversal in the fast path | Adjust loop variables for one iteration of a generic for loop, jump back to the loop header if loop needs to continue. Loop variables are adjusted by calling generator(state, index) and expecting it to return a tuple that's copied to the user variables; the first variable is then copied into index; generator/state are immutable, index isn't visible to user code | 2 |
| `FORNGPREP_INEXT` | `A`: Tar.R. (see `FORGLOOP` for register layout)<br>`D`: Jump offset (-32768..32767) | Prepare `FORGLOOP` with 2 output variables (no AUX encoding), assuming generator is luaB_inext, and jump to `FORGLOOP` | 3 |