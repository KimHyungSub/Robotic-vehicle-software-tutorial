# Robotic-vehicle-software-tutorial

Hello! It is not straightforward to precisely manipulate RVs on simulators when you especially want to simulate cyber-physical attacks/defense. <br>
I will keep sharing how I run RV software on simulators. <br>

## 1. Download and Setup
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#1-download-and-setup" target="_blank"> ArduPilot </a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/PX4#1-download-and-setup" target="_blank"> PX4 </a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/paparazzi#1-quickstart-for-ubuntu-users" target="_blank"> Paparazzi </a>

## 2. Execute control software
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#2-execute-ardupilot" target="_blank"> ArduPilot </a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/PX4#2-execute-px4-with-gazebo-simulator" target="_blank"> PX4 </a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/paparazzi#2-2-compilation-and-demo-simulation" target="_blank"> Paparazzi </a>

## 3. Tutorials
### 3-1. ArduPilot
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#3-injecting-sensor-noise" target="_blank"> Injecting sensor noise</a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#4-leveraging-an-optical-flow-sensor" target="_blank"> Leveraging an optical flow sensor</a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#5-testing-object-avoidance-algorithms" target="_blank"> Testing object avoidance algorithms</a>
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/ArduPilot#6-how-to-change-the-start-time-clock-timestamp" target="_blank"> Changing the start time clock (timestamp)</a>

### 3-2. PX4
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/PX4#3-injecting-sensor-noise-in-gazebo-simulation" target="_blank"> Injecting sensor noise</a> 
<br>
<a href="https://github.com/KimHyungSub/Robotic-vehicle-software-tutorial/tree/main/PX4#4-leveraging-an-optical-flow-sensor" target="_blank"> Leveraging an optical flow sensor</a> 
<br>
<a href="https://github.com/KimHyungSub/mmWave_ROS2_PX4_Gazebo" target="_blank"> Simulating mmWave radar with ROS2 and PX4</a> 

### 3-3. Ground Control Station (GCS) Software

#### 3-3-1. Installing QGroundControl 
When your operating system is Ubuntu 20.04 (or later version), <br>
```
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
sudo apt install libqt5gui5 -y
```
Logout and login again to enable the change to user permissions. <br>

Download <a href="https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage" target="_blank">QGroundControl </a>

When your operating system is Ubuntu 16.04, <br>
Download <a href="https://github.com/mavlink/qgroundcontrol/releases/download/v3.5.6/QGroundControl.AppImage" target="_blank">QGroundControl v3.5.6 </a>

```
chmod +x ./QGroundControl.AppImage
./QGroundControl.AppImage  (or double click)
```
#### 3-3-2. Installing Mission Planner on Ubuntu 20.04
Install dependecies <br>
```
sudo apt install mono-runtime libmono-system-windows-forms4.0-cil libmono-system-core4.0-cil libmono-system-management4.0-cil libmono-system-xml-linq4.0-cil
```

Install mono <br>
```
sudo apt install mono-complete
```
Get the lastest zipped version of Mission Planner (e.g., <a href="https://firmware.ardupilot.org/Tools/MissionPlanner/MissionPlanner-latest.zip">link</a>) <br>

Unzip in the directory you want <br>

Go into the directory <br>

Run Mission Planner with mono <br>
```
mono MissionPlanner.exe
```
