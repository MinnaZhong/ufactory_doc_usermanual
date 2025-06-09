# 如何更新控制器电源板固件？

## 硬件
- AC1303
- AC1304
- DC13xx
- MC13xx
- AC8500
- DC8500

## 固件
- 如果电源板固件为V4.6.5版本，可能遇到C19，C33等错误。需要更新至V4.6.9版本。
- 如果电源板固件不为V4.6.5，不需要升级。

## 下载
[xarm-tool-gui-win-amd64-2.17.1](https://drive.google.com/drive/folders/1DlFYdzB7ARn-aMWK96mjEsWmGnob2RIk?usp=sharing)

## 如何查看电源板固件？
运行xarm-tool-gui，输入<u>控制器IP</u>，点击<u>连接</u>。
![](../assets/powerboard_1.png)

## 如何更新电源板固件？
1. 连接xarm-tool-gui。
2. 切换到1300/850测试工具，勾选电源驱动，点击安装，选择对应的bin文件。
3. 拍下急停并旋起，点击下一步。
![](../assets/powerboard_2.png)
1. 等待大约15s，提示安装成功。
2. 重启控制器，按下控制器上的红色电源按钮。
3. 重新连接xarm-tool-gui，查看电源板版本。