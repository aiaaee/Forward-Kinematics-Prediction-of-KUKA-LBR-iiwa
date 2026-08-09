# Forward Robot Kinematics Prediction with XGBoost

Predict the 3D end-effector position `(x, y, z)` of a robotic arm from its joint angles using a machine learning approach.

This project generates a synthetic dataset of robot configurations with **PyBullet**, visualizes the robot’s joints and links, and trains an **XGBoost** multi-output regressor to learn the forward kinematics mapping.


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
