---
title: "AI体系的思考和梳理"
date: 2026-01-31T15:37:51+01:00
tags:
  - AI
  - LLM
---

这篇文章出现的原因是因为我最近处于一个比较关键的转折点。
接下来要跟未来潜在的导师合作3个月。
可能要做一些LLM&SEC的工作。
因为最近在学校做了一些静态分析的工作，反而错过了一些AI的发展？
而接下来的工作需要我快速的建立我在AI领域的知识体系。

<!--more-->

## 对于AI体系的思考

我之前的AI知识体系还停留在两年前，错过了LLM蓬勃发展的两年。
所以如何快速构建一个适应当下的LLM知识体系尤为重要。

首先是一些名词的层出不穷，LLM distilling, DPO,  agentic RL ...
如何有体系的学习，快速的学习成为了我这几天要思考的问题，因为下周就要上手项目了。


### 自顶向下的学习

如果从自顶向下的学习的角度，以论文 [VulnLLM-R: Specialized Reasoning LLM with Agent Scaffold for Vulnerability Detection](https://arxiv.org/pdf/2512.07533) 为例。

论文的目标：将漏洞检测建模为一个**推理任务（reasoning task）**，而不是分类任务。

论文原话直接拿来：
train a small reasoning model that can correctly identify the vulnerabilities in input functions or projects, even when the vulnerability belongs to OOD CWEs. We further design the model to not only detect the vulnerability but label it with the correct CWE, which is valuable context for triage and patching.

训练一个专业领域的LLM的步骤大概有：
1. Dataset constructions
2. Reasoning data generation

![[Pasted image 20260131160116.png]]







## 现有的LLM体系
知识蒸馏

什么是 reduced reasoning




## LLM的架构
MOE










老师想看到的 一个成熟的训练计划
训练的细节 每一个步骤都要了解

多问问Why
数据集构建的细节，
提供的数据集有两个
两个分别是什么？
reasoning从哪来？是直接从 DeepSeek 推理来的吗
确实要考虑，因为其实reasoning是没有正确答案的。（如何评估或者改进这一部分比较重要，收集writeup？但是数据集又没有这种writeup）
