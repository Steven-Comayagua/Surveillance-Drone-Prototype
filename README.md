# Custom Drone Development: From Hardware to Software

## Main Goal:

### Developer team:
- Javier C.
- Steven C.
- Leandra G.
- Weijie H.
- Edward P.

### Supervisor:
- Pooyan Ghodrati.

### Advisor:
- Dr. Bin Hu.

<br> 

## Part 1: Image classification 

- [x] Setup the Nvidia Jetson Xavier.
- [x] Connect the Intel Real Sense.
- [ ] Publish the live output of the main and depth camera.
- [ ] Implement a mobile version of an image classification model (YOLO 8 for instance), which would be the most advanced one that works the best on the hardware.
- [ ] Provide the CPU usage (you need to have some CPU capacity left for later specific calculations).
- [ ] Publish the live results on a local IP in which the ground station be able to access the results.

### Steps to do the tasks:


## - Setup the Nvidia Jetson Xavier NX

**Requirements**:
- Nvidia Jetson Xavier NX
- MicroSD card (minimum 32GB recommended)
- Linux OS (Ubuntu)
- Micro-USB cable
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


### - Install Python 3.8.10 on the Jetson Xavier NX**

**Requirements**:
- Nvidia Jetson Xavier NX
- Python 3.8.10 source code

### Instructions

1. **Download and Extract Python 3.8.10 Source Code**:
   - Open a terminal and run the following commands:
     ```bash
     cd /usr/src
     sudo wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
     sudo tar xzf Python-3.8.10.tgz
     cd Python-3.8.10
     ```
2. **Configure and Install Python 3.8.10**:
   - Run the following commands to configure and install Python 3.8.10:
     ```bash
     sudo ./configure --enable-optimizations
     sudo make altinstall
     ```
3. **Verify the Python Installation**:
   - Check the installed Python version:
     ```bash
     python3 --version
     ```
     
## - Installing OpenCV with Python 3.8.10

**Requirements**:
- Python 3.8.10 installed on the Jetson Xavier NX
- OpenCV source code and OpenCV contrib modules

### Instructions

1. **Ensure Python 3.8.10 Development Package is Installed**:
   - Ensure the development files for Python 3.8.10 are present (skip if just installed).
     ```bash
     python3 --version
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
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules -D BUILD_EXAMPLES=ON -D PYTHON_EXECUTABLE=/usr/local/bin/python3.8-D PYTHON_INCLUDE_DIR=/usr/local/include/python3.8-D PYTHON_LIBRARY=/usr/local/lib/libpython3.12.so ..
```
   - Build and install OpenCV:
     ```bash
     make -j$(nproc)
     sudo make install
     sudo ldconfig
     ```

<br> 

## Part 2: Drone hardware assembling 

- [x] Assemble the frame.
- [x] Wiring the Pixhawk with sensors.
- [x] Print the pieces for attaching the Nvidia Jetson Xavier, and the Intel Real Sense.
- [ ] Power wirings for Pixhawk and Nvidia Jetson Xavier.
- [ ] Final touch-up.
- [ ] Testing.

### Bill of Materials

**Holybro X500 V2 Frame Kit:**
- Body: Full Carbon Fiber Top & Bottom plate (144 x 144mm, 2mm thick)
- Arm: High strength & ultra-lightweight 16mm carbon fiber tubes
- Landing gear: 16mm & 10mm diameter carbon fiber tubes
- Platform board: With mounting holes for GPS & popular companion computer
- Dual 10mm Ø rod x 250 mm long rail mounting system
- Battery mount with two Battery Straps
- Hand tools for installation
- Holybro Motors: 2216 KV880 x4 (superseded - check spare parts list for the current version)
- Holybro BLHeli S ESC 20A x4 (superseded - check spare parts list for the current version)
- Propellers: 1045 x4 (superseded - check spare parts list for the current version)
- Power Distribution Board: XT60 plug for battery & XT30 plug for ESCs & peripherals
- M8N GPS

**Additional components needed which are NOT included in the kit:**
- Flight controller (Pixhawk 6X recommended)
- RC Radio Transmitter
- RC Receiver (Compatible with RC Radio)
- 433/915 MHz Telemetry Radio
- Battery (Holybro recommends a 4S 5000mAh LiPo battery)
- Camera / Camera Mount

(The Holybro X500 V2 Kit is compatible with most flight controllers supported by ArduPilot.)

### Steps to do the tasks:

Assemble the frame:
![image](https://github.com/Steven-Comayagua/Surveillance-Drone-Prototype/assets/93970988/73684d5a-c248-4cdb-af31-51f15f1df36a)

Holybro X500v2 Assembly Instructions: https://docs.px4.io/main/en/frames_multicopter/holybro_x500V2_pixhawk5x.html#assembly:~:text=Kit%20Hardware-,Assembly,-PX4%20Configuration


Wiring the Pixhawk with sensors:
![image](https://github.com/Steven-Comayagua/Surveillance-Drone-Prototype/assets/93970988/4c4af8c1-6dba-4aee-a217-9528f46ab05f)

Pixhawk 6x Wiring: https://docs.px4.io/main/en/assembly/quick_start_pixhawk6x.html


<br> 

## Part 3: Initial setups

- [x] Initial frameware setup of the Pixhawk.
- [x] Connect SiK Telemetry Radio. 
- [x] Connect the radio connection to the ground station (your PC).
- [ ] Connect the radio transmitter to the manual controller (RC).
- [x] If you have a problem with the compatibility, use the PS4 joystick to connect as RC for the manual flight mode.
- [ ] Provide the configuration for the joystick to be the same controller as a regular flight RC.
- [ ] Make sure to have a KILL switch either if you are using RC or the joystick.
- [x] Test the GPS signal.
- [x] Test - Arm the drone.
- [ ] Manual flight test.

### Steps to do the tasks:
References used to set up the Pixhawk6X frameware: 
- https://ardupilot.org/plane/docs/common-holybro-pixhawk6X.html#loading-firmware
- https://firmware.ardupilot.org/Copter/stable/Pixhawk6X/


### Calibrating SIK Telemetry Radio

We are using a SIK Telemetry Radio v3 connected to the TELEM1 port on the Pixhawk 6x. This needs to be calibrated on ArduPilot before any flight.

1. **Connect the SIK Telemetry Radio**: Plug the SIK telemetry radio into the TELEM1 port on the Pixhawk 6x.
2. **Configure the Radio in Mission Planner**:
    - Open Mission Planner.
    - Navigate to `Initial Setup` > `Optional Hardware` > `SiK Radio`.
    - Follow the instructions to set the correct parameters for the telemetry radio.
3. **Calibrate the Radio**: Ensure the telemetry radios on both the ground and air units are communicating correctly.

References:
- [Mission Planner SiK Radio Setup](https://ardupilot.org/copter/docs/common-3dr-radio.html)
  

#### Optional - Joystick Manual Test Flight

We used ArduPilot to manually fly our drone with an RC (PS4 controller in our case with the joystick option within ArduPilot). 
The radio controller setting RC option is also the best and favored option, but at this moment, our selected receiver (AR410) isn't working correctly with our Pixhawk 6x.

1. Configure all the needed joystick direction flight options, making sure to have an ARM and DISARM button.
2. Follow the steps online for detailed instructions on how to set up and configure the manual test flight.
3. Keep in mind, the manual flight is not using any sensors, so the flight of the drone will be hard to control.

References:
- [ArduPilot Joystick Setup](https://ardupilot.org/copter/docs/common-joystick.html)
- [ArduPilot Manual Flight Mode](https://ardupilot.org/copter/docs/ac2_manualmode.html)

Suggested PS4 Joystick flight mapping and (X O □ ∆)
Set ARM to "X"
Set KILL SWITCH to "O"
Set DISARM to "□"
Set Takeoff to "∆"

<p align="center">
  <img src="https://github.com/Steven-Comayagua/Surveillance-Drone-Prototype/assets/93970988/30b31ecd-bc07-4857-b21f-fc60add3907e" alt="Joystick Mapping to use for PS4 Controller">
</p>


For manual flight using the RC (Radio Control) system, here are the steps:

1. **Connecting the Receiver**:
   - Connect the R86C receiver to the Pixhawk 6x through the `RC-IN` pins.
   - Ensure the ground (black), power (red), and signal (usually white or orange) wires are properly connected to the corresponding RC pins on the Pixhawk.

2. **Binding the TX12 MK2 with the R86C Receiver**:
   - **Set the TX12 MK2 to Bind Mode**:
     - Power on the Radiomaster TX12 MK2.
     - Access the Model Setup page by long-pressing the `SYS` button.
     - Navigate to the `Internal RF` settings and set the `Mode` to `MULTI`, the `Type` to `FrSky X`, and the `Subtype` to `D16`.
     - Scroll down and select the `Bind` option.
   **Bind the R86C Receiver**:
     - Power on the R86C receiver while holding the bind button until the LED indicates it is in binding mode (typically solid or fast blinking).
     - The R86C receiver cycles through different protocols indicated by different LED flash patterns. Ensure it is in the correct mode for your transmitter (FrSky D16).
     - Once bound, the LED on the receiver should change to a solid color or a slow blink indicating a successful bind.
     - Power the receiver off and then on again to confirm the binding.
     - Exit bind mode on the TX12 MK2 and ensure the receiver LED remains in the bound state.

3. **Setting Up RC**:
   - Ensure the Radiomaster TX12 MK2 and R86C receiver are properly bound.
   - Verify the transmitter settings, ensuring channels are correctly mapped and configured.

4. **Calibrating the Controls**:
   - Use ArduPilot Mission Planner or QGroundControl to calibrate your transmitter.
   - Navigate to the `Radio Calibration` section under `Initial Setup` (Mission Planner) or the `Radio` tab under `Vehicle Setup` (QGroundControl).
   - Follow the on-screen instructions to move the sticks and switches on your transmitter to their extremes to calibrate the radio input.

5. **Configuring Safety Features**:
   - Ensure you have ARM and DISARM buttons configured on your transmitter.
   - Set up failsafe options to handle the loss of RC signal. This can be done in the `Failsafe` settings within Mission Planner or QGroundControl.
   - Verify that failsafe actions are correctly configured to ensure the safety of the vehicle in case of signal loss.

References:
- [PX4 RC Setup Guide](https://docs.px4.io/main/en/getting_started/rc_transmitter_receiver.html)
- [ArduPilot RC Input Guide](https://ardupilot.org/copter/docs/common-rc-transmitter-configuration.html)

-Kill Switch Section-


### Testing the GPS Signal

We are using an M8N GPS module connected to the GPS1 port on the Pixhawk 6x.

1. **Connect the GPS Module**: Attach the M8N GPS module to the GPS1 port on the Pixhawk 6x.
2. **Power On the System**: Ensure the drone's power system is correctly connected and power on the system.
3. **Open Mission Planner**: Launch Mission Planner on your computer.
4. **Check GPS Status**: Navigate to the Flight Data screen and check the GPS status


### Test - Arm the drone.

1. Without the blades attached to the motors, power on the drone and ensure all systems are functioning.
2. Test the ARM button: Press the ARM button and verify that the motors begin to spin.
3. Test the DISARM button: Press the DISARM button and ensure that the motors stop spinning.
4. Test the KILL switch: Press the KILL switch to confirm it immediately stops the motors in case of an emergency.

Ensure all these safety features are working correctly before proceeding to attach the blades or perform any flight tests.

  <br> 

## Part 4.01: Local positioning (indoor flight) using Optitrack system.

- [ ] Search for the provided documentation provided for the Pixhawk and follow the steps.
- [ ] Make sure to adjust the drone's predefined coordination (X, Y, Z) with the Optitrack coordination, and fix the initial point.
- [ ] Fix your initial position on the field.
- [ ] Add markers to the drone and make a rigid body.
- [ ] publish the data from the Optitrack side as a ROS node.
- [ ] Print the location data from the ROS node to make sure everything is fine.
- [ ] Do the required wiring connections to connect Jetson to Pixhawk
- [ ] Follow the steps for completing the architecture of ROS on the Jetson device.
- [ ] Test the result by trying POSITION (controlling the drone with complete stability) or HOLD (it means to take off and hover in place at a stable altitude without the need for RC) mode from your flight controller (Ardupilot or QGC).


  <br> 

## Part 4.02 (optional): Visual Inertial Odometry (VIO)

- [ ] Search for the possible ways to activate POSITION (controlling the drone with complete stability) or HOLD (it means to take off and hover in place at a stable altitude without the need for RC) mode for your Pixhawk 6x and go through the steps.
- [ ] Find the Github repository or video related to Visual Inertial Odometry (VIO) for Pixhawk.
- [ ] Follow the instructions and provide any difficulties or changes needed.
- [ ] Test the VIO for indoor HOLD and POSITION mode.
