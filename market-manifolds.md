## Market Manifolds: Geometry-Aware Latent Representations via β-VAE

**Learning and analyzing the intrinsic geometry of financial market manifolds using β-VAE with Riemannian metric computation and geodesic-aware clustering.**

[**GitHub Repository**](https://github.com/levbszabo/market-latent-geometry) | [**Research Paper (PDF)**](/pdf/MarketManifolds_VAE.pdf)

<img src="images/clustering1.jpg?raw=true"/>

**Overview:** This project implements a novel framework for discovering the intrinsic geometry of financial time series through β-variational autoencoders. By treating the VAE decoder as a parameterization of an embedded manifold, we compute Riemannian metric tensors and geodesic distances that respect the learned curvature of market states, enabling geometry-aware analysis and clustering.

### Key Innovations

**Manifold Geometry Discovery:**
- Computes local Riemannian metrics via decoder Jacobians
- Reveals intrinsic curvature in learned latent representations  
- Non-linear relationship between geodesic and Euclidean distances

**Advanced VAE Architecture:**
- β-Variational Autoencoder with specialized loss components
- Posterior collapse prevention through KL capacity scheduling
- Orthogonality regularization for disentangled latent factors
- Robust training pipeline optimized for financial data

### Technical Results

**Clustering Performance Improvements:**
- **Silhouette Score**: 0.07 → 0.50 (geodesic vs Euclidean)
- **Calinski-Harabasz**: 64 → 1,817 (better cluster separation)
- **Davies-Bouldin**: 2.57 → 0.60 (reduced cluster overlap)
- **Statistical Validation**: p < 0.001 via permutation tests

**Architecture Details:**
- **Data**: S&P 500 market data (503 stocks × 2 features)
- **Latent Space**: 12-dimensional manifold representation
- **Framework**: PyTorch implementation with geometric analysis pipeline
- **Validation**: Temporal coherence and chronological ordering

### Research Impact

This framework establishes a foundation for geometry-aware analysis of financial time series through learned manifold representations. The methodology demonstrates that financial markets exhibit intrinsic geometric structure that can be captured and leveraged for improved clustering and analysis.

**Future Applications:**
- Generative modeling of realistic market scenarios along geodesic paths
- Risk management using curvature as early warning signals
- Reinforcement learning agents trained directly on learned manifolds
- Extension to multi-asset classes (FX, commodities, crypto)

**Technical Stack:** Python, PyTorch, NumPy, Scikit-learn, SciPy, Matplotlib, Seaborn