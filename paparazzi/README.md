# Robotic-vehicle-software-tutorial

## 1. Quickstart for Ubuntu users
Ubuntu 16.04 or Ubuntu 18.04:
```
sudo add-apt-repository -y ppa:paparazzi-uav/ppa && sudo add-apt-repository -y ppa:team-gcc-arm-embedded/ppa && sudo apt-get update && sudo apt-get -f -y install paparazzi-dev paparazzi-jsbsim gcc-arm-embedded dfu-util && cd ~ && git clone --origin upstream https://github.com/paparazzi/paparazzi.git && cd ~/paparazzi && git remote update -p && git checkout -b v5.18 upstream/v5.18 && sudo cp conf/system/udev/rules/*.rules /etc/udev/rules.d/ && sudo udevadm control --reload-rules && make && ./paparazzi
```

Ubuntu 20.04
```
sudo add-apt-repository -y ppa:paparazzi-uav/ppa && sudo apt-get update && sudo apt-get -f -y install paparazzi-dev paparazzi-jsbsim python-is-python3 python3-serial gcc-arm-none-eabi gdb-multiarch dfu-util && cd ~ && git clone --origin upstream https://github.com/paparazzi/paparazzi.git && cd ~/paparazzi && git remote update -p && git checkout -b v5.18 upstream/v5.18 && sudo cp conf/system/udev/rules/*.rules /etc/udev/rules.d/ && sudo udevadm control --reload-rules && make && ./paparazzi
```

## 2. Step by step
### 2-1. Download Paparazzi
```
git clone https://github.com/paparazzi/paparazzi.git [folder name]
cd [folder name]
git checkout [commit hash]
git submodule update --init --recursive
```

### 2-2. Compilation and demo simulation
```
cd [paparazzi root folder]
make
./paparazzi
```
- Select the "Microjet" aircraft in the upper-left A/C combo box. <br>
- Select "sim" from upper-middle "target" combo box. Click "Build". <br>
- When the compilation is finished, select "Simulation" from the upper-right session combo box and click "Execute". <br>
- In the GCS, wait about 10s for the aircraft to be in the "Holding point" navigation block. <br>
- Switch to the "Takeoff" block (lower-left blue airway button in the strip). Takeoff with the green launch button. <br>

## 3. Simulating sensor attacks
### 3-1. Adding gyro noise
```
cd [paparazzi root folder]
vim ./sw/simulator/nps/nps_sensor_gyro.c
```
Changing 'bias_initial' and 'bias_random_walk_std_dev'
For example, you can add 30 degrees of noise as follows.
```
VECT3_ASSIGN(gyro->bias_initial,
               RadOfDeg(30), RadOfDeg(30), RadOfDeg(30));
...
VECT3_ASSIGN(gyro->bias_random_walk_std_dev,
               RadOfDeg(30),
               RadOfDeg(30),
               RadOfDeg(30));
```
