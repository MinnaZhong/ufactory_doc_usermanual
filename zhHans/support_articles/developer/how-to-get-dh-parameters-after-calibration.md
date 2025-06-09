# 如何获取校准后的运动学参数
### 介绍
我们在新的产品中添加了运动学校准，实际的运动学参数与我们提供的标准DH参数有所不同，`get_forward_kinematics()`API和studio的结果已经将校准考虑在内。
| 机械臂型号          | 是否做了运动学校准 |
| -------------- | --------- |
| xArm1303或更低版本  | 否         |
| xArm1304或Lite6 | 请提供SN     |
| xArm1305或850   | 是         |


### Python脚本
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

### 获取参数文件
```python
python gen_kinematics_params.py {robot_ip} {kinematics_suffix}
```

- **robot_ip** 表示机械臂IP，需要连接机械臂获取实际的参数
- **kinematics_suffix** 表示生成的参数文件的后缀，如果成功，会在python同目录下生成配置文件, 假如 kinematics_suffix 为 AAA, 那么对应的文件名如下:
  - **xarm5:** user/xarm5_kinematics_AAA.yaml
  - **xarm6:** user/xarm6_kinematics_AAA.yaml
  - **xarm7:** user/xarm7_kinematics_AAA.yaml
  - **lite6:** user/lite6_kinematics_AAA.yaml
  - **uf850:** user/uf850_kinematics_AAA.yaml
- 生成的结果不再是DH格式，它是每个连杆相对于前一个关节坐标系的6DOF(xyzrpy)变换关系，单位为米和弧度。
