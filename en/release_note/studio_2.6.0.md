# v2.6.0

Released Time: 2025.04.10

## Firmware V2.6.0

1. Custom self-collision 3D models now support offset in X/Y/Z directions.  
2. Added read and write interfaces for activation force and torque of 6-axis force-torque sensor in manual mode.  
3. Fixed an issue where providing incomplete Cartesian motion parameters (e.g., specifying only the X component) resulted in abnormal trajectory planning.
4. Fixed an issue where blending radius less than 1 mm or exceeding the length between two points caused abnormal trajectory planning.  
5. Fixed a speed discontinuity issue at the arc-to-line transition in linear motions with arcs.  
6. Fixed various other issues.


## Studio V2.6.0
1. Live Control - Added support for the new end-effector "BIO Gripper G2".  
2. Control - Custom end-effector 3D models now support X/Y direction offset display for cuboid/cylinder models.  
3. Blockly - Added move button in the point editing page; long press to move to the current point.  
4. Blockly - Added blocks to get individual joint angles and TCP position.  
5. Blockly - Position-triggered IO now supports passing variables for position, IO number, and value.  
6. Blockly - Digital output now supports queue execution (Sync) parameter, default set to True.  
7. Blockly - Added new control blocks for "BIO Gripper G2", allowing position/speed/force control.
8. Python IDE - Added a Python IDE entry on the home page.
9. Python IDE - Allowed deletion of files in the "BlocklytoPython" folder.
10. Python IDE - Added a standalone Python IDE page, accessible via browser with "Control Box IP:18333/ide".
11. Debugging tool now limits CSV max record count to 1,000,000 rows and limits saved files to a maximum of 5.
12. Settings - External devices: Modbus TCP now supports UFACTORY private protocol.
13. Settings - Assistive features: Added quick input toggle to enable joint angle and position input in live control page.
14. Settings - Assistive features: Added network detection toggle.
15. Settings - General - Advanced: Added read-only mode toggle, which disallows editing Blockly/Gcode/Python IDE project files.  
16. Error Handling - Optimized handling of error S17; added handling for errors S54 and S55.  
17. Optimized compatibility with older iPad operating systems.  
18. Fixed various other issues.