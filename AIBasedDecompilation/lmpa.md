
# LmPa: Improving Decompilation by Synergy of Large Language Model and Program Analysis

## Summary

The document discusses a novel approach to name recovery for binary executables, which combines the use of a large language model (LLM) and program analysis. The method, called LmPa, breaks down the procedure of name recovery into multiple queries to an LLM and uses program analysis to chain them together. This iterative procedure allows LLMs to gradually improve over time. 

The authors developed a name propagation algorithm that has a similar nature to program semantics, allowing LLM queries in future rounds to have more contextual information. For example, a newly generated callee function name is propagated to the invocation sites in its callers. 

The results of the study showed that LmPa outperformed DIRTY, a previous method, in terms of precision and recall. LmPa achieved 33.85% precision and 31.12% recall, substantially outperforming DIRTY, which had 17.31% precision and 10.89% recall. 

The document also discusses the limitations of existing language models, such as their limited input size, and how LmPa addresses these issues. The authors also discuss the challenges of name recovery, such as the introduction of a large number of intermediate variables at the binary level that may not correspond to any source variable. 

The authors conclude by highlighting the contributions of their research, including the proposal of a novel approach to name recovery for binary executables and the development of a name propagation algorithm. They also discuss future directions for their research, such as improving the precision of their method and exploring other applications of their approach.
