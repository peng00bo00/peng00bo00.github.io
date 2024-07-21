---
layout: page
title:  Shape Analysis Homeworks
description: "Homeworks in MIT 6.838: Shape Analysis."
img: https://i.imgur.com/KrAIIuX.gif
category: Geometry Processing
toc:
  sidebar: left
---


## Homework 1: Discrete and Smooth Curves

### Discrete Curvature of a Plane Curve

#### Gradient $$\nabla_{\mathbf{x}_i} s$$

$$
\nabla_{\mathbf{x}_i} s =\frac{\mathbf{x}_i - \mathbf{x}_{i-1}}{\Vert \mathbf{x}_i - \mathbf{x}_{i-1} \Vert_2} + \frac{\mathbf{x}_i - \mathbf{x}_{i+1}}{\Vert \mathbf{x}_i - \mathbf{x}_{i+1} \Vert_2}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HEp18iQ.png" width="50%">
</div>

#### Discrete Curvature

$$
\kappa = 2 \sin \frac{\theta}{2}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YMaEmwy.png" width="51%">
</div>

#### Curve Shortening Flow

$$
\mathbf{x}_i' = \mathbf{x}_i - (\nabla_{\mathbf{x}_i} s) h
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/J1jChfg.gif" width="50%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lE8Rv1s.gif" width="50%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KE61wET.gif" width="50%">
</div>

### Discrete Elastic Rods

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cHm2IwG.gif" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/VvSJcuN.gif" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HLOl1tB.gif" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KrAIIuX.gif" width="45%">
</div>

## Homework 2: Surfaces and Curvature

### Mean Curvature Flow with Explicit Integrator

$$
\frac{\mathbf{p} (t+\tau) - \mathbf{p} (t)}{\tau} = - \mathbf{M}^{-1} (\mathbf{p} (t)) \cdot \mathbf{L} (\mathbf{p} (t)) \cdot \mathbf{p} (t)
$$

$$
\mathbf{p} (t+\tau) = \mathbf{p} (t) - \tau \mathbf{M}^{-1} (\mathbf{p} (t)) \cdot \mathbf{L} (\mathbf{p} (t)) \cdot \mathbf{p} (t)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/14jPnsN.gif" width="50%">
</div>

### Mean Curvature Flow with (Semi-)Implicit Integrator

$$
\frac{\mathbf{p} (t+\tau) - \mathbf{p} (t)}{\tau} = - \mathbf{M}^{-1} (\mathbf{p} (t)) \cdot \mathbf{L} (\mathbf{p} (t)) \cdot \mathbf{p} (t+\tau)
$$

$$
\mathbf{p} (t+\tau) =\bigg(\mathbf{I} + \tau \mathbf{M}^{-1} (\mathbf{p} (t)) \cdot \mathbf{L} (\mathbf{p} (t)) \bigg)^{-1} \cdot \mathbf{p} (t) 
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Bl0zqwZ.gif" width="50%">
</div>

### Non-Singular Mean Curvature Flow

$$
\mathbf{p} (t+\tau) = \bigg(\mathbf{I} + \tau \mathbf{M}^{-1} (\mathbf{p} (t)) \cdot \mathbf{L} (\mathbf{p} (0)) \bigg)^{-1} \cdot \mathbf{p} (t) 
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RDqlDh7.gif" width="50%">
</div>

## Homework 3: Geodesics, Distance, and Metric Embedding

### Swiss-Roll DataSet

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KiBi29n.png" width="45%">
</div>

### Maximum Variance Unfolding

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s4WDk8X.png" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eyU9fFs.png" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EwffIl4.png" width="45%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1FDabII.png" width="45%">
</div>

## Homework 4: Laplacian and Vector Fields

### Helmholtz Decomposition

1. $$\nabla \cdot V = \nabla \cdot \nabla \zeta = \Delta \zeta$$
2. $$\nabla \times W = V - \nabla \zeta$$

### Geodesic Distance from the Laplacian

#### Heat Kernel & Normalized Gradient

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/92t2TcB.png" width="50%">
</div>

#### Divergences

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/U3vhQx3.png" width="50%">
</div>

#### Geodesic Distances

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JhFYXjq.png" width="50%">
</div>

### Parallel Transport from the Connection Laplacian

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GQYwCBz.png" width="50%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/k5hiqoa.png" width="50%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DCl9ppI.png" width="50%">
</div>

### Operator Approach to Tangent Vector Fields

#### Scalar Field Advection

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/obuaX0R.gif" width="50%">
</div>

## Manifold Optimization and Optimal Transport

### Optimal Transport

(a) $$W(\mathbf{p}, \mathbf{q})$$ measures the minimum cost of transporting $$\mathbf{p}$$ to $$\mathbf{q}$$.

(b) Let $$K_\alpha = e^{-\frac{C}{\alpha}}$$, then

$$
\begin{aligned}
\alpha \cdot \text{KL} (T \Vert K_\alpha) &= \alpha \cdot \sum_{ij} T_{ij} \bigg( \ln{\frac{T_{ij}}{(K_\alpha)_{ij}}} -1 \bigg) = \alpha \cdot \bigg( \sum_{ij} T_{ij} \big( \ln{T_{ij}}-1 \big) - \sum_{ij} T_{ij} \ln{(K_\alpha)_{ij}} \bigg) \\
&= \alpha \cdot \bigg( \sum_{ij} T_{ij} \ln{T_{ij}} - 1 \bigg) + \alpha \cdot \sum_{ij} T_{ij} \cdot \bigg( \frac{C_{ij}}{\alpha} \bigg) \\
&= \sum_{ij} T_{ij} \cdot C_{ij} + \alpha \cdot \bigg( \sum_{ij} T_{ij} \ln{T_{ij}} - 1 \bigg)
\end{aligned}
$$

(c) The Lagrange could be expressed as:

$$
\mathcal{L} (T; \lambda, \mu) = \sum_{ij} T_{ij} \cdot C_{ij} + \alpha \cdot \bigg( \sum_{ij} T_{ij} \ln{T_{ij}} - 1 \bigg) + \sum_i \lambda_i \cdot \bigg( \sum_j T_{ij} - p_i \bigg) + \sum_j \mu_j \cdot \bigg( \sum_i T_{ij} - q_j \bigg)
$$

where $$\lambda_i$$ and $$\mu_j$$ are Lagrange multipliers.

Taking derivative of $$T_{ij}$$, we have:

$$
\frac{\partial \mathcal{L}}{\partial T_{ij}} = C_{ij} + \alpha ( \ln{T_{ij}} + 1 ) + \lambda_i + \mu_j = 0
$$

which implies:

$$
T_{ij} = \exp \bigg\{ - \frac{C_{ij} + \lambda_i + \mu_j}{\alpha} - 1 \bigg\} 
= e^{-\frac{\lambda_i}{\alpha}-\frac{1}{2}} \cdot e^{-\frac{C_{ij}}{\alpha}} \cdot e^{-\frac{\mu_j}{\alpha}-\frac{1}{2}}
$$

Thus, 

$$
T = \text{diag}(\mathbf{v}) \cdot K_\alpha \cdot \text{diag}(\mathbf{w}) 
$$

where

$$
\mathbf{v}_i = e^{-\frac{\lambda_i}{\alpha}-\frac{1}{2}}
$$

$$
\mathbf{w}_j = e^{-\frac{\mu_j}{\alpha}-\frac{1}{2}}
$$

(d) When $$\alpha$$ is small enough, we have

$$
C_{ij} = d^2(x_i, x_j) \approx -2 \times \frac{\alpha}{2} \ln \mathcal{H}_{\frac{\alpha}{2}} (x_i, x_j)
$$

Thus,

$$
\mathcal{H}_{\frac{\alpha}{2}} (x_i, x_j) \approx e^{-\frac{C_{ij}}{\alpha}} = (K_\alpha)_{ij}
$$

(e) If $$k$$ is odd, the Lagrange could be expressed as

$$
\begin{aligned}
\mathcal{L} (T; \lambda) 
&= \text{KL} (T \Vert T^{(k-1)}) + \sum_i \lambda_i \cdot \bigg( \sum_j T_{ij} - p_i \bigg) \\
&= \sum_{ij} T_{ij} \bigg( \ln{T_{ij}} - \ln{T^{(k-1)}_{ij}} -1 \bigg) + \sum_i \lambda_i \cdot \bigg( \sum_j T_{ij} - p_i \bigg) \\
&= \sum_{ij} T_{ij} \bigg( \ln{T_{ij}} - \ln{T^{(k-1)}_{ij}} \bigg) + \sum_i \lambda_i \cdot \bigg( \sum_j T_{ij} - p_i \bigg) - 1
\end{aligned}
$$

Taking derivative of $$T_{ij}$$, we have

$$
\frac{\partial \mathcal{L}}{\partial T_{ij}} = \bigg( \ln{T_{ij}} - \ln{T^{(k-1)}_{ij}} \bigg) + 1 + \lambda_i = 0
$$

Thus,

$$
T_{ij} = \frac{T^{(k-1)}_{ij}}{e^{1+\lambda_i}}
$$

This implies that

$$
T^{(k)} = \text{diag}(\mathbf{\tilde{v}}^{(k)}) \cdot T^{(k-1)}
$$

where

$$
\mathbf{\tilde{v}}^{(k)}_i = \frac{1}{e^{1+\lambda_i}}
$$

Similarly, when $$k$$ is even, we have

$$
T_{ij} = \frac{T^{(k-1)}_{ij}}{e^{1+\mu_j}}
$$

where $$\mu_j$$ is the Lagrange multiplier. Then,

$$
T^{(k)} = T^{(k-1)} \cdot \text{diag}(\mathbf{\tilde{w}}^{(k)}) 
$$

$$
\mathbf{\tilde{w}}^{(k)}_j = \frac{1}{e^{1+\mu_j}}
$$

Putting these together, we derive the iteration

$$
\begin{aligned}
T^{(k)} &= \text{diag}(\mathbf{v}^{(k)}) \cdot T^{(0)} \cdot \text{diag}(\mathbf{w}^{(k)}) \\
&= \text{diag}(\mathbf{v}^{(k)}) \cdot \mathcal{H}_{\frac{\alpha}{2}} \cdot \text{diag}(\mathbf{w}^{(k)})
\end{aligned}
$$

Now we only need to determine the vectors $$\mathbf{v}^{(k)}$$ and $$\mathbf{w}^{(k)}$$. Suppose $$k$$ is odd, the constraints of $$p_i$$ gives

$$
p_i = \sum_j T^{(k)}_{ij} = \mathbf{\tilde{v}}^{(k)}_i \cdot \sum_j T^{(k-1)}_{ij} 
$$

Thus,

$$
\mathbf{\tilde{v}}^{(k)}_i = \frac{p_i}{\sum_j T^{(k-1)}_{ij}} = \frac{p_i}{\sum_j T^{(k-2)}_{ij} \tilde{\mathbf{w}}^{(k-1)}_j}
$$

Similarly, when $$k$$ is even we have

$$
q_j = \sum_i T^{(k)}_{ij} = \mathbf{\tilde{w}}^{(k)}_j \cdot \sum_i T^{(k-1)}_{ij} 
$$

$$
\mathbf{\tilde{w}}^{(k)}_j = \frac{q_j}{\sum_i T^{(k-1)}_{ij}} = \frac{q_j}{\sum_i T^{(k-2)}_{ij} \tilde{\mathbf{v}}^{(k-1)}_i}
$$

Now we derive the **Sinkhorn iteration** step:

$$
\mathbf{v} \leftarrow \mathbf{p} \oslash (T \cdot \mathbf{w})
$$

$$
\mathbf{w} \leftarrow \mathbf{q} \oslash (\mathbf{v} \cdot T)
$$

where $$\oslash$$ is the elementwise division operator.

(f)

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zCVLfjc.jpg" width="40%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YhZo8Tc.jpg" width="42%">
</div>