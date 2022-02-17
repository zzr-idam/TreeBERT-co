# Titel
TreeBERT: A Tree-Based Pre-Trained Model for Programming Language.   
# Abstract
In this paper, we propose TreeBERT, a tree-based pretrained model for programming language. TreeBERT follows the Transformer encoder-decoder architecture. To enable the Transformer to utilize the tree structure, we represent the AST corresponding to the code snippet as the set of the root node to terminal node paths as input and then introduce node position embedding to obtain the position of the node in the tree. We propose a hybrid objective applicable to AST to learn syntactic and semantic knowledge, i.e., tree masked language modeling (TMLM) and node order prediction (NOP). TreeBERT can be applied to a wide range of PL-oriented generation tasks by means of fine-tuning and without extensive modifications to the task-specific architecture.
## Requirements
* Python 3.7
* Numpy
* tree-sitter
* PyTorch >= 1.0
* tqdm

## Pre-training Data Ready
The pre-training dataset we use is the Python and Java pre-training corpus published by [CuBERT](https://github.com/google-research/google-research/tree/master/cubert).   
By running `dataset\ParseTOASTPath.py` you can transform the code snippet into an AST, extract the path from the root node to the terminal node in the AST, and standardize the format of the target code snippet.

## Fine-tuning Data Ready
In method name prediction, we evaluate TreeBERT on two datasets, Python and Java, where the python dataset uses [py150](https://www.sri.inf.ethz.ch/py150) , and the Java dataset uses [java-small](https://s3.amazonaws.com/code2seq/datasets/java-small.tar.gz), [java-med](https://s3.amazonaws.com/code2seq/datasets/java-med.tar.gz), and [java-large](https://s3.amazonaws.com/code2seq/datasets/java-large.tar.gz).
These data sets can be processed into the form required by the code summarization task by running `dataset\Get_FunctionDesc.py`.
```
{
    "function": Function, its function name is replaced by "__",
    "label": Function Name
}
```

In code summarization, we use the Java dataset provided by [DeepCom](https://github.com/xing-hu/DeepCom/blob/master/data.7z) to fine-tune our model.
## Pre-training
#### 1. Create vocab:
```
python dataset/vocab.py -c /home/pretrain_data_AST/ -o data/vocab.large -f 2 -m 32000
```
#### 2.Training TCBERT using GPU:
```
python __main__.py -td /home/pretrain_data_AST_train/ -vd /home/pretrain_data_AST_test/ -v data/vocab.large -o output/treebert.model --with_cuda True
```

## Cite
```
{
author = {Xue Jiang and **Zhuoran Zheng** and Chen Lyu and Liang Li and Lei Lyu},
title  = {TreeBERT: {A} tree-based pre-trained model for programming language},
booktitle = {UAI 2021}
}
```
##  Other work
Titel : Code Representation Based on Hybrid Graph Modelling
```
{
author={Wu, Qiong and Jiang, Xue and **Zheng, Zhuoran** and Gao, Xuejian and Lyu, Chen and Lyu, Lei},
booktitle={ICNIP},
year={2021}
}
```
