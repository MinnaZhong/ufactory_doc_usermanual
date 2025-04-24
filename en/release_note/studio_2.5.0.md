# v2.5.x

Released Time: 2024.09.23

## Firmware V2.5.5

1. Compatible with the xArm XF/XI/XS 1305 series robots.
2. Compatible with the DG1000 series mini control box.
3. Added several new M commands and synchronous/asynchronous logic to G-code.
4. Added a switch for continuous Cartesian velocity control.
5. Increased the maximum number of connections to 4 for the TCP 30000 data reporting port.
6. Controller IO and robot IO support synchronous/asynchronous logic.
7. Added commands for joint motion control for individual or multiple joints.
8. xArm 5 supports iterative solving and absolute precision calibration.
9. Added an interface to query the maximum joint speed of recorded trajectories.
10. Supports Moveit control for simulated robot.
11. Optimized linear motion with TCP offset.
12. Optimized the dynamics parameters of the 850 model.
13. Optimized the issue of false collision detections due to unloaded joint friction at low speeds.
14. Optimized the delay of CI triggered reduced mode during continuous motion.
15. Added logic to disable the six-axis force-torque sensor after the robot is powered off.
16. Optimized the overload threshold for the six-axis force-torque sensor.
17. Optimized the logic for obtaining PID parameters when powered on.
18. Optimized the protective logic in manual mode at joint limit positions.
19. Fixed the issue where the RPY mode did not respond to 0.1 degree step adjustments.
20. Fixed the issue where frequent button presses to set state 6 sometimes did not respond.
21. Fixed the issue of position shifts when switching from the real robot to simulation mode.


## Studio V2.5.1

1. Live Control, enhanced the level of detail and refresh rate of the 3D model.
2. Live Control, added trajectory drawing functionality to the 3D model interface.
3. Live Control, compatible with the xArm 1305 series hardware.
4. Live Control, added validity checks for recorded trajectories.
5. Live Control, added network detection for the motion button.
6. Live Control, added manual mode detection for the motion button.
7. Blockly, long-press file interaction is now consistent with right-click behavior.
8. Blockly, removed the logic that automatically uses [180, 0] for Roll/Pitch in linear motions for xArm 5.
9. Blockly, added support for TI2-4 and TO2-4 for the 850 model.
10. Blockly, added support for importing G-code projects.
11. Blockly, added a "get counter" code block.
12. Blockly, added support for Modbus TCP for external devices.
13. Blockly, added logic to automatically refresh the page after a restart.
14. Blockly, differentiate between the "Move" and "Move [Variable]" modules.
15. Blockly, added name validation logic when importing files.
16. Blockly, fixed the issue where pressing CTRL+Z to undo would clear the content.
17. Blockly, added a prompt for error code A105.
18. Python IDE, added example files.
19. Python IDE, supports creation and manipulation of .csv/.txt files.
20. G-code, added pop-up prompts and cleared printed logs when running a simulation project.
21. Settings-IO, offline tasks now support G-code projects.
22. Settings-IO, compatible with the additional two digital IO at the end of the xArm 1305 robot.
23. Settings-IO, added operations for selecting categories, folders, and files in offline tasks.
24. Settings-IO, fixed the issue where the Blockly "Align" code block would not execute when triggered by an offline task.
25. Settings-External Devices-Torque Sensor, added safety prompts for the J1 joint cylindrical singularity region when enabling torque teaching.
26. Settings-External Devices, fixed the issue where the controller error C52 would not trigger a pop-up prompt when turning on the torque manual mode.
27. Settings-Check for Updates, added SN verification for upgrades.
28. Settings-Debugging Tools, supports parameter observation for all joints.
29. Settings-Debugging Tools, removed shortcut commands.
30. Settings-Advanced, added a switch for continuous Cartesian velocity control.
31. Settings-Advanced, changed the wording of "Bypassing Singularities" to "Use Approximate Solution..
32. Error Handling, C31 printing collision sensitivity, TCP payload, and mounting direction.
33. Error Handling, added S58 torque command timeout error.
34. Error Handling, improved handling of S17 errors in cases without an SN.
35. Error Handling, optimized the looping query logic for some error codes.