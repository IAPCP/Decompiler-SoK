# Asm2Vec: Boosting Static Representation Robustness for Binary Clone Search against Code Obfuscation and Compiler Optimization (S&P 2019)
<img width="654" alt="image" src="https://user-images.githubusercontent.com/24778335/198515973-2682e810-1d2a-437f-bd47-d0c137b2bbe5.png">

## Abstract
Reverse engineering is a manually intensive but necessary technique for understanding the inner workings of new malware,
ﬁnding vulnerabilities in existing systems, and detecting patent infringements in released software. 
An assembly clone search engine facilitates the work of reverse engineers by identifying those duplicated or known parts. 
However, it is challenging to design a robust clone search engine, since there exist various compiler optimization options
and code obfuscation techniques that make logically similar assembly functions appear to be very different.

A practical clone search engine relies on a robust vector representation of assembly code. 
However, the existing clone search approaches, which rely on a manual feature engineering process to form a feature vector for an assembly function, 
fail to consider the relationships between features and identify those unique patterns that can statistically distinguish assembly functions. 
To address this problem, we propose to jointly learn the **lexical semantic relationships and the vector representation** of assembly functions 
based on assembly code. We have developed an assembly code representation learning model Asm2Vec. 
It only needs assembly code as input and does not require any prior knowledge such as the correct mapping between assembly functions. 
It can ﬁnd and incorporate rich semantic relationships among tokens appearing in assembly code. 
We conduct extensive experiments and benchmark the learning model with state-of-the-art static and dynamic clone search approaches. 
We show that the learned representation is more robust and signiﬁcantly outperforms existing methods against changes introduced by obfuscation and optimizations.


## Method
There are four steps: 

- **Step 1:** Given a repository of assembly functions, we ﬁrst build a neural network model for these functions. We only need their assembly code as training data without any prior knowledge. 
- **Step 2:** After the training phase, the model produces a vector representation for each repository function. 
- **Step 3:** Given a target function f t that was not trained with this model, we use the model to estimate its vector representation. 
- **Step 4:** We compare the vector of f t against the other vectors in the repository by using cosine similarity to retrieve the top-k ranked candidates as results.

The training process is a one-time effort and is efﬁcient to learn representation for queries. If a new assembly function is added to the repository, we follow the same procedure in Step 3 to estimate its vector representation. The model can be retrained periodically to guarantee the vectors’ quality.

<img width="784" alt="image" src="https://user-images.githubusercontent.com/24778335/198515424-47543909-b77f-4d35-aef1-9f7209f18f78.png">

<img width="1007" alt="image" src="https://user-images.githubusercontent.com/24778335/198515530-03d2f553-4831-4348-884f-a630cb679842.png">

## T-SNE Visualization
<img width="682" alt="image" src="https://user-images.githubusercontent.com/24778335/198515588-11a4bbaf-cd98-459c-ba62-e3f2a1c008ee.png">

<img width="597" alt="image" src="https://user-images.githubusercontent.com/24778335/198515606-a2d11471-658d-406e-bd2b-5eec3231f324.png">


## Evaluation

#### Compiler Optimization
<img width="917" alt="image" src="https://user-images.githubusercontent.com/24778335/198515703-b4c18869-2b8e-4499-b30e-ad4eae23878e.png">

#### Code Obfuscation
<img width="942" alt="image" src="https://user-images.githubusercontent.com/24778335/198515753-ab8cfa45-91e5-4a72-ba17-47c8c82225e8.png">

#### Searching Vulnerability Functions
![image](https://user-images.githubusercontent.com/24778335/198515805-26975527-3764-4355-acce-b7787eafce5f.png)
<img width="498" alt="image" src="https://user-images.githubusercontent.com/24778335/198515822-4a6155b7-d208-41cc-960c-e195b6f8546b.png">
