# VRX Sensor Simulation
This repository uses the [Virtual RobotX (VRX) simulation environment](https://github.com/osrf/vrx) to simulate camera and LiDAR sensors on an Autonomous Surface Vessel (ASV).

Following the installation instructions from [VRX](https://github.com/osrf/vrx/wiki/installation_tutorial), we source the setup.bash in order to set up our environment.

```
cd ~/vrx_ws
. install/setup.bash
```
We then launch the world

```
ros2 launch vrx_gz competition.launch.py world:=sydney_regatta
```
