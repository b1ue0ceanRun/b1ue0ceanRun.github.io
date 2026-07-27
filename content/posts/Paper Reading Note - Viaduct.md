---
title: "Paper Reading Note - Viaduct"
date: 2025-04-18T15:41:53+02:00
tags:
  - Paper Reading
---

## Introduction
two weeks later, my teammate and I  will present a paper -- Viaduct
Let's follow the instructions given by our prof!

<!--more-->

## How to read a research paper

You may find if useful to ask yourself the following questions as you read the paper, or after you read the paper. 

- What problem is the paper addressing? How well do they succeed in solving the problem? 
- What is the contribution of the paper? How important is this contribution? 
- How would you have addressed the problem? 
- What was unclear to you? 
- How does the paper relate to other papers we have read or studied? 
- Are there any obvious or non-obvious extensions to this work? 
- Can you suggest a two-sentence project idea based on the ideas of this paper? 
- Here is a more elaborate discussion on [how to read a research paper](http://www.eecs.harvard.edu/~michaelm/postscripts/ReadPaper.pdf) by Michael Mitzenmacher.


## Paper Reading
Let's solve these problems step by step!
### 1. What problem is the paper addressing? How well do they succeed in solving the problem? 



### How would you have addressed the problem? 
Using threat model is a good solution I think.

Compiled programs run across multiple hosts, each executing a single thread and communicating via secure, asynchronous message-passing, with no shared memory. Timing attacks are out of scope, and availability is not guaranteed. In Viaduct's model, each host may consider others as potential attackers, depending on the context (e.g., Alice vs. Bob in the millionaires problem). Attacker capabilities are modeled via _labels_ that determine what data they can read (confidentiality) or modify (integrity). Full corruption of a single host is not feasible if mutual trust exists, as corruption of one’s integrity implies corruption of the other's as well.





## Presenting papers

Things to think about when preparing your presentation:

- Your primary goal is to clearly communicate the key ideas of the paper. In order to do this, you may need to read some of the related work, and you may need to present background material.
- Your secondary goal is to prompt discussion about the papers.
- You do not have time to cover every detail of the paper. Focus instead on the important and/or interesting aspects of the paper.
- The structure of the paper is often useful as the structure of your presentation. However, this is not always the case. Make a conscious decision about how you will structure your presentation, and why.
- Do not read aloud the text on your slides.
- For advice on giving good presentations, see, for example, [Mark Hill's advice](http://pages.cs.wisc.edu/~markhill/conference-talk.html), [Matthew Miller's tips](http://www.matthewjmiller.net/ramblings/presentation-tips/), or Simon Peyton Jones' slides on how to give a good research talk (via Mark Leone's advice [website](https://www.cs.cmu.edu/~mleone/how-to.html)).

### related work & background material

#### Historical Millionaires’ Problem

*The Millionaire’s Problem, introduced by Andy Yao in 1982, began the study of privacy-preserving multiparty computation. Alice and Bob want to know who is the richer without revealing how much they are actually worth.*
https://zoo.cs.yale.edu/classes/cs461/2009/lectures/ln21.pdf

<img src="Pasted image 20250418160430.png" alt="示例图片" width="500" height="300">



#### A simple introduction for every protocol


<img src="Pasted image 20250418165358.png" alt="示例图片" width="500" height="300">
