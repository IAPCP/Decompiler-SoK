# A Taxonomy of C Decompiler Fidelity Issues


## Abstract

This article presents a comprehensive study on the fidelity issues in C language decompilation. The authors used open-coding techniques and thematic analysis to identify and categorize defects in decompiled code, comparing the output from four different decompilers. They developed a taxonomy consisting of 15 top-level issue categories, encompassing a total of 52 issues. The findings highlight various types of decompilation defects, ranging from identifier name mismatches to typecast issues. The study contributes to a deeper understanding of the problems in decompilation and suggests potential solutions for improving decompiler fidelity. The results are robust across different decompilers and provide valuable insights for future decompiler development.

## Defects

The article identified a range of fidelity issues in C language decompilation, categorized into 15 main groups:

- **Incorrect Identifier Name:** Mismatches in identifier names, including variable, type, and function names.
- **Non-idiomatic Dereference:** Mismatch in pointer variable usage compared to the original code.
- **Unaligned Code:** Extra or missing code relative to the original.
- **Typecast Issues:** Extra or missing typecasts.
- **Nonequivalent Expression:** Misaligned operators that are not semantically equivalent.
- **Unaligned Variable:** Missing or extra variables in the decompiled code.
- **Non-idiomatic Literal Representation:** Nonstandard use of literals.
- **Obfuscated Control Flow:** Non-idiomatic use of control flow.
- **Issues in Representing Global Variables:** Challenges in accurately representing global variables.
- **Expanded Symbol:** Macros or constructs like sizeof represented by their values.
- **Use of Decompiler-specific Macros:** Use of decompiler-defined macros in decompiled code.
- **Abuse of Memory Layout:** Non-idiomatic memory usage while maintaining semantic equivalence.
- **Incorrect Return Behavior:** Mismatch in function return values.
- **Decomposition of a Composite Variable:** Treating members of composite variables as separate entities.
- **Type-dependent Nonequivalent Expression:** Incorrect types leading to nonequivalent expressions.

These categories provide a framework for understanding and addressing the various issues in decompiled C code.
