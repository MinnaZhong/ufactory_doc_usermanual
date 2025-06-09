# How to get the actual kinematic parameters after kinematics calibration?

### Introduction
We added kinematics calibration to our newer product, the actual kinematic parameters will be a little different from the nominal DH parameters we provide. The results from `get_forward_kinematics()` API and Studio have already taken the calibration into account.

| Robot Arm Model           | Kinematics Calibration              |
| ------------------------- | ----------------------------------- |
| xArm1303 or lower version | No                                  |
| xArm1304 or Lite6         | Please share the SN to support team |
| xArm1305 or 850           | Yes                                 |


### Python Script
gen_kinematics_params.py
```python
import os
import sys
import socket
import struct

try:
    from yaml import dump
except:
    def dump(data, f, indent=0, **kwargs):
        buf = []
        for key, val in data.items():
            if isinstance(val, dict):
                buf.append('{}{}:'.format(' ' * indent, key))
                buf += dump(val, None, indent=indent+2, **kwargs)
            else:
                buf.append('{}{}: {}'.format(' ' * indent, key, val))
        if f is not None:
            f.write('\n'.join(buf))
        return buf

if __name__ == '__main__':
    if len(sys.argv) < 3:
        print('Usage: {} {{robot_ip}} {{kinematics_suffix (to distinguish your multiple arms)}}'.format(sys.argv[0]))
        exit(1)
    robot_ip = sys.argv[1]
    kinematics_suffix = sys.argv[2]
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((robot_ip, 502))

    send_data = [0x00, 0x01, 0x00, 0x02, 0x00, 0x01, 0x08]
    sock.send(bytes(send_data))
    recv_data = sock.recv(179)
    if len(recv_data) == 179 and recv_data[8]:
        robot_dof = recv_data[9]
        robot_type = recv_data[10]
        robot_name = 'uf850' if robot_dof == 6 and robot_type == 12 else 'lite6' if robot_dof == 6 and robot_type == 9 else 'xarm{}'.format(robot_dof)
        ouput_dir = os.path.join(os.path.dirname(__file__), 'user')
        if not os.path.exists(ouput_dir):
            os.makedirs(ouput_dir)
        output_file = os.path.join(ouput_dir, '{}_kinematics_{}.yaml'.format(robot_name, kinematics_suffix))
        params = struct.unpack('<42f', recv_data[11:])
        kinematics = {}
        data = {'kinematics': kinematics}
        for i in range(robot_dof):
            joint_param = {}
            kinematics['joint{}'.format(i + 1)] = joint_param
            joint_param['x'] = params[i * 6]
            joint_param['y'] = params[i * 6 + 1]
            joint_param['z'] = params[i * 6 + 2]
            joint_param['roll'] = params[i * 6 + 3]
            joint_param['pitch'] = params[i * 6 + 4]
            joint_param['yaw'] = params[i * 6 + 5]
        with open(output_file, 'w', encoding='utf-8') as f:
            try:
                dump(data, f, default_flow_style=False, allow_unicode=True, sort_keys=False)
            except:
                dump(data, f, default_flow_style=False, allow_unicode=True)
        print('[Success] save to {}'.format(output_file))
    else:
        print('[Failed] recv_len={}, valid={}'.format(len(recv_data), 0 if len(recv_data) < 9 else recv_data[8]))


```

### Generation of Kinematics parameter file
```python
python gen_kinematics_params.py {robot_ip} {kinematics_suffix}
```

- robot_ip represents the IP of the robot arm. You need to connect to the robot arm to obtain the actual parameters.
- kinematics_suffix represents the suffix of the generated parameter file. If successful, the configuration file will be generated in the xarm_description/config/kinematics/user directory. If kinematics_suffix is AAA, then the corresponding file name is as follows
  - **xarm5:** user/xarm5_kinematics_AAA.yaml
  - **xarm6:** user/xarm6_kinematics_AAA.yaml
  - **xarm7:** user/xarm7_kinematics_AAA.yaml
  - **lite6:** user/lite6_kinematics_AAA.yaml
  - **uf850:** user/uf850_kinematics_AAA.yaml
- the form will not be DH any more, it is the 6DOF (x,y,z, roll pitch yaw) transformation relationship between adjacent joint coordinates, and the unit is in Meters and Radians. 

