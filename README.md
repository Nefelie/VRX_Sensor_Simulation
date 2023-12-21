# VRX Sensor Simulation
This repository uses the [Virtual RobotX (VRX) simulation environment](https://github.com/osrf/vrx) to simulate camera and LiDAR sensors on an Autonomous Surface Vessel (ASV).

Following the installation instructions from [VRX](https://github.com/osrf/vrx/wiki/installation_tutorial), we source the setup.bash in order to set up our environment:

```
cd ~/vrx_ws
. install/setup.bash
```
We then launch the world:

```
ros2 launch vrx_gz competition.launch.py world:=sydney_regatta
```

## Customising The Vessel
Below we use elements from the VRX Wiki Tutorials to customise the sensor arrangement. 

We first create a new directory to store the new vessel:

```
mkdir ~/my_wamv
cd ~/my_wamv
touch empty_thruster_config.yaml
touch empty_component_config.yaml
ros2 launch vrx_gazebo generate_wamv.launch.py component_yaml:=`pwd`/empty_component_config.yaml thruster_yaml:=`pwd`/empty_thruster_config.yaml wamv_target:=`pwd`/wamv_target.urdf wamv_locked:=False
```

The last line creates a new `urdf`. We can now launch the example world with the new model:

```
ros2 launch vrx_gz competition.launch.py world:=sydney_regatta urdf:=`pwd`/wamv_target.urdf
```
