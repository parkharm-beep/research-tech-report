---
title: "$δ$-mem: Efficient Online Memory for Large Language Models"
source: "https://arxiv.org/abs/2605.12357"
author:
  - "[[Jingdi Lei]]"
  - "[[Di Zhang]]"
  - "[[Junxian Li]]"
  - "[[Weida Wang]]"
  - "[[Kaixuan Fan]]"
  - "[[Xiang Liu]]"
  - "[[Qihan Liu]]"
  - "[[Xiaoteng Ma]]"
  - "[[Baian Chen]]"
  - "[[Soujanya Poria]]"
published:
created: 2026-05-14
description: "Abstract page for arXiv paper 2605.12357: $δ$-mem: Efficient Online Memory for Large Language Models"
tags:
  - "clippings"
---
## Title:δ-mem: Efficient Online Memory for Large Language Models

Authors:[Jingdi Lei](https://arxiv.org/search/cs?searchtype=author&query=Lei,+J), [Di Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang,+D), [Junxian Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+J), [Weida Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang,+W), [Kaixuan Fan](https://arxiv.org/search/cs?searchtype=author&query=Fan,+K), [Xiang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu,+X), [Qihan Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu,+Q), [Xiaoteng Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma,+X), [Baian Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen,+B), [Soujanya Poria](https://arxiv.org/search/cs?searchtype=author&query=Poria,+S)

[View PDF](https://arxiv.org/pdf/2605.12357)

> Abstract:Large language models increasingly need to accumulate and reuse historical information in long-term assistants and agent systems. Simply expanding the context window is costly and often fails to ensure effective context utilization. We propose $\delta$ -mem, a lightweight memory mechanism that augments a frozen full-attention backbone with a compact online state of associative memory. -mem compresses past information into a fixed-size state matrix updated by delta-rule learning, and uses its readout to generate low-rank corrections to the backbone's attention computation during generation. With only an online memory state, -mem improves the average score to that of the frozen backbone and that of the strongest non--mem memory baseline. It achieves larger gains on memory-heavy benchmarks, reaching on MemoryAgentBench and on LoCoMo, while largely preserving general capabilities. These results show that effective memory can be realized through a compact online state directly coupled with attention computation, without full fine-tuning, backbone replacement, or explicit context extension.

| Subjects: | Artificial Intelligence (cs.AI) |
| --- | --- |
| Cite as: | [arXiv:2605.12357](https://arxiv.org/abs/2605.12357) \[cs.AI\] |
|  | (or [arXiv:2605.12357v1](https://arxiv.org/abs/2605.12357v1) \[cs.AI\] for this version) |
|  | [https://doi.org/10.48550/arXiv.2605.12357](https://doi.org/10.48550/arXiv.2605.12357) |

## Submission history

From: Jingdi Lei \[[view email](https://arxiv.org/show-email/3b79a046/2605.12357)\]  
**\[v1\]** Tue, 12 May 2026 16:31:44 UTC (609 KB)

[Which authors of this paper are endorsers?](https://arxiv.org/auth/show-endorsers/2605.12357) | Disable MathJax ([What is MathJax?](https://info.arxiv.org/help/mathjax.html))