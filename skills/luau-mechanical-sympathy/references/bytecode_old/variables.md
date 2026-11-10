*   `Tar.R.`: Target register
*   `Src.R.`: Source register
*   `Ps.I.`: Predicted slot index
*   `Cnst.I.`: Constant table index

| INST.         | OP.       | DESC.         | COST  |
| :----------   | :------   | :----------   | ---: |
| `MOVE`        | `A`: Tar.R.<br>`B`: Src.R. | Move (copy) value from one register to another | 1 |
| `LOADNIL`     | `A`: Tar.R. | Sets register to `nil` | 1 |
| `LOADB`       | `A`: Tar.R.<br>`B`: Value<br>`C`: Jump offset | Sets register to `boolean` and jumps to a given short offset | 1 |
| `LOADN`       | `A`: Tar.R.<br>`D`: Value (-32768..32767) | Sets register to a `number` literal | 1 |
| `LOADK`       | `A`: Tar.R.<br>`D`: Cnst.I. (0..32767) | Sets register to an entry from the constant table from the proto (`number`/`vector`/`string`) | 1 |
| `LOADKX`      | `A`: Tar.R.<br>`AUX`: Cnst.I. | Sets register to an entry from the constant table from the proto (`number`/`string`) | 2 |go