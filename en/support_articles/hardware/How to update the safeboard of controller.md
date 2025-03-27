# How to update the SafeBoard of controller?

## Hardware
Controller: 
* AC300
* AC1304
* DC13xx
* MC13xx

## Firmware
* If the safeboard is V4.6.10, you may meet C1, C39, S40 error, then need to update the safeboard to V4.6.12 to solve it.
* If the safeboard is not V4.6.12, no need to do the update.

## Download
[xarm-tool-gui-2.16.10.zip](https://drive.google.com/drive/folders/1m96yfoUb2SrXt25c-ClZ6JqEgjaukS7e)

## How to check the SafeBoard Vesrion?
Launch xarm-tool-gui, enter the <u>Robot IP</u> and click <u>Connect</u>.
![](../assets/safeboard_en_1.jpg)

## How to update the SafeBoard?
1. Connect with xarm-tool-gui.
2.	Switch to <u>1300/850 Test tool</u>, choose <u>Safe driver board</u>, click <u>Install</u>.
3.	Press down the Emergency stop button and release, click <u>Next</u>, it will load the new firmware V4.0.12 from <u>... \resources\firmwares\xarmboard1300</u> folder automatically.
![](../assets/safeboard_en_2.jpg)
4.	Wait for 15 seconds, it will prompt ‘Installation Success’. The arm will reboot automatically.
![](../assets/safeboard_en_3.jpg)
5.	**Power off** the controller and Power it on.
6.	Enter the <u>Robot IP</u> and click <u>Connect</u> again, check the Safeboard Version, it should be V4.6.12.
![](../assets/safeboard_en_4.jpg)

