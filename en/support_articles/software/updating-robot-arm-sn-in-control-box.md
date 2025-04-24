# Updating Robot Arm SN in Control Box

## Storage and Usage of Friction Files
### Storage of Friction Files
The SN displayed on the software is part of the friction file name. A friction file name looks like dynconfig_XI130501F53A17.yaml, where XI130501F53A17 is the robot arm's SN.
In early xArm models, friction files were only stored in the control box. Later, as it was discovered that users frequently replaced control boxes, causing inconsistencies between the SN stored in the control box and the SN on the robot arm label, xArm from version 1300 onwards began storing friction files both in the control box and on the robot arm. For Lite 6 and 850, SN has been stored simultaneously on both the control box and the robot arm since the first version.

### Verification of Friction Files

* Firmware version < V2.4.0
If there is no friction file in the control box, the firmware can start and run normally.
However, due to the absence of friction files, collision detection and drag teaching functions will experience abnormalities. For example, collision detection may falsely report when no collision has occurred, or fail to trigger when a collision does occur. Also, when manual mode is enabled, a joint may slowly sink, or certain joints may be very difficult to drag during teaching.

* Firmware version ≥ 2.4.0
If there is no friction file in the control box, the firmware will report a C44 error after startup, and the robot arm cannot operate.

## Methods for Reloading Friction Files After Replacing the Control Box

### Friction Loading Methods for Different Robot Arm Models

Except for earlier products like xArm 5/6/7 (versions before XX1300), friction files for robot arms are stored on the robot arm itself. After the controller firmware starts, simply press and release the emergency stop button once to reload the friction files. Refer to the table below.

| Robot Arm Model | Friction Storage Location | Friction Reload Method |
| --- | --- | --- |
| 850 | Robot Arm & Control Box | Press and release emergency stop once |
| Lite 6 | Robot Arm & Control Box | Press and release emergency stop once |
| xArm 5/6/7 (XX1300 and later versions) | Robot Arm & Control Box | Press and release emergency stop once |
| xArm 5/6/7 (versions before XX1300) | Controller | Use GUI tool to load |

### Method for Loading Friction Files Using GUI Tool for xArm 5/6/7 (Versions Before XX1300)

Contact UFACTORY technical support, send them the robot arm SN, and they will send you the friction files.
Download and run the GUI tool, download link: https://drive.google.com/drive/folders/1WOcMMRXo0XACg48d3BR2Ki-kUKioCyGm?usp=drive_link
Click the load friction file button to load the friction file. Reboot the control box to take effect.
![](../assets/load_fric.png)