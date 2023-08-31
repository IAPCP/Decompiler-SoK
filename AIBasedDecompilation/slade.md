# SLaDe: A Portable Small Language Model Decompiler for Optimized Assembler

The article discusses the challenges and limitations of existing decompilation techniques and introduces a new approach called SLaDe. SLaDe is a neural decompiler that aims to generate readable and correct code from low-level assembler programs. It uses type inference and neural machine translation techniques to achieve this goal.

The article evaluates SLaDe on various benchmarks and compares its performance with other decompilation techniques such as Ghidra and ChatGPT. SLaDe demonstrates superior accuracy and readability compared to these approaches, especially on optimized code. It also shows portability across different instruction set architectures (ISAs) and optimization levels.

The authors highlight the importance of type inference in improving the accuracy of decompiled code and discuss the correlation between different features of assembler programs and decompilation performance. They also address the limitations and failure cases of SLaDe.

Overall, the article presents SLaDe as a promising approach to decompilation, combining the accuracy of traditional techniques with the readability of neural approaches. It offers potential benefits for reverse engineering, code analysis, and software maintenance tasks.


## Model

- Training Data Set: AnghaBench
- a sequence-to-sequence transformer with 6 encoder layers, 6 decoder layers, and an embedding size of 1024. The context window for both the source and target sequences is set to 1024 positions. The model is trained using the Adam optimizer, and the parameters are initialized from a normal distribution with a mean of 0 and a standard deviation of 0.02. Dropout regularization is not used in the model.

## Evaluation

- Metric: The evaluation metrics used in the study are input-output (IO) accuracy and edit similarity. 
- Result

The evaluation results show that SLaDe outperforms other decompilation techniques in terms of both accuracy and readability. Here are some key findings:

1. Accuracy: SLaDe achieves higher IO accuracy compared to Ghidra, ChatGPT, and BTC (a neural decompiler) across different benchmarks, ISAs, and optimization levels. It is up to 6 times more accurate than Ghidra and up to 4 times more accurate than ChatGPT.

2. Readability: SLaDe generates significantly more readable code compared to Ghidra and ChatGPT. It achieves higher edit similarity, indicating that the decompiled code closely resembles the original source code.

3. Portability: SLaDe demonstrates portability across different ISAs (ARM and X86) and code optimization levels. It can decompile programs written in x86 and ARM assembly languages, both in optimized and unoptimized forms.

4. Type Inference: SLaDe incorporates type inference to handle external type declarations, which improves the accuracy of decompilation compared to other neural decompilers.

Overall, SLaDe provides accurate and readable decompilation results, making it a promising approach for reverse engineering and program analysis tasks.


