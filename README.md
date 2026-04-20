# Polynomial Frame Interpolation with Gradient Descent Refinement

## Overview

This project implements a simple frame interpolation pipeline using polynomial interpolation per pixel, followed by gradient descent optimization to refine the result.

The goal is to reconstruct an intermediate frame between existing frames and evaluate how optimization improves accuracy.

## Key Idea

Each pixel is treated independently as a time series:

* Given 3 frames → fit a quadratic polynomial per pixel
* Evaluate that polynomial at a new time step
* Refine coefficients using gradient descent against a target frame

## Pipeline

1. **Input Frames**

   * Three grayscale frames with a moving object (synthetic square)

2. **Polynomial Interpolation**

   * Construct a Vandermonde matrix
   * Solve for coefficients using Gaussian elimination
   * Evaluate polynomial at `t = 1.5`

3. **Simulated Ground Truth**

   * Approximation: midpoint of frame1 and frame2
   * Add small Gaussian noise

4. **Gradient Descent Optimization**

   * Loss: Mean Squared Error (per pixel)
   * Update polynomial coefficients iteratively

5. **Evaluation**

   * Compare:

     * Initial interpolation
     * Refined interpolation
   * Compute MSE
   * Visualize differences

## Core Components

### Gaussian Elimination

Custom implementation to solve linear systems:

```
Ax = b
```

### Polynomial Model

For each pixel:

```
f(t) = a t^2 + b t + c
```

### Optimization Step

Gradient descent update:

```
coeffs = coeffs - lr * gradient
```

### Loss Function

```
MSE = mean((prediction - ground_truth)^2)
```

## Output

The script generates:

* Original frames
* Simulated ground truth
* Initial interpolated frame
* Refined interpolated frame
* Error heatmaps
* Printed MSE values

## Example Results

* Initial interpolation: captures general motion but introduces artifacts
* Refined interpolation: reduces error and aligns better with target frame

## Dependencies

* numpy
* matplotlib

Install with:

```
pip install numpy matplotlib
```

## How to Run

```
python main.py
```

## Notes / Limitations

* Treats pixels independently (no spatial awareness)
* Uses synthetic ground truth (not real data)
* Polynomial interpolation struggles with sharp motion (edges)

## Possible Improvements

* Add spatial smoothing (neighbor awareness)
* Replace polynomial model with spline interpolation
* Use optical flow or neural approaches
* Vectorize implementation (current version is slow due to nested loops)

## Purpose

This project was built to explore:

* Polynomial interpolation
* Numerical linear algebra (Gaussian elimination)
* Optimization via gradient descent
* Error analysis (MSE, visualization)

Made for my Mathematical Foundations of Computing class.
