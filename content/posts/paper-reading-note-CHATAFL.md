---
title: "Paper Reading Note - CHATAFL"
date: 2023-10-21T15:03:05+08:00
tags: ["fuzz","Paper Reading"]
categroies: ["fuzz"]
---



Actually, this work is very similar to my research direction in my internship project. I also think some ideas to use LLM to fuzz, but because my work ability and timeline, I haven't finish my project yet. So, today I will summarize  the good idea contributed from this paper.

<!--more-->
## Before Reading
Before diving into it, I have a few questions:
- Where and how can LLM be applied to fuzzing?
    - In state transitions?
    - In testcase generation?
    - In mutation?
- Regarding LLM:
    - Which LLM was used by the author?
    - Did the author fine-tune it?
    - If so, how was it fine-tuned?
- Which protocol did the author research?
- What are the cost and efficiency considerations?

With these questions in mind, I will begin my reading.
## Start Reading
### Abstract

>Our protocol fuzzer CHATAFL constructs grammars for each message type in a protocol, and then mutates messages or predicts the next messages in a message sequence via interactions with LLMs. |

And the CHATAFL improve the coverage compare with NSFUZZ.

>CHATAFL covers 47.60% and 42.69% more state transitions, 29.55% and 25.75% more states, and 5.81% and 6.74% more code, respectively.


### INTRODUCTION

The concept of "LLM-guided protocol fuzzing"
1. the fuzzer uses the LLM to extract a machine-readable grammar for a protocol that is used for structure-aware mutation
2. the fuzzer uses the LLM to increase the diversity of messages in the recorded message sequences that are used as initial seeds. 
3. the fuzzer uses the LLM to break out of a coverage plateau, where the LLM is prompted to generate messages to reach new states.

Than the author compare the efficiency of chatafl with [protocol fuzzer benchmark](https://github.com/profuzzbench/profuzzbench).

### BACKGROUND AND MOTIVATION

Here, the author suggest some challenges
>1. (C1) Dependence on initial seeds. The effectiveness of mutation-based protocol fuzzers is severely limited by the provided initial seed inputs. The pre-recorded message sequences will hardly cover the great diversity of protocol states and input structures as discussed in the protocol specification.
>2. (C2) Unknown message structure. Without machinereadable information about the message structure, the fuzzer cannot make structurally interesting changes to the seed messages, e.g., to construct messages of unseen types or to remove, substitute, or add an entire, coherent data structure to a seed message.
>3. (C3) Unknown state space. Without machinereadable information about the state space, the fuzzer cannot identify the current state or be directed to explore previously unseen states.


Actually, what's most appealing here is the solution for the "state," the rest is just so so. Regarding the "state," I need to find out which specific state the author is referring to later.

There are three state in Fuzzing

![](/pictures/20231021161745.png)


Next, let's see the Motivation, here the author introduce their solutions for the three challenges mentioned above.
1. Addressing seed dependence by having the LLM **add a random message** to a seed message sequence, with a focus on improving message diversity and validity (C1).
2. Dealing with unknown message structures by **requesting the LLM to provide machine-readable information** about message grammar for various message types, with a focus on assessing the quality and coverage of these grammars compared to the ground truth (C2).
3. Navigating the unknown state space by **requesting the LLM to generate a message that leads to a new state**, with an investigation into the effectiveness of this approach (C3).

How to tell LLM to generate, wondering to know!

### CASE STUDY: TESTING THE CAPABILITIES OF LLMS FOR PROTOCOL FUZZING

Skip to the third part: Inducing Interesting State Transitions
>We provide the LLM the message exchange between fuzzer and the protocol implementation and ask it to return a message that would lead to a new state. We evaluate how likely the message induce a transition to a new state. Specifically, we provide the LLM with existing communication history, enabling a server respectively to reach each state (i.e., INIT, READY, PLAY, and RECORD). Afterward, we query the LLM to determine the next client requests that can affect the server’s state. To mitigate the influence of the LLM’s stochastic behavior, we prompted the LLM 100 times for each state.

Actually, I had proposed the same idea before, but I hadn't put it into practice because we selected a more complex target. Perhaps this event is educational for me. I should do some practices from the simplest target.

Perhaps considering how to minimize the LLM cost in algorithm design could also be a promising research direction?
### LLM-GUIDED PROTOCOL FUZZING

#### A. Grammar-guided Mutation
##### 1) Grammar Extraction

Here, the author use a method to generate message grammar called -- in-context few-shot learning. Cool, this is a super idea!
>In-context learning serves as an effective approach to fine-tuning the model. Few-shot learning is utilized to enhance the context with a few examples of desired inputs and outputs. This enables the LLM to recognize the input prompt syntax and output patterns.

##### 2) Mutation based on Grammar
skip!

#### B. Enriching Initial Seeds
skip!

#### C. Surpassing Coverage Plateau
A complex design, let's skip it! For have my own ideas, but they're not suitable to share.

<img src="/pictures/20231021170616.png" width="600" height="1200">