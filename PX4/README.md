# Robotic-vehicle-software-tutorial

## 1. Download and Setup
```
git clone https://github.com/PX4/PX4-Autopilot.git PX4
CD PX4
git checkout [commit hash]
git submodule update --init --recursive
./Tools/setup/ubuntu.sh -y
```

## 2. Execute PX4 with Gazebo simulator
### 2-1. Quadrotor	
```
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
