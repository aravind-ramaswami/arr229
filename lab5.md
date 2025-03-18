# Lab 5: Linear PID control and Linear interpolation

In this lab, I implemented PID loop and used it to control my robot to a setpoint. 

# Prelab

In this lab, I developed a linear PID loop to enable my robot to stop at a given distance before the wall. I wrote python and Arduino code to easily send and receive data from the robot to the computer. To start, I wrote four more commands that would start the PID loop, stop the PID loop, set PID gains, and then send the relevant data to the computer. 

Start/Stop:
![image](https://github.com/user-attachments/assets/2029eb5c-5667-4e45-9a66-c85487b2b4af)

Set gains:
![image](https://github.com/user-attachments/assets/2589bea1-110f-49c4-89dd-494f3dfb4cf2)

Send data:
![image](https://github.com/user-attachments/assets/80a1b411-597f-4da1-85f6-ffc4e4b7ba52)

On the computer side, I wrote a callback function to extract the relevant data and store it in separate lists for further processing.

![image](https://github.com/user-attachments/assets/19e6453b-a70d-4db8-afcc-048712530cfa)

# Lab
The governing equation for PID control is:

![image](https://github.com/user-attachments/assets/0057a58f-da07-4433-aadf-8f5793e96067)

This controller is very versatile because the three separate gains allow the controller to be tuned for a wide range of circumstances. Additionally, PID does not require any knowledge of the system’s dynamics, making it a very general controller. 

# Position Control

I developed a full PID controller to achieve the best possible position control over my robot. I initially wanted to use the short mode distance (since it has the highest sampling rate) but quickly realized that it would not give accurate measurements at 2 and 3m. I had to use the long mode to get accurate distance measurements but sacrificed sensor speed. This was a neccessary tradeoff because my robot had to work beyond 1 meter, exceeding the range of the short-mode distance sensor. I built my PID control in two separate functions.

One function (PID_Control) took the setpoint and the robot’s current position as inputs and returned a motor value between 0 and 1. The PID gains were scaled so the maximum output they would give was approximately 1 when the robot was at the maximum distance (starting position) from the wall, and zero when the robot was exactly at the desired position. I based this approach on Stephan Wagner’s and Anunth Ramaswami’s approach. I also added windup protection for the integrator term. This was tuned experimentally, but I caped the maximum integrator output at 20% of the maximum motor input. This prevents the integral term from dominating the control signal and driving the robot into the wall. I added a detivative term but did not need to implement a low pass filter or deal with a derivative kick. My performance with the raw derivative term is sufficient and I did not need to add anything more to it. 

![image](https://github.com/user-attachments/assets/3863b342-927b-464a-93fa-7597fe2994e1)

The 2nd function (drive_motors) took the motor value and direction as inputs and supplied the corresponding PWM signal to the motor. This function accounted for the deadband region of the robot’s wheels when computing the PWM value from the motor input. 

![image](https://github.com/user-attachments/assets/4c2c1dec-6099-4cb6-b6d5-ba298767fbe9)

The main loop code is constantly updating the robot’s position from the distance sensors and supplying this to the PID_control function. I automatically stopped the robot if it was within 10mm (tuned experimentally) of the target position to minimize oscillations and increase settling time. 

![image](https://github.com/user-attachments/assets/f66fa6c4-3f0d-48e3-995f-2cc0eff5aa40)



# Extrapolation




