*   `Tar.R.`: Target register
*   `Src.R.`: Source register
*   `Src.arg.R.`: Source argument register
*   `Ps.I.`: Predicted slot index
*   `Cnst.I.`: Constant table index
*   `Built.fn.`: Builtin function id

| INST.         | OP.       | DESC.         | COST  |
| :----------   | :------   | :----------   | ---:  |
| `GETUPVAL`    | `A`: Tar.R.<br>`B`: Upvalue index | Load upvalue from the upvalue table for the current function | 2 |
| `SETUPVAL`    | `A`: Tar.R.<br>`B`: Upvalue index | Store value into the upvalue table for the current function | 2 |
| `CLOSEUPVALS` | `A`: Tar.R. | Close (migrate to heap) all upvalues that were captured for `registers >= target` | 3 |
| `CMPPROTO`    | `A`: Closure register<br>`D`: Jump offset if proto doesn't match<br>`AUX`: Proto id | Check if a register contains a closure with a specified Luau function proto id | 2 |
| `DUPCLOSURE`  | `A`: Tar.R.<br>`D`: Cnst.I. (0..32767) | Creates a closure from a pre-existing function object (reusing it, unless the environments diverge) | 2 |
| `NEWCLOSURE`  | `A`: Tar.R.<br>`D`: Child proto index (0..32767) | Creates a closure from a child prototype; followed by a `CAPTURE` instruction for each upvalue | 2 |
| `PREPVARARGS` | `A`: Number of fixed arguments | Prepares the stack for variadic functions, so that `GETVARARGS` works correctly | 1 |
| `GETVARARGS`  | `A`: Tar.R.<br>`B`: variable count+1 | Copy variables into the target register from vararg storage for current function  | 2 |
| `GETGLOBAL`   | `A`: Tar.R.<br>`C`: Ps.I. (0..32767)<br>`AUX`: Cnst.I. | Load value from global table using constant string as a key | 4 |
| `SETGLOBAL`   | `A`: Tar.R.<br>`C`: Ps.I.<br>`AUX`: Cnst.I. | Set value in global table using constant string as a key | 4 |
| `RETURN`      | `A`: Register where the returned values start<br>`B`: Number of returned values + 1<br> | Returns specified values from the function | 2 |
| `CAPTURE`     | `A`: Capture type<br>`B`: Src.R. (for `VAL`/`REF`) or upvalue index (for `UPVAL`)<br> | Captures a local variable or an *upvalue* as an *upvalue* in a newly created *closure*; valid only after `NEWCLOSURE` | 3 |
| `CALL`        | `A`: Register where the `function` is, followed by arguments (results are placed starting from the same register)<br>`B`: Argument count+1<br>`C`: Result count+1 | Call specified `function` | 2 |
| `NAMECALL`    | `A`: Tar.R.<br>`B`: Src.I.<br>`C`: Ps.I.<br>`AUX`: Cnst.I. | Prepare a method call (specified by name) by loading the function from a source register, using a constant index into a destination register, and copying the contents of the source register to the destination register+1. This instruction must be followed by a `CALL` instruction; it's roughly equivalent to the `GETTABLEKS` + `MOVE` pair, and needs a special instruction to support the custom `__namecall` metamethod | 3 |
| `CALLFB`      | `A`: Register where the function object resides, followed by the arguments; the results are placed starting from that same register<br>`B`: Argument count+1<br>`C`: Result count+1<br>`AUX`: Feedback slot id (0xFFFFFFFF - sealed) | Call specified `function` with collecting runtime stats in a feedback slot | 2 |
| `FASTCALL`   | `A`: Built.fn.<br>`C`: Jump offset for following `CALL` | Performs a fast call to a native (built-in) `function`. It is followed by one of the instructions (`GETIMPORT`, `MOVE`, or `GETUPVAL`) and a `CALL` instruction; this ensures that if the `FASTCALL` cannot perform the call *inline*, normal execution can proceed. If the `FASTCALL` *is* able to perform the call, it skips the subsequent instructions, as well as the following `CALL` instruction. Note that the `FASTCALL` retrieves the actual arguments for the call, such as the argument/result registers and their respective quantities from the `CALL` instruction | 1 |
| `FASTCALL1`   | `A`: Built.fn.<br>`B`: Src.arg.R.<br>`C`: Jump offset for following `CALL` | Perform a fast call of a built-in `function` using 1 register argument | 1 |
| `FASTCALL2`   | `A`: Built.fn.<br>`B`: Src.arg.R.<br>`C`: Jump offset for following `CALL`<br>`AUX`: Src.R. 2 in least-significant byte | Perform a fast call of a built-in `function` using 2 register arguments | 1 |
| `FASTCALL2K`  | `A`: Built.fn.<br>`B`: Src.arg.R.<br>`C`: Jump offset for following `CALL`<br>`AUX`: Constant index | Perform a fast call of a built-in `function` using 1 register argument and 1 constant argument | 1 |
| `FASTCALL3`   | `A`: Built.fn.<br>`B`: Src.arg.R.<br>`C`: Jump offset for following `CALL`<br>`AUX`: Src.R. 2 in least-significant byte<br>`AUX`: Src.R. 3 in second least-significant byte | Perform a fast call of a built-in `function` using 3 register arguments | 2 |
| `FASTPCALL`   | `A`: Protected function id (0 - `pcall`, 1 - `xpcall`)<br>`B`: Number of explicit arguments before a variadic tail<br>`C`: Jump offset for following `CALL` |  Perform a fast call of a built-in protected call `function` | 3 |
| `NATIVECALL`  | - | Initiates the execution of a new `function` in native code. This is a pseudo-instruction that is never generated by the bytecode compiler, but which can be constructed at runtime to accelerate the dispatch of native code | 0 |
| `BREAK`       | - | Debugger break | 0 |