# Decompiler-SoK

This website is only used for collecting, grouping and summarizing the related paper of decompilation. If there are any paper need to be updated, you can contribute PR.

## [Tools](./Tools)

This section introduces decompilers currently available to users.
Some of them are commonly used in reverse engineering pratice, for example, **Ghidra**, **JEB**, and **Hex-Rays**.
Others are not yet complete. Some are still under development.
The readers can try out each tool at <https://dogbolt.org>.

<!-- - Ghidra
- Hex-Rays
- JEB
- RetDec
- angr
- Binary Ninja
- Boomerang
- RecStudio
- Reko
- Relyze
- Snowman -->

## [General Overview](./GeneralOverview)

This section introduces the development, theories and principles of decompilation.
We list some theses or books about decompilation.

## [Compiler](./Compiler)

This section introduces the architecture and principles of compiler.
We summarize the relationship between compilers and decompilers. 

## [Program Analysis](./ProgramAnalysis)

This section introduces the theory of program analysis and reverse engineering.
We introduce the relationship among decompilation, program analysis and reverse engineering.

## [Framework](./Framework)

This section introduces the work of decompilation framework.
Different from the Section "General Overview", this section aims at the degisn principles and details of implementation of decompilers in practice.

## [Intermediate Representation](./IntermediateRepresentation)

This section introduces the intermediate representation designed for reverse engineering.
Some of them have been applyed to decompilers in Section "Tools", for example, pcode used in Ghidra and micro code used in Hex-Rays. 

<!-- - LLVM IR
- Ghidra Pcode
- VEX
- Hex-Rays microcode
- BAP BIL
- REIL
- ESIL
- LLIL
- BTIL -->


## [Type Reconstruction](./TypeReconstruction)

This section introduces the papers aim to reconstruct the type information of variable, including general type and struct.


## [Control Structures Reconstruction](./ControlFlowReconstruction)

This section introduces the work of control structures reconstruction.
Control structures include *if-else*, *switch-case*, *while*, *do-while*, *for* and so on.

## [Debug Information Recovery](./DebugInformationRecovery)

This section introduces the work of debug information recovery including identifier recogonizing and renaming.

## [Evaluation](./Evaluation)

This section introduces the evaluation related work of decompilation.

## [C++ Decompilation](./C++Decompiler)

This section introduces the work aim to recover the object-oriented related features in C++.

## [AI-Based Decompilation](./AIBasedDecompilation)

This section introduces the work of AI-based decompilation.

## [Application](./Application)

This section introduces the work aim to recover the features of specific application scenarios.

## [Other Topics](./OtherTopic)

Other topics in decompilation, for example, **nn decompiler**, **code representation** and so on.


