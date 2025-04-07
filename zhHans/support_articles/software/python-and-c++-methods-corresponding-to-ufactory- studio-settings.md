# 介绍Studio 的设置对应的API


* 文档说明
这篇文档主要介绍UFACTORY Studio设置页面的各项设置对应的通过Python-Python-SDK 和xArm-CPLUS-SDK 设置的方法。

* 适用范围
xArm 5/6/7/, Lite 6 和850 机械臂。基于以下版本软件测试
    * 固件版本≥2.5.0
    * UFACTORY Studio ≥2.5.0
    * xArm-Python-SDK ≥1.14.8
    * xArm-CPlus-SDK ≥1.14.2

* 注意事项
所有的设置有需要使用save_conf()接口保存设置，否则控制器重启后设置会失效。

```python
# Python 保存设置
arm.save_conf()
```
```c++
// C++ 保存设置
arm->save_conf();
```



## 设置-运动设置
### 碰撞灵敏度设置
* 功能简介
用户设置机械臂碰撞触发报错的力的大小，可设置0-5档，灵敏度值越高，则越容易触发碰撞检测。灵敏度设置为0时，碰撞检测功能将关闭。出厂默认值为3.
* xArm-Python-SDk 设置方法
```python
# 设置碰撞灵敏度3
arm.set_collision_sensitivity(3)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置碰撞灵敏度3
arm->set_collision_sensitivity(3);
```

### TCP 负载与偏移设置

#### TCP 负载设置

* 功能简介
设置机械臂末端负载的重量和重心位置，以便控制系统能够更准确地计算动力学模型，用于碰撞检测和手动模式(拖动示教)

* xArm-Python-SDK 设置方法
```python
# 设置末端负载重量为0.5kg，重心位置为[0, 0, 50]mm
arm.set_tcp_load(0.5, [0, 0, 50])
```

* xArm-CPLUS-SDK 设置方法
```c++
// 设置末端负载重量为0.5kg，重心位置为[0, 0, 50]mm
float weight = 0.5;
float center_of_gravity[3] = {0, 0, 50};
arm->set_tcp_load(weight, center_of_gravity);
```


#### TCP 偏移设置
* 功能简介
设置工具坐标系相对于机械臂末端法兰中心的偏移量，包括位置和姿态偏移。正确设置TCP偏移可以使机械臂末端工具点精确定位和运动。

* xArm-Python-SDk 设置方法
```python
# 设置TCP偏移，参数为[x, y, z, roll, pitch, yaw]，单位为mm和°
arm.set_tcp_offset([0, 0, 100, 0, 0, 0])
```

* xArm-CPlus-SDK 设置方法
```c++
// 设置TCP偏移，参数为[x, y, z, roll, pitch, yaw]，单位为mm和°
float offset[6] = {0, 0, 100, 0, 0, 0};
ret = arm->set_tcp_offset(offset);
```


### 坐标系设置
* 功能简介
设置基坐标系的偏移量，用于定义用户坐标系。通过设置基坐标系偏移，可以在不同的工作空间内方便地进行定位和操作。

* xArm-Python-SDk 设置方法
```python
# 设置基坐标系偏移，参数为[x, y, z, roll, pitch, yaw]，单位为mm和°
arm.set_world_offset([100, 100, 0, 0, 0, 0])
```

* xArm-CPlus-SDK 设置方法
```c++
// 设置基坐标系偏移，参数为[x, y, z, roll, pitch, yaw]，单位为mm和°
float offset[6] = {100, 100, 0, 0, 0, 0};
ret = arm->set_world_offset(offset);
```


### 安装方向设置
* 功能简介
设置机械臂的安装方向，用于补偿机械臂在不同安装方向下的重力。如果安装方向设置错误，可能导致机械臂的碰撞检测、手动模式(拖动示教)等功能出现异常。

* xArm-Python-SDK 设置方法

xArm-Python-SDK提供了2种设置安装方向的方法，分别是直接设置安装方向和设置重力方向向量，这两种方法是等效的。


```python
#通过设置旋转角、倾斜角直接设置安装方向，单位为°。
arm.set_mount_direction(base_tilt_deg=0,rotation_deg=0) # 如果机械臂水平安装
arm.set_mount_direction(base_tilt_deg=180,rotation_deg=0)# 如果机械臂吊装


# 通过设置重力方向向量设置安装方向，参数为[x, y, z]方向向量，
arm.set_gravity_direction([0, 0, -1]) #如果机械臂水平安装
arm.set_gravity_direction([0, 0, 1]) # 如果机械臂吊装
```

* xArm-CPLUS-SDK 设置方法
xArm-CPlus-SDK目前仅支持通过设置重力方向向量来设置安装方向。

```c++
// 设置重力方向，参数为[x, y, z]方向向量
// 如果机械臂水平安装
float gravity_dir[3] = {0, 0, -1};
ret = arm->set_gravity_direction(gravity_dir);

// 如果机械臂吊装
float gravity_dir2[3] = {0, 0, 1};
ret = arm->set_gravity_direction(gravity_dir2);
```

## 设置-安全设置

### 安全边界设置
* 功能简介
设置安全边界，用于限制机械臂的运动范围。机械臂运动过程中会实时检测位置，防止机械臂超出安全区域。
安全边界包含2项设置，安全便捷的开关和安全边界的范围。


* xArm-Python-SDK 设置方法
```python
#开启安全边界
arm.set_fence_mode(on=True)

# 设置安全边界，参数为[x_max, x_min, y_max, y_min, z_max, z_min]，单位为mm
arm.set_reduced_tcp_boundary([200, -200, 200, -200, 200, -200])
```

* xArm-CPLUS-SDK 设置方法
```c++
// 开启安全边界
arm->set_fence_mode(1);

// 设置安全边界，参数为[x_max, x_min, y_max, y_min, z_max, z_min]，单位为mm

int boundary[6] = {200, -200, 200, -200, 200, -200};
ret = arm->set_reduced_tcp_boundary(boundary);
```


### 缩减模式设置

* 功能简介
设置机械臂的缩减模式开关，用于限制机械臂的运动范围和速度。
缩减模式包含2项设置，缩减模式开关，和速度缩减，关节范围缩减。


#### 缩减模式开关
xArm-Python-SDK 设置方法
```python
# 设置缩减模式开关，参数为[enable]，取值为0或1，1为开启，0为关闭。
arm.set_reduced_mode(1)
```

* xArm-CPLUS-SDK 设置方法
```c++
// 设置缩减模式开关，参数为[enable]，取值为0或1，1为开启，0为关闭。
int enable = 1;
arm->set_reduced_mode(enable);
```

#### 设置速度缩减

设置直线运动和关节运动速度的最大运动速度。

* xArm-Python-SDK 设置方法
```python
# 设置直线运动速度缩减，单位为mm/s,设置后机械臂直线运动速度不会超过这个值。
arm.set_reduced_max_tcp_speed(230)

# 设置关节运动速度缩减，单位为°/s,设置后机械臂关节运动速度不会超过这个值。
arm.set_reduced_max_joint_speed(40)
```


* xArm-CPLUS-SDK 设置方法
```c++ 
//设置直线运动速度缩减，单位为mm/s,设置后机械臂直线运动速度不会超过这个值。
arm->set_reduced_max_tcp_speed(230);

//设置关节运动速度缩减，单位为°/s,设置后机械臂关节运动速度不会超过这个值。
arm->set_reduced_max_joint_speed(40);

```
#### 设置关节运动范围缩减

设置关节运动范围缩减，用于限制机械臂各关节的运动范围。

* xArm-Python-SDk 设置方法


```python
# 设置关节范围缩减，参数为[joint-1-min, joint-1-max, ..., joint-6-min, joint-6-max]，单位为°；
#例如设置xArm 6的关节1范围缩减[-100,100],其他关节不限制。
arm.set_reduced_joint_range([-100, 100, -117, 116, -219, 10, -360, 360, -97, 180, -360, 360])
```

* xArm-CPLUS-SDK 设置方法
```c++
//设置关节范围缩减，参数为[joint-1-min, joint-1-max, ..., joint-6-min, joint-6-max]，单位为°；
//例如设置xArm 6的关节1范围缩减[-100,100],其他关节不限制。

float joint_range[12] = {-100, 100, -117, 116, -219, 10, -360, 360, -97, 180, -360, 360};
ret = arm->set_reduced_joint_range(joint_range);
```

## 通用设置

### 辅助功能

#### 手动模式灵敏度设置
* 功能说明
设置手动模式(拖动示教)的灵敏度，范围为1-5。灵敏度为5时，拖起来最轻松。出厂默认值为3。

* xArm-Python-SDK 设置方法
```python
# 设置手动模式灵敏度为3
arm.set_teach_sensitivity(3)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置手动模式灵敏度为3
arm->set_teach_sensitivity(3);
```
### 高级设置

#### 关节加加速度设置

* 功能说明
设置关节运动的加加速度，范围[1, 28647]，单位为°/s³
* xArm-Python-SDK 设置方法
```python
# 设置关节加加速度为10,000 °/s³
arm.set_joint_jerk(10000)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置关节加加速度为10,000 °/s³
arm->set_joint_jerk(10000);
```

#### TCP 加加速度设置

* 功能说明
设置直线运动的加加速度，范围[1, 100,000]，单位为mm/s³
* xArm-Python-SDK 设置方法
```python  
# 设置TCP加加速度为10,000 mm/s³
arm.set_tcp_jerk(10000)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置TCP加加速度为10,000 mm/s³
arm->set_tcp_jerk(10000);
```

#### 碰撞回弹设置

* 功能说明
设置机械臂碰撞检测后是否要进行回弹，1为开启，0为关闭。出厂默认开启。
* xArm-Python-SDK 设置方法
```python
# 设置碰撞回弹为开启
arm.set_collision_rebound(1)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置碰撞回弹为开启
arm->set_collision_rebound(1);
```
#### 自碰撞检测设置

##### 自碰撞检测开关

* 功能说明
设置机械臂是否进行自碰撞检测，1为开启，0为关闭。出厂默认开启。
* xArm-Python-SDK 设置方法
```python
# 设置自碰撞检测为开启
arm.set_self_collision_detection(1)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 设置自碰撞检测为开启
arm->set_self_collision_detection(1);
```
##### 自碰撞模型设置

* 功能说明
设置机械臂自碰撞检测的三维模型，机械臂会基于设置的三维模型实时检测碰撞。三维模型参数如下。
    * 0: 无末端执行器 
    * 1: 机械爪 
    * 2: 真空吸头 
    * 3: xArm BIO 夹爪 (含xArm BIO G2)
    * 4: Robotiq-2F-85 机械爪
    * 5: Robotiq-2F-140  机械爪
    * 21: 自定义圆柱的半径和高度，单位为 mm 
    * 22: 自定义长方体的长宽高，单位为 mm 


例如，设置一个长、宽、高为100、150、200 mm的长方体，x/y/z方向与机械臂末端工具坐标系方向一致。
* xArm-Python-SDK 设置方法
```python
arm.set_collision_tool_model(22, x=100, y=150, z=200)
```
* xArm-CPLUS-SDK 设置方法
```c++
arm->set_collision_tool_model(22, 100, 150, 200);
```

#### 是否使用近似解

* 功能说明
设置机械臂直线运动是否使用近似解，1为开启，0为关闭，出厂默认开启。
如果开启近似解，机械臂在直线运动过程中，将轻微调整位置和姿态以保证能通过直线区域，能够有效降低直线运动过程中由奇异点导致的关节超速的概率。

* xArm-Python-SDK 设置方法
```python
# 开启使用近似解
arm.set_allow_approx_motion(1)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 开启使用近似解
arm->set_allow_approx_motion(1);
```
#### 笛卡尔速度连续
* 功能说明
开启后，可实现圆弧向直线过渡的速度连续。1为开启，0为关闭，出厂默认关闭。

* xArm-Python-SDK 设置方法
```python
# 开启笛卡尔速度连续
arm.set_cartesian_velo_continuous(1)
```
* xArm-CPLUS-SDK 设置方法
```c++
// 开启笛卡尔速度连续
arm->set_cartesian_velo_continuous(1);
```
下面是一个完成的Python脚本，演示笛卡尔速度连续开关的作用。
```python
import time
from xarm.wrapper import XArmAPI
arm = XArmAPI('192.168.1.xxx') #修改为你的机械臂控制器的IP
time.sleep(0.5)

# 笛卡尔速度连续测开关，开启后圆弧与直线的过渡将变为连续。
arm.set_cartesian_velo_continuous(0)
arm.save_conf()

arm.set_mode(0)
arm.set_state(0)

# 三点画圆
arm.set_position(*[300,-100,200,180,0,0],speed=100,wait=False)
circle_pos_1=[200,0,200,180,0,0]
circle_pos_2=[300,100,200,180,0,0]
arm.move_circle(circle_pos_1,circle_pos_2,percent=100,speed=100,wait=False)
# 笛卡尔运动
arm.set_position(*[200,-100,200,180,0,0],speed=100,wait=False)
```
