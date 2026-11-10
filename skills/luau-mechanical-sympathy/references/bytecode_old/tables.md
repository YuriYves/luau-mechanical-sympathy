*   `Tar.R.`: Target register
*   `Src.R.`: Source register
*   `Tab.R.`: Table register
*   `Idx.R.`: Index register
*   `Cnst.I.`: Constant table index
*   `Ps.I.`: Predicted slot index
*   `Im.P.`: Import path

| INST.         | OP.       | DESC.         | COST  |
| :----------   | :------   | :----------   | ---: |
| `GETIMPORT`   | `A`: Tar.R.<br>`D`: Cnst.I. (0..32767)<br>`AUX`: Im.P. | Load imported global table global from the constant table | 2 |
| `NEWTABLE`    | `A`: Tar.R.<br>`B`: Table size (stored as 0 for `v=0`, and `ceil(log2(v))+1` for `v!=0`)<br>`AUX`: Array size | Create table in target register | 3 |
| `DUPTABLE`    | `A`: Tar.R.<br>`D`: Cnst.I. (0..32767) | Duplicate table using the constant table template to target register | 2 |
| `GETTABLE`    | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Idx.R. | Load value from table into target register using key from register | 2 |
| `GETTABLEKS`  | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Ps.I.<br>`AUX`: Cnst.I. | Load value from table into target register using constant string as a key | 1 |
| `GETTABLEN`   | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Index-1 (1..256) | Load value from table into target register using small integer index as a key | 1 |
| `SETTABLE`    | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Idx.R. | Store source register into table using key from register | 2 |
| `SETTABLEKS`  | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Ps.I.<br>`AUX`: Cnst.I. | Store source register into table using constant string as a key | 1 |
| `SETTABLEN`   | `A`: Tar.R.<br>`B`: Tab.R.<br>`C`: Index-1 (1..256) | Store source register into table using small integer index as a key | 1 |
| `SETLIST`     | `A`: Tar.R.<br>`B`: Src.R. (Start)<br>`C`: Value count + 1<br>`AUX`: Table index start | Set a list of values to table in target register | 2 |
| `CONCAT`      | `A`: Tar.R.<br>`B`: Src.R. (Start)<br>`C`: Src.R. (End) | Concatenate all strings between B and C (inclusive) and place the result in A | 3 |