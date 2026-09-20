# UR Robot Tic-Tac-Toe (ROS 2 + ArUco)

This is an adaptation of a project I originally built in 2023 (ROS 1 → ROS 2).

The goal of the project is to let two users play tic-tac-toe against each other using a UR robot: a player calls out a cell (1–9), and the robot places a cube on the corresponding spot on a 3x3 board. The robot locates the board and its cells using ArUco marker detection.

This project was built on Ubuntu Noble Numbat (24.04) with ROS 2 Jazzy and Gazebo Sim (Harmonic).

## Status

Work in progress. Currently implemented:
- ArUco marker generation and detection scripts (OpenCV)
- A UR5e + Robotiq 2F-85 simulation in Gazebo Sim, adapted from [cambel/ur3](https://github.com/cambel/ur3)
- A wrist-mounted camera (eye-in-hand), added to the robot's URDF, with a working Gazebo camera sensor bridged to ROS 2
- A custom simulation world
- An ArUco grid board model for the simulated environment
- MoveIt integration for motion planning

Not yet implemented:
- ROS 2 service/client for requesting marker detections
- Game logic (board state tracking, turn handling)
- Full pick-and-place sequence tying detection to robot motion

## Dependencies

This repo replaces the `ur_gripper_gz` package inside a clone of:
- [cambel/ur3](https://github.com/cambel/ur3)

Which itself pulls in (via that repo's own setup):
- `ur_description`
- `robotiq_description`
- `cartesian_controllers`

## Setup

1. Clone the base repo into your workspace:
```bash
   cd ~/ros2_ws/src
   git clone https://github.com/cambel/ur3
```
2. Follow `cambel/ur3`'s own setup instructions to install its dependencies.
3. Remove the original `ur_gripper_gz` package from that clone and replace it with this repo instead (clone it into the same location, and rename the folder to `ur_gripper_gz` so the package name matches what other packages expect).
4. Build the workspace:
```bash
   cd ~/ros2_ws
   colcon build --symlink-install
   source install/setup.bash
```

## Running

Launch the simulation:
```bash
ros2 launch ur_gripper_gz ur_2f85_gz_control.launch.py gui:=false
```

Launch MoveIt (in a separate terminal):
```bash
ros2 launch ur_gripper_gz_moveit_config ur_moveit.launch.py ur_type:=ur5e gripper:=robotiq_2f85
```

## Acknowledgments

Based on the UR + Gazebo simulation setup from [cambel/ur3](https://github.com/cambel/ur3). This project adapts and extends it with a camera sensor, custom world, and ArUco-based marker detection for the tic-tac-toe game logic.
