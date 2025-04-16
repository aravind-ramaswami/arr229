# Lab 9: Mapping

# Overview

In this lab, I used the robot to generate a room map by taking sensor measurements of various fixed points in the room. At each fixed point, the robot would spin around, taking measurements every 15 degrees. I chose to use PID on position control to achieve the best control over my robot. I was not confident in my robot's ability to turn slowly at a constant rate, so I opted to use position control. Furthermore, I can guarantee that the robot is stationary with position control, further increasing the accuracy of the measurements. I used the same orientation PID controller and motor commands from lab 6. I modified my main loop code to take four sensor measurements at every angle in 15-degree increments starting from 0 degrees. Importantly, I recorded the actual yaw angle when these sensor measurements were recorded, since I did not trust that my controller would drive the robot to the exact yaw angle. Since the actual angles were recorded, it did not matter if my orientation controller did not drive the robot to exactly the correct angle. The code is shown below.

# Orientation Controller

Main Loop Code:

![code_1](https://github.com/user-attachments/assets/644f07d0-534f-46cd-ba56-349dbc80c1d3)
![code_2](https://github.com/user-attachments/assets/edad9f07-9545-4278-a759-b8df6087923b)

The functions angular_PID_control(), angular_drive_motors(), and get_IMU_dmp() are the same functions used in lab 6. 

Orientation Control:

<iframe width="560" height="315" src="https://www.youtube.com/embed/f3X488UfSFA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The video above shows my robot turning in a full circle. Note, I did not realize that I had to make a video of this until I was writing up the lab report, so I recorded my robot spinning in a circle in an Upson hallway instead of the actual environment. From the video, we can see that the robot turns relatively on axis, but it does drift slightly. From the physical tests in the lab, my robot does not drift outside of the 1ft x 1ft grid tiles. My robot appears to drift laterally for a maximum of about 2in on either side, so I would expect the maximum error in my map to be 4in and the average error to be 2in. However, since I take 4 sensor values, they should partially compensate for this drift and increase my accuracy. 

My PID controller was quite accurate in sending the robot to the correct orientation. I wrote a line of code in my numpy array that calculates the mean error between my robot's actual yaw values and its target yaw values. During the runs, the mean error was between -0.5 and -1.5 degrees, which is good enough to create the map.

![image](https://github.com/user-attachments/assets/d96dee67-b445-4eaa-93ca-b775ae9d0b0f)

# Generating the Map

I recorded sensor measurements from the ToF sensors and generated a polar plot and Cartesian plot for each of the following fixed points: (-3,-2), (0,3), (5,3), (5,-3)

After recording the data, I converted it from distance and angle to Cartesian coordinates. I did this using the following homogenous transformation matrices. 

The 1st transformation converts the sensor measurements from the ToF sensor's frame of reference to the robot's frame of reference. Since the ToF sensor was mounted to the front of my robot and aligned with the axis of the robot, we only have to worry about the linear transformation between the center of the robot and the center of the ToF sensor. For my robot, I measured that the ToF sensor was 2.5 inches (0.0635 m) in front of the center of the robot. The ToF sensor was aligned in the y and z directions. From this, we can derive the following transformation (T1):

![image](https://github.com/user-attachments/assets/e5ef1fd3-ff07-45e5-b9c1-b62f6a15e67d)

The 2nd transformation converts the robot's frame of reference to the inertial coordinates. Assuming the robot is rotated by theta degrees, the rotation matrix is given by the standard rotation matrix about the z-axis by angle theta. Assuming the robot is at some x_r, y_r, z_r = 0 position relative to the origin, we can write down this transformation (T2):

![image](https://github.com/user-attachments/assets/6ada4f2c-eb36-40fc-980a-32010555cf25)

Let us define a vector P that represents the ToF sensor's value. Given a sensor measurement d, we can write P as:

![image](https://github.com/user-attachments/assets/ba512e06-9647-4db1-b5da-65dbdf7182d0)

Given P, we can compute the Cartesian coordinates (x_c,y_c) with the following transformation equation:

![image](https://github.com/user-attachments/assets/46aee826-ec42-416d-9fd2-f4bc61637134)

The following Python code shows how I implemented this transformation for one of the fixed points.

![tf_python](https://github.com/user-attachments/assets/5db2d7bf-4a44-4317-a13e-ae9bc7002d3c)

I applied this procedure to generate Cartesian coordinates for the measurements from each fixed point. I also plotted the polar plot for those same measurements. They are shown below. 

![map_-3_-2](https://github.com/user-attachments/assets/a75ca007-1ffd-4af7-9c5f-f543f3a5fc4e)
![map_0_3](https://github.com/user-attachments/assets/e4727d20-63c1-4a60-9c7e-65c06952e076)
![map_5_3](https://github.com/user-attachments/assets/49344087-96b1-4fd3-ba78-3947ac355e69)
![image](https://github.com/user-attachments/assets/5a20b71c-e50f-46c9-9d47-7b8a4489689f)


The polar plots and Cartesian maps seem reasonable given the measurement point. 

# Final Map

I combined the four Cartesian maps generated below into the full map by overlaying the maps generated by the robot at each point.

![map_overall](https://github.com/user-attachments/assets/318a0603-0806-441a-8298-75c42535a5ea)

I overlaid grid lines onto this map to generate a grid line version of this map. 

![final_map](https://github.com/user-attachments/assets/0b0771e2-7a1e-47ad-a98f-8887af63d501)

# Conclusion

My robot did a reasonable job of generating an accurate map of the room. It was really cool to see the robot able to map a room and apply the transformation matrices studied in class to a physical system. 
