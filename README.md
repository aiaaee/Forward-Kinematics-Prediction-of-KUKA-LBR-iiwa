# Forward Robot Kinematics Prediction with CatBoost


<img width="632" height="316" alt="image" src="https://github.com/user-attachments/assets/691f80fa-d6d4-4051-908a-a1afbc68e97f" />


Predict the 3D end-effector position `(x, y, z)` of a **KUKA LBR iiwa** from its joint angles using a pure data-driven approach.

## Project Overview

Classical forward kinematics relies on analytical equations.  
In this project, I treat FK as a **supervised regression problem**:

- **Input**: 7 joint angles of the KUKA LBR iiwa  
- **Output**: Cartesian position of the end-effector `(x, y, z)`

I generated a high-quality synthetic dataset with **PyBullet**, visualized the robot, and trained an **CatBoost** multi-output regressor to learn the kinematic mapping.

---

### Tech Stack

- **Simulation & Dataset**: PyBullet + KUKA LBR iiwa URDF  
- **Machine Learning**: CatBoost (multi-output regression)  
- **Language**: Python

---

### Pipeline

1. Load KUKA LBR iiwa in PyBullet  
2. Sample random joint configurations within joint limits  
3. Record corresponding end-effector positions  
4. Train CatBoost to map joint angles → `(x, y, z)`  
5. Evaluate prediction accuracy  

---

### Why CatBoost?

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/f81cac3b-5033-45ae-877a-464e9262759e" />

CatBoost is particularly well-suited for this task:

- Excellent at capturing the highly non-linear relationship between joint angles and Cartesian position  
- Extremely fast training and inference  
- Strong performance on tabular kinematic data  
- Requires minimal hyperparameter tuning compared to neural networks  

It serves as a powerful and practical surrogate model when an analytical FK solution is unavailable or when a fast approximate model is needed.

---

### Result 

The Forward Kinematics model was evaluated on a held-out test set using both correlation-based and task-space metrics.

### Evaluation Results

| Metric                        | Value       |
|-------------------------------|-------------|
| **Mean Position Error**       | 252.68 mm   |
| **Median Position Error**     | 247.22 mm   |
| Standard Deviation            | 95.87 mm    |
| Maximum Position Error        | 628.23 mm   |
| Minimum Position Error        | 21.12 mm    |
| Success Rate (< 50 mm)        | 0.3%        |
| Success Rate (< 100 mm)       | 3.7%        |
| Success Rate (< 200 mm)       | 32.3%       |
| Success Rate (< 300 mm)       | 69.6%       |

The model achieves a relatively high R² score (~0.92), showing that it successfully captures the overall nonlinear relationship between joint angles and end-effector position. However, the absolute position error remains in the range of approximately 25 cm on average.

This discrepancy is expected: while the model explains most of the variance in the data, the residual error is still large relative to the precision typically required in robotic applications. The success rates further confirm that only a small fraction of predictions fall within tight error thresholds (under 10 cm), while a majority of predictions are within 30 cm of the true position.

These results serve as a solid baseline and highlight that task-space error (Mean Position Error) is a more informative metric than R² alone for evaluating kinematics models.
