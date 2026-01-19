## Reproducibility and Ablation Controls

This repository supports the ablation studies reported in the paper via simple switches in `main.py`.

---

### A1. Sparsity-Aware Segmentation (SAS) vs. Fixed-Length Segmentation

In `main.py`:

- `use_sas = True`  → enable **SAS** (adaptive segmentation for sparse GPS)
- `use_sas = False` → use **fixed-length segmentation**

---

### A2. LTIGA Smoothing Impact

In `main.py`:

- `use_ltiga = True`  → apply **LTIGA** smoothing/refinement
- `use_ltiga = False` → skip LTIGA (use pre-LTIGA indicators directly)

---

### A3. Baseline: GCN with Fixed-Length Segmentation + No LTIGA

In `main.py`:

- `use_sas = False`  → disable SAS (use fixed-length segmentation)
- `use_ltiga = False` → disable LTIGA (no indicator refinement)

---

## Multiple Runs with Different Random Seeds

In the terminal, run multiple trials with different random seeds to report mean ± std performance:

- `python main.py --seed 42 --trial_id run1`  
- `python main.py --seed 43 --trial_id run2`  
- `python main.py --seed 44 --trial_id run3`  
- `python main.py --seed 45 --trial_id run4`  
- `python main.py --seed 46 --trial_id run5`  

---


## SAS Sensitivity: Tuning α and β

To evaluate the sensitivity of **Sparsity-Aware Segmentation (SAS)**, edit the parameters in `main.py` under:

> **Step 2: Segment the road**

Test different values of:

- α, β ∈ {0.5, 1.0 (default), 1.5}

---

## LTIGA Sensitivity: Tuning K (Number of Neighbors)

To evaluate the sensitivity of **LTIGA smoothing**, edit `main.py` under:

> **Step 3: Calculate indicators before LTIGA (no confidence weighting)**

Locate the line defining the neighborhood size (e.g., `k = 3`) and change it.

Suggested values:

- K ∈ {2, 3 (default), 5}
