# Graph Neural Networks for Program Analysis

Research project (coursework) at HSE University, Faculty of Computer Science.
Author: Zosia Shamina · Supervisor: Danil Shaikhelislamov

## Overview

Source code written in any language or style can be represented as a graph. This project studies
graph representations of programs and how Graph Neural Networks (GNNs) can be applied to program
analysis tasks, with a focus on **software defect prediction**.

The project includes:
- a review of program graph representations (AST, CFG, DFG, call graph), illustrated on a toy ML pipeline;
- a classification of GNN-based approaches by program analysis task and graph type;
- a transferability study of an existing GNN model on a new task and dataset;
- an architectural improvement of the model on its original task.

## Experiments

### 1. Transferring an existing model to a new task

- **Model:** CFG2AT (*Control Flow Graph and Graph Attention Network-Based Software Defect Prediction*)
- **New task:** variable misuse detection
- **Dataset:** GREAT
- **Result:** ROC AUC ≈ 0.52 on test, so the architecture does not transfer well to this setting.

### 2. Improving the architecture on the original task

- **Task:** software defect prediction (original CFG2AT setup)
- **Change:** the graph attention network is replaced with a **Gated Attention Network (GaAN)**,
  where a convolutional sub-network computes a gate for each attention head and controls its
  contribution to the final representation.
- **Data:** control flow graphs built from the Python source code of `pandas` with
  [StaticCFG](https://github.com/coetaur0/staticfg). Train: v2.2.0 → Test: v2.2.1.

## Results

| Method | Precision | Recall | F1 | AUC | MCC |
|---|---|---|---|---|---|
| GAT (CFG2AT baseline) | 0.801 | 0.767 | 0.784 | 0.698 | 0.389 |
| **GaAN (ours)** | **0.902** | **0.878** | **0.890** | **0.846** | **0.685** |
| Relative improvement | +12.6% | +14.5% | +13.5% | **+21.2%** | +76.1% |

GaAN outperforms the baseline on every metric, with a 21.2% gain in AUC.
A model like this could be integrated into CI/CD tools to flag potentially defective code changes.

## Future work

- Combining several program graph representations into a single knowledge graph
- Adapting the architecture to other program analysis tasks
- Extending experiments to multilingual datasets
- Integrating the model into software development tools
