# How to update the powerboard of controller?

## Hardware
- AC1303
- AC1304
- DC13xx
- MC13xx
- AC8500
- DC8500

## Firmware
- If the powerboard is V4.6.5, you may meet C19, C33 error, then need to update the powerboard to V4.6.9 to solve it.
- If the powerboard is not V4.6.5, no need to do the update.

## Download
- Windows: [xarm-tool-gui-win-amd64-2.17.1](https://drive.google.com/drive/folders/1DlFYdzB7ARn-aMWK96mjEsWmGnob2RIk?usp=sharing)

## How to check the PowerBoard Version?
Launch xarm-tool-gui, enter the <u>Robot IP</u> and click <u>Connect</u>.
![](../assets/powerboard_1.png)

## How to update the PowerBoard Version?
1. Connect with xarm-tool-gui.
2. Switch to <u>1300/850 Test tool</u>, choose <u>power driver board</u>, click <u>Install</u>, choose the bin file.
![](../assets/powerboard_2.png)
3. Press down the Emergency stop button and release, click <u>Next</u>.
4. Wait for 15 seconds, it will prompt ‘Installation Success’. The arm will reboot automatically.
5. **Power off the controller** and Power it on.
6. Enter the Robot IP and click Connect again, check the Powerboard Version.