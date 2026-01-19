# Few-Shot Semi-Supervised Learning for Abnormal Stop Detection from Sparse GPS Trajectories

This repository provides the implementation of our **graph-based semi-supervised framework** for **abnormal stop detection (ASD)** in **long-distance coach transportation** under **low-frequency GPS sampling** and **extreme label scarcity**. 

---

## Overview

Abnormal stop detection aims to identify **unauthorized or atypical stops** (e.g., non-designated pick-up/drop-off points or unusual dwell behavior) that may introduce safety, operational, and regulatory risks.
In practice, ASD is challenging because:

- **Sparse GPS trajectories** can obscure short or irregular stop behaviors. 
- **Labeled abnormal stops are extremely scarce**, making fully supervised training unreliable. 

To address these issues, we propose a **four-stage pipeline** that jointly improves segmentation quality, feature reliability, and semi-supervised learning on a segment graph.
---

## Method Summary (Four-Stage Pipeline)

### Stage 1 — Sparsity-Aware Segmentation (SAS)
We introduce **Sparsity-Aware Segmentation (SAS)** to adaptively segment each trip into behaviorally consistent units using trajectory-specific spatio-temporal continuity statistics, improving stability under sparse sampling. 

### Stage 2 — Domain-Specific Indicators
For each SAS segment, we compute three interpretable indicators to characterize abnormal stop behavior:

- **TIS**: Temporal Influence Score  
- **MSD**: Maximum Speed Deviation  
- **TTA@k**: Top-k Aggregated Temporal Score 

### Stage 3 — LTIGA Noise-Robust Enhancement
To mitigate indicator noise caused by irregular sampling, we propose **Locally Temporal-Indicator Guided Adjustment (LTIGA)**, which smooths segment indicators using local similarity graphs before graph construction. 

### Stage 4 — Graph-Based Semi-Supervised Learning
We construct a **segment-level spatio-temporal graph** where each segment is a node with **LTIGA-refined features**, then:

1. Apply **label propagation** to expand weak supervision across the graph 
2. Train a **GCN** to learn relational patterns among trajectory segments for ASD prediction

Pseudo-labels are accepted only under strict confidence thresholds (e.g., abnormal probability ≥ 0.995 / ≤ 0.005) to avoid error amplification under extreme label scarcity. 

---

## Dataset

### Collection and Supervision
The dataset was collected via field operation on the **Liuliqiao–Zhangjiakou intercity route**. Abnormal-stop supervision is provided by **10 pre-identified abnormal-stop GPS locations** recorded during data collection. :contentReference[oaicite:11]{index=11}

### Key Statistics
- Route: Liuliqiao (Beijing) → Zhangjiakou (Hebei) 
- Approx. route length: ~180 km 
- GPS records: 2,410 
- Unique vehicles: 16 
- Sampling interval: 30–60 s 
- Abnormal-stop supervision: 10 abnormal-stop locations (GPS coordinates) 

For dataset format and column definitions, see `dataset.md`.

---

## Results (Label-Efficient ASD)

With **only 10 labeled instances**, the method achieves **AUC = 0.854** and **AP = 0.866** on real-world coach data.
Across multiple runs, the model consistently outperforms existing baselines and remains robust under sparse sampling and label scarcity. 

---

