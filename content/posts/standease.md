---
title: "Stand Ease first"
date: 2026-04-04
draft: true
---

## Introduction

Attention is all you need is a ground breaking research that made modern LLM scalable and usable at the same time.Published in 2017 by Google Brain.Before attention mechanism Natural Language Processing was heavily dependend on RNN/CNN.

But as RNN process text seq2seq and hidden state h*t cannot be calculated before h*(t-1),it bacame bottleneck for processing(no any parallism).It also struggle with long-term dependencies.

Similarly CNNs have local receptive fields.They have this issue that they find it more difficult to learn dependencies between distant position.To capture longrange dependencies:

- You need deep stacks or large kernels
- Which becomes inefficient and harder to

Transformers solves these issue by removing RNN and CNN entirely and using self attention as primary mechanism combined with feedforward layers and positional encoding.

## Model Architecure.

![modelarch](/images/attentionarch.png)

### Encoder stack

### Decoder stack
