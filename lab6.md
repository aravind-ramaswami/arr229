# LAB 6: Orientation Control

# Prelab

I used the same Bluetooth setup described in LAB 5 to send the data here. I slightly modified some of the commands to send back angle data instead of orientation data. These commands allow me to start/stop the PID loop, receive data, and change the PID gains even while the robot is running. This will be useful in future labs. I also reset the DMP (discussed below) queue values whenever I stopped the PID loop and initialized the DMP angle (to start correcting for noise and bias) whenever the PID loop was started. 

--insert images here--

Additionally, I set up a command to change the setpoint while the car is running. While I did not use this for this lab, it will be useful in the future to be able to send a constantly changing setpoint. 

--insert image here--

# Lab Tasks

# Orientation Control

The equation for the PID loop is identical to the previous lab:

--insert image here--

In this case, the target is a specific yaw value, which is a rotation about the z-axis given how I set up my IMU data. Similar to the last lab, I decided to implement a full PID controller to maximize the performance and give myself an extra challenge. 

I followed the advice given by Stephan Wagner and used the DMP to calculate the yaw values. The DMP gives a noise-resistant, bias-correcting measurement of the orientation of the IMU. It's sampling rate is also very high, ensuring a good controller performance. I followed the setup instructions described by Stephan to set up my DMP. I modified the files according to the Artemis Lab 6 DMP example script to enable the DMP and setup the following visualization. 

This video shows the DMP successfully working. 

--Insert Video here--

I added code to the setup function which initialized the DMP. 

--insert image here--

I implemented a function that gave the yaw value outputs from the DMP data.

--insert image here--

After setting up the DMP, I started to design my controller. I used a very similar code to the last lab, but I had to implement a low-pass filter for the derivative term. I did not need to implement any derivative kick protection as the low pass filter successfully eliminated most of the spikes in the derivative term. The low pass filter will also prevent any rapid spikes in the control input when the setpoint is changed. Since the DMP gives discrete values, it makes to differentiate them. I also implemented an integrator windup protection to prevent the car from spinning out of control. I used a very similar function to drive the robot given the output from the controller. Similar to the last lab, I accounted for the, now higher, deadband for the motor. 

PID loop:

-insert image here--

drive motors:

--insert image here--

main loop:

--insert image here--

I tuned the PID loop to get these gains:

Here are some videos of the PID loop working and the corresponding data in the Jupyter Notebook:

--insert video here

--insert video here--

In this video, the robot returns to its starting point after receiving a disturbance input. 

--insert video here--

--insert image here--

--insert image here--



# 5000 Level Tasks

I implemented windup protection to prevent the car from rapidly spinning out of control. The videos above show the robot with integrator windup protection enabled. The image below shows how I implemented integrator windup protection. I chose to limit the error term to 5% of the maximum control gain to prevent the car from spiraling out of control. 

--insert image--


