# 如何更新控制器安全板固件？

## 硬件
控制器版本：
* AC1303
* AC1304
* DC13xx
* MC13xx
* AC8500
* DC8500

## 固件
* 如果安全板固件为V4.6.10版本，可能会遇到C1，C19，C39，S0，S40等错误。需要更新至V4.6.12.
* 如果安全板固件不为V4.6.10版本，不需要升级。

## 下载
下载[xarm-tool-gui-2.16.10.zip](http://update.ufactory.cc/xarm-tool-gui-2.16.10.zip)

## 如何查看安全板版本？
运行xarm-tool-gui, 输入<u>控制器IP</u>，点击<u>连接</u>。
![](../assets/safeboard_1.jpg)

## 如何更新安全板固件？
1.	连接xarm-tool-gui。
2. 切换到<u>1300/850测试工具</u>，勾选<u>安全驱动</u>，点击<u>安装</u>。
3.	拍下急停并旋起，点击下一步。
工具会自动从 <u>...\resources\firmwares\xarmboard1300</u> 文件夹加载新的固件。
![](../assets/safeboard_2.jpg)

4.	等待大约15s，提示安装成功。
![](../assets/safeboard_3.jpg)
5.	重启控制器，按下控制器上的红色电源按钮按钮。
6.	重新连接xarm-tool-gui，查看安全版版本，显示为V4.6.12则更新成功。
![](../assets/safeboard_4.jpg)
