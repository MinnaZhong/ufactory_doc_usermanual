# Introduction to Studio Settings API


* Document Description
This document introduces the methods to configure settings in UFACTORY Studio through Python-SDK and xArm-CPLUS-SDK, corresponding to the settings page in UFACTORY Studio.

* Scope of Application
xArm 5/6/7/, Lite 6 and 850 robotic arms. Tested with the following software versions:
    * Firmware version ≥2.5.0
    * UFACTORY Studio ≥2.5.0
    * xArm-Python-SDK ≥1.14.8
    * xArm-CPlus-SDK ≥1.14.2

* Important Notes
All settings need to be saved using the save_conf() interface, otherwise the settings will be lost after controller restart.

```python
# Python save settings
arm.save_conf()
```
```c++
// C++ save settings
arm->save_conf();
```



## Settings - Motion Settings
### Collision Sensitivity Settings
* Function Overview
Users can set the force threshold for triggering collision detection errors, with settings from 0-5. Higher sensitivity values make it easier to trigger collision detection. When sensitivity is set to 0, collision detection is disabled. The factory default value is 3.
* xArm-Python-SDK Setting Method
```python
# Set collision sensitivity to 3
arm.set_collision_sensitivity(3)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set collision sensitivity to 3
arm->set_collision_sensitivity(3);
```

### TCP Payload and TCP Offset Settings

#### TCP Payload Settings

* Function Overview
Set the weight and center of gravity of the end effector so that the control system can calculate the dynamic model more accurately, used for collision detection and manual mode (drag teaching)

* xArm-Python-SDK Setting Method
```python
# Set the payload to 0.5 kg with its center of gravity at [0, 0, 50] mm.
arm.set_tcp_load(0.5, [0, 0, 50])
```

* xArm-CPLUS-SDK Setting Method
```c++
// Set the payload to 0.5 kg with its center of gravity at [0, 0, 50] mm.
float weight = 0.5;
float center_of_gravity[3] = {0, 0, 50};
arm->set_tcp_load(weight, center_of_gravity);
```


#### TCP Offset Settings
* Function Overview
Set the offset of the tool coordinate system relative to the center of the mechanical arm's end flange, including position and attitude offset. Correctly setting the TCP offset enables precise positioning and movement of the end tool point.

* xArm-Python-SDK Setting Method
```python
# Set TCP offset, parameters are [x, y, z, roll, pitch, yaw], units are mm and °
arm.set_tcp_offset([0, 0, 100, 0, 0, 0])
```

* xArm-CPlus-SDK Setting Method
```c++
// Set TCP offset, parameters are [x, y, z, roll, pitch, yaw], units are mm and °
float offset[6] = {0, 0, 100, 0, 0, 0};
ret = arm->set_tcp_offset(offset);
```


### Coordinate System Settings
* Function Overview
Set the offset of the base coordinate system to define the user coordinate system. By setting the base coordinate system offset, positioning and operations can be conveniently performed in different workspaces.

* xArm-Python-SDK Setting Method
```python
# Set base coordinate system offset, parameters are [x, y, z, roll, pitch, yaw], units are mm and °
arm.set_world_offset([100, 100, 0, 0, 0, 0])
```

* xArm-CPlus-SDK Setting Method
```c++
// Set base coordinate system offset, parameters are [x, y, z, roll, pitch, yaw], units are mm and °
float offset[6] = {100, 100, 0, 0, 0, 0};
ret = arm->set_world_offset(offset);
```


### Mounting Direction Settings
* Function Overview
Set the mounting direction of the robotic arm to compensate for gravity in different mounting directions. If the mounting direction is set incorrectly, it may cause abnormalities in functions such as collision detection and manual mode (drag teaching).

* xArm-Python-SDK Setting Method

xArm-Python-SDK provides two methods to set the mounting direction: directly setting the mounting direction or setting the gravity direction vector. These two methods are equivalent.


```python
# Set mounting direction directly by setting rotation angle and tilt angle, units are °.
arm.set_mount_direction(base_tilt_deg=0,rotation_deg=0) # If the robotic arm is horizontally installed
arm.set_mount_direction(base_tilt_deg=180,rotation_deg=0) # If the robotic arm is ceiling mounted


# Set mounting direction by setting gravity direction vector, parameters are [x, y, z] direction vector
arm.set_gravity_direction([0, 0, -1]) # If the robotic arm is horizontally installed
arm.set_gravity_direction([0, 0, 1]) # If the robotic arm is ceiling mounted
```

* xArm-CPLUS-SDK Setting Method
xArm-CPlus-SDK currently only supports setting the mounting direction through the gravity direction vector.

```c++
// Set gravity direction, parameters are [x, y, z] direction vector
// If the robotic arm is horizontally installed
float gravity_dir[3] = {0, 0, -1};
ret = arm->set_gravity_direction(gravity_dir);

// If the robotic arm is ceiling mounted
float gravity_dir2[3] = {0, 0, 1};
ret = arm->set_gravity_direction(gravity_dir2);
```

## Settings - Safety Settings

### Safety Boundary Settings
* Function Overview
Set safety boundaries to limit the range of motion of the robotic arm. The robotic arm will detect its position in real-time during movement to prevent it from exceeding the safe area.
Safety boundary settings include 2 items: the safety boundary switch and the safety boundary range.


* xArm-Python-SDK Setting Method
```python
# Enable safety boundary
arm.set_fence_mode(on=True)

# Set safety boundary, parameters are [x_max, x_min, y_max, y_min, z_max, z_min], units are mm
arm.set_reduced_tcp_boundary([200, -200, 200, -200, 200, -200])
```

* xArm-CPLUS-SDK Setting Method
```c++
// Enable safety boundary
arm->set_fence_mode(1);

// Set safety boundary, parameters are [x_max, x_min, y_max, y_min, z_max, z_min], units are mm

int boundary[6] = {200, -200, 200, -200, 200, -200};
ret = arm->set_reduced_tcp_boundary(boundary);
```


### Reduced Mode Settings

* Function Overview
Set the reduced mode switch for the robotic arm to limit its range of motion and speed.
Reduced mode includes 2 settings: reduced mode switch, and speed reduction, joint range reduction.


#### Reduced Mode Switch
xArm-Python-SDK Setting Method
```python
# Set reduced mode switch, parameter [enable] takes value 0 or 1, 1 to enable, 0 to disable.
arm.set_reduced_mode(1)
```

* xArm-CPLUS-SDK Setting Method
```c++
// Set reduced mode switch, parameter [enable] takes value 0 or 1, 1 to enable, 0 to disable.
int enable = 1;
arm->set_reduced_mode(enable);
```

#### Set Speed Reduction

Set the maximum movement speed for linear motion and joint motion.

* xArm-Python-SDK Setting Method
```python
# Set linear motion speed reduction, unit is mm/s, after setting, the linear motion speed of the robotic arm will not exceed this value.
arm.set_reduced_max_tcp_speed(230)

# Set joint motion speed reduction, unit is °/s, after setting, the joint motion speed of the robotic arm will not exceed this value.
arm.set_reduced_max_joint_speed(40)
```


* xArm-CPLUS-SDK Setting Method
```c++ 
// Set linear motion speed reduction, unit is mm/s, after setting, the linear motion speed of the robotic arm will not exceed this value.
arm->set_reduced_max_tcp_speed(230);

// Set joint motion speed reduction, unit is °/s, after setting, the joint motion speed of the robotic arm will not exceed this value.
arm->set_reduced_max_joint_speed(40);

```
#### Set Joint Motion Range Reduction

Set joint motion range reduction to limit the range of motion for each joint of the robotic arm.

* xArm-Python-SDK Setting Method


```python
# Set joint range reduction, parameters are [joint-1-min, joint-1-max, ..., joint-6-min, joint-6-max], units are °;
# For example, set xArm 6's joint 1 range reduction to [-100,100], other joints are not limited.
arm.set_reduced_joint_range([-100, 100, -117, 116, -219, 10, -360, 360, -97, 180, -360, 360])
```

* xArm-CPLUS-SDK Setting Method
```c++
// Set joint range reduction, parameters are [joint-1-min, joint-1-max, ..., joint-6-min, joint-6-max], units are °;
// For example, set xArm 6's joint 1 range reduction to [-100,100], other joints are not limited.

float joint_range[12] = {-100, 100, -117, 116, -219, 10, -360, 360, -97, 180, -360, 360};
ret = arm->set_reduced_joint_range(joint_range);
```

## General Settings

### Assistive  Features

#### Manual Mode Sensitivity Settings
* Function Overview
Set the sensitivity of manual mode (drag teaching), range 1-5. At sensitivity 5, dragging is the easiest. The factory default value is 3.

* xArm-Python-SDK Setting Method
```python
# Set manual mode sensitivity to 3
arm.set_teach_sensitivity(3)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set manual mode sensitivity to 3
arm->set_teach_sensitivity(3);
```
### Advanced Settings

#### Joint Jerk Settings

* Function Overview
Set the jerk for joint motion, range [1, 28647], unit is °/s³
* xArm-Python-SDK Setting Method
```python
# Set joint jerk to 10,000 °/s³
arm.set_joint_jerk(10000)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set joint jerk to 10,000 °/s³
arm->set_joint_jerk(10000);
```

#### TCP Jerk Settings

* Function Overview
Set the jerk for linear motion, range [1, 100,000], unit is mm/s³
* xArm-Python-SDK Setting Method
```python  
# Set TCP jerk to 10,000 mm/s³
arm.set_tcp_jerk(10000)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set TCP jerk to 10,000 mm/s³
arm->set_tcp_jerk(10000);
```

#### Collision Rebound Settings

* Function Overview
Set whether the robotic arm should rebound after collision detection, 1 to enable, 0 to disable. Enabled by default from the factory.
* xArm-Python-SDK Setting Method
```python
# Set collision rebound to enabled
arm.set_collision_rebound(1)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set collision rebound to enabled
arm->set_collision_rebound(1);
```
#### Self-Collision Detection Settings

##### Self-Collision Detection Switch

* Function Overview
Set whether the robotic arm performs self-collision detection, 1 to enable, 0 to disable. Enabled by default from the factory.
* xArm-Python-SDK Setting Method
```python
# Set self-collision detection to enabled
arm.set_self_collision_detection(1)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Set self-collision detection to enabled
arm->set_self_collision_detection(1);
```
##### Self-Collision Model Settings

* Function Overview
Set the three-dimensional model for the robotic arm's self-collision detection. The robotic arm will detect collisions in real-time based on the set three-dimensional model. The three-dimensional model parameters are as follows.
    * 0: No end effector 
    * 1: Gripper 
    * 2: Vacuum suction head 
    * 3: xArm BIO Gripper (including xArm BIO G2)
    * 4: Robotiq-2F-85 Gripper
    * 5: Robotiq-2F-140 Gripper
    * 21: Custom cylinder radius and height, unit is mm 
    * 22: Custom cuboid length, width, and height, unit is mm 


For example, set a cuboid with length, width, and height of 100, 150, 200 mm, with x/y/z directions consistent with the robotic arm's end tool coordinate system directions.
* xArm-Python-SDK Setting Method
```python
arm.set_collision_tool_model(22, x=100, y=150, z=200)
```
* xArm-CPLUS-SDK Setting Method
```c++
arm->set_collision_tool_model(22, 100, 150, 200);
```

#### Whether to Use Approximate Solution

* Function Overview
Set whether the robotic arm uses an approximate solution for linear motion, 1 to enable, 0 to disable, enabled by default from the factory.
If the approximate solution is enabled, the robotic arm will slightly adjust its position and attitude during linear motion to ensure it can pass through the linear area, which can effectively reduce the probability of joint overspeed caused by singular points during linear motion.

* xArm-Python-SDK Setting Method
```python
# Enable approximate solution
arm.set_allow_approx_motion(1)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Enable approximate solution
arm->set_allow_approx_motion(1);
```
#### Cartesian Velocity Continuity
* Function Overview
When enabled, it can achieve velocity continuity when transitioning from an arc to a line. 1 to enable, 0 to disable, disabled by default from the factory.

* xArm-Python-SDK Setting Method
```python
# Enable Cartesian velocity continuity
arm.set_cartesian_velo_continuous(1)
```
* xArm-CPLUS-SDK Setting Method
```c++
// Enable Cartesian velocity continuity
arm->set_cartesian_velo_continuous(1);
```
Below is a complete Python script demonstrating the effect of the Cartesian velocity continuity switch.
```python
import time
from xarm.wrapper import XArmAPI
arm = XArmAPI('192.168.1.xxx') # Modify to your robotic arm controller's IP
time.sleep(0.5)

# Cartesian velocity continuity switch, when enabled, the transition between arc and line will be continuous.
arm.set_cartesian_velo_continuous(0)
arm.save_conf()

arm.set_mode(0)
arm.set_state(0)

# Draw a circle with three points
arm.set_position(*[300,-100,200,180,0,0],speed=100,wait=False)
circle_pos_1=[200,0,200,180,0,0]
circle_pos_2=[300,100,200,180,0,0]
arm.move_circle(circle_pos_1,circle_pos_2,percent=100,speed=100,wait=False)
# Cartesian motion
arm.set_position(*[200,-100,200,180,0,0],speed=100,wait=False)
```