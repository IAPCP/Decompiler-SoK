# Decompiling x86 Deep Neural Network Executables

Zhibo Liu, The Hong Kong University of Science and Technology

## Abstract

They present BTD (Bin to DNN), a decompiler for DNN executables. BTD takes DNN executables and outputs full model specifications, including types of DNN operators, network topology, dimensions, and parameters that are (nearly) identical to those of the input models. **Supports different DL compilers and with full optimizations enabled on x86 platforms.**

**Employs learning-based techniques to infer DNN operators, dynamic analysis to reveal network architectures, and symbolic execution to facilitate inferring dimensions and parameters of DNN operators.**

BTD enables accurate recovery of full specifications of complex DNNs with millions of parameters (e.g., ResNet). The recivered DNN specifications can be re-compiled into a new DNN executable exhibiting identical behavior to the input executable.

They also demonstrate cross-architecture legacy code reuse using BTD, and envision BTD being used for other critical downstream tasks like DNN security hardening and patching.

## Introduction

A DL compiler takes a high-level model specification (e.g., in ONNX format) and generates corresponding low-level optimized binary code for a variety of hardware backends (from cloud servers to embedded devices, GPUs, CPUs, and FPGAs).

Compilation of high-level models into binary code typically involves multiple optimization cycles. DL compiler can optimize code utilizing domain-specific hardware features and abstractions. However, they observe that different low-level representaions of the same DNN operator in executables generally retain invariant high-level semantics, as DNN operators like ReLU and Sigmoid, are mathematically defined in a rigorous manner.

They proposed a three-step approach for full recovery of DNN operators, network topology, dimensions, and parameters.

- BTD conducts representation learning over disassembler-emitted assembly code to classify assembly functions as DNN operators, such as convolution layers (Conv).
- Dynamic analysis is then used to chain DNN operators together, thus recovering their topological connectivity.
- To further recover dimensions and parameters of certain DNN operators (e.g., Conv), they launch trace-based symbolic execution to generate symbolic constraints, primarily over floating-point-related computations. The human-readable symbolic constraints denote semantics of corresponding DNN operators that are invariant across different compilation settings. To deliver an automated pipeline, they then define patterns over symbolic constraints to automatically recover dimensions and memory layouts of parameters. They incorporate taint analysis to largely reduce the cost of symbolic execution which is more heavy weight.

- BTD is scalable to recover DNN models from 65 DNN executables, including nearly 3 million instructions, in 60 hours with negligible errors.

- Moreover, to demonstrate BTD's correctness, they rebuild decompiled model specifications with PyTroch. The results show that almost all decompiled DNN models can be recompiled into new executables that behave identically to the reference executables. 

## Preliminary

<img width="1050" alt="image" src="https://user-images.githubusercontent.com/11942934/199477789-de6e9013-fa4f-4cde-ae1f-a6036c9416c2.png">

**DNN Compiler Frontend: Graph IRs and Optimizations.** Convert DNN computation graphs into graph IRs. Graph IRs specify high-level inputs and outputs of each operator, but do not restrict how each operator is implemented.
Transformation and optimization of computation grpahs (IRs).

**DNN Compiler Backend: Low-Level IRs and Optimizations.** Graph IR operators can be converted into low-level linear algebra operators. For example, a fully connected (FC) operator can be representated as matrix multiplication followed be addition. Low-level IRs are usually memory related. Hence, optimizations at this step can include hardware intrinsic (固有的) mapping, memory allocation, loop-related optimizations, and parallelization.
Transformation and optimization of low-level linear algebra operators.

**DNN Compiler Backend: Scheduling and Tuning.** Policies mapping an operator to low-level code are called *schedules*. 

**DNN Compiler Backend: Code Gen.** When generating machine code, a DNN operator (or several fused operators) is typically compiled into an individual assembly function. 

## Decompiling DNN Executables

**Definition.** The full specifications include: (1) DNN operators(e.g., ReLU, Pooling, and Conv) and their topological connectivity, (2) dimensions of each DNN operator, such as #channels in Conv, and (3) parameters of each DNN operator, such as weights and biases, which are important configurations learned during model training.

**Comparison with C/C++ Decompilation.**
- Statements vs. Higher-Level Semantics: Software decompilation line-by-line translates machine instructions into C/C++ statements. 
- Common Uncertainty: There is no fixed mapping between C/C++ statements and assembly instructions. DL compilers may adopt different optimizations for compiling the same DNN operators. The compiled code may exhibit distinct syntactic forms. Nevertheless, the semantics of DNN operators are retained.
- End Goal: Software decompilation is fundamentally undecidable, and decompiled C/C++ code mainly aids (human-based) analysis and comprehension, not recompilation. Besides helping (human-based) comprehension, BTD boosts model reuse, migration, security hardening, and adversarial attacks.

NN的反编译主要是一个分类问题，C/C++的反编译是生成问题。 

**Opacity in DNN Executables.** 

Different compilers and optimizations can result in complex and distinct machine code realizations. 

**Design Focus.** BTD is designed to process common DNN models compiled by standard DL compilers. 

## Design

Deduce high-level model specifications from low-level instructions. 

We advocate DL decompilers to satisfy the following criteria:
- **R1 (Generalizability):** Avoid brittle assumptions. Generalize across compilers, optimizations, and versions.
- **R2 (Correctness):** Use effective, resilient methods and produce correct outputs.
- **R3 (Performance):** Be efficient when necessary.
- **R4 (Automation):** Avoid manual analysis and automate the decompilation process.

<img width="1078" alt="image" src="https://user-images.githubusercontent.com/11942934/199477934-eafd67dd-56b4-4178-8ca0-a97b21f1c5a6.png">


#### Workflow

(1) Learning-based techniques for recognizing assembly function as DNN operators like Conv.

(2) Reconstruct the network topology using dynamic analysis.

(3) Use trace-based symbolic execution to extract operator semantics from assembly code and then recover dimensions and parameters with semantics-based patterns. Some operators are too costly for symbolic execution to analyze. They use taint analysis to keep only tainted sub-traces for more expensive symbolic execution to analyze.

(4) BTD produces model specifications that behave identically to original models. BTD does not guarantee 100% correct outputs. Procedures users can follow to fix errors.

- **Type I** operators, including activation functions like ReLU and element-wise arithmetic operators, do not ship with parameters; recovering their dimensions is trivial.

- **Type II and III** operators require dimensions or parameters, such as Polling's stride *S* and kernel size *K*.

- **Type IV** operators require both parameters and dimensions.

**Compilation Provenance.** (1) which DL compiler is used, and (2) whether e is compiled with full optimization -O3 or no optimization -O0. The authors extend their learning-based method to predict compilation provenance from assembly code.

Some patterns are designed separately for Glow- and TVM-emitted executables.

### DNN Operator Recovery

Train a neural model to map assembly functions to DNN operators. Use representation learning and treat x86 opcodes as language tokens.

**Atomic OPs.** Define atomic OPs over x86 opcodes. Each opcode is thus split into atomic OPs. Use atomic OP sequences represent a DNN operator.

**Dividing Opcodes into Atomic OPs.** BPE. They split each opcode into a sequence of characters and counted consecutive characters to find the most frequent ones.

**Learning over Atomic OPs.** Train a neural identifier model with a sequence of atomic OPs from an assembly function as inputs. The model outputs a 1D vector with N dimensions (N is the total number of unique DNN operators), where multiple "1" in the vector implies that this assembly function represents several fused DNN operators (融合算子). 

**From Operators to Compilation Provenance.** 直接使用之前的函数的embeddings分类，认为这个任务很简单

### DNN Network Topology Recovery

A DNN operator has a fixed number of inputs and outputs. Use Intel Pin, a dynamic instrucmentation tool, to hook every callsite. Record the memory addresses of inputs/outputs passed to callsites and connect two operators if the successor's inputs match the predecessor's outputs.

This step does not rely on any compiler-specific assumptions, however, dynamic analysis is needed. (Intel Pin 也局限了使用的平台，其他架构或许没有这种工具或者无法监控内存，动态分析开销也比较大）

This dynamic analysis is not limited by "coverage". （存在疑问，依赖于DNN编译器的先验知识）

### Dimension and Parameter Recovery

Inputs and parameters are typically stored separately in memory, whereas neighbor input/parameter elements are stored contiguously. （存在疑问，是栈空间还是堆空间，连续可能是偶然的）

**General Workflow.** summarize operator invariant semantics with symbolic execution.

(1) log execution traces and use taint analysis to shroten the traces.

(2) use symbolic execution to summarize the input-output constraint of each assembly function.

(3) infer dimensions using patterns defined over constraints and futher extract parameters.

#### Trace Logging and Taint Analysis

Use Intel Pin to log the execution trace of an operator's assembly function.

**Pin takes several hours to log one trace.**

Hence, analyzing a subtrace containing one iteration of the outermost loop is sufficient (as long as a complete calculation of an output element is reflected in this subtrace).

**Taint Analysis.** Use backward taint analysis to rule out instructions that are not involved in computing outputs.

Trace logging records each instructions's execution context, including concrete memory address values. For each memory acess during taint propagation, they compute concrete addresses to taint/untaint memory cells accordingly.

#### Symbolic Execution

Launch SE over tainted x86 instructions.

**Identifying Memory Layouts.** 建立了一个表，对应每个算子传入的参数应该是inputs还是parameters，还是outputs. Then collect and identify inputs and parameters' memory addresses by querying the configuration. Identify memory addresses and classify them into weights and inputs. Cluster all addresses of the same parameter to scope that parameter's memory region (i.e., the starting address and size).

#### Dimension Recovery

Conv operator

**Kernel Size K, Input Channel $I_C$, Zero Padding P.** 

**Output Channels $O_C$.**
这一部分需要了解相关函数的参数所代表的含义，根据内存大小和布局推测这几个要素。

#### Recover Parameters

Idenfity their starting addresses and memory layouts.
识别存储参数的内存区域。
Use Pin to dump parameters to disk at runtime.
同样依赖于运行时。

**Handling Compiler Optimizations.** SSE parallelism by reading 4 (or 8) floating numbers from contiguous memory into one register. These modify Conv's standard memory layout, impeding parameter recovery. Similar to dimension inference, they use patterns to identify optimized layouts. 

### Executables Emitted by NNFusion

Some DL compilers generate executables statically linked with kernel libraries.

It is easier to decompile NNFusion- and XLA-emitted executables since they contain warpper function to invoke target operator implementations in kernel libraries. 

运行时，通过Pin恢复网络拓扑和劫持通过warpper发送的数据。劫持的数据包括dimensions和指向参数的pointers. 然后从内存中获取参数。


## Evaluation

<img width="813" alt="image" src="https://user-images.githubusercontent.com/11942934/199478119-4450c16f-d547-4b0d-8214-3655d4fc5010.png">


**RQ1 (Comprehensiveness and Correctness):** Is BTD comprehensive and correct to process all operators used in common DL models compiled with **different compilers** and **optimization options**?

- Predicting DNN Operator Type: Glow has 14 types and TVM has 30. 
<img width="507" alt="image" src="https://user-images.githubusercontent.com/11942934/199478217-69600f8f-dfe8-49af-a9d5-2bba0776967c.png">


- 数据偏差：基于统计分布的模型仍然会考虑到某一操作经常出现在另一操作之后使得分类结果偏向于出现概率大的类别，从而产生错误。

- 具有相似汇编代码的操作：也会存在误分类

- **DNN Network Topology Recovery:** compare the recovered network topology with the reference DNN's computation graphs.

- **Parameter and Dimension Recovery:** Except for TVM -O0, it is difficult to compare the recovered dimensions/parameters with the reference due to compiler optimization.
<img width="510" alt="image" src="https://user-images.githubusercontent.com/11942934/199478338-ebcd0abe-e9d6-4e70-8b48-398d20de4be3.png">
<img width="505" alt="image" src="https://user-images.githubusercontent.com/11942934/199478473-13b32a8a-ce74-43cd-ae6b-68d56b787c29.png">


- **Recompilation** They re-implement DNN models in PyTorch using recovered DNN models, then export models as ONNX files and compiled into DNN executables using the same compilation provenance.
compare the predicted labels and confidence scores yielded by recompiled and reference executables over every input from validation dataset.

<img width="524" alt="image" src="https://user-images.githubusercontent.com/11942934/199478603-5e595665-4f47-4165-a814-61274c11d3be.png">


- **Decompiling NNFusion Outputs** 

- **Other Models** NLP Models, Audio Processing Models.

**RQ2 (Robutness):** Is BTD robust to survive frequent DL compiler implementation changes?

Evaluated BTD with prior versions of DL compilers released in the past two years.

<img width="387" alt="image" src="https://user-images.githubusercontent.com/11942934/199478744-58522f82-213b-4ae2-9daf-c933aeb7acc9.png">


BTD is robust enough against changes in current and prior versions of DL compilers. We anticipate that compiler changes are unlikely to affect the robustness of BTD in the near future.

**RQ3 (Extensibility):** Can BTD be easily extended to support new operators and models? What efforts are needed?

Recovery of parameters/dimensions of complex operator. Supporting a new operator may need new or existing patterns. Symbolic constraints are generally human readable. We typically need several hours to desion and validate a new pattern for operators without complex optimization.

Users experienced in DL models can spend reasonable effort to add support for new operators and models by modifying existing patterns in BTD.

**RQ4 (Error Fixing):** How does BTD handle decompilation errors?

On one hand, with rules presented in Sec. 6, BTD can detect and automatically fix the errors exposed when decompiling the ResNet18 executable.

To cope with decompilation defects, BTD provides error detection & automated fixing mechanism, including a collection of rules derived from domain-specific knowledge and observations.

## Discussion

DL compilers produce distinct executables on GPUs and CPUs. For example, TVM creates a standalone DNN executable on CPU, but a runtime library, including detailed model information, and an OpenCL/CUDA executable on GPU.



