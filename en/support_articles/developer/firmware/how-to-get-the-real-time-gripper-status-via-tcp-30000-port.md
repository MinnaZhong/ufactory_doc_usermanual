# How to Obtain Real-Time Gripper Status via TCP Port 30000
## Introduction
* Firmware: ≥V2.7.100; [Download](https://drive.google.com/drive/folders/1BVaVGUbnvQjmIUxTzW0uuFDwyIS89ze_?usp=sharing)
* Python SDK: V1.17.0+; [Download](https://drive.google.com/drive/folders/1BVaVGUbnvQjmIUxTzW0uuFDwyIS89ze_?usp=sharing)
* The TCP 30000 port does not report the gripper's position, speed, current/force by default. If you need this data, you must use the SDK to enable it.

The specifications for gripper data reporting are as follows:

| Items                    | Type  | Bytes   | Length | Big_endian | Description |
| ------------------------ | ----- | ------- | ------ | ---------- | ----------- |
| Gripper Position         | INT16 | 737-738 | 2      | Big        | mm          |
| Gripper Speed            | INT16 | 739-740 | 2      | Big        | mm/s        |
| Gripper Current or Force | INT16 | 741-742 | 2      | Big        | mA          |

For the complete TCP 30000 port reporting data, please refer to:
[Data Description of TCP port](data-description-of-tcp-port.md)

## 2. Python Interface
You need to enable gripper data reporting first.
Parameter 1: Reporting Type
* 1 - Gripper;
* 2 - Gripper G2;
* 3 - BIO Gripper;
* 4/5 - Robotiq Gripper;

Parameter 2: Frequency

```python
//set gripper report type as Gripper G2, Frequency=250HZ
ret = arm.set_external_device_monitor_params(2, 250)
```

## 3. Python Example

```python
"""
Description: Get xArm gripper position from TCP 30000 port
    1. Connect to xArm
    2. Enable external device monitor for Gripper on TCP port 30000
    3. Read real-time gripper data from TCP 30000:
       - Position: bytes 737-738 (INT16 big-endian), mm
       - Speed: bytes 739-740 (INT16 big-endian), mm/s
       - Current(Force): bytes 741-742 (INT16 big-endian), mA
"""

import os
import sys
import time
import socket
import struct

sys.path.append(os.path.join(os.path.dirname(__file__), '../../..'))
from xarm.wrapper import XArmAPI

def bytes_to_u32(data):
    data_u32 = data[0] << 24 | data[1] << 16 | data[2] << 8 | data[3]
    return data_u32


def bytes_to_fp32(bytes_data, is_big_endian=False):
    return struct.unpack('>f' if is_big_endian else '<f', bytes_data)[0]


def bytes_to_int16(bytes_data, is_big_endian=False):
    return struct.unpack('>h' if is_big_endian else '<h', bytes_data)[0]


def hangle_err_warn_changed(item):
    print('ErrorCode: {}, WarnCode: {}'.format(item['error_code'], item['warn_code']))


robot_ip='192.168.1.36'
# Connect to xArm and enable external device monitor
arm = XArmAPI(robot_ip)


# Set mode: position control mode
arm.set_mode(0)
# Set state: motion state
arm.set_state(state=0)

time.sleep(0.5)

# Enable external device monitor for xArm gripper on TCP 30000
# dev_type, 1: Gripper, 2: Gripper G2, 3: BIO Gripper
# frequency=250: request frequency
print('Enabling external device monitor for xArm gripper...')
ret = arm.set_external_device_monitor_params(2, 250)
if ret == 0:
    print('External device monitor enabled successfully')
else:
    print('Failed to enable external device monitor, code:', ret)
    arm.disconnect()
    sys.exit(1)

# Wait for the setting to take effect
time.sleep(1)

# Connect to TCP 30000 port to get real-time data

robot_port = 30000
print('Connecting to TCP port 30000...')
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.setblocking(True)
sock.settimeout(1)

try:
    sock.connect((robot_ip, robot_port))
    print('Connected to TCP 30000')
except Exception as e:
    print('Failed to connect to TCP 30000:', e)
    arm.disconnect()
    sys.exit(1)

# Read data header
buffer = sock.recv(4)
while len(buffer) < 4:
    buffer += sock.recv(4 - len(buffer))
size = bytes_to_u32(buffer[:4])

print('Reading gripper data (position, speed, current)...')
print('Press Ctrl+C to exit')
print('-' * 50)


try:
    while True:
        # Receive full data packet
        buffer += sock.recv(size - len(buffer))
        if len(buffer) < size:
            continue
        
        data = buffer[:size]
        buffer = buffer[size:]
        
        # Extract gripper data (INT16 big-endian format)
        if len(data) >= 742:
            gripper_pos = bytes_to_int16(data[736:738], is_big_endian=True)
            gripper_speed = bytes_to_int16(data[738:740], is_big_endian=True)
            gripper_force = bytes_to_int16(data[740:742], is_big_endian=True)
            print('Position: {}, Speed: {}, Current: {}'.format(gripper_pos, gripper_speed, gripper_force/1000))
        else:
            print('Warning: data packet size {} is too small'.format(len(data)))

except KeyboardInterrupt:
    print('\nExiting...')
finally:
    sock.close()
    arm.disconnect()
    print('Disconnected')

```