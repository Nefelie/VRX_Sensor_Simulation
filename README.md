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

The last line generates a new `urdf` using the `generate_wamv.launch.py`, which reads the configuration YAML files. We can now launch the example world with the new model:

```
ros2 launch vrx_gz competition.launch.py world:=sydney_regatta urdf:=`pwd`/wamv_target.urdf
```

We setup a dual aft thruster configuration within the `example_thruster_config.yaml` file:
```
engine:
  - prefix: "left"
    position: "-2.373776 1.027135 0.318237"
    orientation: "0.0 0.0 0.0"
  - prefix: "right"
    position: "-2.373776 -1.027135 0.318237"
    orientation: "0.0 0.0 0.0"

```

And the sensor configuration:

```
wamv_camera:
    - name: front_left_camera
      visualize: False
      x: 0.75
      y: 0.1
      z: 1.5
      R: 0.0
      P: ${radians(15)}
      Y: 0.0
      post_Y: 0.0
    - name: front_right_camera
      visualize: False
      x: 0.75
      y: -0.1
      z: 1.5
      R: 0.0
      P: ${radians(15)}
      Y: 0.0
      post_Y: 0.0
    - name: far_left_camera
      visualize: False
      x: 0.75
      y: 0.3
      z: 1.5
      R: 0.0
      P: ${radians(15)}
      Y: 0.0
      post_Y: 0.0
wamv_gps:
    - name: gps_wamv
      x: -0.85
      y: 0.0
      z: 1.3
      R: 0.0
      P: 0.0
      Y: 0.0
wamv_imu:
    - name: imu_wamv
      x: 0.3
      y: -0.2
      z: 1.3
      R: 0.0
      P: 0.0
      Y: 0.0
lidar:
    - name: lidar_wamv
      type: 16_beam
      x: 0.7
      y: 0.0
      z: 1.8
      R: 0.0
      P: ${radians(8)}
      Y: 0.0
      post_Y: 0.0
wamv_pinger:
    - sensor_name: receiver
      position: 1.0 0 -1.0
```


Then we generate the vessel URDF with the above components:

```
ros2 launch vrx_gazebo generate_wamv.launch.py component_yaml:=`pwd`/empty_component_config.yaml thruster_yaml:=`pwd`/example_thruster_config.yaml wamv_target:=`pwd`/wamv_target.urdf wamv_locked:=False
```
And then launch the simulation environment with these changes:
```
ros2 launch vrx_gz competition.launch.py world:=sydney_regatta urdf:=`pwd`/wamv_target.urdf
```
