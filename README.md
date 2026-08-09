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
