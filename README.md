# Custom Drone Development: From Hardware to Software

## Main Goal:

### Developer team:

<br> 

## Part 1: Image classification 

- [ ] Setup the Nvidia Jetson Xavier.
- [ ] Connect the Intel Real Sense.
- [ ] Publish the live output of the main and depth camera.
- [ ] Implement a mobile version of an image classification model (YOLO 8 for instance), which would be the most advanced one that works the best on the hardware.
- [ ] Provide the CPU usage (you need to have some CPU capacity left for later specific calculations).
- [ ] Publish the live results on a local IP in which the ground station be able to access the results.

### Steps to do the tasks:


## - Setup the Nvidia Jetson Xavier NX

**Requirements**:
- Nvidia Jetson Xavier NX
- MicroSD card (minimum 32GB recommended)
- Linux environment (or virtual machine with Linux)
- USB-C cable
- Power supply cable for the Jetson Xavier NX
- Monitor, keyboard, and mouse for initial setup

### Instructions

1. **Download SDK Manager**:
   - Using a Linux environment, visit the NVIDIA Developer website and go to the NVIDIA SDK Manager section: https://developer.nvidia.com/nvidia-sdk-manager download the SDK Manager for Linux.
2. **Install SDK Manager**:
   - Open a terminal on your Linux computer and install the downloaded SDK Manager:
     ```bash
     sudo apt update
     sudo apt install ./sdkmanager_*_amd64.deb
     ```
3. **Prepare the MicroSD Card**:
   - Download the JetPack 5.1.3 image from the JetPack Download Center: https://developer.nvidia.com/embedded/jetpack
   - Use Etcher: https://www.balena.io/etcher/ to flash and formart the JetPack image onto the microSD card:
     ```bash
     sudo apt install etcher-electron
     etcher-electron
     ```
4. **Insert the MicroSD Card**:
   - Insert the microSD card into the Jetson Xavier NX slot (located under the fan).
5. **Connect and Power On the Jetson Xavier NX**:
   - Connect the USB-C cable from the Jetson Xavier NX to your Linux computer.
   - Connect the power supply to the Jetson Xavier NX.
   - Power on the Jetson Xavier NX.
6. **Launch SDK Manager**:
   - Open the SDK Manager on your Linux computer.
   - Log in with your NVIDIA Developer account.
   - Select the Jetson Xavier NX as the target hardware.
   - Choose JetPack 5.1.3 as the SDK version to install.
7. **Flash the Jetson Xavier NX**:
   - Follow the prompts in the SDK Manager to flash the Jetson Xavier NX. This process includes installing the operating system and additional SDK components.
   - Ensure that the connection type is set to `USB` and enter the correct details as prompted by the SDK Manager.
   - Complete any remaining setup steps as instructed by the SDK Manager, such as configuring network settings and installing additional software.
8. **Complete the Ubuntu Jetson Setup**:
   - After flashing, the Jetson Xavier NX will reboot.
   - Connect the monitor, keyboard, and mouse to the Jetson Xavier NX.
   - Follow the on-screen instructions to complete the Ubuntu setup, including configuring language, time zone, and user account settings.
9. **Verify Installation**:
   - Verify that the Jetson Xavier NX is correctly set up and all components are working properly.


## - Connect the Intel Real Sense


## - Install Python 3.12.2 on the Jetson Xavier NX

**Requirements**:
- Nvidia Jetson Xavier NX
- Python 3.12.2 source code

### Instructions

1. **Download and Extract Python 3.12.2 Source Code**:
   - Open a terminal and run the following commands:
     ```bash
     cd /usr/src
     sudo wget https://www.python.org/ftp/python/3.12.2/Python-3.12.2.tgz
     sudo tar xzf Python-3.12.2.tgz
     cd Python-3.12.2
     ```
2. **Configure and Install Python 3.12.2**:
   - Run the following commands to configure and install Python 3.12.2:
     ```bash
     sudo ./configure --enable-optimizations
     sudo make altinstall
     ```
3. **Verify the Python Installation**:
   - Check the installed Python version:
     ```bash
     python3.12 --version
     ```
     
## - Installing OpenCV with Python 3.12.2

**Requirements**:
- Python 3.12.2 installed on the Jetson Xavier NX
- OpenCV source code and OpenCV contrib modules

### Instructions

1. **Ensure Python 3.12.2 Development Package is Installed**:
   - Ensure the development files for Python 3.12.2 are present (skip if just installed).
     ```bash
     python3.12 --version
     ```
2. **Prepare the Build Environment for OpenCV**:
   - Install necessary dependencies by running the following command:
     ```bash
     sudo apt install -y build-essential cmake git pkg-config libgtk-3-dev \
     libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev \
     libx264-dev libjpeg-dev libpng-dev libtiff-dev gfortran openexr \
     libatlas-base-dev python3-dev python3-numpy libtbb2 libtbb-dev libdc1394-22-dev
     ```
3. **Download OpenCV and OpenCV Contrib Modules**:
   - Download the necessary source code by running the following commands:
     ```bash
     mkdir -p ~/opencv_build && cd ~/opencv_build
     git clone https://github.com/opencv/opencv.git
     git clone https://github.com/opencv/opencv_contrib.git
     ```
4. **Build and Install OpenCV**:
   - Navigate to the OpenCV directory:
     ```bash
     cd ~/opencv_build/opencv
     rm -rf build
     mkdir build && cd build
     ```
   - Run the `cmake` command with the necessary options, specifying the paths to Python 3.12:
```bash
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules -D BUILD_EXAMPLES=ON -D PYTHON_EXECUTABLE=/usr/local/bin/python3.12 -D PYTHON_INCLUDE_DIR=/usr/local/include/python3.12 -D PYTHON_LIBRARY=/usr/local/lib/libpython3.12.so ..
```
   - Build and install OpenCV:
     ```bash
     make -j$(nproc)
     sudo make install
     sudo ldconfig
     ```

<br> 

## Part 2: Drone hardware assembling 

- [ ] Assemble the frame.
- [ ] Wiring the Pixhawk with sensors.
- [ ] Power wirings for Pixhawk and Nvidia Jetson Xavier.
- [ ] Final touch-up.
- [ ] Testing.

### Steps to do the tasks:

<br> 

## Part 3: Initial setups

- [ ] Initial frameware setup of the Pixhawk.
- [ ] Connect the radio connection to the ground station (your PC).
- [ ] Connect the radio transmitter to the manual controller (RC).
- [ ] Test the GPS signal.
- [ ] Arm the drone.
- [ ] Manual flight test.

### Steps to do the tasks:
