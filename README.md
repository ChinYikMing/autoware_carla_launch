# autoware_carla_launch

The package includes launch file to run Autoware, Carla agent, and bridge ([zenoh-bridge-dds](https://github.com/eclipse-zenoh/zenoh-plugin-dds) + [zenoh_carla_bridge](https://github.com/evshary/zenoh_carla_bridge)).

## Demo

[![IMAGE ALT TEXT](http://img.youtube.com/vi/lrFucLUWbDo/0.jpg)](https://youtu.be/lrFucLUWbDo "Run multiple vehicles with Autoware Humble in Carla")

# Architecture

![image](https://user-images.githubusercontent.com/456210/232400804-e0e0a755-0f6d-4873-a8ad-f1188011c993.png)

# Prerequisites

Make sure you meet the following system requirements

* Ubuntu 20.04
* Carla 0.9.13

Install rocker for containers

```shell
sudo apt install docker.io python3-rocker
```

# Build

## Build the container for Carla bridge

* Enter into docker

```shell
./container/run-bridge-docker.sh
```

* Build Carla bridge

```shell
cd autoware_carla_launch
source env.sh
make prepare_bridge
make build_bridge
```

## Build the container for Zenoh+Autoware

* Enter into docker

```shell
./container/run-autoware-docker.sh
```

* Build Zenoh+Autoware

```shell
cd autoware_carla_launch
source env.sh
make prepare_autoware
make build_autoware
```

## Clean

* Clean the Carla bridge container

```shell
# Enter into docker
./container/run-bridge-docker.sh
# Clean
cd autoware_carla_launch
source env.sh
make clean_bridge
```

* Clean Zenoh+Autoware container

```shell
# Enter into docker
./container/run-autoware-docker.sh
# Clean
cd autoware_carla_launch
source env.sh
make clean_autoware
```

# Usage

## Carla with Autoware

The section shows how to run Autoware in Carla simulator.

1. Run Carla simulator (In native host)

```shell
./CarlaUE4.sh -quality-level=Epic -world-port=2000 -RenderOffScreen
```

2. Run zenoh_carla_bridge and Python Agent (In Carla bridge container)

```shell
# Go inside "Carla bridge container"
./container/run-bridge-docker.sh
# Run zenoh_carla_bridge and Python Agent
cd autoware_carla_launch
source env.sh
./script/run-bridge.sh
```

3. Run zenoh-bridge-dds and Autoware (In Autoware container)

```shell
# Go inside "Autoware container"
./container/run-autoware-docker.sh
# Run zenoh-bridge-dds and Autoware
cd autoware_carla_launch
source env.sh
./script/run-autoware.sh

# (Optional) If you want to drive manually, split the terminal and run the following command
source env.sh
ros2 run autoware_manual_control keyboard_control
```

## Run multiple vehicles with Autoware in Carla at the same time

1. Run Carla simulator (In native host)

```shell
./CarlaUE4.sh -quality-level=Epic -world-port=2000 -RenderOffScreen
```

2. Run zenoh_carla_bridge and Python Agent (In Carla bridge container)

```shell
# Go inside "Carla bridge container"
./run-bridge-docker.sh
# Run zenoh_carla_bridge and Python Agent
cd autoware_carla_launch
source env.sh
./script/run-bridge-two-vehicles.sh
```

3. Run zenoh-bridge-dds and Autoware for 1st vehicle (In Autoware container)

```shell
# Go inside "Autoware container"
./run-autoware-docker.sh
# Run zenoh-bridge-dds and Autoware
cd autoware_carla_launch
source env.sh
./script/run-autoware.sh v1
# Optional: If you want to assign Carla IP and FMS IP
./script/run-autoware.sh v1 127.0.0.1:7447 127.0.0.1:7887
```

4. Run zenoh-bridge-dds and Autoware for 2nd vehicle (In Autoware container)

```shell
# Go inside "Autoware container"
./run-autoware-docker.sh
# Run zenoh-bridge-dds and Autoware
cd autoware_carla_launch
source env.sh
./script/run-autoware.sh v2
# Optional: If you want to assign Carla IP and FMS IP
./script/run-autoware.sh v2 127.0.0.1:7447 127.0.0.1:7887
```

5. Now there are two rviz with separated Autoware at the same time. You can control them separately!

# FAQ

* [How to change the map](carla_map/README.md)

# Known Issues

* Generate the map: Now we're using the maps generated by Hatem.
