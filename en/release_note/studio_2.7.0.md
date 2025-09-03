# v2.7.0

Released Time: 2025.08.27

## Firmware V2.7.0
1. Added self-collision detection models for the UFACTORY Gripper G2, BIO Gripper G2, RH56DFX dexterous hand, and DH-PGC-140-50 gripper.
2. Optimized self-collision detection logic: the system no longer checks for collisions between the end-effector and Joint 6 or its link.
3. G-code now supports position, velocity, and force control for UFACTORY Gripper G2, BIO Gripper G2, and Robotiq 2F-85/140 grippers.
4. G-code now supports control of the pin-connection vacuum gripper.
5. Added a G-code command to close the Lite 6 vacuum gripper.
6. Modbus TCP: added a method to trigger trajectory execution or Python scripts.
7. Added torque-sensor-based collision detection and rebound functions.
8. Added compatibility for xArm XX1305-series and xArm 850 arms whose serial number has an "E" as the third character from the end.
9. Removed invalid register commands at addresses 56 and 57.
10. Set the default RS-485 timeout to 50 ms.
11. Current-based collision detection now logs additional diagnostic data.
12. Fixed an issue where sending zero speed in joint-speed mode could still cause unintended arm motion.


## Studio V2.7.0
1. Live Control – End Effector: added Z-axis offset setting and display for custom 3D models.
2. Live Control – End Effector: the vacuum gripper now has an option button to choose the connection type in the pop-up window.
3. Live Control – End Effector: added compatibility with UFACTORY Gripper G2, RH56DFX dexterous hand, and DH-PGC-140-50 gripper.
4. Blockly – End-effector blocks: compatible with UFACTORY Gripper G2, RH56DFX dexterous hand, and DH-PGC-140-50.
5. Blockly – Filename display now shows folders, and filenames are now sorted.
6. Blockly – External Devices: added an RS-485 pass-through code block.
7. Blockly – Motion commands: the position-editing pop-up now includes end-effector selection.
8. Blockly – Motion commands: when clicking “Move”, the system now chooses axis-angle or RPY control as appropriate.
9. Blockly – Fixed an issue where variables passed into the “Wait” block were not applied.
10. Settings – External devices – Torque sensor: for xArm 850, manual mode now uses its own equivalent-mass parameter (M).
11. Settings – Motion parameters – TCP: added TCP payload and TCP offset parameters for the RH56DFX and DH-PGC-140-50.
12. Settings – External devices – I/O: all outputs from the control box and robot arm now use asynchronous commands.
13. Settings – External devices – RS-485 page: added a pass-through protocol option.
14. Settings – Torque sensor: added a collision-detection switch and parameter configuration.
15. Settings –  Assistive Features: added an environment-simulation(beta) toggle.
16. Error Handling – torque-sensor overload messages now show the overload direction.
17. Error Handling – C19 pop-up now shows the robot-arm serial number.
18. Error Handling – C33 pop-up now shows the control-box serial number.
19. Error Handling – C31 now displays the robot-arm serial number; theoretical and actual torque values are logged when the error occurs.
20. Error Handling – C60 pop-up now shows the actual linear velocity and its threshold.
21. Error Handling – S17 (status code 16): for servo firmware ≥ 4.0.18, added a “Clear Error” button.
22. Error Handling – S20 pop-up now shows the status code.
23. Error Handling – all pop-ups now include the control-box timestamp and a copy-text button.
24. Fixed various other issues.