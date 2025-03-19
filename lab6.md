# LAB 6: Orientation Control

# Prelab

I used the same Bluetooth setup described in LAB 5 to send the data here. I slightly modified some of the commands to send back angle data instead of orientation data. These commands allow me to start/stop the PID loop, receive data, and change the PID gains even while the robot is running. This will be useful in future labs. I also reset the DMP (discussed below) queue values whenever I stopped the PID loop and initialized the DMP angle (to start correcting for noise and bias) whenever the PID loop was started. 

![image](https://github.com/user-attachments/assets/6ee609b7-8fa3-4211-91bc-4622cc2664aa)

![image](https://github.com/user-attachments/assets/808a800e-eac9-458b-914c-ae8e52770640)

![image](https://github.com/user-attachments/assets/3893c8c7-7295-4a7f-a56a-6fe83e5ee95b)


Additionally, I set up a command to change the setpoint while the car is running. While I did not use this for this lab, it will be useful in the future to be able to send a constantly changing setpoint. 

![image](https://github.com/user-attachments/assets/58a0afb5-c9fd-41a4-88d1-2982147ab020)


# Lab Tasks

# Orientation Control

The equation for the PID loop is identical to the previous lab:

![image](https://github.com/user-attachments/assets/4073f73f-609b-4a52-a1d4-0544f290199c)

In this case, the target is a specific yaw value, which is a rotation about the z-axis given how I set up my IMU data. Similar to the last lab, I decided to implement a full PID controller to maximize the performance and give myself an extra challenge. 

I followed the advice given by Stephan Wagner and used the DMP to calculate the yaw values. The DMP gives a noise-resistant, bias-correcting measurement of the orientation of the IMU. Its sampling rate is also very high, ensuring a good controller performance. I followed the setup instructions described by Stephan to set up my DMP. I modified the files according to the Artemis Lab 6 DMP example script to enable the DMP and set up the following visualization. 

This video shows the DMP successfully working. 



I added code to the setup function which initialized the DMP. 

![image](https://github.com/user-attachments/assets/76817842-7e48-42c0-96d7-6b87dcfa2287)

I implemented a function (based on Stephan Wagner's code) that gave the yaw value outputs from the DMP data.

![image](https://github.com/user-attachments/assets/16767820-92ec-4b5f-8762-5e2023a9a286)

After setting up the DMP, I started to design my controller. I used a very similar code to the last lab, but I had to implement a low-pass filter for the derivative term. I also implemented derivative kick protection to protect against sudden spikes in the setpoint. The low pass filter successfully eliminated most of the spikes in the derivative term from noisy sensor measurements. The low pass filter and the derivative kick protection prevent any rapid spikes in the control input during the PID control motion. Since the DMP gives discrete values, it makes sense to differentiate them. I also implemented an integrator windup protection to prevent the car from spinning out of control. I used a very similar function to drive the robot given the output from the controller. Similar to the last lab, I accounted for the motor's now higher deadband. 

PID loop:

![image](https://github.com/user-attachments/assets/23ccda57-a3cc-436d-a6c2-8348a97df978)

drive motors:

![image](https://github.com/user-attachments/assets/7b3a66cc-9356-465a-b085-89c2c9179c57)

main loop:

![image](https://github.com/user-attachments/assets/ac1c9d2b-2ec8-4469-aa0a-fafb157522ee)

I tuned the PID loop to get these gains: kp = 0.04, ki = 0.0001, kd = 0.01

Here are some videos of the PID loop working and the corresponding data in the Jupyter Notebook. In some of these plots, the angle jumps from -180 to 180, this is because of the range of the DMP. You can see that the steady-state error of the car is very low and there is very little overshoot in my controller outputs.

Run 1: initial = 0, setpoint = 60, final angle = 61.74

<iframe width="560" height="315" src="https://www.youtube.com/embed/0Enht-ZLk4U?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Run 2: Initial = 0, setpoint = -90, final angle = -88.74

<iframe width="560" height="315" src="https://www.youtube.com/embed/_OTk-5bVp1I?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Run 3: Initial = 0, setpoint = 90, final angle = 88.74

<iframe width="560" height="315" src="https://www.youtube.com/embed/YkQFPm0epak?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jupyter Notebook data:

Run 1: initial = 0, setpoint = 60, final angle = 61.74

![lab 6_60_y](https://github.com/user-attachments/assets/aca0754f-663a-41a0-8c4f-ea2c7800beeb)
![lab 6_60_m](https://github.com/user-attachments/assets/3aa0b598-0b77-4e64-8ac7-2165efe662c4)

Run 2: Initial = 0, setpoint = -90, final angle = -88.74

![lab 6_-90_y](https://github.com/user-attachments/assets/9cdab27b-0317-4525-a34e-3105ed1cf859)
![lab 6_-90_m](https://github.com/user-attachments/assets/83ba6b67-a6ed-4879-a8f9-b0086e329636)

Run 3: Initial = 0, setpoint = 90, final angle = 88.74

![lab 6_90_y](https://github.com/user-attachments/assets/3e610867-16cb-4207-b65c-fdb7e70de5bb)
![lab 6_90_m](https://github.com/user-attachments/assets/5f8274f7-eb81-4791-9455-82299fe1c68c)


Finally, from this data, we can see that the DMP's frequency is far faster than the TOF sensor data (from the last lab). The loop frequency was about x hz and the DMP frequency is y hz, so they are much closer than the TOF sensor. This will lead to a better controller. 

# 5000 Level Tasks

I implemented windup protection to prevent the car from rapidly spinning out of control. The videos above show the robot with integrator windup protection enabled. The image below shows how I implemented integrator windup protection. I chose to limit the error term to 10% of the maximum control gain to prevent the car from spiraling out of control. The linked videos in this lab demonstrate the controller with integrator windup protection enabled. Without enabling it, the car will rapidly spin. 

![image](https://github.com/user-attachments/assets/e2edc3fd-9abc-4429-a996-091838498a2f)



