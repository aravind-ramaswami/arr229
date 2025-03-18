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

I followed the advice given by Stephan Wagner and used the DMP to calculate the yaw values. The DMP gives a noise-resistant, bias-correcting measurement of the orientation of the IMU. It's sampling rate is also very high, ensuring a good controller performance. I followed the setup instructions described by Stephan to set up my DMP. I modified the files according to the Artemis Lab 6 DMP example script to enable the DMP and set up the following visualization. 

This video shows the DMP successfully working. 

--Insert Video here--

I added code to the setup function which initialized the DMP. 

![image](https://github.com/user-attachments/assets/76817842-7e48-42c0-96d7-6b87dcfa2287)

I implemented a function (based on Stephan Wagner's code) that gave the yaw value outputs from the DMP data.

![image](https://github.com/user-attachments/assets/16767820-92ec-4b5f-8762-5e2023a9a286)

After setting up the DMP, I started to design my controller. I used a very similar code to the last lab, but I had to implement a low-pass filter for the derivative term. I did not need to implement any derivative kick protection as the low pass filter successfully eliminated most of the spikes in the derivative term. The low pass filter will also prevent any rapid spikes in the control input when the setpoint is changed. Since the DMP gives discrete values, it makes to differentiate them. I also implemented an integrator windup protection to prevent the car from spinning out of control. I used a very similar function to drive the robot given the output from the controller. Similar to the last lab, I accounted for the, now higher, deadband for the motor. 

PID loop:

![image](https://github.com/user-attachments/assets/5758e83e-7ef8-44d2-9d99-5d476bddcbb5)

drive motors:

![image](https://github.com/user-attachments/assets/7b3a66cc-9356-465a-b085-89c2c9179c57)

main loop:

![image](https://github.com/user-attachments/assets/ac1c9d2b-2ec8-4469-aa0a-fafb157522ee)

I tuned the PID loop to get these gains: kp = 0.04, ki = 0.003, kd = 0.02

Here are some videos of the PID loop working and the corresponding data in the Jupyter Notebook. In some of these plots, the angle jumps from -180 to 180, this is because of the range of the DMP. You can also note that the car settles on the correct value very fast, often within 2 seconds. I paid a price that the car often overshoots the target before correcting back. I valued the quick settling time over low overshoot because an overshoot in angle is not very harmful since the car quickly corrects back to the right position. In practice, this means that the car will quickly get to the correct angle. 

Run 1: initial = 0, setpoint = -160, final angle = -162.74

<iframe width="560" height="315" src="https://www.youtube.com/embed/0Enht-ZLk4U?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Run 2: Initial = 0, setpoint = 30, final angle = 32.7

<iframe width="560" height="315" src="https://www.youtube.com/embed/_OTk-5bVp1I?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Run 3: Initial = 0, setpoint = - 30, final angle = -34.7

<iframe width="560" height="315" src="https://www.youtube.com/embed/YkQFPm0epak?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Run 4: initial = 0, setpoint = -90, final angle = -92.5

<iframe width="560" height="315" src="https://www.youtube.com/embed/76U9s_rmXx0?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jupyter Notebook data:

Run 1: initial = 0, setpoint = -160, final angle = -162.74

![o1n_a](https://github.com/user-attachments/assets/77cfe32c-2291-4066-b2f9-94c671c0946d)

![o1n_m](https://github.com/user-attachments/assets/380aa29a-4b14-4d3d-b40f-80c59e5c4b71)

Run 2: Initial = 0, setpoint = 30, final angle = 32.7

![o2n_a](https://github.com/user-attachments/assets/6612908e-a133-4598-8890-15efdbf2a753)

![o2n_m](https://github.com/user-attachments/assets/bc72d7e4-f601-4bd4-89e3-aefbf693a4e2)

Run 3: Initial = 0, setpoint = - 30, final angle = -34.7

![o_t3_n](https://github.com/user-attachments/assets/207cede5-f844-4fc9-98fe-d66cadd9231f)

![o_3n_m](https://github.com/user-attachments/assets/89182580-d625-4d29-9bfe-edd8d0a496bd)

Run 4: initial = 0, setpoint = -90, final angle = -92.5

![o_t4_n_m](https://github.com/user-attachments/assets/9522787c-527f-4c61-9f8f-6c7f6e89d8b1)

![o_t4_n_m_m](https://github.com/user-attachments/assets/71747e5d-68d4-4088-8c0b-167425198b24)

Finally, from this data, we can see that the DMP's frequency is far faster than the TOF sensor data (from last lab). The loop frequency was about x hz and the DMP frequency is y hz, so they are much closer than the TOF sensor. This will lead to a better controller. 

# 5000 Level Tasks

I implemented windup protection to prevent the car from rapidly spinning out of control. The videos above show the robot with integrator windup protection enabled. The image below shows how I implemented integrator windup protection. I chose to limit the error term to 5% of the maximum control gain to prevent the car from spiraling out of control. 

![image](https://github.com/user-attachments/assets/e2edc3fd-9abc-4429-a996-091838498a2f)



