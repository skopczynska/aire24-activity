# CiRA: An Open-Source Python Package for Automated Generation of Test Case Descriptions from Natural Language Requirements

[![arXiv](https://img.shields.io/badge/arXiv-2310.08234-b31b1b.svg)](https://arxiv.org/abs/2310.08234)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.8033534.svg)](https://doi.org/10.5281/zenodo.8033534)
[![GitHub](https://img.shields.io/badge/https://github.com/JulianFrattini/cira--eval/-%23121011.svg?logo=github&logoColor=white)](https://github.com/JulianFrattini/cira-eval/)

The Causality in Requirements Artifacts (CiRA) tool is a BERT-based software that processes conditional, natural language requirements sentences and automatically generates test case descriptions from it.

Below, you find a detailed description of this use case. 
In the [activity-report.md](activity-report.md) file, you can document the LLM of your choice and the prompt you develop.
The [data.md](data.md) file contains (1) input data that you can feed your LLM, (2) the expected output (i.e., the ground truth), and (3) the output that the CiRA tool produced.

## Use Case

### Overview

For example, when providing the following sentence to CiRA ...

> When the red button is pushed or the power fails the system shuts down.

... the tool generates (after a few intermediate steps) the following table of test cases

| TC | the red button | the power | the system |
|---|---|---|---|
| 1 | is pushed | not fails | shuts down |
| 2 | not is pushed | fails | shuts down |
| 3 | not is pushed | not fails | not shuts down |

You can explore the tool with own requirements via our [interactive CiRA tool demo](http://www.cira.bth.se/demo) or inspect some [examples](http://www.cira.bth.se/examples).

### Details

#### Step 1: Classification

The tool determines whether the sentence is causal or not. Non-causal sentences cannot be processed.

#### Step 2: Labeling

The tool assigns up to two labels to each token in the sentence: (1) whether it represents a cause or effect, and (2) which role it plays in the cause or effect (e.g., a variable, like *the red button*, or a condition, like *is pushed*).
The example sentence would be labeled as follows:

![CiRA Demonstration: Step 2](/studies/aire-23-frattini/figures/cira-demo-2-labels.PNG)

#### Step 3: Graph Generation

From these labels, the tool generates a directed cause-effect graph.
In this graph, every node represents a cause, effect, or a junctor (`&&` (and) or `||` (or)), and edges represent causal relationships as implied by the sentence.

![CiRA Demonstration: Step 3](/studies/aire-23-frattini/figures/cira-demo-3-ceg.PNG)

#### Step 4: Test Suite Generation

Finally, the tool generates test cases.
For this, all relevant combinations of values for the cause events are combined and associated with the expected values of the effect events.

![CiRA Demonstration: Step 4](/studies/aire-23-frattini/figures/cira-demo-4-tests.PNG)

## Reference

Frattini, J., Fischbach, J., & Bauer, A. (2023, September). CiRA: An Open-Source Python Package for Automated Generation of Test Case Descriptions from Natural Language Requirements. In 2023 IEEE 31st International Requirements Engineering Conference Workshops (REW) (pp. 68-71). IEEE. https://doi.org/10.1109/REW57809.2023.00019
