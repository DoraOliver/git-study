# 6-Axis-Robot Task by Sereact
Author: Wei Cao

## Contents

- [Development Framwork](#1)
- [Object Descripition](#2)
- [Simple User Guide](#3)

## Dependencies

This Task is based on [Robotics Toolbox for Python](https://github.com/petercorke/spatialmath-python).
This Toolbox comes from the Matlab version [Robotics Toolbox for Matlab](https://github.com/petercorke/spatialmath-python)
I used for my master's thesis by Matlab at that time because of it's stable inverse kinematics solver. 

The toolbox needs Python >= 3.6

### Using pip

Install a snapshot from PyPI

```shell script
pip3 install roboticstoolbox-python
```

### From GitHub

To install the bleeding-edge version from GitHub

```shell script
git clone https://github.com/petercorke/robotics-toolbox-python.git
cd robotics-toolbox-python
pip3 install -e .
```

## Object Description

### Object structure

├── Readme.md                   // Task Description                  
├── sixarobot                   // Application
│   ├── tests
│   │   └── test_robot.py       // Test Units
│   ├── MyRobot.py              // Robot configuration
│   └── test.py                 // test

In the task Folder:
```python
MyRobot.py
```
This is the configuration Python script for the robot configuration. 


```python
tests/test_robot.py
```
This is the test units with Python `unittest` Framework.

### Details Description

- Library Select
 Considering the objectives and requirements of the task, this project uses the **Robotics Toolbox 
 Python** library to complete the project. This library is developed by the same group that created 
 the toolbox I used in my graduation project, so there are many similar usage processes. I am more 
 familiar with C and C++, and in the future, if possible, I hope to use C++ to build low-level 
 code in order to achieve higher computational efficiency and real-time performance.

- Robot configuration method
 Robot configuration is based on DH-Parameters method. If the task needed, I can also utilize serial ETH 
 method or utilize the general URDF datei to construct this robot. Considering the task goal that not 
 including the dynamics so I chose DH-Parameters method to construct the 6-axis robot.

- Task completion
 Task 1-4 are accomplished, Taks 5 is not accomplished.

 Task 4: 
  Considering the time constraints, I did not refine the boundary condition issues, and this is 
  just a simple example. The Robotics Toolbox Python provides a method for trajectory generation, which 
  uses a polynomial approach to construct a trajectory interpolation for smooth motion. This method is 
  well-established and stable, so in the final motion generation, I prioritized using the trajectory 
  generation method provided by the toolbox. 

 Task 5:
  Firstly, due to time constraints, and secondly, because there is some ambiguity in the task description. 
  The task description mentions avoiding collisions in the robot's workspace, but considering the potential 
  for collisions within the robotic arm itself, I have not yet completed Task 5. Here, I will only provide 
  a rough idea:
  - "Work-Point collision": Considering that the task is only about avoiding collisions at the end-effector, 
  a distance radius check will be added during simulation, with the end-effector calculated using forward kinematics. 
  In the detection area, slight 'perturbations' will be added to the joints to check for potential collisions. 
  Alternatively, a **task trajectory planning** method can be used to directly avoid collision points. 
  - "Collistion of the robot arm":  Model files need to be included to detect collisions. Similarly, within the 
  collision area, algorithms such as RTT (which is currently theoretical knowledge for me) can be used to avoid 
  collisions. In a real environment, sensors and other devices would be considered to assist in collision avoidance.

## Simple User Guide
A simply example was wrote in `test.py`.

### Import Robot Model

If you want to set the custormized robot model in Robotics Toolbox Python:

 1. find the path of the package you installed, copy `Myrobot.py` datei and put in `roboticstoolbox\models\DH` folder.
 2.  In DH folder ipen `__init__.py` and add
    ```python
    from roboticstoolbox.models.DH.MyRobot import MyRobot
    __all__ = ['Myrobot']
    ```
 3. Then you can use the method shown below to create the robot.
    ```python
    import roboticstoolbox as rtb
    robot = rtb.models.DH.Myrobot()
    ```
 4. The normal import method also can create the robot.
    ```python
    from Myrobot import MyRobot
    robot = Myrobot()
    ```

### Set User Definied Input
The robot class was integrated the task goal functions.
`set_joint_angle`: Set the user input pose of each joint of the robot
```python
robot.set_joint_angle([q1, q2, q3, q4, q5, q6])
```

### Show Simulation Result
```python
robot.show_move()
```
At the same time will generate a gif to reappear the simulation result.

### Get the Smooth Movement Trajectory
```python
[q, qd] = robot.trapvel_Jtraj(qs, qe, steps, endtime, v_max, a_max)
```
q:  joint space trajectories (List[])
qd: joint velocity (List[])
qs: start pose (List[])
qe: target pose (List[])
steps: interpolation number 
endtime: the movement time 
v_max: maximal velocity of each joint (List[])
a_max: maximal accelaration of each joint (List[])
