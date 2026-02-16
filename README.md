# Learning Local Robot Control from Simulator Data (Reacher3 → Reacher4 → Reacher6)

This project learns a **data-driven local inverse model** to control simulated robotic arms with different degrees of freedom (DoF): **Reacher3**, **Reacher4**, and **Reacher6**.

## Project Goal

Train a supervised regression model that approximates **local inverse kinematics**.  
Given the robot’s current state and a small desired displacement of the end-effector, the model predicts the corresponding small joint displacement.

### Data Format

For a robot with joint vector \( q = (q_1, \dots, q_n) \) and end-effector position \( x \in \mathbb{R}^m \), the dataset contains samples:

- Current state: \( (x, q) \)
- Desired end-effector displacement: \( dx \)
- Resulting joint displacement: \( dq \)

Learning objective:

\[
f(x, q, dx) \approx dq
\]

## Why This Works

The learned inverse model enables **local model-free control**: by repeatedly applying small joint increments predicted by the model, the robot can move toward a target **without** using:

- forward kinematics
- inverse kinematics
- Jacobians
- analytic robot models

## Robots Covered

The same learning setup is applied across robots of increasing complexity:

### Reacher3
- End-effector: \( x \in \mathbb{R}^2 \)
- Joints: \( q \in \mathbb{R}^3 \)
- Input dimension: **7**
- Output dimension: **3**

Used first because it is low-dimensional and ideal for exploring models and tuning.

### Reacher4
- \( x \in \mathbb{R}^3 \), \( q \in \mathbb{R}^4 \)
- Input: **10**, Output: **4**

### Reacher6
- \( x \in \mathbb{R}^3 \), \( q \in \mathbb{R}^6 \)
- Input: **12**, Output: **6**

These higher-dimensional settings help analyze how performance scales with DoF and model capacity.

## Workflow Overview

1. **Reacher3**
   - Exploratory analysis
   - Train classical regressors (e.g., linear models, tree-based methods) and neural networks (MLPs)
   - Compare performance using metrics like MSE, MAE, and R²
   - Select the best-performing approach

2. **Reacher4 & Reacher6**
   - Re-train the selected approach on the new datasets
   - Evaluate how accuracy and generalization change as DoF increases
   - Study scaling behavior and model capacity requirements