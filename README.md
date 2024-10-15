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
- [x] Publish the live output of the main and depth camera.
- [ ] Implement a mobile version of an image classification model (YOLO 8 for instance), which would be the most advanced one that works the best on the hardware.
- [x] Provide the CPU usage (you need to have some CPU capacity left for later specific calculations).
- [x] Publish the live results on a local IP in which the ground station be able to access the results.
- [x] You need to make the drone fully wireless, so you should make sure you will not need to have any initializations on the Jetson before your flight with getting a wired connection to it (hint: the Jetson can turn on automatically and get connected to specific wifi or even provide its own hotspot. So, by knowing its IP, you should be able to SHH into it from the ground station and run your codes.).

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
- [x] Power wirings for Pixhawk and Nvidia Jetson Xavier.
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
<p align="center">
<img width="459" alt="Screenshot 2024-08-14 at 1 38 38 PM" src="https://github.com/user-attachments/assets/36447569-fd5e-42a6-badd-03e6b59d2bb4">
</p>
<p align="center"><strong>Figure 1.</strong></p>

Holybro X500v2 Assembly Instructions: https://docs.px4.io/main/en/frames_multicopter/holybro_x500V2_pixhawk5x.html#assembly:~:text=Kit%20Hardware-,Assembly,-PX4%20Configuration

### Assembly Instructions 

<p align="center">
<img width="682" alt="Screenshot 2024-08-14 at 1 37 46 PM" src="https://github.com/user-attachments/assets/341bb125-ca4a-4818-a6fd-764b95169bb1">
</p>
<p align="center"><strong>Figure 2.</strong></p>

#### Assembling the Frame
   1. Push the rubber grippers into the holders (Do not use any sharp items).
   2. Pass the holders through the holder bars with the battery holder bases.

<p align="center">
<img width="536" alt="Screenshot 2024-08-14 at 1 39 27 PM" src="https://github.com/user-attachments/assets/9d42c5f1-f797-434e-a664-3d52e5e7cc11">
</p>
<p align="center"><strong>Figure 3.</strong></p>

<p align="center">
<img width="537" alt="Screenshot 2024-08-14 at 1 39 46 PM" src="https://github.com/user-attachments/assets/dc6a2718-17bb-4b87-9efe-a68b73efd071">
</p>
<p align="center"><strong>Figure 4.</strong></p>

**Attach the bottom plate to the payload holder:**
   1. Gather parts as shown in Figure 5.
   2. Mount the base for the power distribution board using nylon nuts (Figure 6).
   3. Join the bottom plate to the payload holder using 8 hex screws.

<p align="center">
<img width="537" alt="Screenshot 2024-08-14 at 1 40 32 PM" src="https://github.com/user-attachments/assets/13225104-7438-4a0b-8c47-d5d162a80df6">
</p>
<p align="center"><strong>Figure 5.</strong></p>

<p align="center">
<img width="449" alt="Screenshot 2024-08-14 at 1 40 56 PM" src="https://github.com/user-attachments/assets/1ece0a77-135e-4e35-ab52-0e30adfb9391">
</p>
<p align="center"><strong>Figure 6.</strong></p>

<p align="center">
<img width="535" alt="Screenshot 2024-08-14 at 1 41 53 PM" src="https://github.com/user-attachments/assets/8864527e-eee6-4b23-98b0-fe909623d280">
</p>
<p align="center"><strong>Figure 7.</strong></p>

<p align="center">
<img width="538" alt="Screenshot 2024-08-14 at 1 42 15 PM" src="https://github.com/user-attachments/assets/2cda3cfc-3c8d-4914-b034-b248b174c72d">
</p>
<p align="center"><strong>Figure 8.</strong></p>

#### Mounting the Landing Gear
**Attach the landing gear:**
   1. Gather required parts as shown in Figure 9.
   2. Use hex screws to join the landing gears to the bottom plate.
   3. Open three hex screws on each leg stand, push them into carbon fiber pipes, and tighten them back (Figure 10).
(PICTURES TO BE ADDED)
#### Installing the Top Plate and Arms
**Mount the top plate:**
   1. Gather arms and top plate as shown in Figure 11.
   2. Ensure motor numbers on arms match those on the top plate.
   3. Pass screws through the arms, tighten nylon nuts, and connect XT30 power connectors to the power board.
   4. Pass signal wires through the top plate for later connection to Pixhawk (Figure 12).
   5. Tighten all 16 screws and nuts using a hex wrench and nut driver (Figure 13).
#### Mounting the Pixhawk 6X and M8N GPS  
**Mount the Pixhawk 6X:** 
   1. Use stickers to mount Pixhawk on the top plate, aligning the arrow on Pixhawk with the top plate arrow (Figure 13).  
**Install the M8N GPS:**  
   2. Secure the GPS mount onto the companion computer plate using 4 screws and nuts (Figure 14).  
   3. Stick the GPS to the top of the GPS mast and mount the mast, ensuring the GPS arrow points forward (Figures 15 and 16).  
(ADDPICTURESLATER) 

Wiring the Pixhawk with sensors:
![image](https://github.com/Steven-Comayagua/Surveillance-Drone-Prototype/assets/93970988/4c4af8c1-6dba-4aee-a217-9528f46ab05f)

Pixhawk 6x Wiring: https://docs.px4.io/main/en/assembly/quick_start_pixhawk6x.html

#### Printing the Mounting Pieces for Nvidia Jetson Xavier and Intel RealSense

To securely attach the Nvidia Jetson Xavier and Intel RealSense to the drone, you will need to 3D print custom mounting pieces.

1. **Download the CAD Files**:
   - For the Intel RealSense mounting, use the provided CAD print files available on the Holybro website. These files are also attached to this GitHub repository for convenience.
   - The Nvidia Jetson Xavier case was sourced from Yeggi. Ensure you download the appropriate files compatible with your model of Jetson Xavier.

2. **Prepare the 3D Printer**:
   - Ensure your 3D printer is calibrated and ready for printing.
   - Use the appropriate filament material recommended for your drone's environment (e.g., PLA, ABS, or PETG).

3. **Print the Parts**:
   - Load the CAD files into your slicing software.
   - Configure the print settings based on the filament and the desired strength of the mounting pieces.
   - Start the print and monitor the process to ensure quality.

4. **Assemble the Mounting Pieces**:
   - Once printed, assemble the parts according to the instructions provided in the CAD file documentation.
   - Test fit the Nvidia Jetson Xavier and Intel RealSense on the printed mounts to ensure a secure attachment.

<br> 

## Part 3: Initial setups

- [x] Initial frameware setup of the Pixhawk.
- [x] Connect SiK Telemetry Radio. 
- [x] Connect the radio connection to the ground station (your PC).
- [x] Connect the radio transmitter to the manual controller (RC).
- [x] If you have a problem with the compatibility, use the PS4 joystick to connect as RC for the manual flight mode.
- [x] Provide the configuration for the joystick to be the same controller as a regular flight RC.
- [x] Make sure to have a KILL switch either if you are using RC or the joystick.
- [x] Test the GPS signal.
- [x] Test - Arm the drone.
- [x] Manual flight test.

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
The radio controller setting RC option is also the best and favored option, but at this moment, our selected receiver (R86C) isn't working correctly with our Pixhawk 6x.

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

References:
- [PX4 RC Setup Guide](https://docs.px4.io/main/en/getting_started/rc_transmitter_receiver.html)
- [ArduPilot RC Input Guide](https://ardupilot.org/copter/docs/common-rc-transmitter-configuration.html)

### Testing the GPS Signal

We are using an M8N GPS module connected to the GPS1 port on the Pixhawk 6x.

1. **Connect the GPS Module**: Attach the M8N GPS module to the GPS1 port on the Pixhawk 6x.
2. **Power On the System**: Ensure the drone's power system is correctly connected and power on the system.
3. **Open Mission Planner**: Launch Mission Planner on your computer.
4. **Check GPS Status**: Navigate to the Flight Data screen and check the GPS status

### Configure ARM/DISARM Buttons in ArduPilot

2. **Set ARM/DISARM Parameters:**
   - Connect the drone to ArduPilot (QGroundControl or Mission Planner).
   - Find `ARMING_CHECK` and enable all necessary safety checks.
   - Assign ARM to `RCx_OPTION = (**CHECK ARDU**)` and DISARM to `RCx_OPTION = (**CHECK ARDU**)` (where `x` is the channel number).

4. **Bind ARM/DISARM on RC TX12 MK2:**
   - On RC TX12 MK2, open the model setup menu and map a two-position switch (e.g., Switch (**CHECK ARDU**)) to the ARM/DISARM channel.

### Step 2: Configure the KILL Switch

1. **Assign KILL Switch in ArduPilot:**
   - Set `RCx_OPTION = (**CHECK ARDU**)` (where `x` is the channel number for the KILL switch).
   - Ensure it’s on a different channel than ARM/DISARM.

2. **Bind KILL Switch on RC TX12 MK2:**
   - On RC TX12 MK2, assign a three-position switch (e.g., Switch (**CHECK ARDU**)) to the KILL function for quick emergency access.

### Step 3: Testing the Setup

1. **Test ARM Function:**
   - Power on the drone (without blades) and press the ARM switch on RC TX12 MK2.
   - Verify the motors start spinning.

2. **Test DISARM Function:**
   - Turn the DISARM switch on RC TX12 MK2.
   - Ensure that all the motors stopped moving.

3. **Test KILL Switch:**
   - Rearm the drone
   - While motors are spinning, activate the KILL switch.
   - Confirm the motors stop immediately.

Ensure all these safety features are working correctly before proceeding to attach the blades or perform any flight tests.

  <br> 

## Part 4.01: Local positioning (indoor flight) using Optitrack system.

- [x] Search for the provided documentation provided for the Pixhawk and follow the steps.
- [x] Make sure to adjust the drone's predefined coordination (X, Y, Z) with the Optitrack coordination, and fix the initial point.
- [x] Fix your initial position on the field.
- [x] Add markers to the drone and make a rigid body.
- [x] publish the data from the Optitrack side as a ROS node.
- [x] Print the location data from the ROS node to make sure everything is fine.
- [x] Do the required wiring connections to connect Jetson to Pixhawk
- [ ] Follow the steps for completing the architecture of ROS on the Jetson device.
- [x] Test the result by trying POSITION (controlling the drone with complete stability) or HOLD (it means to take off and hover in place at a stable altitude without the need for RC) mode from your flight controller (Ardupilot or QGC).

### OptiTrack Setup Guide

### Step 1: Install OptiTrack Motive
1. Visit the [OptiTrack Motive Download Page](https://optitrack.com/software/motive/) and download the latest version of Motive.
2. Run the installer and follow the prompts to install Motive on your computer.
3. Once the installation is complete, launch Motive.

### Step 2: Calibrate the Cameras
1. **Setup the Calibration Environment**
   - Ensure that all OptiTrack cameras are positioned securely and connected to the system.
   - Open Motive and select the **Calibration** tab.
2. **Start Wand Calibration**
   - Select **Wand Calibration** from the calibration options.
   - Use the OptiTrack calibration wand and wave it around the capture volume.
   - Make sure that the wand is visible to as many cameras as possible from different angles.
   - Monitor the calibration progress in Motive until it indicates sufficient data has been captured.
3. **Analyze the Calibration**
   - Once enough wand data is captured, click **Calculate** to process the calibration.
   - The software will calculate individual camera positions and orientations.
4. **Review Calibration Results**
   - After the calculation, check the calibration error values for each camera.
   - Ensure that the calibration error is within an acceptable range (generally less than 1 mm).
   - If the error is high, repeat the calibration until you get an optimal result.

### Step 3: Create a Rigid Body
1. **Setup the Rigid Body Markers**
   - Attach reflective markers to the object you want to create as a rigid body.
   - Make sure the markers are visible from multiple angles and spread across the object.
   - Mask out any unnecessary elements that were accidentally captured.
2. **Define the Rigid Body in Motive**
   - In Motive, go to the **Rigid Body** tab.
   - Select the **Markers** that you want to use to create the rigid body.
   - Right-click and select **Create Rigid Body**.
3. **Adjust Rigid Body Properties**
   - Name the rigid body appropriately.
   - Check the **Marker Count** and adjust the settings to ensure the rigid body is tracked reliably.
   - Set the **Pivot Point** if needed for specific applications.

### Step 4: Stream Data
1. **Configure Streaming Settings**

  <br> 

## Part 4.02 (optional): Visual Inertial Odometry (VIO)

- [ ] Search for the possible ways to activate POSITION (controlling the drone with complete stability) or HOLD (it means to take off and hover in place at a stable altitude without the need for RC) mode for your Pixhawk 6x and go through the steps.
- [ ] Find the Github repository or video related to Visual Inertial Odometry (VIO) for Pixhawk.
- [ ] Follow the instructions and provide any difficulties or changes needed.
- [ ] Test the VIO for indoor HOLD and POSITION mode.
