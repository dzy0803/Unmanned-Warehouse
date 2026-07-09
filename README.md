# Unmanned Warehouse Multi-Agent TurtleBot System

## Overview

This repository presents an unmanned warehouse management prototype built around two TurtleBot3 Burger robots, with simulation support for larger multi-agent scenes. The system models a fetch-and-deliver workflow: both robots start from assigned starting positions, navigate through an obstacle-filled warehouse arena, visit their respective Fetch Points identified by ArUco markers, and continue toward goal positions while avoiding robot-robot and robot-obstacle collisions.

The project combines real TurtleBot experiments with PyBullet simulation. It explores single-agent and multi-agent navigation, A* path planning, Conflict-Based Search (CBS), proportional/PID-style motion control, camera calibration, ArUco marker pose estimation, and the transfer of planned simulation waypoints into a physical arena.

<p align="center">
  <img src="assests/imgs/examplemap.jpeg" alt="Warehouse map and path planning example" width="850">
</p>

<p align="center">
  <img src="assests/videos/examplesolution.gif" alt="Simulation solution animation" width="850">
</p>

## What This Project Does

- Coordinates TurtleBot agents in a warehouse-style environment with shelves, narrow aisles, Fetch Points, and final destinations.
- Uses A* as the low-level planner for shortest-path search on a grid map.
- Uses CBS as the high-level multi-agent planner to resolve vertex and edge conflicts between robots.
- Converts planned grid schedules into TurtleBot wheel commands for simulation execution.
- Uses ArUco markers for real-world pose estimation, including robot and destination marker tracking.
- Includes ROS launch/configuration files for TurtleBot navigation, obstacle avoidance, ArUco detection, and move_base/DWA navigation.
- Provides PyBullet scenes for testing single-agent, two-agent, and larger multi-agent scenarios.

## System Pipeline

1. Define the warehouse map, obstacles, starts, Fetch Points, and goals in YAML scene files.
2. Run the path planner:
   - A* finds each robot's shortest local route.
   - CBS detects collisions and adds constraints until all robot paths are conflict-free.
3. Export the resulting schedule to `output.yaml`.
4. Execute the planned route in PyBullet using TurtleBot URDF models and warehouse obstacle assets.
5. Transfer the navigation logic into a real arena using camera calibration and ArUco marker pose topics.
6. Validate single-agent and multi-agent fetch-and-deliver runs in both simulation and physical tests.

## Key Components

### Path Planning

The planner is implemented in `turtlebot_simulation_pybullet/cbs/`.

- `a_star.py` implements low-level A* search using Manhattan-distance heuristics.
- `cbs.py` implements Conflict-Based Search with vertex constraints and edge constraints, allowing multiple robots to share a map without occupying the same cell or swapping positions at the same time step.

### PyBullet Simulation

The PyBullet simulation in `turtlebot_simulation_pybullet/` builds warehouse scenes from URDF obstacle assets, loads TurtleBot models, runs CBS planning, and drives robots through their schedules with wheel velocity control.

Useful entry points include:

- `multi_robot_navigation.py`
- `multi_robot_navigation_2.py`
- `multi_robot_navigation_3.py`
- `multi_robot_navigation_deliver.py`
- `final_challenge.py`

### ROS TurtleBot Navigation

The ROS workspace in `turtlebot3_burger_auto_navigation/` contains launch files and parameters for:

- ArUco marker detection with marker IDs such as `100` and `101`.
- Autonomous navigation from robot marker pose to target marker pose.
- TurtleBot3 Burger move_base navigation using DWA local planning.
- Naive and multi-robot obstacle avoidance experiments.

## Results Showcase

### Path Search

<p align="center">
  <img src="assests/videos/path_search_planning.png" alt="Path search planning result" width="850">
</p>

### Simulation

<p align="center">
  <img src="assests/videos/simulation%20results.png" alt="PyBullet simulation result" width="850">
</p>

### Physical Arena

<p align="center">
  <video src="assests/videos/physical_arena_video.mp4" controls width="850"></video>
</p>

## Installation

For the PyBullet simulation:

```bash
cd turtlebot_simulation_pybullet
python3 -m pip install -r requirement.txt
```

The main Python dependencies are:

- `pybullet`
- `pyyaml`
- `numpy`
- `matplotlib`
- `scipy`
- `ffmpeg-python`

For the ROS/TurtleBot side, use an Ubuntu/ROS environment compatible with TurtleBot3 Burger, `move_base`, `dwa_local_planner`, `usb_cam`, and `aruco_ros`.

## Running the Simulation

From the simulation directory:

```bash
cd turtlebot_simulation_pybullet
python3 multi_robot_navigation_deliver.py
```

To run CBS directly on an input YAML file:

```bash
cd turtlebot_simulation_pybullet
python3 -m cbs.cbs input.yaml output.yaml
```

Scene files are stored under `turtlebot_simulation_pybullet/scene/`, including warehouse layouts for one robot, two-stage tasks, four robots, and final challenge experiments.

## Report

The full technical report is available here:

[Download the PDF report](assests/Multi_agent_System%20Report-1.pdf)

The report documents the methodology and results in detail, including navigation strategies, camera calibration, TurtleBot3 setup, simulation setup, Fetch Point design, real arena integration, PID control ideas, SLAM/map experiments, and final results analysis.

## Report Preview

<p align="center">
  <img src="report_screenshots/Multi_agent_System Report-1_01.png" alt="Report page 01" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_02.png" alt="Report page 02" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_03.png" alt="Report page 03" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_04.png" alt="Report page 04" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_05.png" alt="Report page 05" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_06.png" alt="Report page 06" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_07.png" alt="Report page 07" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_08.png" alt="Report page 08" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_09.png" alt="Report page 09" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_10.png" alt="Report page 10" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_11.png" alt="Report page 11" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_12.png" alt="Report page 12" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_13.png" alt="Report page 13" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_14.png" alt="Report page 14" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_15.png" alt="Report page 15" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_16.png" alt="Report page 16" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_17.png" alt="Report page 17" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_18.png" alt="Report page 18" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_19.png" alt="Report page 19" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_20.png" alt="Report page 20" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_21.png" alt="Report page 21" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_22.png" alt="Report page 22" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_23.png" alt="Report page 23" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_24.png" alt="Report page 24" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_25.png" alt="Report page 25" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_26.png" alt="Report page 26" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_27.png" alt="Report page 27" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_28.png" alt="Report page 28" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_29.png" alt="Report page 29" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_30.png" alt="Report page 30" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_31.png" alt="Report page 31" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_32.png" alt="Report page 32" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_33.png" alt="Report page 33" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_34.png" alt="Report page 34" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_35.png" alt="Report page 35" width="850">
  <img src="report_screenshots/Multi_agent_System Report-1_36.png" alt="Report page 36" width="850">
</p>
