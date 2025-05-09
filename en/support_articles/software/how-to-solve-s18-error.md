
# How to solve S18 error?

S18: Multi-turn Encoder Error'

Product: xArm series, UF 850, Lite6

UFactory version: V2.4.0+

Please note that if the arm is not used for a long time(**≥3 months**), you need to power on the arm for 6 hours every 3 months to charge the built-in battery of the arm, otherwise the encoder data may be lost and report S18 error.

![](../assets/S18.png)

### Status code = 0/8:

If the status code is 0 or 8:
* Enter 'Settings-General-Debugging Tools-Joint';
* Enter '**H101D0813V256I\***', and send;
* Press down the E stop button, **wait for 10 seconds**, and then release.


(*)stands for joint ID, can be 1-7.  
For example, H101D0813V256I3.

![](../assets/S18_2.png)



### Status code ≠ 0/8:

Please provide the SN of the robot, and take a screenshot of the error page to <[support@ufactory.cc](mailto:support@ufactory.cc)>.

###
