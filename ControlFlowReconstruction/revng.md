# 

## Summary

This paper presents a novel algorithm for control flow restructuring in the context of decompiling binary code. The algorithm is designed to produce well-structured, idiomatic, and readable C code that fully exploits the expressiveness of the C language. The algorithm has been implemented on top of the rev.ng static binary analysis framework, resulting in a decompiler called revng-c.

The process involves several stages:

- Preprocessing: The input Control Flow Graph (CFG) is transformed into a shape that can be processed by the Combing stage. This involves enforcing properties such as Two Predecessors and Two Successors, and turning Regions into Directed Acyclic Graphs (DAGs).
- Combing: This stage groups the incoming edges in a node into two sets, one dominated by a conditional node, and the other not. This helps to disentangle complex overlapping paths in the CFG.
- Matching: This stage generates a C Abstract Syntax Tree (AST) representation from the combed Region Tree. The AST is then manipulated to match idiomatic C constructs, such as short-circuited ifs, switch statements, and loops.
  
The decompiler, revng-c, was compared with state-of-the-art commercial and open-source tools on real-world binaries. The results show that the decompilation process introduces between 40% and 50% less extra cyclomatic complexity, indicating a significant improvement in the readability and understandability of the decompiled code.

In conclusion, the paper presents a new approach to control flow restructuring that produces well-structured, idiomatic C code, and demonstrates its effectiveness through the implementation of the revng-c decompiler and comparison with existing tools.

## Evaluation
The effectiveness of the proposed approach in the paper is evaluated based on the following dimensions:
- Gotos: The number of emitted goto statements in the decompiled code. The use of goto statements can make the code harder to understand, so fewer gotos is considered better.
- Cyclomatic Complexity: This is a measure of the complexity of the code, based on the number of linearly independent paths through the code. Lower cyclomatic complexity indicates simpler, more readable code.
- Duplication: The increase in the size of the decompiled code due to the duplication introduced by the approach. While duplication can sometimes make the code easier to understand, excessive duplication can also make the code larger and harder to manage.

![SVypVVQND6](https://github.com/IAPCP/Decompiler-SoK/assets/11942934/62603691-8a79-417c-a434-065cc9a6cfa8)
