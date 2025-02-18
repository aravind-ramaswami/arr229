# LAB 3: TOF

The purpose of this lab was to get acquainted with the TOF sensors

# Prelab

From the TOF datasheet, the i2c address was 0x52. I plan to use both TOF sensors simultaneously, so I will need to change the i2c address of one of them. I wired the XSHUT pin to A3 for one of the TOF sensors. Upon startup, I will temporarily disable this TOF sensor, assign a new i2c address, and enable it. Using both sensors simultaneously is ideal because it lets you capture data from different directions without turning, enabling better obstacle avoidance and path planning. 

Currently, I plan to place one i2c sensor on the front and one i2c sensor on the right side of the robot. With sensors on the front and side, the robot will detect obstacles in front and on the side. This could avoid colliding with walls when making turns. This sensor configuration will miss obstacles behind the robot, so I will be careful when backing up. Overall, I think having sensors facing two different directions will give the most information to the robot. 




