*   `Tar.R.`: Target register
*   `Src.R.`: Source register
*   `Cnst.I.`: Constant table index

| INST.         | OP.       | DESC.         | COST  |
| :----------   | :------   | :----------   | ---: |
| `ADD`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | Compute arithmetic operation between two source registers and put the result into target register | 1 |
| `SUB`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | - | 1 |
| `MUL`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | - | 1 |
| `DIV`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | - | 1 |
| `MOD`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | - | 1 |
| `POW`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | - | 1 |
| `IDIV`        | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | Compute floor division between two source registers and put the result into target register | 1 |
| `ADDK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | Compute arithmetic operation between the source register and a constant and put the result into target register | 1 |
| `SUBK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `MULK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `DIVK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `MODK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `POWK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `IDIVK`       | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | Compute floor division between the source register and a constant and put the result into target register | 1 |
| `SUBRK`       | `A`: Tar.R.<br>`B`: Cnst.I. (0..255)<br>`C`: Src.R. | Compute arithmetic operation between the constant and a source register and put the result into target register | 1 |
| `DIVRK`       | `A`: Tar.R.<br>`B`: Cnst.I. (0..255)<br>`C`: Src.R. | - | 1 |
| `NOT`         | `A`: Tar.R.<br>`B`: Src.R. | Compute unary operation for source register and put the result into target register | 1 |
| `MINUS`       | `A`: Tar.R.<br>`B`: Src.R. | - | 1 |
| `LENGTH`      | `A`: Tar.R.<br>`B`: Src.R. | - | 1 |
| `AND`         | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | Perform `and` operation | 1 |
| `ANDK`        | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |
| `OR`          | `A`: Tar.R.<br>`B`: Src.R. (1)<br>`C`: Src.R. (2) | Perform `or` operation | 1 |
| `ORK`         | `A`: Tar.R.<br>`B`: Src.R.<br>`C`: Cnst.I. (0..255) | - | 1 |