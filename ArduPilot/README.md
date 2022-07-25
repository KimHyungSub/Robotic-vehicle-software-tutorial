# Robotic-vehicle-software-tutorial

## 1. Download and Setup
```
sudo apt-get update
sudo apt-get install git
sudo apt-get install gitk git-gui

git clone https://github.com/ArduPilot/ardupilot.git ArduPilot
CD ArduPilot
git checkout [commit hash]
git submodule update --init --recursive

Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```

## 2. Execute ArduPilot
### 2-1. SITL
```
./Tools/autotest/sim_vehicle.py -v ArduCopter --console
```

### 2-1. Gazebo
#### Install Gazebo simulator
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt update
sudo apt-get install gazebo11 libgazebo11-dev
```

#### Run Gazebo simulator (Terminal 1)
```
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world
```

Open a new command window (Terminal 2)
```
./Tools/autotest/sim_vehicle.py -v ArduCopter -f gazebo-iris --console
```

## 3. Injecting sensor noise 
### 3-1. Gyro sensor
```
param set SIM_GYR1_RND 3
param set SIM_GYR2_RND 3
param set SIM_GYR3_RND 3
```

### 3-2. Accelerometer sensor
```
param set SIM_ACC1_RND 3
param set SIM_ACC2_RND 3
param set SIM_ACC3_RND 3
```

### 3-3. Air speed sensor
```
param set SIM_ARSPD1_RND 3
param set SIM_ARSPD2_RND 3
param set SIM_ARSPD3_RND 3
```

### 3-4. Barometer sensor
```
param set SIM_BARO1_RND 3
param set SIM_BARO2_RND 3
param set SIM_BARO3_RND 3
```

### 3-5. Optical flow sensor
```
param set SIM_FLOW_RND 3
```

### 3-6. Magnetometer sensor
```
param set SIM_MAG_RND 3
```

### 3-7. Sonar sensor
```
param set SIM_SONAR_RND 3
```

### 3-8. GPS
You need to set 'SIM_GPS_TYPE' parameter (controling how GPS is used) before adding GPS noise. <br>
<br>
SIM_GPS_TYPE:
- 0: Use 3D velocity & 2D position from GPS
- 1: Use 2D velocity & 2D position (GPS velocity does not contribute to altitude estimate)
- 2: Use 2D position
- 3: No GPS (will use optical flow only if available)

```
param set SIM_GPS_NOISE 3 # Add noise
param set SIM_GPS2_NOISE 3

param set SIM_GPS_VERR_X 3 # Add velocity errors
param set SIM_GPS_VERR_Y 3
param set SIM_GPS_VERR_Z 3

param set SIM_GPS2_VERR_X 3
param set SIM_GPS2_VERR_Y 3
param set SIM_GPS2_VERR_Z 3
```
