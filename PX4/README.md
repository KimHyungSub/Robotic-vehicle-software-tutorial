# Robotic-vehicle-software-tutorial

## 1. Download and Setup
```
git clone https://github.com/PX4/PX4-Autopilot.git PX4
CD PX4
git checkout [commit hash]
git submodule update --init --recursive
./Tools/setup/ubuntu.sh -y
```
Running jMAVSim with SITL to ensure that the simulation prerequisites are installed on the system
```
make px4_sitl_default jmavsim 
```
## 2. Execute PX4 with Gazebo simulator
### Takeoff position in the simulator
The default starting position is Zurich Irchel Park (lat: 47.397742, lon: 8.545594, alt: 488.0).<br>
The takeoff location in SITL Gazebo can be set using environment variables. <br>
```
export PX4_HOME_LAT=28.452386
export PX4_HOME_LON=-13.867138
export PX4_HOME_ALT=28.5
```

### 2-1. Quadrotor	
```
make clean
make distclean
make px4_sitl gazebo
```

### 2-2. Quadrotor with Optical Flow	
```
make px4_sitl gazebo_iris_opt_flow
```

### 2-3. 3DR Solo (Quadrotor)	
```
make px4_sitl gazebo_solo
```

### 2-4. Typhoon H480 (Hexrotor) (supports video streaming)	
```
make px4_sitl gazebo_typhoon_h480
```

### 2-5. Standard Plane	
```
make px4_sitl gazebo_plane
```

### 2-6. Standard Plane (with catapult launch)	
```
make px4_sitl gazebo_plane_catapult
```

### 2-7. Standard VTOL	
```
make px4_sitl gazebo_standard_vtol
```

### 2-8. Tailsitter VTOL	
```
make px4_sitl gazebo_tailsitter
```

### 2-9. Ackerman UGV (Rover)	
```
make px4_sitl gazebo_rover
```

### 2-10. Differential UGV (Rover)	
```
make px4_sitl gazebo_r1_rover
```

### 2-11. HippoCampus TUHH (UUV: Unmanned Underwater Vehicle)	
```
make px4_sitl gazebo_uuv_hippocampus
```

### 2-12. Boat (USV: Unmanned Surface Vehicle)	
```
make px4_sitl gazebo_boat
```

### 2-13. Cloudship (Airship)	
```
make px4_sitl gazebo_cloudship
```

## 3. Injecting sensor noise in Gazebo simulation

Manually add noise into each sensor in <a href="https://github.com/PX4/PX4-SITL_gazebo/blob/5610c3fb441a2f3babc8ad7a63c8c4ce3e40abfa/src/gazebo_imu_plugin.cpp#L187" target="_blank">this file</a>

This is an original code snippet. 
```
gyroscope_bias_[i] = phi_g_d * gyroscope_bias_[i] +
        sigma_b_g_d * standard_normal_distribution_(random_generator_);
```
This example code lines add noise into gyro sensors.
```
gyroscope_bias_[i] = phi_g_d * gyroscope_bias_[i] +
        sigma_b_g_d * standard_normal_distribution_(random_generator_) * 50;
```

Q. Why should we modify PX4 source code to add noise? Is there any more easy way (e.g., changing configuration parameters)?<br>
A. Unfortunately no because PX4's failure injection is broken. <br>

## 4. Leveraging an optical flow sensor
```
cd [PX4 source folder]
make px4_sitl gazebo_iris_opt_flow
```
You can configure EKF2_AID_MASK parameter to control sensor fusion sources. <br>
- 0: use GPS
- 1: use optical flow
- 2: inhibit IMU bias estimation
- 3: vision position fusion
- 4: vision yaw fusion
- 5: multi-rotor drag fusion
- 6: rotate external vision
- 7: GPS yaw fusion
- 8: vision velocity fusion

## 5. Deploying PX4 v.1.13.0 into Crazyflie 2.1
### 5-1. Build PX4
```
git clone https://github.com/PX4/PX4-Autopilot.git PX4
CD PX4 
git checkout 6823cbc4140e29568f00e1211ae60e057adb1a1f 
git submodule update --init --recursive
```

### 5-2. Upload firmware into Crazyflie 2.1
```
make bitcraze_crazyflie21_default upload
```

### 5-3. Build cfbridge for Crazyradio PA
```
git clone https://github.com/dennisss/cfbridge.git
git submodule update --init
make build
sudo make run
```

### 5-4. Open QGC and enjoy!

### 5-5. Flying Crazyflie 2.1
Altitude mode works well and is supported by the current setup.

## 6. Troubleshooting
### 6-1. No package 'eigen3' found
```
sudo apt-add-repository universe
sudo apt-get install libeigen3-dev
```

### 6-2. Could not find a package configuration file provided by "OpenCV"
#### Installing required build dependencies
```
sudo apt-get install cmake
sudo apt-get install gcc g++

sudo apt-get install python-dev python-numpy
sudo apt-get install python3-dev python3-numpy

sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev

sudo apt-get install libgtk2.0-dev
sudo apt-get install libgtk-3-dev

sudo apt-get install libpng-dev
sudo apt-get install libjpeg-dev
sudo apt-get install libopenexr-dev
sudo apt-get install libtiff-dev
sudo apt-get install libwebp-dev
```

#### Downloading and installing OpenCV
```
sudo apt-get install git
git clone https://github.com/opencv/opencv.git

mkdir build
cd build

cmake ../
make
sudo make install
```

#### Verifying the installation
```
python -c "import cv2; print(cv2.__version__)"
python3 -c "import cv2; print(cv2.__version__)"
```
```
Output
4.6.0-dev
3.2.0
```
### 6-3. Failed to import jinja2: No module named 'jinja2'
```
sudo apt remove python-jinja2
sudo apt remove python3-jinja2

pip3 install --user jinja2
```
