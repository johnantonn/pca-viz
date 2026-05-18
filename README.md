# pca-viz

Educational **Principal Component Analysis** in **ℝ²** or **ℝ³**: sample covariance **S**, spectrum, eigenvectors **V**, and rank-*k* reconstruction — from scratch in NumPy, stepped through in **Streamlit** + **Plotly**.

## What you’ll see

1. **Raw data** and the sample mean **μ**
2. **Centred** cloud **X̃** = **X** − **1μ**ᵀ
3. **Mahalanobis geometry** — 95% ellipse (2-D) or ellipsoid surface (3-D), same **V** as PCA
4. **Principal axes** — eigenvectors scaled by 2σ (√λ) in centred space
5. **Reconstructions** — *k* = 0 … *d* with residuals and **MSE** (mean squared entry)

**3-D mode** adds a **marginal (x₁, x₂)** plot: the same story looking down the *x₃* axis (with a statistically correct **2×2 marginal** covariance ellipse where relevant).

**Algebra panel** (left column): **S**, eigenvalues / variance shares, **V**, per-frame MSE and captured variance, plus the reconstruction formula — all updated as you scrub frames.

Point colours follow synthetic labels so you can see that **PCA never uses labels**.

## Quick start

```bash
cd pca-viz
uv sync
uv run streamlit run app.py
```

## Project layout

```
pca-viz/
├── pyproject.toml
├── app.py              # Streamlit UI + algebra tables + playback
├── pca/
│   ├── __init__.py
│   ├── data.py         # 2-D / 3-D shapes
│   ├── algorithm.py    # PCA + Snapshot sequence
│   └── visualize.py    # 2-D, 3-D, marginal figures
└── README.md
```

## Related visualisers (dimensionality reduction)

- **[umap-viz](https://github.com/johnantonn/umap-viz)** — **Nonlinear** 2-D embedding from a **high-D kNN fuzzy graph** and layout SGD. No single linear subspace captures the final map.
- **[tsne-viz](https://github.com/johnantonn/tsne-viz)** — **Nonlinear** 2-D embedding via **symmetric affinities** and **KL(P‖Q)** with a Student‑t kernel; focuses on **local** structure (perplexity controls effective neighbourhood size).

**Connection:** PCA here is the **linear** tool: spectrum, axes, and **reconstruction error** are explicit. **UMAP** and **t-SNE** produce **nonlinear** pictures for exploration; they can separate clusters that overlap in every linear projection. Large-*D* workflows often use **PCA as a first stage** before nonlinear embedders for speed or denoising.

## Math

For **X** ∈ ℝⁿˣᵈ (*d* = 2 or 3):

- **S** = **X̃**ᵀ**X̃** / (n−1), **S** = **VΛV**ᵀ  
- **X̂** = **X̃ V**₁:ₖ**V**₁:ₖᵀ + **1μ**ᵀ  
- Reported **MSE** = ‖**X** − **X̂**‖_F² / (n·d); **Var. captured** = (∑ⱼ≤ₖ λⱼ) / tr(**S**)

Playback uses Streamlit **▶ / ⏸** controls (no overlapping Plotly animation buttons).
