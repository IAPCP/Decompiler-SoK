# DnD: A Cross-Architecture Deep Neural Network Decompiler

Ruoyu Wu, Purdue University

## Abstract

This paper proposes DnD, the first compiler and ISA-agnostic DNN decompiler.
It aims to lift the binary code compiled from a DNN on **edge-device** to a novel intermediate representation, able to express the high-level mathematical DNN operations.
They evaluate DnD on two compilers (**Glow and TVM**) and three ISAs (**Thump, AArch64, and x86-64**).
Dnd enables extracting the DNN models used by real-wrold micro-controllers and attacking them using white-box adversarial machine learning techniques.

## Introduction

- Traditional decompilers cannot capture the mathematical semantics of compiled DNN models.

- DnD works with a compiled DNN model and recover its parameters, hyper-parameters and topology, and express the decompiled model in a high-level representation, encoded in the ONNX modeling language.

- Techniques: 1. Uses **symboloc execution** in conjunction with a dedicated loop analysis to capture precise mathematical formulas representing how different DNN operators process the received data. 2. Uses a novel IR to express the high-level mathematical DNN operations in a compiler- and ISA-agnostic way. 3. Identifies the type and location of the DNN operators in a target binary by matching the extracted mathematical operations with template mathematical DNN operations, recovering hyper-parameters and parameters of all the identified DNN operators, as well as the overall network topology.

- The recovered DNN model can be used to boost adversarial attacks against the original DNN, enabling the usage of the white-box attacks, in place of less efficient black-box ones.

- Contributions: design and implement DnD (including decompiling of stripped binaries), design IR

- Artifacts: https://github.com/purseclab/DnD

## Background and Motivation

- ONNX: the open standard for ML interoperability developed by Linux Foundation.
![image](https://user-images.githubusercontent.com/11942934/199419365-9d9bebf8-2a55-4ebd-8fe9-a6291c2d75ed.png)

- DNN Operators: the building blocks of DNNs. A DNN operator takes the output of previous operators as its input and computes its output based on its operator type and its parameters.(174 different DNN operators defined in ONNX).

- DNN Hyper-parameters and Parameters: (1) the algorithm hyper-parameters: only used during the training phase (2) the model hyper-parameters: define the netwrok structure and how the operators function (Total number of operators and the type of each operator. The DNN topology. The attributes of each operator that define its detailed semantics.)

- DNN Compilers: Glow, TVM, XLA, NNFusion. Frontend-Backend component. 

- Frontend: transforms a DNN model into a high-level IR and performs hardware-independent optimizations, such as operator fusion.

- Backend: transforms a high-level IR to a low-level IR and performs hardware-specific optimizations (vectorization and loop-related optimizations).

- Compilation Scheme: interpreter-based and ahead-of-time(AOT) compilation schemes.

- Interpreter-based: generate DNN binaries at runtime.They usually produce two artifacts: a DNN configuration file describing the DNN model and a runtime library that contains all the DNN operator implementations.

- AOT: Specialize the operator implementation for the specific compiled operator instance's context.

- Glow and TVM: An application feeds the input data to this inference function and obtains the predicted label as output.


## System Design

#### Workflow.
![image](https://user-images.githubusercontent.com/11942934/199419491-a705416e-026b-47a1-914c-25265cc2e804.png)


(1) **DNN Operator Location Identification.** Recovers the CFG and identifies the location of inference function and DNN operators from the input (stripped) DNN binary.

(2) **Operator Symmary Generation.** 
- Conducts loop analysis to identify loops' information. 
- Leverages loop's information to perform selective symbolic execution that extracts the output of a DNN operator as symbolic expressions of its input and parameters, which capture the mathematical semantic of a DNN operator.
- Lift the symbolic expressions to the operator symmary of a DNN operator in their IR format includes the ASTs and other information.
- Generates template ASTs through the afore-mentioned operator summary generation.

(3) DNN Model Lifting
Lifts each operator summary to a DNN operator and convert it to a high-level DNN representation (i.e., an ONNX model).
- Matches the AST in each operator summary with a template AST to determine its DNN operator type.
- Recovers the DNN topology by identify the data dependencies between DNN operators.
- Recovers each DNN operator's attributes and parameters leveraging the identified DNN operator type and DNN topology, and converts the fully-recovered DNN model to an ONNX model.

In summary, they first recover the functions and CFGs. Then, they identify the intresting functions (inference function and NN operators). Then, they extract the symbolic expressions by SSE and transform them to their IR. Then, they **match the IR with their AST template and determines the NN operator type**. Then, they **recovers the topology by identifying the data dependencies between NN operators**. Finally, they recovers each NN operator's **attributes and parameters** using the operator type and topology and convert it to ONNX language.

## Evaluation

#### Generality

![image](https://user-images.githubusercontent.com/11942934/199419588-d98a6964-2dfb-4b96-bd4d-cf1113c7df38.png)


How many commonly-used DNN operators and models can be suported.

Support 59 (84%) DNN operators.
Fully support 30 (81%) DNN models out of the collected 37 DNN models.

#### Correctness Accross different DNN compilers, ISAs, and DNN models

![image](https://user-images.githubusercontent.com/11942934/199419671-76dccf6c-de4c-4cd5-89e5-5fe4a6a9368e.png)


Compare the model architecture (operators and topology) and inference results of original DNN models and decompiled DNN models.

-Models: MNIST, MobileNets v2, ResNet v1

- ISAs: Thumb, AArch64, x86-64

- Decompiler: Glow, TVM

Evaluated 15 DNN binaries in total.

没有说明测试的二进制是否是strip的

## Case Study

- Extraction Attack
- Boosting Adversarial Attacks

## Discussion and Limitations

- Compilers (XLA, NNFusion) which generate DNN binaries linked with open-souce mathematical libraries to leverage the tensor operations of these libraries. To support these additional compilers, we will need to implement a dedicated analysis to identify these tensor-specific library functions. This analysis could take advantage of function matching approaches.

- Decompiling Binary on DNN Acceleartors. GPUs, FPGAs have very diverse ISAs that are usually not supported by the general-purpose disassemblers and the symbolic execution framework. **NVIDIA provides closed-source disassemblers cuobjdump and nvidiaasm**, which translate the CUDA binary into SASS assembly code. However, most details of SASS assembly code are kept secret.

## Related Work

reverse engineering techniques targeting smart contract[1], control firmware[2] and Bluetooth firmware[3].

[1] Yi Zhou, Deepak Kumar, Surya Bakshi, Joshua Mason, Andrew Miller, and Michael Bailey. Erays: reverse engineering ethereum’s opaque smart contracts. In Proceedings of the USENIX Security Symposium (Usenix SEC), 2018.

[2] Taegyu Kim, Aolin Ding, Sriharsha Etigowni, Pengfei Sun, Jizhou Chen, Luis Garcia, Saman Zonouz, Dongyan Xu, and Dave (Jing) Tian. Reverse engineering and retroﬁtting robotic aerial vehicle control ﬁrmware using dispatch. In Proceedings of the ACM International Conference on Mobile Systems, Applications, and Services (MobiSys), 2022.

[3] Jianliang Wu, Ruoyu Wu, Daniele Antonioli, Mathias Payer, Nils Ole Tippenhauer, Dongyan Xu, Dave Jing Tian, and Antonio Bianchi. LIGHTBLUE: Automatic Proﬁle-Aware debloating of bluetooth stacks. In Proceedings of the USENIX Security Symposium (Usenix SEC), 2021.

