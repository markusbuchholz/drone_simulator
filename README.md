# Drone Simulator
![drone](https://github.com/user-attachments/assets/f632a5d2-2a75-47c8-a1cc-09f20dacc4ea)

This repository offers a simulation framework designed to evaluate motion control algorithms for drones.
The framework utilizes GazeboSim, enhanced with realistic marine environment plugins and ArduPilot's Software-in-the-Loop (SITL) mode.


## Prerequisites

- Download and Install [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html) (optional).
- Install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) to support Docker to access GPU (required).
- Repository has been tested on: Ubuntu 22.04, Ubuntu 24.04, ArchLinux (Kernel 6.8).


## Build

```bash
git clone https://github.com/markusbuchholz/drone_simulator.git

cd drone_simulator/bluerov2_ardupilot_SITL/docker

sudo ./build.sh

```

## Build in Docker

Adjust in ```run.sh```.

```bash
local_gz_ws="/home/markus/underwater/drone_simulator/gz_ws"
local_SITL_Models="/home/markus/underwater/drone_simulator/SITL_Models"
```

```bash
sudo ./run.sh

colcon build

source install/setup.bash

cd ../gz_ws

colcon build --symlink-install --merge-install --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo -DBUILD_TESTING=ON -DCMAKE_CXX_STANDARD=17

source install/setup.bash

source gazebo_exports.sh
 
```
---

## Run Quadroped


Note:
- IMPORTANT: Run GazeboSim first as it contains the ArduPilot plugin. Later, start the ArduPilot SITL (ArduSub, Rover). Lastly, you can run QGroundControl.

```bash
ros2 launch move_blueboat iris_mission_simulation.launch.py

```

## Run SITL

Notes:

- The flag ```-l``` is the localization (lat,lon,alt,heading). Check your favorite location with Google Maps.
- ```sim_vehicle.py --help ``` -prints all available commands and flags.
- in ```run.sh``` adjust these two lines for your host specific:


```bash

sudo docker exec -it marine_robotics_sitl /bin/bash

cd ../ardupilot

sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --console --map

```

## Start QGC (outside Docker)

```bash
./QGroundControl.AppImage
```
Expected results,

![image](https://github.com/user-attachments/assets/f098c7f8-51c9-499f-a96f-4a36c616e2cb)


## Extra links

- [IRIS ros2-gazebo](https://ardupilot.org/dev/docs/ros2-gazebo.html#)
- [dronekit](https://dronekit-python.readthedocs.io/en/latest/about/index.html)

---

## Run SkyCat 

Similarly, it is possible to simulate the SkyCat drone. 

![image](https://github.com/user-attachments/assets/43908dab-041c-418b-b2b9-28257b66d989)

Note:
- IMPORTANT: Run GazeboSim first as it contains the ArduPilot plugin. Later, start the ArduPilot SITL (ArduSub, Rover). Lastly, you can run QGroundControl.

```bash
ros2 launch move_blueboat skycat_mission_simulation.launch.py

```

## Run SITL


```bash

sudo docker exec -it marine_robotics_sitl /bin/bash

cd ../ardupilot

sim_vehicle.py -v ArduPlane --model JSON --add-param-file=/home/blueboat_sitl/SITL_Models/Gazebo/config/skycat_tvbs.param --console --map

```

---

## PlotJuggler

```bash
ros2 run plotjuggler plotjuggler
```
