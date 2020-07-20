
**本项目已不再维护，感兴趣的小伙伴请移步：https://github.com/amov-lab/Prometheus**

This project is no longer maintained. If you are interested, please move: https://github.com/amov-lab/Prometheus
px4_command
Introduction

px4_command function package is an open source project based on PX4 open source firmware and Mavros function package, which aims to provide a more concise and fast development experience for PX4 developers. At present, it has integrated UAV outer loop controller modification, target tracking and obstacle avoidance and other upper-level development codes. Subsequent releases will cover mission decision-making, path planning, filter navigation, single/multi-machine control and other UAV scientific research and development fields. Features.
installation

    Install Mavros feature pack via binary method

    Please refer to: https://github.com/mavlink/mavros

    If you have installed the Mavros feature pack using source code, please delete it first.

    Create a workspace named "px4_ws" in the home directory

    mkdir -p ~/px4_ws/src

    cd ~/px4_ws/src

    catkin_init_workspace

    Most of the time, you need to manually source and open a new terminal

    gedit .bashrc

    bashrc.txtAdd in open filesource /home/$(your computer name)/px4_ws/devel/setup.bash

    Download and compile px4_commandFeature Pack

    cd ~/px4_ws/src

    git clone https://github.com/potato77/px4_command

    cd ..

    catkin_make

Project overview
Unnamed form

    Read flight control state_from_mavros.h

    Send control commands to the flight controller command_to_mavros.h

    Position loop controller implementation (currently provides five kinds of outer loop controllers, namely cascade PID, PID, UDE, Passivity+UDE, NE+UDE) pos_controller_cascade_PID.h pos_controller_PID.h pos_controller_UDE.h pos_controller_Passivity.h pos_controller_NE.h Among them, The cascade PID is a position controller that imitates the PX4, Passivity+UDE is a position controller without speed feedback, and NE+UDE is due to other controllers when the speed measurement is noisy.

    External positioning implementation px4_pos_estimator.cpp

    Control logic main program px4_pos_controller.cpp

    Ground station (need to cooperate with ROS multi-machine) ground_station.cpp

    Autonomous landing autonomous_landing.cpp

    Simple obstacle avoidance collision_avoidance.cpp

    Binocular Simple Obstacle Avoidance collision_avoidance_streo.cpp

    Formation flight (currently only supports gazebo simulation) formation_control_sitl.cpp

    Load throwing payload_drop.cpp

    Waypoint tracking square.cpp

    Target tracking target_tracking.cpp

    Description: 1. The autonomous landing, target tracking, and simple binocular obstacle avoidance need to be used with the vision part of the code. 2. The function package also contains some simple filtering and testing small codes, which are not explained here and can be consulted by themselves.

Video presentation

Autonomous landing

Load throwing

Simple obstacle avoidance, target tracking

Outer loop controller modifies NE + UDE and outer loop controller modifies Passivity-based UDE

Modification of the inner loop controller (PX4 firmware)
Coordinate system description

All variables in this function package are ENU coordinate system (same as Mavros, different from PX4 firmware)

    MAVROS does translate Aerospace NED frames, used in FCUs to ROS ENU frames and vice-versa. For translate airframe related data we simply apply rotation 180° about ROLL (X) axis. For local we apply 180° about ROLL (X) and 90° about YAW (Z) axes

PX4 firmware and parameters

Please use the PX4 firmware provided in the provided code- firmware

The SYS_COMPANION parameter is set to Companion (921600).

If you need to use external positioning, the parameter EKF2_AID_MASK is set to 24 (default is 1), EKF2_HGT_MODE is set to VISION (default is a barometer).
Other tutorials

Feel free to write
Multi-machine Gazebo simulation

Before multi-machine simulation, please ensure that the PX4 firmware can run the single-machine and dual-machine simulation routines smoothly.

Please place the file iris_3 in the PX4 firmware Firmware/posix-configs/SITL/init/ekf2/ directory (firmware version v1.8.2), and then run the script sitl_gazebo_formation .
Remote Desktop

It is recommended to use nomachine as a remote desktop.
About the author

Head of Scientific Research UAV Technology of Amu Lab, Lecturer of Mavros Training Course, PhD of BIT

If you are interested in any topic related to quadrotor drones, welcome to communicate.

Personal WeChat: qyp0210
