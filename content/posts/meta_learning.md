---
title: Meta learning
date: 2024-03-15T14:52:36+08:00
tags:
  - ML
categroies:
  - ML
draft: true
---
## Introduction

Because I need to use this ML method in my project, so I'd like to write a blog to take notes about my learning experiences.

First and foremost, let's find out what is meta learning from high level.

Let's just google it.


> Meta-learning, described as “learning to learn”, is a subset of machine learning in the field of computer science. It is used to improve the results and performance of the learning algorithm by changing some aspects of the learning algorithm based on the results of the experiment.


Recently I read a paper *Few-shot encrypted traffic classification via multi-task representation enhanced meta-learning* which made me contact the concept of meta learning at the first time, so I'm going to practice some basic skills before using it further.

Here are three popular algorithms
- **Prototypical Networks** ([Snell et al., 2017](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Farxiv.org%2Fpdf%2F1703.05175.pdf))
- **Model-Agnostic Meta-Learning / MAML** ([Finn et al., 2017](https://colab.research.google.com/corgiredirector?site=http%3A%2F%2Fproceedings.mlr.press%2Fv70%2Ffinn17a.html))
- **Proto-MAML** ([Triantafillou et al., 2020](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fopenreview.net%2Fpdf%3Fid%3DrkgAGAVKPr)).


## Process

1. We would train the model on the binary classifications of cats-birds and flowers-bikes
2. but during test time, the model would need to learn from 4 examples each the difference between dogs and otters, two classes we have not seen during training (Figure credit - [Lilian Weng](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Flilianweng.github.io%2Flil-log%2F2018%2F11%2F30%2Fmeta-learning.html)).

<img src="/pictures/20240315152702.png">

That is to say, if you have a basic model and you can use meta learning to make the few-show learning possible.

Why meta learning can be used in encrypted traffic classification?





## Reference
https://jameskle.com/writes/meta-learning-is-all-you-need

https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial16/Meta_Learning.html