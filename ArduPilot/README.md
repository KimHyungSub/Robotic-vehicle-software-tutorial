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

# If the command above fails to load submodule, you can try to use *https* instead of *git*.
git config --global url."https://".insteadOf git://

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
<a href="https://github.com/ArduPilot/ardupilot/blob/fe10f15e179054ff2cb48177eab195a509badc8b/libraries/AP_InertialSensor/AP_InertialSensor_SITL.cpp#L227" target="_blank">Source code</a>
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
param set SIM_FLOW_DELAY 3
```

### 3-6. Magnetometer sensor
```
param set SIM_MAG2_FAIL 1
param set SIM_MAG3_FAIL 1
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

Changing parameters below triggers GPS glitch fail-safe logic on a software version (d0210f7b8930dc336e7de2c09dfe2029290f596b).
```
param set SIM_GPS_NOISE 3 # Add noise
param set SIM_GPS2_NOISE 3

param set SIM_GPS_VERR_X 3 # Add velocity errors
param set SIM_GPS_VERR_Y 3
param set SIM_GPS_VERR_Z 3

param set SIM_GPS2_VERR_X 3
param set SIM_GPS2_VERR_Y 3
param set SIM_GPS2_VERR_Z 3

param set SIM_GPS_GLITCH_X 0.0002
param set SIM_GPS_GLITCH_Y 0.0002
param set SIM_GPS_GLITCH_Z 0.0002
```

Changing parameters below does not trigger GPS glitch fail-safe logic on a software version (d0210f7b8930dc336e7de2c09dfe2029290f596b).
```
param set SIM_GPS_NOISE 1 # Add noise
param set SIM_GPS2_NOISE 1

param set SIM_GPS_VERR_X 1 # Add velocity errors
param set SIM_GPS_VERR_Y 1
param set SIM_GPS_VERR_Z 1

param set SIM_GPS2_VERR_X 1
param set SIM_GPS2_VERR_Y 1
param set SIM_GPS2_VERR_Z 1

param set SIM_GPS_GLITCH_X 0.00001
param set SIM_GPS_GLITCH_Y 0.00001
param set SIM_GPS_GLITCH_Z 0.00001
```
### 3-9. EMI Signal Injection Attack
Reference: Paralyzing Drones via EMI Signal Injection on Sensory Communication Channels, NDSS'23

(Option 1) Turning off all sensors
```
param set SIM_GYR_FAIL_MSK 1
param set SIM_ACCEL1_FAIL 1
param set SIM_ACCEL2_FAIL 1
param set SIM_ACCEL3_FAIL 1
param set SIM_BARO_DISABLE 1
param set SIM_MAG1_FAIL 1
param set SIM_MAG2_FAIL 1
param set SIM_MAG3_FAIL 1
```

(Option 2) Add noises into all sensors except for GNSS
```
param set SIM_GYR1_RND 700
param set SIM_GYR2_RND 700
param set SIM_GYR3_RND 700
param set SIM_ACC1_RND 700
param set SIM_ACC2_RND 700
param set SIM_ACC3_RND 700
param set SIM_BARO1_RND 700
param set SIM_BARO2_RND 700
param set SIM_BARO3_RND 700
param set SIM_MAG_RND 700
param set SIM_MAG2_FAIL 1
param set SIM_MAG3_FAIL 1
```

## 4. Turning off filters
This is useful when you want to measure each filter's effect and performance.

### 4-1. Turning off Harmonic Notch filter
```
param set INS_HNTCH_ENABLE 0
```

### 4-2. Turning off Low Pass filter
Comment out the code line below (<a href="https://github.com/ArduPilot/ardupilot/blob/3521090dd5d5a5f5345d3cf437d9657f7b00630e/libraries/AP_InertialSensor/AP_InertialSensor_Backend.cpp#L209" target="_blank">here</a>)
```
// apply the low pass filter last to attentuate any notch induced noise
        gyro_filtered = _imu._gyro_filter[instance].apply(gyro_filtered);
```

## 5. Leveraging an optical flow sensor 
### 5-1. Adding a rangefinder sensor
```
param set SIM_SONAR_SCALE 10
param set RNGFND1_TYPE 1
param set RNGFND1_SCALING 10
param set RNGFND1_PIN 0
param set RNGFND1_MAX_CM 5000
param set RNGFND1_MIN_CM 0

module load graph
graph RANGEFINDER.distance # You can check measured distances.
```

### 5-2. Adding an optical flow sensor
```
param set SIM_FLOW_ENABLE 1
param set FLOW_TYPE 10

module load graph
graph OPTICAL_FLOW.flow_comp_m_x OPTICAL_FLOW.flow_comp_m_y # You can check measured (x, y) positions.
```

### 5-3. Parameter setting for fusing optical flow sensor data
If you use EKF version 3, 
```
param set EK3_SRC1_VELXY 5 # Velocity Horizontal Source
param set EK3_SRC1_POSXY 0 # Position Horizontal Source
param set EK3_SRC_OPTIONS 0
param set EK3_SRC1_POSZ 1
param set EK3_SRC1_VELZ 0
param set EK3_SRC1_YAW 1
```

If you use EKF version 2, 
```
param set EK2_SRC1_VELXY 5
param set EK2_SRC1_POSXY 0
param set EK2_SRC_OPTIONS 0
param set EK2_SRC1_POSZ 1
param set EK2_SRC1_VELZ 0
param set EK2_SRC1_YAW 1
```

### 5-4. Controling sensor fusion source
By setting EK2_GPS_TYPE/SIM_GPS_TYPE parameters, you can decide whether ArduPilot uses (i) GPS and optical flow data or (ii) just optical flow. <br>
- 0:	GPS 3D Vel and 2D Pos
- 1:	GPS 2D vel and 2D pos
- 2:	GPS 2D pos
- 3:	No GPS
```
# Stop to use GPS data when you fly a real drone
param set EK2_GPS_TYPE 3 

# Stop to use GPS data when you fly a drone on a simulator
param set SIM_GPS_TYPE 3 
```

### 5-5. Running Gazebo simulator to test the optical flow sensor
Open Terminal 1
```
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world
```

Open Terminal 2
```
./Tools/autotest/sim_vehicle.py -v ArduCopter -f gazebo-iris
```

## 6. Testing object avoidance algorithms
```
param set AVOID_ENABLE 7
param set FENCE_ENABLE 1
param set FENCE_RADIUS 10000
param set OA_TYPE 2
param set OA_MARGIN_MAX 10
```
By setting OA_TYPE parameter, you can choose the object avoidance algorithm.
- 0:	Disabled
- 1:	BendyRuler
- 2:	Dijkstra
- 3:	Dijkstra with BendyRuler

## 7. How to change the start time clock (timestamp)?
You need to change a code line (<a href="https://github.com/ArduPilot/ardupilot/blob/15cef55e97daf6800b6a7dbd82ff1994a86c6d4a/libraries/SITL/SIM_Aircraft.cpp#L53" target="_blank">here</a>).

```
# Original code line
time_now_us(0),

# Modify the code line as follows, which leads to system time wrap after 60 seconds
time_now_us(4294900000ULL * 1000ULL),
```
Q. What is the purpose of the changed time clock? <br>
A. In case of me, it was useful to test <a href="https://ardupilot.org/mavproxy/docs/modules/signing.html" target="_blank">MAVLink 2.0 Packet Signing</a>.

## 8. Troubleshooting
### 8-1. "No module named console" or "No module named map"
İf you using anaconda, you can download Mavproxy requirements with conda.
```
conda install -c anaconda wxpython
```
