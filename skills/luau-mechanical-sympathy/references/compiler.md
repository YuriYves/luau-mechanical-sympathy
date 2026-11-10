# Compiler

# Bytecode
Bytecode instructions use "word code", where each instruction consists of one or more 32-bit words. The first word of an instruction is the *header*, which must contain the OpCode in its least significant byte. In some cases, this is followed by an additional word—denoted as `AUX`, which is decoded according to the specification for that particular opcode.

For every opcode, the encoding is static; that is, based on the opcode alone, you know *a priori* the width of the instruction—with the exception of `NEWCLOSURE`.