# Forward Robot Kinematics Prediction with XGBoost


<img width="632" height="316" alt="image" src="https://github.com/user-attachments/assets/691f80fa-d6d4-4051-908a-a1afbc68e97f" />


Predict the 3D end-effector position `(x, y, z)` of a **KUKA LBR iiwa** from its joint angles using a pure data-driven approach.

## Project Overview

Classical forward kinematics relies on analytical equations.  
In this project, I treat FK as a **supervised regression problem**:

- **Input**: 7 joint angles of the KUKA LBR iiwa  
- **Output**: Cartesian position of the end-effector `(x, y, z)`

I generated a high-quality synthetic dataset with **PyBullet**, visualized the robot, and trained an **XGBoost** multi-output regressor to learn the kinematic mapping.

---

### Tech Stack

- **Simulation & Dataset**: PyBullet + KUKA LBR iiwa URDF  
- **Machine Learning**: XGBoost (multi-output regression)  
- **Language**: Python

---

### Pipeline

1. Load KUKA LBR iiwa in PyBullet  
2. Sample random joint configurations within joint limits  
3. Record corresponding end-effector positions  
4. Train XGBoost to map joint angles → `(x, y, z)`  
5. Evaluate prediction accuracy  

---

### Why XGBoost?

XGBoost is particularly well-suited for this task:

- Excellent at capturing the highly non-linear relationship between joint angles and Cartesian position  
- Extremely fast training and inference  
- Strong performance on tabular kinematic data  
- Requires minimal hyperparameter tuning compared to neural networks  

It serves as a powerful and practical surrogate model when an analytical FK solution is unavailable or when a fast approximate model is needed.


### Result 

The model achieves a relatively high R² score (~0.92), showing that it successfully captures the overall nonlinear relationship between joint angles and end-effector position. However, the absolute position error remains in the range of approximately 25 cm on average.
This discrepancy is expected: while the model explains most of the variance in the data, the residual error is still large relative to the precision typically required in robotic applications. The success rates further confirm that only a small fraction of predictions fall within tight error thresholds (under 10 cm), while a majority of predictions are within 30 cm of the true position.
These results serve as a solid baseline and highlight that task-space error (Mean Position Error) is a more informative metric than R² alone for evaluating kinematics models.
